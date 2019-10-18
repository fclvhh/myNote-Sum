[参考链接]<https://juejin.im/post/5b1cebece51d4506ae71addf>

# 定调--什么是AJAX?

Async  Javascript  and  XML

说人话:

​	异步的javascript 和 XML

还是不懂?

​	就是一种用js发送请求的技术!



# 历史过程

## 如何发请求？

用 form 可以发请求，但是会刷新页面或新开页面
 用 a 可以发 get 请求，但是也会刷新页面或新开页面
 用 img 可以发 get 请求，但是只能以图片的形式展示
 用 link 可以发 get 请求，但是只能以 CSS、favicon 的形式展示
 用 script 可以发 get 请求，但是只能以脚本的形式运行

有没有什么方式可以实现

1. get、post、put、delete 请求都行

2. 想以什么形式展示就以什么形式展示



## 微软的突破

IE 5 率先在 JS 中引入 ActiveX 对象（API），使得 JS 可以直接发起 HTTP 请求。
 随后 Mozilla、 Safari、 Opera 也跟进（抄袭）了，取名 XMLHttpRequest，并被纳入 W3C 规范



## 为什么不用XML了?

JSON 好用啊!为什么还要用API 不好用的XML?



## 如何使用XMLHttpRequest

```javascript
myButton.addEventListener('click', (e)=>{
  let request = new XMLHttpRequest()
  request.open('get', '/xxx') // 配置request
  request.send()
  request.onreadystatechange = ()=>{
    if(request.readyState === 4){
      console.log('请求响应都完毕了')
      console.log(request.status)
      if(request.status >= 200 && request.status < 300){
        console.log('说明请求成功')
        console.log(typeof request.responseText)
        console.log(request.responseText)
        let string = request.responseText
        // 把符合 JSON 语法的字符串
        // 转换成 JS 对应的值
        let object = window.JSON.parse(string) 
        // JSON.parse 是浏览器提供的
        console.log(typeof object)
        console.log(object)
        console.log('object.note')
        console.log(object.note)

      }else if(request.status >= 400){
        console.log('说明请求失败') 
      }

    }
  }
})

// 后端代码
  }else if(path==='/xxx'){
    response.statusCode = 200
    response.setHeader('Content-Type', 'text/json;charset=utf-8')
    response.setHeader('Access-Control-Allow-Origin', 'http://frank.com:8001')
    response.write(`
    {
      "note":{
        "to": "小谷",
        "from": "方方",
        "heading": "打招呼",
        "content": "hi"
      }
    }
    `)
    response.end()


```





# AJAX的同源策略

只有 协议+端口+域名 **一模一样**才允许发 AJAX 请求

1. [http://baidu.com](http://baidu.com/) 可以向 [http://www.baidu.com](http://www.baidu.com/) 发 AJAX 请求吗 no

2. [http://baidu.com:80](http://baidu.com/) 可以向 [http://baidu.com:81](http://baidu.com:81/) 发 AJAX 请求吗 no



浏览器必须保证:只有 协议+端口+域名 一模一样才允许发 AJAX 请求

处于保护隐私的角度考虑,也是互联网的基石



原因分析:

举个例子:

​	只有你本人可以看你的日记,如果别人发的AJAX请求,你的日记本服务器响应了,别人就可以看你的日记了!!!





# CORS 跨域

就一句代码

```javascript
res.setHeader('Access-Control-Allow-Origin','地址')
```







# jQuery实现AJAX!

```javascript
$(function(){

		$("#btn").click(function(){
			$.ajax({
				url:"data.php",
				dataType:"text",
				type:"get",
				success:function(data){
					alert(data);
					//$("#showInfo").html(data);
				},
				error:function(e){
					console.log(e);
				}
			});
		});
```



完整版本：

```
$.ajax({
    type: "post", 【以POST或GET的方式请求。默认GET。PUT和DELETE也可以用，有的浏览器不支持】
    url: url,   【请求的目的地址，须是一个字符串。】
    contentType: "application/json",       【以哪种数据类型发送请求】
    data: data,    【请求的数据】
    dataType: "json",  【想从服务器得到的数据类型。html,json,jsonp,text】
    async:false,【默认为true异步请求，设置为false时为同步请求】
    beforeSend:function(){......},  【传递异步请求之前的事件】
    success:function(){......},  【请求成功之后的回调】
    error:function(){......},    【请求失败之后的回调】
    complete(function(){......},  【不管请求成功还是错误，只要请求完成，可以执行的事件。】 
});
```



格式举例：

```javascript
$.ajax({
        url:'01.php',//请求地址
        data:'name=fox&age=18',//发送的数据
        type:'GET',//请求的方式
        success:function (argument) {},// 请求成功执行的方法
        beforeSend:function (argument) {},// 在发送请求之前调用,可以做一些验证之类的处理
        error:function (argument) {console.log(argument);},//请求失败调用
    })
```

