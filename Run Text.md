# Run Text handle logic
```javascript
	$.Name.splitContent = function(options){ //v1.0
		var defaults = {
				//Default Options
				selector: null,
			},
			s = $.extend(defaults,options);
		if (!(s.selector && s.selector.length)) return {active: false}

		var items_array = [];
		/*---- INIT ----*/
		function init(){
			var items = s.selector[0].childNodes;
			for (var i = 0; i < items.length; i++){
				if (items[i].nodeType == 3){
					items_array = items_array.concat(items[i].nodeValue.split('').map(function(char){ return $('<span class="character">' + char + '</span>')}))
				}else if(items[i].nodeType == 1){
					if (items[i].childNodes.length){
						if (items[i].children.length){
							items_array.push($(items[i]).clone());
						}else{
							items_array = items_array.concat(items[i].textContent.split('').map(function(char){ 
								return $('<' + items[i].localName + ' class="character ' + items[i].className + '">' + char + '</' + items[i].localName + '>')}))
						}
					}else{
						items_array.push($(items[i]).clone());
					}
				}
			}
			return items_array;
		};
	
		/*---- EVENT ----*/
	
		/*---- PRIVATE FUNCTION ----*/
	
		/*---- PUBLIC ----*/
		return {
			//public function and variables
			active: true,
			getContent: init
		}
	}
```
```javascript
    if ($('#tmp_kyouso_main_visual').length) {
        var main_visual_kyoudo_slide_1 = [
                new $.Name.splitContent({
                    selector: $('#tmp_kyouso_main_visual .main_visual_scene1 .des_text_1'),
                }).getContent(), [$('#tmp_kyouso_main_visual .main_visual_scene1 .des_img').clone()],
                [$('#tmp_kyouso_main_visual .main_visual_scene1 .line_curve_wrap').clone()],
                $.Name.splitContent({
                    selector: $('#tmp_kyouso_main_visual .main_visual_scene1 .des_text_2'),
                }).getContent()
            ],
            main_visual_kyoudo_slide_2 = [
                new $.Name.splitContent({
                    selector: $('#tmp_kyouso_main_visual .main_visual_scene2 .des_text'),
                }).getContent()
            ],
            main_visual_kyoudo_slide_3 = [
                [$('#tmp_kyouso_main_visual .main_visual_scene3 .logo_left').clone()],
                new $.Name.splitContent({
                    selector: $('#tmp_kyouso_main_visual .main_visual_scene3 .des_text'),
                }).getContent()
            ],
            main_visual_animation_lauch = [main_visual_animation_1, main_visual_animation_2, main_visual_animation_3],
            autoplay_control = $('<a href="javascript:void(0);" class="main_visual_control stop"><span>stop<span></a>');

        $('#tmp_kyouso_main_visual .main_visual_wrap .slick_slides').slick({
            arrows: false,
            dots: false,
            speed: 500,
            autoplaySpeed: 6500,
            slidesToShow: 1,
            fade: true,
            slidesToScroll: 1,
            infinite: true,
            autoplay: true,
            pauseOnHover: false,
            pauseOnFocus: false,
            pauseOnDotsHover: false
        });
        $('#tmp_kyouso_main_visual .main_visual_wrap .slick_slides').on('touchstart touchend', function() {
            if (autoplay_control.hasClass('stop')) $('#tmp_kyouso_main_visual .main_visual_wrap .slick_slides').slick('slickPlay');
        });

        autoplay_control.on('click', function() {
            if (autoplay_control.hasClass('stop')) {
                $('#tmp_kyouso_main_visual .main_visual_wrap .slick_slides').slick('slickPause');
                autoplay_control.removeClass('stop');
                autoplay_control.addClass('start');
                autoplay_control.find('span').text('start');
            } else {
                $('#tmp_kyouso_main_visual .main_visual_wrap .slick_slides').slick('slickPlay');
                autoplay_control.addClass('stop');
                autoplay_control.removeClass('start');
                autoplay_control.find('span').text('stop');
            }
        });

        $('#tmp_kyouso_main_visual .main_visual_wrap').append(autoplay_control);

        $('#tmp_kyouso_main_visual .slick_slides').on('beforeChange', function(event, slick, currentSlide) {
            $(slick.$slides.get(currentSlide)).find('.main_visual_bg').removeClass('active');
            $(slick.$slides.get(currentSlide)).find('.run_text').remove();
        });

        $('#tmp_kyouso_main_visual .slick_slides').on('afterChange', function(event, slick, currentSlide) {
            $(slick.$slides.get(currentSlide)).find('.main_visual_bg').addClass('active');
            main_visual_animation_lauch[currentSlide]()
        });

        $(window).on('load', main_visual_animation_lauch[0]);
        $(window).on('resize', line_curve_1_correct_position);

        function main_visual_animation_1() {
            main_visual_animation(
                $('#tmp_kyouso_main_visual .main_visual_scene1 .main_visual_text'),
                main_visual_kyoudo_slide_1, [140, 400, 0, 140]
            );
            line_curve_1_correct_position();
        }

        function main_visual_animation_2() {
            main_visual_animation(
                $('#tmp_kyouso_main_visual .main_visual_scene2 .main_visual_text'),
                main_visual_kyoudo_slide_2, [100]
            );
        }

        function main_visual_animation_3() {
            main_visual_animation(
                $('#tmp_kyouso_main_visual .main_visual_scene3 .main_visual_text'),
                main_visual_kyoudo_slide_3, [1000, 100],
                200
            );
        }

        function main_visual_animation(selector, slide, timeline, start_time) {
            var current = 0,
                counter = 0,
                time = (start_time) ? start_time : 0;
            selector.html('<div class="run_text"></div>');
            lauch_animation();

            function lauch_animation() {
                if (counter < slide[current].length) {
                    var item = slide[current][counter].clone();
                    if (current || counter) {
                        if (counter)
                            time += timeline[current];
                        else
                            time += timeline[current - 1];
                    }
                    if (item.hasClass('line_curve_wrap') || item.hasClass('logo_left') || item.hasClass('des_img')) {
                        item.children().css({
                            'animation-delay': time + 'ms'
                        });
                    } else {
                        item.css({
                            'animation-delay': time + 'ms'
                        });
                    }
                    selector.children('.run_text').append(item);
                    counter++;
                } else {
                    counter = 0;
                    current++;
                }
                if (current < slide.length) {
                    lauch_animation()
                }
            }
        }

        function line_curve_1_correct_position() {
            window.setTimeout(function() {
                $('#tmp_kyouso_main_visual .main_visual_scene1 .run_text .line_curve_wrap').css({
                    'left': $('#tmp_kyouso_main_visual .main_visual_scene1 .run_text .des_img').offset().left + $('#tmp_kyouso_main_visual .main_visual_scene1 .run_text .des_img').width()
                });
                // console.log($('#tmp_kyouso_main_visual .main_visual_scene1 .run_text .des_img').width())
                $('#tmp_kyouso_main_visual .main_visual_scene1 .run_text .line_curve_wrap img').css({
                    width: $('#tmp_kyouso_main_visual .main_visual_scene1 .run_text .line_curve_wrap').width()
                });
            }, 100)
        }
    }
```
