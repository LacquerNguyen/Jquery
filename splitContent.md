# splitContent
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
  var a=  new $.Name.splitContent({
      selector: $('#tmp_kyouso_main_visual .main_visual_scene1 .des_text_1'),
    }).getContent(),
```
