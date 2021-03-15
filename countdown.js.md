# countdown.js
```javascrpit
	// Set
	// =================================
	var countdownHref = $("#js_countdown a").attr('href');
	$("#js_countdown").replaceWith($('<div class="session_countdown">')); //カウントダウン全体のID
	var countdownObj = $('.row_countdown');

	//オリンピックの設定
	var olympic = {
		name: 'olympic',
		title: '東京オリンピック', //名前
		start : new Date("2021/07/23"), //開始日
		end : 	new Date("2021/08/08"), //終了日
		//url: '',
		cnt : 	$('<div class="olympic_count"><div class="img_event_countdown"><p><img src="/cms8341/shared/templates/top_rn/images/main/img_olympic.jpg" width="50" height="90" alt=""></p></div></div>'),
	}

	//パラリンピックの設定
	var paralympic = {
		name: 'paralympic',
		title: '東京パラリンピック', //名前
		start : new Date("2021/08/24"), //開始日
		end : 	new Date("2021/09/05"), //終了日
		//url: '',
		cnt : 	$('<div class="paralympic_count"><div class="img_event_countdown"><p><img src="/cms8341/shared/templates/top_rn/images/main/img_paralympic.jpg" width="50" height="90" alt=""></p></div></div>')
	}

	//時間
	var SECOND = 1000;
	var MINUTE = SECOND * 60;
	var HOUR = MINUTE * 60;
	var DAY = HOUR * 24;

	// Proc
	// =================================
	countdown(olympic);
	countdown(paralympic);

	// Func
	// =================================
	function countdown(obj){
		//今の時間
		var now = new Date(new Date().getFullYear(), new Date().getMonth(), new Date().getDate());
		var distance_start = Math.ceil((obj.start - now) / DAY); //開催日までの日数
		var distance_end = Math.ceil((obj.end - now) / DAY); //終了日までの日数

		if(distance_start <= 0) {
			if(distance_end < 0){
				countdownObj.addClass(obj.name+'_end');
			} else {
				var frame;
				var alt = $('<div class="cnt_event_coundown"><p>'+ obj.title +'</p><p><span class="txt_count">開催中</span></p></div>');
				obj.cnt.append(alt);
				//画面出力
				countdownObj.append(obj.cnt);
			}
		} else {
			var alt = $('<div class="cnt_event_coundown"><p>'+ obj.title +'まで</p><p class="txt_remaining"><span class="txt_ttl">あと</span><span class="txt_count">'+ distance_start +'</span><span class="txt_day">日</span></p></div>');
			//画面出力
			obj.cnt.append(alt);
			countdownObj.append(obj.cnt);
		}
		if(countdownHref || obj.url) {
			obj.cnt.css({'cursor':'pointer'});
			obj.cnt.on('click', function() {
				if(obj.url) {
					window.location.href = obj.url;
				}
				else {
					window.location.href = countdownHref;
				}
			});
		}
	}
	if (!countdownObj.find('div').length) {
		$('.session_countdown').remove();
	}
});
```
