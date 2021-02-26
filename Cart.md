# Cart
```javascript
    $.Name.Cart = function(options){ //v1.0
        var defaults = {
                //Default Options
            },
            s = $.extend(defaults,options),
            //Private Variables\
            cart = new Array();
 		cart = [{
                name:'a',
                price:'2',
                image:'image',
                quantity:0
            	}];
        init();
        /*---- INIT ----*/
        function init(){
            var temp = localStorage.getItem("cart");
            if(temp){
                console.log(temp)
                cart = JSON.parse(temp)
            }
        }

        /*---- EVENT ----*/

        /*---- PRIVATE FUNCTION ----*/
        function update_local_storage(){
            localStorage.setItem("cart",JSON.stringify(cart));
        }
        function add_to_cart(name,price,image, quantity){
            var position = cart.map(function(item){ return item.name }).indexOf(name);
            if (position > -1){
                cart[position].quantity++;
            }else{
                var num_price = parseInt(price.split(',').join(''));
                cart.push({
                    name:name,
                    price:num_price,
                    image:image,
                    quantity:quantity
                })
            }
            update_local_storage();
            //popup
        }
        function render(){
            cart.forEach(function(item,index){
                cart_item = $('<li>'+
                '<div class="product-txt">'+
                    '<p class="item-img">' +
                        '<img src="'+ item.image +'" width="80" height="80" alt="">'+
                    '</p>'+
                    '<p class="item-txt ">'+ item.name +'</p>'+
                '</div>'+
                '<div class="unit-price clearfix"><p class="item-number">'+toCurrencyFormat(item.price)+'</p></div>'+
                '<div class="quantity-txt">'+
                    '<input type="number" class="item-number" value="'+item.quantity+'">'+
                    '<button type="button" class="btn-plus item-submit"><i class="fas fa-plus"></i></button>'+
                    '<button type="button" class="btn-minus item-submit"><i class="fas fa-minus"></i></button>'+
                '</div>'+
                '<div class="subtotal-txt">' +
                    '<p class="item-number">'+ toCurrencyFormat(item.price * item.quantity) +' 円</p>'+
                    '<button type="button" class="btn item-delete" data-toggle="modal" data-target="#model-confirm"><i class="fas fa-trash-alt"></i>削除</button>'+
                '</div>'
                +'</li>');
                $(".infor-cart .list-show-cnt").append(cart_item);
                cart_item.find('.item-delete').on("click",function(){
                    var cartItem = $(this).parents('li');
                    modalConfirm(function(confirm){
                        if(confirm){
                            delete_cart(cartItem.find('.product-txt .item-txt').html(),cartItem);
                        }
                    });
                });
                var modalConfirm = function(callback){
                    $("#btn-yes").on("click", function(){
                      callback(true);
                    });
                    
                    $("#btn-no").on("click", function(){
                      callback(false);
                    });
                };
                cart_item.find('.btn-plus').on("click",function () {
                    $(this).parents('li').find('.quantity-txt .item-number').val(parseInt($(this).parents('li').find('.quantity-txt .item-number').val()) + 1);
                    change_quantity($(this).parents('li'),$(this).parents('li').find('.quantity-txt .item-number').val(),$(this).parents('li').find('.subtotal-txt .item-number'))
                });
                cart_item.find('.btn-minus').on("click",function () {
                    $(this).parents('li').find('.quantity-txt .item-number').val(Math.max(parseInt($(this).parents('li').find('.quantity-txt .item-number').val()) - 1,1))
                    change_quantity($(this).parents('li'),$(this).parents('li').find('.quantity-txt .item-number').val(),$(this).parents('li').find('.subtotal-txt .item-number'))
                });
                cart_item.find(".quantity-txt .item-number").on("change",function(){
                    change_quantity($(this).parents('li'),$(this).parents('li').find('.quantity-txt .item-number').val(),$(this).parents('li').find('.subtotal-txt .item-number'))
                });
            })
        }
        
        function change_quantity(cartItem,quantity,selector){
            cartItem.find('.quantity-txt').find('.item-number').val(quantity);
            var itemPrice = parseInt(cartItem.find('.unit-price ').find('.item-number').html().split(',').join(''));
            selector.text(toCurrencyFormat(quantity*itemPrice) +' 円');
            var itemName = cartItem.find('.product-txt').find('.item-txt').html();
            var itemPrice = cartItem.find('.unit-price ').find('.item-number').html();
            var itemImg = cartItem.find('.product-txt ').find('img').attr('src');
            var itemQuantity = cartItem.find('.quantity-txt').find('.item-number').val();
            add_to_cart(itemName, itemPrice, itemImg, itemQuantity);
        }
        function delete_cart(itemName,selector){
            cart.forEach(function(item,index){
                if(cart[index].name == itemName) {
                    cart.splice(index,1);
                }
            });
            selector.remove();
            update_local_storage();
        }
        function toCurrencyFormat(number){
            return number.toString().replace(/(\d)(?=(\d{3})+(?!\d))/g, '$1,')
        }
        /*---- PUBLIC ----*/
        return {
            //public function and variables
            AddToCart : add_to_cart,
            Render: render
        }
    }
    	var Cart= new $.GDITFUNC.Cart();
		$('.add_cart').on("click", function(){
			var name = $(this).parents('.product-block').find('.product-ttl').text();
			var price = $(this).parents('.product-block').find('.product-price .quantity').text();
			var image = $(this).parents('.product-block').find('.media-left .product-img img').attr('src');
			var quantity = 1;
			Cart.AddToCart(name,price,image, quantity);
			alert('カートに入りました')
		});
			if($('.infor-cart .list-show-cnt').length){
				Cart.Render();
			}
			if($('.infor-cart .list-show-cnt li').length == false){
				$('.infor-cart .list-show-info').remove()
				$('.infor-cart .notification-txt p').remove();
				$('.infor-cart .notification-txt').append(
						'<p>お客様のショッピングカートに商品がありません。</p>'+
						'<p><a href="../image_PC-03.html">ショッピングを続ける</a></p>'
					)
				$('.infor-cart .the-cart-total').remove();
			}
```
