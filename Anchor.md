# Anchor 1
```javascript
$(function () {
    $('a[href*="#"]').click(function(){
        var href= $(this).attr("href");
        var target = $(href == "#" || href == "" ? 'html' : href);
        if (href!='#tmp_sma_menu') {
            var position = target.offset().top - 40; //繝倥ャ繝縺ｮ鬮倥＆蛻�ｽ咲ｽｮ繧偵★繧峨☆
            $("html, body").animate({scrollTop:position}, 550, "swing");
            return false;
        };
    });
});
```
# Anchor active
```javascript
$.Name.activeMenu = function() {
		var activeBlock = ['#build_website','#ipa','#contents_management_system','#sales_management_system'],
			activePostion = [];
		function init(){
			activePostion = [];
			for(i = 0; i < activeBlock.length; i++) {
				var activeTop = $(activeBlock[i]).offset().top;
				activePostion.push(activeTop);
			}
		}
		function activeMenu(scroll){
			init();
			$('.block_business_list_inner').find('li').removeClass('current_menu');
			for(i = 0; i < activeBlock.length; i++) {
				if(activePostion[i+1]) {
					if(scroll >= activePostion[i] && scroll < activePostion[i+1]) {
						$('.block_business_list_inner').find('li').eq(i).addClass('current_menu');
					}
				} else {
					if(scroll >= activePostion[i]) {
						$('.block_business_list_inner').find('li').eq(i).addClass('current_menu');
					}
				}
			}
		}
		return {
			activeMenu: activeMenu
		}
```
