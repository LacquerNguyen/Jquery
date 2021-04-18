# Jquery
```javascript

var n = $(".js-sort-target");
$(window).on("click", function() {
	$(".js-sort-list").stop().slideUp(), n.removeClass("is-active"), n.parent(".js-sort-select").removeClass("is-active")
}), n.on("click", function(e) {
	e.stopPropagation();
	var t = $(this);
	t.hasClass("is-active") || ($(".js-sort-list").stop().slideUp(), n.removeClass("is-active"), n.parent(".js-sort-select").removeClass("is-active")), t.toggleClass("is-active"), t.parent(".js-sort-select").toggleClass("is-active"), t.siblings(".js-sort-list").stop().slideToggle()
});
var news_year = $("#js-news-year"),
news_category = $("#js-news-category"),
news_tag = $("#js-news-tag"),
news_results = $("#js-news-results");
$(document).on("click", ".sort-select-item a", function(e) {
	e.preventDefault();
	var _this = $(this)
		a = _this.parent().parent().prev(),
		s = a.data("select-value");
		a.html(_this.find(">span").text())
		// a.data("select-value", _this.data("value"));
	var r = _this.parent().parent().data("type"),
	data_value = _this.data("value");
	// data_value = _this
		switch (r) {
			case "year":
				"year-all" == data_value ? news_year.val("") : news_year.val(data_value);
				break;
			case "category":
				"category-all" == data_value ? news_category.val("") : news_category.val(_this[0].innerText);
				break;
			case "tag":
				"tag-all" == data_value ? news_tag.val("") : news_tag.val(_this[0].innerText);
				break;
		}
	var arr = [];
	data = {};
	console.log(data_value)
	$('.news-list li').each(function( index, value ){
		var day = $(this).find('time').text().split(/\\|\//);
		var year = parseInt(day[0]);
		var year_txt = $(this).find('time').text();

		var category = $(this).find('.category').text();
		var tag = $(this).find('.news-article-tag span').text();
		var article_title = $(this).find('.news-article-title').text();
		var article_description = $(this).find('.news-article-description').text();
		var status = true;
		data = {
			year: year,
			category : category,
			tag : tag,
			title: article_title,
			description: article_description,
			yearTxt : year_txt
		};
			status = getStatusSearch(status, year,  news_year.val());
			status = getStatusSearch(status, category, news_category.val());
			status = getStatusSearch(status, tag, news_tag.val());
			if(status == true) {
				arr.push(data);
			}
	});
	render(arr);
})
function getStatusSearch(status, data, data_value) {
	if (status == true) {
		if(data_value != data && data_value != '') {
			status = false;
		}
	}
	return status;
}
function render(data){
	$('.l-news-list .row').empty();
	data.forEach(function( value, index ){
		var html = 
		'<li class="news-list news-list-item l-news l-news-item">'+
			'<article class="news-article news-article-wrapper">'+
				'<a class="news-article news-article-link" href="https://www.livesense.co.jp/news/2021/03/02/3216/">'+
					'<div class="news-article news-article-inner">'+
						'<div class="news-article news-article-header">'+
							'<span class="category category-media news-article news-article-category">'+value.category+'</span>'+
							'<time class="news-article news-article-date">'+value.yearTxt +'</time>'+
						'</div>'+
						'<div class="news-article news-article-title-container">'+
							'<h2 class="news-article news-article-title">'+value.title +'</h2>'+
					'</div>'+
						'<p class="news-article news-article-description">'+value.description+'</p>'+
						'<div class="news-article news-article-tag"><span>'+value.tag+'</span>'+'</div>'+
					'</div>'+
				'</a>'+
			'</article>'+
		'</li>'
		$('.l-news-list .row').append(html);
	})
}
```
