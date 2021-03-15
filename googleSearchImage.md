# Jquery
```javascript
//========================================
	//▼googleカスタム検索のロゴ表示
	//========================================
	$.Name.googleSearchImage = function(options){
		var c = $.extend({
			area: '#tmp_query',
			backgroundProperty: '#FFFFFF url(/cms8341/shared/images/gsearch/googlelogo_lightgrey_46x16dp.png) no-repeat left center',
			focusBackgroundProperty: '#FFFFFF',
			placeholderTxt: 'カスタム検索',
			placeholderIndent: 50
		},options);
		$(c.area).each(function(){
			var self = $(this);
			self
				.css({ 'background': c.backgroundProperty, 'text-indent': c.placeholderIndent })
				.attr('placeholder', c.placeholderTxt)
				.on('focus.googleSearchImage', function(){
					$(this).removeAttr('placeholder')	.css({ 'background': c.focusBackgroundProperty ,'text-indent': '0' });
				})
				.on('blur.googleSearchImage', function(){
					if($(this).val() == ''){
						$(this).css({ 'background': c.backgroundProperty, 'text-indent': c.placeholderIndent })
						.attr('placeholder', c.placeholderTxt);
					}
				});
			if(self.val() != ''){
				self.css({ background: c.focusBackgroundProperty });
			}
		});
	};
  		$.Name.googleSearchImage({
			area: '#tmp_query',
			placeholderTxt: '',
			backgroundProperty: '#ffffff url() no-repeat 31px 8px',
		});
```
