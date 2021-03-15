# inViewObserver
```javascript
	//========================================
	//customEvent
	//========================================
	$.Name.customEvent = function(){
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
						if (args && Array.isArray(args)){
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
    $.Name.inViewObserver = function(options){ //v1.0
        var defaults = {
                selector: null,
                inviewCondition: function(self_percent,window_percent,inview_px){
                    return self_percent > 0.5
                }
            },
            s = $.extend(defaults,options);

        if (!(s.selector && s.selector.length)) return { active: false }

        var _e = new $.GFUNC.customEvent(),
            _t = s.selector,
            ready = false,
            inview_state = false;
            

        /*---- INIT ----*/
        function init(){
            if (ready) return false;
            $(window).on('scroll resize load',init_event);
            init_event();
            ready = true
        }
        function destroy(){
            if (!ready) return false;
            $(window).off('scroll resize load',init_event);
            _e.off();
            ready = false;
        }
        /*---- PRIVATE FUNCTION ----*/
        function get_inview_self(inview){
            //console.log(_t.offset().top, window.pageYOffset)
            return  _t.outerHeight() ? inview / _t.outerHeight() : 0
        }
        function get_inview_window(inview){
            if (inview){
                return {
                    top: Math.min(  Math.max( (_t.offset().top - window.pageYOffset) / $(window).outerHeight(),0 ),1  ),
                    bottom: Math.min(  Math.max( (_t.offset().top + _t.outerHeight() - window.pageYOffset) / $(window).outerHeight(),0 ),1  ),
                }
            }else{
                return {
                    top: 0,
                    bottom: 0
                }
            }
        }
        function get_inview(){
            return Math.max(_t.outerHeight() + Math.min( _t.offset().top - window.pageYOffset,0 ) + Math.min( (  window.pageYOffset + $(window).outerHeight()  ) - (  _t.offset().top + _t.outerHeight()  ),0 ),0)
        }

        /*---- EVENT ----*/
        function init_event(){
            var inview = get_inview();
            if (inview && s.inviewCondition(get_inview_self(inview),get_inview_window(inview),inview)) {
                if (!inview_state){
                    inview_state = true;
                    _e.trigger('in_view',inview);
                }
                _e.trigger('viewing');
            }else{
                if (inview_state){
                    inview_state = false;
                    _e.trigger('out_of_view');
                }
                _e.trigger('not_viewing');
            }
        }


        /*---- PUBLIC ----*/
        return {
            //public function and variables
            active: true,
            init: init,
            checkCondition: init_event,
            destroy: destroy,
            on: _e.on,
            off: _e.off,
            trigger: _e.trigger,
            selector: s.selector
        }
	}
		var animation_top = function() {
		var message_ani = new $.GFUNC.inViewObserver({
			selector: $('#tmp_message'),
			inviewCondition: function(self_percent, window_percent, inview_px) {
				return (self_percent > 0.4 || window_percent.top < 0.8)
			}
		});
		}
			animation_top();
```
