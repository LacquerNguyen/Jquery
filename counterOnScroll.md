# counterOnScroll
```javascript
		counterOnScroll: function () {
			var a = 0;
			if($('#counter').length == 0) return;
			$(window).scroll(function () {

				var oTop = $('#counter').offset().top - window.innerHeight;
				if (a == 0 && $(window).scrollTop() > oTop) {
					$('.counter-value').each(function () {
						var $this = $(this),
							countTo = $this.attr('data-count');
						$({
							countNum: $this.text()
						}).animate({
							countNum: countTo
						},
							{
								duration: 2000,
								easing: 'swing',
								step: function () {
									$this.text(Math.floor(this.countNum));
								},
								complete: function () {
									$this.text(this.countNum);
									//alert('finished');
								}

							});
					});
					a = 1;
				}

			});
		},
		$('.count').each(function () {
		    $(this).prop('Counter',0).animate({
			Counter: $(this).text()
		    }, {
			duration: 4000,
			easing: 'swing',
			step: function (now) {
			    $(this).text(Math.ceil(now));
			}
		    });
		});
```
