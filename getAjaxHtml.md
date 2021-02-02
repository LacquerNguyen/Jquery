# getAjaxHtm
```javascript
function loadPopup(model){
    var removePop = $.cookie('removePopup');
    var wrapper = $('.format_top #tmp_wrapper');
    var puHtmlUrl = $('.link_getajax').find('a').attr('href');
    var currentDate = new Date();
    currentDate.setTime( currentDate.getTime() + (30*60*1000));
    if(wrapper.length && removePop != 'disable'){
        function noScroll() {
            window.scrollTo(0, 0);
        }
        window.addEventListener('scroll', noScroll);
        $('.pnavi').addClass('disp_popup');
        $.ajax({
            url: puHtmlUrl,
            dataType: 'html',
            cache: false
        })
        .done(function(data){
            var targetHtml = $('#tmp_emergency_infor', data);
            if(targetHtml.parents('#tmp_contents').length == 1){
                var insertHtml = $('<div>', { id:'tmp_emergency_infor', html:targetHtml });
                var original_scroll = $(window).scrollTop();
                var position_popup = 0;
                max_scroll = 0,
                insertHtml.find('h1').replaceWith(function(){
                    $(this).replaceWith("<p class='emergency_ttl'><span>"+ $(this).text() +"</span></p>");
                });
                if(targetHtml.length){
                    wrapper.before(targetHtml);
                }
                $(document).on('click touchend', function(event) {
                    if (!$(event.target).closest('.emergency_infor_wrap').length && $('#tmp_emergency_infor').length) {
                        targetHtml.animate({
                            opacity: 0
                        },{
                            duration: 200,
                            complete: function(){
                                targetHtml.remove();
                                $.cookie( "removePopup", "disable", {
                                    expires: currentDate,
                                    path:'/'
                                });
                                $('.pnavi').removeClass('disp_popup');
                                window.removeEventListener('scroll', noScroll);
                                $('#tmp_wrapper').addClass('loaded');
                                delayTime = 500;
                                loadFirst.run();
                            }
                        });
                    }
                });
                $(window).on("scroll load", function(){
                    var windowScroll = $(window).scrollTop();
                    if(model == 'pc' ){
                        position_popup = position_popup + original_scroll - windowScroll;
                        position_popup = Math.max(0 - Math.max(0,$('.emergency_infor_wrap').outerHeight() - $(window).height() * $('#tmp_wrapper').width() / $(window).width()),position_popup);
                        position_popup = Math.min(position_popup,max_scroll);
                        $('.emergency_infor_wrap').css({
                            'top' : position_popup
                        });
                        original_scroll = windowScroll;
                    }
                })
            }
        });
    }
}
```
