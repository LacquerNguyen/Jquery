# Jquery
```javascript
// ======================
// Gallery
// ======================
var swipeTarget = false;
function gallery_content(options) {
	var defaults = {
		parent: $('#tmp_gallery'),
		selector: $('#tmp_gallery .wrap_day'),
		itemClass: 'gallery_item'
	},
	s = $.extend(defaults, options);

	/*---- INIT ----*/
	if (s.parent.length){
		// 5. The API calls this function when the player's state changes.
		//    The function indicates that when playing a video (state=1),
		//    the player should play for six seconds and then stop.
		s.selector.each(function() {
			var _this = $(this);
			var slideItem = _this.find('.' + s.itemClass).length;
			var slides = $('<div class="gallery_modal"><div class="gallery_modal_cnt"><a href="javascript:void(0);" class="gallery_modal_close">Close</a><div class="gallery_slides"></div></div></div>'),
				slide_temp = '<div class="gallery_slide">' +
								'<div class="gallery_cnt"><%--gallery_cnt--%></div>' +
								'<div class="gallery_txt">' +
									'<span class="number">Image <%--number--%> of <%--count--%></span>' +
									'<span class="caption"><%--caption--%></span>' +
								'</div>' +
							'</div>';
			_this.find('.' + s.itemClass).each(function(index, item) {
				var slide_item = slide_temp;
				if($(this).find('.item_movie').length) {
					var video_id = $(this).find('img').data('movie');
					$.GFUNC.videoArray.push({
						'id' : 'tmp_' + video_id,
						'videoId' : video_id
					})
					slide_item = slide_item.replace('<%--gallery_cnt--%>','<div class="video_gallery"><div id="tmp_' + video_id + '"></div></div>');
					slide_item = slide_item.replace('<%--caption--%>', '');
				} else {
					var img_url = $(this).find('img').attr('src');
					var img_alt = $(this).find('img').attr('alt');
					slide_item = slide_item.replace('<%--gallery_cnt--%>','<div class="image_gallery"><p><img src="'+ img_url +'" alt="'+ img_alt +'"></p></div>');
					slide_item = slide_item.replace('<%--caption--%>', img_alt);
				}
				slide_item = slide_item.replace('<%--number--%>',index + 1);
				slide_item = slide_item.replace('<%--count--%>', slideItem);
				slides.find('.gallery_slides').append(slide_item);
			})
			_this.prepend(slides);
			
			_this.find('.gallery_slides').slick({
				infinite: false,
				slidesToShow: 1,
				adaptiveHeight: true
			});
			_this.find('.gallery_slides').on('beforeChange', function(event, slick, currentSlide, nextSlide){
				var currentVideo = $('[data-slick-index=' + currentSlide + ']');
				if(currentVideo.find('iframe').length) {
					currentVideo.find('iframe')[0].contentWindow.postMessage('{"event":"command","func":"pauseVideo","args":""}', '*')
				}
				if(nextSlide == slideItem - 1) {
					$('.slick-next').hide();
				} else {
					$('.slick-next').show();
				}
				if(nextSlide == 0) {
					$('.slick-prev').hide();
				} else {
					$('.slick-prev').show();
				}
			});
			_this.find('.gallery_slides').on('afterChange', function(event, slick, currentSlide, nextSlide){
				var currentIframe = $('[data-slick-index=' + currentSlide + ']');
				if(currentIframe.find('iframe').length) {
					_this.on('keydown',function(e){
						if(e.key == "Enter") {
							currentIframe.find('iframe')[0].contentWindow.postMessage('{"event":"command","func":"playVideo","args":""}', '*');
							e.preventDefault();
						}
						_this.off('keydown');
					});
				}
			});
			_this.find('.gallery_slides').on('swipe', function(event, slick, direction){
				swipeTarget = true;
				setTimeout(function() {
					swipeTarget = false;
				},2000)
			})
			_this.find('.gallery_items a').on('click',function(){
				var index = _this.find('.gallery_items a').index($(this));
				_this.find('.gallery_slides').slick('slickGoTo',index,true);
				_this.find('.gallery_modal').addClass('active');
				_this.find('iframe').attr('tabindex','-1');
			});
			_this.find('.gallery_modal').on('click',function(e){
				var _target = $(e.target);
				if(swipeTarget == false) {
					if (_target.hasClass('gallery_modal_cnt') || _target.parents('.gallery_modal_cnt').length){
						if (_target.hasClass('gallery_modal_close') || _target.parents('.gallery_modal_close').length){
							_this.find('.gallery_modal').removeClass('active');
							s.parent.find('iframe').each(function(){
								this.contentWindow.postMessage('{"event":"command","func":"pauseVideo","args":""}', '*')
							});
						}
					}else{
						_this.find('.gallery_modal').removeClass('active');
						s.parent.find('iframe').each(function(){
							this.contentWindow.postMessage('{"event":"command","func":"pauseVideo","args":""}', '*')
						});
					}
				}
			})
			$('body').on('keydown',function(e){
				if(e.key === "Escape") {
					_this.find('.gallery_modal').removeClass('active');
					s.parent.find('iframe').each(function(){
						this.contentWindow.postMessage('{"event":"command","func":"pauseVideo","args":""}', '*')
					});
				}
			});
		})
	}
}
gallery_content({
	parent: $('#tmp_gallery'),
	selector: $('#tmp_gallery .wrap_day'),
	itemClass: 'gallery_item'
})

```
```html
				<div id="tmp_gallery">
									<div class="wrap_day">
										<h2>●月●日</h2>
										<div class="gallery_items">
											<div class="gallery_item">
												<div class="item_img">
													<p>
														<a href="javascript:void(0)"><img src="#" width="500" height="300" alt="caption01" /></a>
													</p>
												</div>
											</div>
											<div class="gallery_item">
												<div class="item_img">
													<p>
														<a href="javascript:void(0)"><img src="#" width="500" height="300" alt="caption02" /></a>
													</p>
												</div>
											</div>
											<div class="gallery_item">
												<div class="item_img">
													<p>
														<a href="javascript:void(0)"><img src="#" width="500" height="300" alt="caption03" /></a>
													</p>
												</div>
											</div>
											<div class="gallery_item">
												<div class="item_movie">
													<p>
														<a href="javascript:void(0)"><img src="#" width="500" height="300" alt="caption video" data-movie="VuMJ3NWIQeo" /></a>
													</p>
												</div>
											</div>
											<div class="gallery_item">
												<div class="item_img">
													<p>
														<a href="javascript:void(0)"><img src="#" width="500" height="300" alt="caption05" /></a>
													</p>
												</div>
											</div>
											<div class="gallery_item">
												<div class="item_img">
													<p>
														<a href="javascript:void(0)"><img src="#" width="500" height="300" alt="caption06" /></a>
													</p>
												</div>
											</div>
											<div class="gallery_item">
												<div class="item_movie">
													<p>
														<a href="javascript:void(0)"><img src="#" width="500" height="300" alt="caption video" data-movie="vS5Ag8xMh7Y" /></a>
													</p>
												</div>
											</div>
											<div class="gallery_item">
												<div class="item_img">
													<p>
														<a href="javascript:void(0)"><img src="#" width="500" height="300" alt="caption08" /></a>
													</p>
												</div>
											</div>
										</div>
									</div>
								</div>
```
