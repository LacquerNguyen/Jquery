# menuScrollmove
```javascript
	$.GFUNC.scrollMenuFixed = function(options){ //v1.0
		var defaults = {
				elementFixed: $('.header_fixed_wrap'),
				headerWrap: $('#header_fixed'),
				menuPcParent :'#tmp_menu_pc',
				btnMenuPc :$('#tmp_menu_pc .menu_btn'),
				replaceHref: $('.browsing_support_btn'),
				menuPcCnt:'.menu_pc_cnt',
				menuPcInner: $('.menu_pc_cnt_inner'),
				hnavi2Append: $('#tmp_hnavi2'),
				hnavi3Append: $('#tmp_hnavi3')
			},
			s = $.extend(defaults,options);
	
		//Restrict
		if (!s.elementFixed && s.elementFixed.length) return {active: false};
		var visible = false;
		//Private Variables
		/*---- INIT ----*/
		function init(model){
			if(model == 'pc'){
				handleScroll()
				resizeScrollLeft()
			}else{
				handleScrollReset()
				$('.header_fixed_wrap').css({'left': 0});
				$('#tmp_gl').css({'left':'0'});
			}
			handleEffects()
		}
		/*---- EVENT ----*/
		function handleScroll(){
			var header_height = $(s.elementFixed).height(); 
			$(window).on('scroll load',function() {
				var windowScroll = $(window).scrollTop();
				if(windowScroll > header_height){
						$(s.elementFixed).addClass('posi_fixed').css("top",-header_height);
						$(s.elementFixed).stop().addClass('active').animate({top: '0'},100);
						$(s.headerWrap).css("height", header_height + 18);
						if(!visible) {
							handleAppendElem()
							visible = true;
						}
				}else{
					handleScrollReset()
				}
			})
		}
		function handleAppendElem(){
			var attr_hnavi = $('#tmp_hnavi .last').find('a').attr('href');
			$(s.replaceHref).find('a').attr('href', attr_hnavi);
			if($(s.elementFixed).hasClass('posi_fixed')){
				$(s.hnavi2Append).appendTo(s.menuPcInner)
				$(s.hnavi3Append).appendTo(s.menuPcInner)
			}
		}
		function handleEffects(){
			$(s.btnMenuPc).on('click',function(){
				if($(s.menuPcParent).hasClass('effects_show')){
					$(s.menuPcParent).removeClass('effects_show');
					$(s.menuPcParent).addClass('effects_hide');
					$(s.menuPcCnt).stop().slideUp(300);
				}else{
					$(s.menuPcParent).removeClass('effects_hide');
					$(s.menuPcParent).addClass('effects_show');
					$(s.menuPcCnt).stop().slideDown(300);
				}
			})
			$("#tmp_wrapper").click(function (event) {
				var target = $(event.target);
				if (!(target.parents(s.menuPcParent).length || target.hasClass(s.menuPcCnt))) {
					$(s.menuPcParent).removeClass('effects_show');
					$(s.menuPcParent).addClass('effects_hide');
					$(s.menuPcCnt).stop().slideUp(300);
				}
			});
		}
		function handleScrollReset(){
			$(s.headerWrap).css("height",'auto');
			$(s.elementFixed).removeClass('posi_fixed').removeAttr("style");
			$(s.elementFixed).removeClass('active').removeAttr("style");
			if(visible) {
				$(s.hnavi2Append).appendTo('#tmp_means_primary')
				$(s.hnavi3Append).insertAfter(s.menuPcParent)
				visible = false;
			}
		}
		function resizeScrollLeft(){
			$(window).on('resize load',function () {
				$('#tmp_wrap_header').css({'left':'0'});
				$('#tmp_gl').css({'left':'0'});
				$(document).on('scroll',function () { 
					if($(window).width() > 980){
						$('#tmp_wrap_header').css({'left': 0});
						$('#tmp_gl').css({'left':'0'});
					}else{
						if($(window).width() > 640){
							$('#tmp_wrap_header').css({'left': -$(this).scrollLeft()});
							$('#tmp_gl').css({'left': -$(this).scrollLeft()});
						}else{
							$('#tmp_wrap_header').css({'left':'0'});
							$('#tmp_gl').css({'left':'0'});
						}
					}
				})
			})
		}
		/*---- END EVENT ----*/
		/*---- PUBLIC ----*/
		return {
			//public function and variables
			active: true,
			init: init
		}
	}
```
# menuScroll
```javascript
var mq = window.matchMedia( "(max-width: 640px)" );
	function appendElement(e) {
		var elemnt= $('.header_fixed');
		var windowScroll;
		var headerH = $('#tmp_header').innerHeight();
			$( window ).on('scroll load',function() {
				windowScroll = $(window).scrollTop()
				if (e.matches) {
					$('#tmp_header').css('height','auto');
					$(elemnt).removeClass('fixed');
					$(elemnt).removeClass('show');
					$(elemnt).removeAttr('style');	
				}else{
					if(windowScroll > headerH + 20){
						$('#tmp_header').css('height',headerH);
						$(elemnt).addClass('fixed');
					}else{
						$('#tmp_header').css('height','auto');
						$(elemnt).removeClass('fixed');
						$(elemnt).removeAttr('style');
				}
			}
		});
	}
	$(document).ready(function(){
		appendElement(mq);
		mq.addListener(appendElement);
	});
```
```css
.header_fixed{
	padding: 10px 0 15px;
    top: -96px;
	-moz-transition: top 0.5s ease-in-out;
	-webkit-transition: top 0.5s ease-in-out;
	transition: top 0.5s ease-in-out;
}
.header_fixed.fixed{
	position: fixed;
	width: 100%;
	left: 0;
	background-color: #FFFFFF;
	z-index: 99;
	top: 0;
}
```
