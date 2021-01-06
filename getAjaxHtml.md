# getAjaxHtm
```javascript
$(window).on('load',function(){
    var rePopFlg = $.cookie('rePopFlg_jp');
    var puTargetObj = $('#tmp_wrapper');
    var puHtmlUrl = '/popup_admin/popup_jp.html';
    var currentDate = new Date();
    currentDate.setTime( currentDate.getTime() + ( 30 * 60 * 1000 ));
    if(puTargetObj.length && rePopFlg != 'disable'){
        $.ajax({
            url: puHtmlUrl,
            dataType: 'html',
            cache: false
        })
        .done(function(data){
            var puFlg = $('#tmp_popup_flg', data);
            if(puFlg.length){
                var puTargetHtml = $('#tmp_emergency_pu_box', data);
                var insertHtml = $('<div>', { id:'tmp_emergency_pu_box', html:puTargetHtml });
                var insertCloseBtn = $('<div id="tmp_emergency_pu_close"><p class="pu_close"><a href="javascript:void(0);">閉じる</a></p></div>');
                insertHtml.find('.wrap_emergency_pu').append(insertCloseBtn);
                insertHtml.find('h1').replaceWith(function(){
                    $(this).replaceWith("<p class='pu_ttl'><span class='pu_ttl_text'>"+$(this).text()+"</span></p>");
                });
                if (puTargetHtml.length) {
                    puTargetObj.before(puTargetHtml);
                }
                insertCloseBtn.on('click',function(){
                    puTargetHtml.remove();
                    $.cookie( "rePopFlg_jp", "disable", {
                        expires: currentDate,
                        path:'/'
                    });
                });
                $(document).on('click touchend', function(event) {
                    if (!$(event.target).closest('.wrap_emergency_pu').length && $('#tmp_emergency_pu_box').length) {
                        insertCloseBtn.trigger("click");
                    }
                });
            }

        });
    }
});
```
