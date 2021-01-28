# Jquery
```javascript
$.GFUNC.readMore = function(options){ //v1.0
	var defaults = {
			//Default Options
			selector: $('.action_button a'),
			item: $('.month_category .month_slide'),
			button_readmore: $('.action_button'),
			count: 0,
			limit: 6
		},
		s = $.extend(defaults,options),
		length = s.item.length,
		end = s.limit;

	//Restrict
	if (!(s.selector && s.selector.length && s.item.length)) return {active: false};
	//Private Variables

	/*---- INIT ----*/
	function init(){
		// Handles default item loading
		load_item();
		// The item load processed by clicking
		s.selector.on('click', function(e){
			e.preventDefault();
			s.count = s.count + s.limit;
			end = s.count + s.limit;
			if(s.item.length > end){
				for(var i = 0; i < end; i++){
					s.item.eq(i).fadeIn();
				}
			}else{
				for(var i = 0; i < s.item.length; i++){
					s.item.eq(i).fadeIn();
				}
				s.button_readmore.hide();
			}
			$('html, body').animate({
				scrollTop: s.item.eq(s.count-1).offset().top
			}, 1000);
		});
	}
	/*---- PRIVATE FUNCTION ----*/
	function load_item(){
		s.item.hide();
		for(var i = 0; i < end; i++){
			s.item.eq(i).fadeIn();
		}
	}
	/*---- PUBLIC ----*/
	return {
		//public function and variables
		active: true,
		init: init
	}
}
var readMorePhoto = $.GFUNC.readMore({
	selector: $('.action_button a'),
	item: $('.month_category .month_slide'),
	count: 0,
	limit: 6
});
```
