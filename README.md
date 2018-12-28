# Jquery 
Menu sp 

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
