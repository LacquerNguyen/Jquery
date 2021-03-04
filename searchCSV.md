# searchCSV
```javascript

    $.Name.readCSV = function(options){ //v1.0
		var defaults = {
				//Default Options
				url: '',
				complete: function(){}
			},
			s = $.extend(defaults,options),
			data = []
			//Private Variables
			;
		
		/*---- INIT ----*/
		function init(){
			if (s.url){
				$.ajax({
					type: 'GET',
					url: s.url,
					dataType: 'text',
					success: function(res){
						process_data(res.split(/\r\n|\r|\n/g));
						s.complete(data);
					},
					error: function(res){
						s.complete(data);
					}
				});
			}
		}
		function process_data(res){
			read_line(res,0,false);
		}
		function read_line(res,line,resume){
			var position = resume ? data.length - 1 : data.length,
				counter = resume ? data[position].length - 1 : 0;
			if (!resume){
				data[position] = new Array();
			}
			for (var i = 0;i < res[line].length;i++){
				if (res[line][i] == '"'){
					if (resume){
						if (i == res[line].length - 1 || res[line][i + 1] == ','){
							counter++;//New cell
							resume = false;
						}else{
							data[position][counter] += res[line][i + 1];
							i++;//Skip to next character
						}
					}else{
						resume = true;
					}
				}else{
					if (resume){
						if (data[position][counter]) data[position][counter] += res[line][i]; else data[position][counter] = res[line][i];
						if (i == res[line].length - 1) data[position][counter] += '\n';
					}else{
                        if (res[line][i] == ','){
                            counter++;
                        }else{
                            if (data[position][counter]) data[position][counter] += res[line][i]; else data[position][counter] = res[line][i];
                            if (i == res[line].length - 1){
                                counter++
                            }
                        }
					}
				}
			}
			if (line + 1 < res.length){
				read_line(res,line + 1,resume);
			}
		}

		/*---- EVENT ----*/

		/*---- PRIVATE FUNCTION ----*/

		/*---- PUBLIC ----*/
		return {
			//public function and variables
			init: init
		}
    }
    $.Name.partialSearch = function(options){
		var defaults = {
			field: '',
            dataType : 'string',
            acceptSearchAllValue : false,
            searchAllValue : '*'
		},s = $.extend(defaults,options);
		if (!(s.field)) return { active: false }

		var change = true,
			value = '';

		function test(item){
			if (value){
                if (s.acceptSearchAllValue && value == s.searchAllValue) return true;
				if (s.dataType == 'array'){
					var check = false;
					for (var count = 0; count < item[s.field].length; count++){
						if (item[s.field][count].toUpperCase().indexOf(value.toUpperCase()) !== -1){
							check = true;
							break;
						}
					}
					return check;
				}else{
					if (item[s.field].toUpperCase().indexOf(value.toUpperCase()) !== -1)
						return true;
					else
						return false
				}
			}else{
				return true
			}
		}

		return {
			active: true,
			setFilter : function(new_value){ 
				if (value != new_value.trim()){
					value = new_value.trim();
					change = true;
				}
			},
			getChangeState : function(){ return change },
			setChangeState : function(new_value){ change = new_value },
			test: test
		}
    }
    $.Name.exactlySearch = function(options){
		var defaults = {
			field: '',
            searchAllValue : '*'
		},s = $.extend(defaults,options);
		if (!(s.field)) return { active: false }

		var change = true,
			value = '';

		function test(item){
			if (value == '' || value == s.searchAllValue){
                return true
			}else{
                return item[s.field] == value;
			}
		}

		return {
			active: true,
			setFilter : function(new_value){ 
				if (value != new_value.trim()){
					value = new_value.trim();
					change = true;
				}
			},
			getChangeState : function(){ return change },
			setChangeState : function(new_value){ change = new_value },
			test: test
		}
	}
    $.Name.ageSearch = function(options){
		var defaults = {
			field: '',
            searchAllValue : '*'
		},s = $.extend(defaults,options);
		if (!(s.field)) return { active: false }

		var change = true,
			value = '';

		function test(item){
			if (value){
                return (value == s.searchAllValue) ? (item[s.field].indexOf('0') > -1) : (item[s.field][value] == '0');
			}else{
				return true
			}
		}

		return {
			active: true,
			setFilter : function(new_value){ 
				if (value != new_value.trim()){
					value = new_value.trim();
					change = true;
				}
			},
			getChangeState : function(){ return change },
			setChangeState : function(new_value){ change = new_value },
			test: test
		}
	}
    $.Name.searchAndFilter = function(options){ //v1.0
		var defaults = {
			filters: [],
			data: [],
			limit: 0
		},s = $.extend(defaults,options);

		if (!(s.filters.length)) return { active: false }
		var data = [],
			_e = new $.GFUNC.customEvent();

		function init(){
			data.length = 0;
			data = s.data.concat([]);
			_e.on('update_data',update_data);
		}

		function update_data(new_data){
			data.length = 0;
			new_data.data.forEach(function(item){
				data.push(item);
			});
			render(true);
		}

		function render(force){
			//Check change state
			var change = false,
				limit = 0,
				data_render = [];
			if (force){
				change = true;
			}else{
				for (var count = 0; count < s.filters.length; count++){
					if (s.filters[count].getChangeState()){
						change = true;
						break;
					}
				}
			}
			if (change){ //If any filters changed then it's render time
				for (var count = 0; count < s.filters.length; count++){
					s.filters[count].setChangeState(false);
				}
				data_render = data.filter(function(item){
					if (!(s.limit && limit == s.limit)){
						var pass = true;
						for (var count = 0; count < s.filters.length; count++){
							if (!s.filters[count].test(item)){
								pass = false;
								break;
							}
						}
						if (s.limit && pass) limit++;
						return pass;
					}else{
						return false;
					}
				});
				_e.trigger('render',{
					data : data_render
				});
			}
		}

		return {
			active: true,
			init: init,
			render: render,
			on: _e.on,
			off: _e.off,
			trigger: _e.trigger,
		}
	}
  if ($('#tmp_form_search .search_form').length){
    search_form();
}
function search_form(){
    var form = $('#tmp_form_search .search_form'),
        input = {
            age : form.find('[name="age_child"]'),
            type : form.find('[name="facility"]'),
            area : form.find('[name="area"]'),
            station : form.find('[name="station"]'),
            keyword : form.find('[name="keyword"]'),
            reset : form.find('[type="reset"]'),
            submit : form.find('.submit_btn input'),
        },
        backup_type = input.type.clone(),
        res = {
            wrap: $('.search_results'),
        },
        title = {
            wrap: $('.result_station'),
            list: $('.result_station .result_station_ttl ul'),
            number: $('.result_station .result_station_ttl p span')
        },
        csv_url = '/cms8341/shared/templates/childcare_free/csv/import.csv',
        data = [],
        age_search,keyword_search,type_search,area_search,station_search,filter,
    _e = new $.GFUNC.customEvent();

    res.item = res.wrap.find('.search_results_list').first();
    res.name = res.item.find('.search_results_ttl h2');
    res.link = res.item.find('.search_results_ttl a');
    res.image = res.item.find('.search_results_cnt .results_image img');
    res.tag = {
        type: res.item.find('.label_txt .type_tag'),
        area: res.item.find('.label_txt .area_tag'),
        station: res.item.find('.label_txt .station_tag'),
    };
    res.info = {
        address: res.item.find('.result_info_list .txt_address_info'),
        childcare: res.item.find('.result_info_list .txt_childcare_info'),
        open_time: res.item.find('.result_info_list .txt_open_time_info'),
        parking: res.item.find('.result_info_list .txt_parking_info'),
        phone: res.item.find('.result_info_list .txt_phone_info'),
        remark: res.item.find('.result_info_list .txt_remark_info'),
        staff: res.item.find('.result_info_list .txt_staff_info'),
    }
    res.age = [
        res.item.find('.availability_age_cnt .txt_age0_info'),
        res.item.find('.availability_age_cnt .txt_age1_info'),
        res.item.find('.availability_age_cnt .txt_age2_info'),
        res.item.find('.availability_age_cnt .txt_age3_info'),
        res.item.find('.availability_age_cnt .txt_age4_info'),
        res.item.find('.availability_age_cnt .txt_age5_info'),
    ]

    _e.on('init',load_data_from_CSV);
    _e.on('database_inited',init_filter);
    _e.on('filter_inited',init_search_form);
    _e.on('search_form_inited',render_base_on_url);
    _e.on('render',render_title);
    _e.on('render',render_list);
    _e.on('render_manual',render_list);

    _e.trigger('init');
    
    function load_data_from_CSV(){
        var csv_test = new $.GFUNC.readCSV({
            url : csv_url,
            complete: function(res){
                if (res && res.length){
                    var counter = 0;
                    for (var i = 1; i < res.length; i++){
                        if (res[i].length){
                            data[counter] = {
                                name: res[i][1],
                                image: res[i][2] ? res[i][2].split('|') : [],
                                link: res[i][3],
                                age: [res[i][4],res[i][5],res[i][6],res[i][7],res[i][8],res[i][9]],
                                type: res[i][10],
                                area: res[i][11],
                                station: res[i][12],
                                info: {
                                    address: res[i][13],
                                    phone: res[i][14],
                                    staff: res[i][15],
                                    open_time: res[i][16],
                                    access: res[i][17],
                                    parking: res[i][18],
                                    childcare: res[i][19],
                                    remark: res[i][20]
                                }
                            }
                            counter++
                        }
                    }
                    _e.trigger('database_inited');
                }
            }
        });
        csv_test.init();
    }

    function init_filter(){
        age_search = new $.GFUNC.ageSearch({
            field: 'age',
        });
        keyword_search = new $.GFUNC.partialSearch({
            field: 'name'
        });
        type_search = new $.GFUNC.exactlySearch({
            field: 'type'
        });
        area_search = new $.GFUNC.exactlySearch({
            field: 'area'
        });
        station_search = new $.GFUNC.exactlySearch({
            field: 'station'
        });
        filter = new $.GFUNC.searchAndFilter({
            filters: [age_search,keyword_search,type_search,area_search,station_search],
            data: data
        });
        if(filter.active){
            filter.on('render',function(e){
                _e.trigger('render',e);
            });
            filter.init();
            _e.trigger('filter_inited');
        }
    }
    function init_search_form(){
        title.wrap.hide();
        res.wrap.hide();
        res.item.detach();
        //Reinit select search
        input.type.find('option').each(function(){
            var val = $(this).val();
            if (val){
                $(this).text($(this).text() + '(' + data.filter(function(item){
                    return item.type == val
                }).length + '園)');
            }
        });
        //Init event
        input.submit.on('click',function(){
            if ((input.keyword.val() && input.keyword.val() !== '施設名を入力') ||input.age.val() || input.station.val() || input.area.val() || input.type.val()){
                age_search.setFilter(input.age.val());
                keyword_search.setFilter(input.keyword.val() == '施設名を入力' ? '' : input.keyword.val());
                type_search.setFilter(input.type.val());
                area_search.setFilter(input.area.val());
                station_search.setFilter(input.station.val());
                filter.render();
                title.wrap.show();
                res.wrap.show();
            }else{
                title.wrap.hide();
                res.wrap.hide();
            }
        });
        input.reset.on('click',function(){
            title.wrap.hide();
            res.wrap.hide();
        });


        _e.trigger('search_form_inited');
    }
    function render_list(e){
        res.wrap.html('');
        for (var i = 0; i< e.data.length; i++){
            res.name.text(e.data[i].name);
            res.link.attr('href',e.data[i].link);
            res.image.attr('src',e.data[i].image[0]);
            res.image.attr('alt',e.data[i].image[1] ? e.data[i].image[1] : '');
            res.tag.type.text(backup_type.find('option[value="' + e.data[i].type + '"]').text());
            res.tag.area.text(input.area.find('option[value="' + e.data[i].area + '"]').text());
            res.tag.station.text(input.station.find('option[value="' + e.data[i].station + '"]').text());
            res.info.address.text(e.data[i].info.address);
            res.info.childcare.text(e.data[i].info.childcare);
            res.info.open_time.text(e.data[i].info.open_time);
            res.info.parking.text(e.data[i].info.parking);
            res.info.phone.text(e.data[i].info.phone);
            res.info.remark.text(e.data[i].info.remark);
            res.info.staff.text(e.data[i].info.staff);
            for (var j = 0; j < 6; j++){
                switch(e.data[i].age[j].trim()){
                    case '-':
                    case 'ー':
                    case '': res.age[j].html('<span>ー</span>'); break;
                    case '0': res.age[j].html('<span>〇</span><span>(0)</span>'); break;
                    case 'a': res.age[j].html('<span>△</span>'); break;
                    default: res.age[j].html('<span>×</span><span>(' + e.data[i].age[j].trim() + ')</span>')
                }
            }
            res.item.clone().appendTo(res.wrap);
        }
    }
    function render_title(e){
        title.list.html('');
        if (input.age.val()) title.list.append('<li>' + (input.age.val() == '*' ? input.age.find('option[value="*"]').text() : input.age.val() + '歳' ) + '</li>');
        if (input.type.val()) title.list.append('<li>' + backup_type.find('option[value="' + input.type.val() + '"]').text() + '</li>');
        if (input.area.val()) title.list.append('<li>' + input.area.find('option[value="' + input.area.val() + '"]').text() + '</li>');
        if (input.station.val()) title.list.append('<li>' + input.station.find('option[value="' + input.station.val() + '"]').text() + '</li>');
        if (input.keyword.val() == '施設名を入力' ? '' : input.keyword.val()) title.list.append('<li>「' + input.keyword.val() + '」</li>');
        title.number.text(e.data.length);
    }
    function render_base_on_url(){
        var uri = new $.GFUNC.uriObj(window.location.href);
        if (uri.querys && uri.querys.n && data[parseInt(uri.querys.n,10) - 1]){
            res.wrap.show();
            _e.trigger('render_manual',{
                data: [data[parseInt(uri.querys.n,10) - 1]]
            });
        }
    }

}
  
  
```
