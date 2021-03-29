# MenuSp
```javascript
var settingMenuSp = {
    init: function() {
        settingMenuSp.bind();
    },
    bind: function() {
        var $menuBtn = $('.mobile_control li a');
        var $closeBtn = $('.close_btn a');
        $menuBtn.on('click', function(e) {
            var _this = $(this);
            var _id = _this.attr('href');
            if ($(_id).length > 0) {
                e.preventDefault();
                if (!_this.hasClass('open')) {
                    settingMenuSp.show(_this, _id);
                } else {
                    settingMenuSp.hide();
                }
            }
        });
        $closeBtn.on('click', function(e) {
            e.preventDefault();
            settingMenuSp.hide();
        });
        settingMenuSp.resize();
    },
    show: function(_this, _id) {
        $('.mobile_control li a').removeClass('open');
        $('#tmp_wrapper').addClass('overlay');
        _this.addClass('open');
        settingMenuSp.resetText();
        _this.find('.nav_text').text('閉じる');
        $('.menu_sp').hide();
        $(_id).slideDown(300);
    },
    hide: function() {
        settingMenuSp.resetText();
        $('.menu_sp').slideUp(200);
        $('#tmp_wrapper').removeClass('overlay');
        $('.mobile_control li a').removeClass('open');
    },
    resetText: function() { 
        $('.navigation_link').find('.nav_text').html('検索<br />メニュー');
    },
    resize: function(){
        $(window).on('resize', function(){
            if($(window).width() > 640){
                $('#tmp_wrapper').removeClass('overlay');
            }
        });
    }
};
settingMenuSp.init();



	$.name.spMenu = function(options) {
		var o = $.extend({
			menuBtn: [{
					oBtn:'#tmp_hnavi_lmenu a', //メニューボタン
					target:'#tmp_sma_lmenu' //展開するメニュー
				}],
			closeBtn: '.close_btn', //閉じるボタン
			addClass: 'spmenu_open' //bodyに付与するクラス
		},options);
		var l = o.menuBtn.length;
		if(l >= 0){
			for(i=0;i<l;i++) {
				$(o.menuBtn[i].oBtn).on('click', {elem:o.menuBtn[i].target}, function(e) {
					var self = $(this);
					if(self.hasClass('active')){
						self.removeClass('active');
						$(e.data.elem).hide();
						$('body').removeClass(o.addClass);
					} else {
						for(var i=0;i<o.menuBtn.length;i++){
							if($(o.menuBtn[i].oBtn).hasClass('active')) $(o.menuBtn[i].oBtn).removeClass('active');
							$(o.menuBtn[i].target).hide();
						}
						self.addClass('active');
						$(e.data.elem).show();
						if(o.addClass) $('body').addClass(o.addClass);
					}
				});
				$(o.menuBtn[i].target).on('click', o.closeBtn, {elem:o.menuBtn[i]}, function(ev) {
					$(ev.data.elem.oBtn).removeClass('active');
					$(ev.data.elem.target).hide();
					$('body').removeClass(o.addClass);
				});

				// 画面外をタップした際にメニューを閉じる処理
				$(document).on('click touchstart', {elem:o.menuBtn[i]}, function(e) {
				    //タップされた要素の親がhtml要素の場合は閉じる
				    if(($(e.target).parent().is($('html')))){
						$(o.menuBtn).each(function(){
							if($(this.oBtn).hasClass('active')){
								$(this.oBtn).removeClass('active');
							}
						});
						$('body').removeClass(o.addClass);
						$(e.data.elem.target).hide();
				    }
			    });
			}
		}
	};


	//スマホメニュー設定
	$.Name.spMenu({
		menuBtn: [{
                slideEffect: true,
				oBtn:'#tmp_hnavi_rmenu a',
				target:'#tmp_sma_rmenu'
			}],
		closeBtn: '.close_btn', //閉じるボタン
		addClass: 'spmenu_open' //bodyに付与するクラス(不要の場合空にする)
	});

```
# Hml
```html
 <ul id="tmp_hnavi_s">
      <li id="tmp_hnavi_rmenu">
          <a href="javascript:void(0);" class="sma_menu_open">
              <span class="menu_icon">&nbsp;</span>
              <span class="menu_text">メニュー</span>
          </a>
      </li>
  </ul>
```



# Css Icon Menu
```css
.sma_menu_open .menu_icon{
    position: absolute;
    top: 11px;
    left: 0;
    margin-left: 5px;
    width: 20px;
    height: 2px;
    color: inherit;
    z-index: 3;
	-moz-transition: all 0.2s ease 0s;
	-webkit-transition: all 0.2s ease 0s;
	transition: all 0.2s ease 0s;
}
.sma_menu_open .menu_icon{
	background-color: #dc0011;
}
.sma_menu_open .menu_icon:before,
.sma_menu_open .menu_icon:after {
    content: "";
    width: 20px;
    height: 2px;
    left: 0;
    color: inherit;
    position: absolute;
    top: 0;
    z-index: 1;
	-moz-transition: all 0.2s ease;
	-webkit-transition: all 0.2s ease;
	transition: all 0.2s ease;
}
.sma_menu_open .menu_icon:before,
.sma_menu_open .menu_icon:after{
	background-color: #dc0011;
}
.sma_menu_open .menu_icon:before {
	-moz-transform: translate(0, -9px);
	-webkit-transform: translate(0, -9px);
	transform: translate(0, -9px);
	top: 3px;
}
.sma_menu_open .menu_icon:after {
	-moz-transform: translate(0, 9px);
	-webkit-transform: translate(0, 9px);
	transform: translate(0, 9px);
	top: -3px;
}
.sma_menu_open.active .menu_icon:before {
	top: 0;
	-moz-transform: rotate(45deg);
	-webkit-transform: rotate(45deg);
	transform: rotate(45deg);
	background-color: #dc0011;
}
.sma_menu_open.active .menu_icon:after {
	-moz-transform: rotate(-45deg);
	-webkit-transform: rotate(-45deg);
	transform: rotate(-45deg);
	top: 0;
	background-color: #dc0011;
}
.sma_menu_open.active .menu_icon {
    background-color: transparent;
}
```
