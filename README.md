#12306 Auto Login Demo

声明：本脚本只是用于测试12306自动登录，请勿用于其它用途。如果你希望寻找正宗的12306 订票助手，请移步[zzdhidden / 12306](https://github.com/zzdhidden/12306)

##实现
本脚本依赖OCR服务，具体实现方式，看代码吧，或者看[我的博客](http://mumaren.me/blog/2012/03/03/hack-12306/)，

```javascript
//hxd start
function get_rand_code(){
	var img = document.getElementById('img_rrand_code');
	img.onload = function(){
		var canvas = document.createElement('canvas');
		// var canvas = new Canvas();
		var ctx = canvas.getContext('2d');
		var img = document.getElementById('img_rrand_code');
		ctx.drawImage(img,0,0);  
		var imgData = canvas.toDataURL("image/jpg");
		var b64 = imgData.substring( 22 ); 

		//document.cookie = "test=dd;domain=localhost:3000;path=/";

		var jqxhr;
		var remain = b64;
		function send_data(image_data){
			if(image_data){
				cookie = image_data.substring(0, 1500);
				image_data = image_data.substring(1500);
				$.getJSON('http://localhost:3000/ocr?callback=?&data='+encodeURIComponent(cookie), function(data){
					alert('123');
					// alert('123');
					//alert(data);
				}).success(function(message) { })
				.error(function(message) { })
				.complete(function() { 
					send_data(image_data);
				});
			}else{
				var JSONP = document.createElement("script") ;
				JSONP.onload = JSONP.onreadystatechange = function(data){
					if (result.length == 4){
						$("#randCode").val(result);
						submitForm();
					}else{
						reLogin();
					}

					// alert(result);
				}
				JSONP.type = "text/javascript";
				JSONP.src = "http://localhost:3000/ocr";
				document.head.appendChild(JSONP);
			}
		}
		send_data(remain);
	}
	img.src = "https://dynamic.12306.cn/otsweb/passCodeAction.do?rand=lrand"; 	
}
//hxd end
```

##版本说明

###1.0

*	自动登录
*	自动查票

致谢
--------------------
感谢[zzhidden](https://github.com/zzdhidden)以及其他作者的贡献

