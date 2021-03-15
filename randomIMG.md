# randomIMG
```javascript
    $.customEvent = function(){
        var listeners = [];
        function add_event(event_name,event_handler){
            if (listeners[event_name]){
                listeners[event_name].push(event_handler);
            }else{
                listeners[event_name] = [event_handler];
            }
        }
        function remove_event(event_name,event_handler){
            if (listeners[event_name]){
                if (event_handler){
                    while (listeners[event_name].indexOf(event_handler) > -1){
                        listeners[event_name].splice(listeners[event_name].indexOf(event_handler),1);
                    }
                }else{
                    listeners[event_name] = [];
                }
            }
        }
        return {
            on: function(event_name,event_handler){
                if (typeof event_name == 'string'){
                    add_event(event_name,event_handler)
                }else if(Array.isArray(event_name)){
                    event_name.forEach(function(name){
                        add_event(name,event_handler);
                    });
                }
            },
            off: function(event_name,event_handler){
                if (typeof event_name == 'undefined'){
                    listeners = []
                }else if (typeof event_name == 'string'){
                    remove_event(event_name,event_handler)
                }else if(Array.isArray(event_name)){
                    event_name.forEach(function(name){
                        remove_event(name,event_handler);
                    });
                }
            },
            trigger: function(event_name,args,_this){
                if (!_this) _this = null;
                if (listeners[event_name] && listeners[event_name].length){
                    var max = listeners[event_name].length;
                    for (var i = 0; i < max; i++){
                        if (args && args.hasOwnProperty('length')){
                            listeners[event_name][i].apply(_this,args);
                        }else{
                            listeners[event_name][i].call(_this,args);
                        }
                    }
                }
            },
            getEventListeners: function(event_name){
                if (listeners[event_name]){
                    return listeners[event_name];
                }else{
                    return [];
                }
            }
        }
    }
    function effectLoading() {
        var _e = new $.customEvent(),
            fDuration = 1500,
            imgLength = 7,
            stepLoad = 0;
        
        var bigLogo = $('.big_logo');
        var imgList = $('.img_list li');
        var localList = $('.local_list div');
        localList.css('top', 0 - localList.height());
        var arr = [];
        function init() {
            var lcLength = localList.length;
            for(var i = 0; i < localList.length; i++) {
                localList.eq(i).css('z-index',lcLength);
                lcLength = lcLength - 1;
            }
            $(window).on('resize load', function() {
                imgList.css('height', $('#tmp_effect_cnt').height() / 2);
                $('.local_list').css('height', $('#tmp_effect_cnt').height() / 2);
                bigLogo.css('height', $('#tmp_effect_cnt').height());
                localList.find('a').css('height', $('#tmp_effect_cnt').height() / 14)
            })
            while(arr.length < imgLength){
                var r = Math.floor(Math.random() * imgLength);
                if(arr.indexOf(r) == -1) arr.push(r);
            }
            init_event();
        }
        function firstLoad() {
            bigLogo.animate({
                opacity: 1,
            }, {
                duration: fDuration,
                complete: function() {
                    _e.trigger('loadLft');
                }
            });
        }
        function load_lft() {
            $('#tmp_cnt_lft').animate({
                left: 0,
            }, {
                duration: fDuration-300,
            });
            bigLogo.animate({
                'width': '62.5%',
            }, {
                duration: fDuration-300,
                complete: function() {
                    _e.trigger('loadImg');
                }
            });
        }
        function load_img() {
            var j = 0;
            for(var i = 0; i < arr.length; i++) {
                imgList.eq(arr[i]).stop(true,true).delay(stepLoad).queue(function(){
                    j = j + 1;
                    $(this).addClass("showed").dequeue();
                    if(j == 3) {
                        _e.trigger('loadLocation');
                    }
                });
                stepLoad = stepLoad + 300;
            }
        }
        function load_location() {
            var stepDown = 100;
            for(var i = 0; i < localList.length; i++) {
                localList.eq(i).delay(stepDown).delay(stepDown).queue(function(){
                    $(this).addClass("showed").dequeue();
                });
                stepDown = stepDown + 100;
            }
        }
        function init_event() {
            _e.on('loadLft',load_lft);
            _e.on('loadImg',load_img);
            _e.on('loadLocation',load_location);
        }
        return {
            init: init,
            run: firstLoad,
        }
    }
```
