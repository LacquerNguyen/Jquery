$(function () {
			$(".pickup_list_cnt .list_cnt_img").click(function () {
				//get data
				var title = $(this).parents('.pickup_list_cnt').find('.title').text();
				var img = $(this).find('img').attr('src');
				$('.pubup_inner .img_name span').text(title);
				$('.pubup_inner .pubup_img img').attr('src',img);
				$(".box_pubup").fadeIn();
				if ($('.pubup_inner').outerHeight() > $(window).height()) $("body").addClass('box_open');
			});
			$(".bnt_exits").click(function () {
				$(".box_pubup").fadeOut();
				$("body").removeClass('box_open');
			});
			$(".box_pubup").click(function (event) {
				var target = $(event.target);
				if (!(target.parents('.pubup_inner').length || target.hasClass('pubup_inner'))){ //tìm class
					$(".box_pubup").fadeOut();
					$("body").removeClass('box_open');
				}
			});
      //kiểm tra chiều cao của box so với window
			$(window).on('resize',function(){
				if ($('.pubup_inner').outerHeight() > $(window).height()){ 
					$("body").addClass('box_open');
				}else{
					$("body").removeClass('box_open');
				}
			})
		});
