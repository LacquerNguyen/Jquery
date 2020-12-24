# PubupJS
```javascript
$(function () {
  $(".pickup_list_cnt .list_cnt_img").click(function () {
    //get data
    var title = $(this).parents('.pickup_list_cnt').find('.title').text();
    var img = $(this).find('img').attr('src');
    $('.pubup_inner .img_name span').text(title);
    $('.pubup_inner .pubup_img img').attr('src',img);
    $(".box_pubup").fadeIn();
    if ($('.pubup_inner').outerHeight() > $(window).height()) $("body").addClass('box_open');
  });
  $(".bnt_exits").click(function () {
    $(".box_pubup").fadeOut();
    $("body").removeClass('box_open');
  });
  $(".box_pubup").click(function (event) {
    var target = $(event.target);
    if (!(target.parents('.pubup_inner').length || target.hasClass('pubup_inner'))){ //tìm class
      $(".box_pubup").fadeOut();
      $("body").removeClass('box_open');
    }
  });
  //kiểm tra chiều cao của box so với window
  $(window).on('resize',function(){
    if ($('.pubup_inner').outerHeight() > $(window).height()){ 
      $("body").addClass('box_open');
    }else{
      $("body").removeClass('box_open');
    }
  })
});
```
# PubupJS SLICK
```javascript
$(function () {
  // $('.popup-list-cnt').on('click', '.popup-cnt .box-img img',function(){
  $('.popup-list-cnt .popup-cnt .box-img img').click(function () {
      $('.pubup-show').fadeIn();
      var img = $(this).data('bigimg');
      var html = $('#popup-slider').clone();
      $('.pubup-show-slider').html(html);
      html.find('.popup-cnt .box-img img').prop('src', function () {
          return $(this).data('bigimg');
      });
      $('.popup-list-cnt').addClass('popup-slick');
      var index = $('.popup-list-cnt .popup-cnt .box-img img').index(this);
      $('.popup-slick:last').slick({
          infinite: true,
          speed: 300,
          dots: false,
          arrows: true,
          slidesToScroll: 1,
          slidesToShow: 1,
          fade: true,
          initialSlide: index
      });
  });
  $('.pubup-show-cnt .off-pupup').click(function(){
      $(".pubup-show").fadeOut();
      $("body").removeClass('box_open');   
  });
  $(".pubup-show").click(function (event) {
      var target = $(event.target);
      if (!(target.parents('.pubup-show-cnt').length || target.hasClass('pubup-show-cnt'))) {
          $(".pubup-show").fadeOut();
          $("body").removeClass('box_open');
      }
  });
  $(window).on('resize', function () {
      if ($('.pubup-show-cnt').outerHeight() > $(window).height()) {
          $("body").addClass('open_pupup');
      } else {
          $("body").removeClass('open_pupup');
      }
  })
});
```
# CSS
```CSS
//style lớp cha xuất hiện pubup
.box_pubup {
    display: none;
    position: fixed;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    z-index: 1000000;
    width: 100%;
    height: 100%;
    overflow: hidden;
    background-color: rgba(0, 0, 0, 0.5);
}
		
// style luôn nằm giữa vs màn hình window
.box_pubup .inside {
  /* position: absolute; */
  top: 50%;
  -moz-transform: translate(0,-50%);
  -webkit-transform: translate(0,-50%);
  transform: translate(0,-50%);
  width: 100%;
  max-height: 100%;
  overflow: auto;
 }
```
