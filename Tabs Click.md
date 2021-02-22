
# Tabs Click 
```javascript
$('.section_tab').each(function() {
    $(this).find('.section').hide();

    var current = $(this).find('.item_title_tab').children('.current');
    if (current.length == 0) {
      $(this).find('.item_title_tab').children(':first-child').addClass('current');
      $($(this).find('.item_title_tab').children(':first-child').find('a').attr('href')).show();
    }

    $(this).find('.item_title_tab').find('a').click(function() {
      var current = $(this).parent().hasClass('current');
      if (current == false) {
        $(this).parent()
          .addClass('current')
          .siblings().each(function() {
            $(this).removeClass('current');
            $($(this).find('a').attr('href')).hide();
          });
        $($(this).attr('href')).fadeIn();
      }
      return false;
    });
  });
  ```
  ```html
<div class="section_tab">
	<div class="container">
	    <ul class="item_title_tab">
		<li class="notifi_item"><a href="#tab_notifi">手続き・届出</a></li>
		<li class="refuse_item"><a href="#tab_refuse">ごみ</a></li>
		<li class="event_item"><a href="#tab_event">イベント</a></li>
		<li class="facility_item"><a href="#tab_facility">施設</a></li>
	    </ul>
	    <div class="tab_cnt_group">
		<div class="section" id="tab_notifi">

		</div>
		<div class="section" id="tab_refuse">

		</div>
		<div class="section" id="tab_event">

		</div>
		<div class="section" id="tab_facility">

		</div>
	    </div>
	</div>
</div>
  
  ```
  # Tabs Click FUNCTION
  ```javascript
  $.NAME.tabLayout = function(options){
	var defaults = {
		parent_tab: $('.tab .tab-pane'),
		tab_click: '.tab_ttl',
		tab_cnt: $('#tmp_infor_tab .tab_infor_cnt'),
		winW: 480,
	},
	s = $.extend(defaults, options);
	/*---- INIT ----*/
	click_tab();
	/*---- PRIVATE FUNCTION ----*/
	function click_tab() {
		$(s.parent_tab).each(function(){
			$(this).find($(s.tab_click)).click(function(){
				var iscurrent = $(this).parent().hasClass('active');
				if(!s.sp || $(window).width() > s.winW || $('body').hasClass('disp_pc')) {
					if(!iscurrent){
						$(s.parent_tab).removeClass('active');
						$(s.tab_cnt).hide();
						$(this).parent().addClass('active');
						$(this).next().show();
					}
				}else{
					var iscurrent = $(this).parent().hasClass('active');
					if(iscurrent){
						$(this).parent().removeClass('active');
						$(this).next().slideUp(300);
					}else {
						$(this).parent().addClass('active');
						$(this).next().slideDown(300);
					}
				}
				return false;
			});

		});
	}
	function reset_active() {
		$(s.tab_click).parent().removeClass('active');
		$(s.tab_cnt).css('display','');
		if(!s.sp || $(window).width() > s.winW || $('body').hasClass('disp_pc')){
			$(s.parent_tab).each(function(index){
				if(!$(this).hasClass('active') && index == 0) $(this).addClass('active');
			});
		}
	}
	/*---- PUBLIC ----*/
	return {
		resize: function() {
			reset_active();	
		}
	}
};
  ```
 ### Sử dụng
  ```javascript
  //============================================================================
  //============================================================================
	var tabSwitch = new $.GFUNC.tabLayout({
	parent_tab: $('#tmp_infor_tab .infor_tab'),
	tab_click: '.tab_infor_ttl',
	tab_cnt: $('#tmp_infor_tab .tab_infor_cnt'),
  ```
```html
<div class="box_tab_info">
	<div class="tab_info">
		<div class="tab_info_ttl">
		<p><a href="javascript:void(0)">注目情報</a></p>
		</div>
		<div class="tab_info_cnt">
		</div>
	</div>
	<div class="tab_info">
		<div class="tab_info_ttl">
		<p><a href="javascript:void(0)">新着情報</a></p>
		</div>
		<div class="tab_info_cnt">

		</div>
	</div>
</div>
```
```css
.box_tab_info {
	position: relative;
	background-color: #ffffff;
	color: #222222;
	padding: 75px 0 0;
}
.box_tab_info .tab_info .tab_info_ttl {
	background-color: #ececec;
	color: #222222;
	position: absolute;
	width: 50%;
	top: 0;
	left: 0;
	padding: 19px 0 10px;
}

#tmp_notices .box_tab_info .tab_info.active .tab_info_ttl {
	background-color: #ffffff;
}
#tmp_notices .box_tab_info .tab_info+.tab_info .tab_info_ttl {
	left: 50%;
	border-left: 1px solid #dfdfdf;
}
#tmp_notices .box_tab_info .tab_info .tab_info_cnt {
	display: none;
}
#tmp_notices .box_tab_info .tab_info.active .tab_info_cnt {
	display: block;
}
#tmp_notices .box_tab_info .tab_info_cnt{
	padding: 5px 14px;
}

```
