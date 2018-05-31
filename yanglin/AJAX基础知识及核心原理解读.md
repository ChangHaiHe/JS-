##AJAX基础知识及核心原理解读
@(201712)

###AJAX基础知识
> 什么是AJAX？
> async javascript and xml，异步的JS和XML

`xml：可扩展的标记语言`
> 作用是用来存储数据的（通过自己扩展的标记名称清晰的展示出数据结构）
>  
> ajax之所以称为异步的js和xml，主要原因是：当初最开始用ajax实现客户端和服务器端数据通信的时候，传输的数据格式一般都是xml格式的数据，我们我们把它称之为异步js和xml（现在一般都是基于JSON格式来进行数据传输的）
```xml
<?xml version="1.0" encoding="UTF-8"?>
<root>
    <student>
        <name>张三</name>
        <age>25</age>
        <score>
            <english>90</english>
            <math>100</math>
            <chinese>98</chinese>
        </score>
    </student>
    <student>
        <name>李四</name>
        <age>24</age>
        <score>
            <english>80</english>
            <math>90</math>
            <chinese>100</chinese>
        </score>
    </student>
</root>
```

`异步的JS`
> 这里的异步不是说ajax只能基于异步进行请求（虽然建议都是使用异步编程），这里的异步特指的是 **“局部刷新”**


`局部刷新 VS 全局刷新` 
> 在非完全前后端分离项目中，前端开发只需要完成页面的制作，并且把一些基础的人机交互效果使用js完成即可，页面中需要动态呈现内容的部分，都是交给后台开发工程师做数据绑定和基于服务器进行渲染的（服务器端渲染）
>  
> [优势]
> 1、动态展示的数据在页面的原代码中可以看见，有利于SEO优化推广（有利于搜索引擎的收录和抓取）
> 2、从服务器端获取的结果就已经是最后要呈现的结果了，不需要客户端做额外的事情，所以页面加载速度快（前提是服务器端处理的速度够快，能够处理过来），所以类似于京东、淘宝这些网站，首屏数据一般都是经由服务器端渲染的
>  
> [弊端]
> 1、如果页面中存在需要实时更新的数据，每一次想要展示最新的数据，页面都要重新的刷新一次，这样肯定不行
> 2、都交给服务器端做数据渲染，服务器端的压力太大，如果服务器处理不过来，页面呈现的速度更慢（所以京东淘宝这类网站，除了首屏是服务器端渲染的，其它屏一般都是客户端做数据渲染绑定）
> 3、这种模式不利于开发（开发效率低）
![Alt text](./1513152523913.png)

> 目前市场上大部分项目都是前后端完全分离的项目（也有非完全前后端分离的）
> 
> 前后端完全分离的项目，页面中需要动态绑定的数据是交给客户端完成渲染的
> 1、向服务器端发送AJAX请求
> 2、把从服务器端获取的数据解析处理，拼接成为我们需要展示的HTML字符串
> 3、把拼接好的字符串替换页面中某一部分的内容（局部刷新），页面整体不需要重新加载，局部渲染即可
>  
> [优势]
> 1、我们可以根据需求，任意修改页面中某一部分的内容（例如实时刷新），整体页面不刷新，性能好，体验好（所有表单验证、需要实时刷新的等需求都要基于AJAX实现）
> 
> 2、有利于开发，提高开发的效率
> 1）前后端的完全分离，后台不需要考虑前端如何实现，前端也不需要考虑后台用什么技术，真正意义上实现了技术的划分
> 2）可以同时进行开发：项目开发开始，首先制定前后端数据交互的接口文档（文档中包含了，调取哪个接口或者哪些数据等协议规范），后台把接口先写好（目前很多公司也需要前端自己拿NODE来模拟这些接口），客户端按照接口调取即可，后台再次去实现接口功能即可 
>  
> [弊端]
> 1、不利于SEO优化：第一次从服务器端获取的内容不包含需要动态绑定的数据，所以页面的源代码中没有这些内容，不利于SEO收录，后期通过JS添加到页面中的内容，并不会写在页面的源代码中（是源代码不是页面结构）
> 2、交由客户端渲染，首先需要把页面呈现，然后再通过JS的异步AJAX请求获取数据，然后数据绑定，浏览器在把动态增加的部分重新渲染，无形中浪费了一些时间，没有服务器端渲染页面呈现速度快
> 3、AJAX干掉了Back和History功能，即对浏览器机制的破坏
![Alt text](./1513153932539.png)

###Ajax的工作原理
相当于在客户端与服务端之间加了一个抽象层(Ajax引擎)，使用户请求和服务器响应异步化，并不是所有的请求都提交给服务器，像一些数据验证和数据处理
都交给Ajax引擎来完成，只有确认需要向服务器读取新数据时才右Ajax引擎向服务器提交请求

###基于原生JS实现AJAX
```javascript
//=>创建一个AJAX对象
let xhr=new XMLHttpRequest();//=>不兼容IE6及更低版本浏览器(IE6：ActiveXObject)

//=>打开请求地址(可以理解为一些基础配置，但是并没有发送请求呢)
xhr.open([method],[url],[async],[user name],[user password]);

//=>设置HTTP请求头信息
setRequestHeader("Content-type","application/x-www-form-urlencoded");

//=>监听AJAX状态改变，获取响应信息(获取响应头信息、获取响应主体信息)
xhr.onreadystatechange=()=>{
	if(xhr.readyState===4 && xhr.status===200){
		let result=xhr.responseText;//=>获取响应主体中的内容
	}
};

//=>发送AJAX请求（括号中传递的内容就是请求主体的内容）
xhr.send(null);
```
**`分析第二步中的细节点`**
> xhr.open([method],[url],[async],[user name],[user password])

---
> [AJAX请求方式]
> 1、GET系列的请求（获取）
> - get
> - delete：从服务器上删除某些资源文件
> - head：只想获取服务器返回的响应头信息（响应主体内容不需要获取）
> - ...
> 
> 2、POST系列请求（推送）
> - post
> - put：向服务器中增加指定的资源文件
> - ...
>   
> 不管哪一种请求方式，客户端都可以把信息传递给服务器，服务器也可以把信息返回给客户端，只是GET系列一般以获取为主（给的少，拿回来的多），而POST系列一般以推送为主（给的多，拿回来的少）
> 1）我们想获取一些动态展示的信息，一般使用GET请求，因为只需要向服务器端发送请求，告诉服务器端我们想要什么，服务器端就会把需要的数据返回
> 2）在实现注册功能的时候，我们需要把客户输入的信息发送给服务器进行存储，服务器一般返回成功还是失败等状态，此时我们一般都是基于POST请求完成
> ...
>  
> GET系列请求和POST系列请求，在项目实战中存在很多的区别
> 1、GET请求传递给服务器的内容一般没有POST请求传递给服务器的内容多
> 原因：GET请求传递给服务器内容一般都是基于`url地址问号传递参数`来实现的，而POST请求一般都是基于`设置请求主体`来实现的。
> 各浏览器都有自己的关于URL最大长度的限制（谷歌：8KB、火狐：7KB、IE：2KB...）超过限制长度的部分，浏览器会自动截取掉，导致传递给服务器的数据缺失。
> 理论上POST请求通过请求主体传递是没有大小限制的，真实项目中为了保证传输的速率，我们也会限制大小（例如：上传的资料或者图片我们会做大小的限制）
>  
> 2、GET请求很容易出现缓存（这个缓存不可控：一般我们都不需要），而POST不会出现缓存（除非自己做特殊处理）；
> 原因：GET是通过URL问号传参传递给服务器信息，而POST是设置请求主体；
> 设置请求主体不会出现缓存，但是URL传递参数就会了。
```javascript
//=>每个一分钟从新请求服务器端最新的数据，然后展示在页面中（页面中某些数据实时刷新）
setTimeout(()=>{
	$.ajax({
		url:'getList?lx=news',
		...
		success:result=>{
			//=>第一次请求数据回来，间隔一分钟后，浏览器又发送一次请求，但是新发送的请求，不管是地址还是传递的参数都和第一次一样，浏览器很有可能会把上一次数据获取，而不是获取最新的数据
		}
	});
},60000);

//=>解决方案：每一次重新请求的时候，在URL的末尾追加一个随机数，保证每一次请求的地址不完全一致，就可以避免是从缓存中读取的数据
setTimeout(()=>{
	$.ajax({
		url:'getList?lx=news&_='+Math.random(),
		...
		success:result=>{}
	});
},60000);
```
> 3、GET请求没有POST请求安全（POST也并不是十分安全，只是相对安全）
> 原因：还是因为GET是URL传参给服务器
> 有一种比较简单的黑客技术：URL劫持，也就是可以把客户端传递给服务器的数据劫持掉，导致信息泄露

---
> URL：请求数据的地址（API地址），真实项目中，后台开发工程师会编写一个API文档，在API文档中汇总了获取哪些数据需要使用哪些地址，我们按照文档操作即可
>  
> ASYNC：异步（SYNC同步），设置当前AJAX请求是异步的还是同步的，不写默认是异步（TRUE），如果设置为FALSE，则代表当前请求是同步的
>  
> 用户名和密码：这两个参数一般不用，如果你请求的URL地址所在的服务器设定了访问权限，则需要我们提供可通行的用户名和密码才可以（一般服务器都是可以允许匿名访问的）

**`第三部分细节研究`**
```javascript
//=>监听AJAX状态改变，获取响应信息(获取响应头信息、获取响应主体信息)
xhr.onreadystatechange=()=>{
	if(xhr.readyState===4 && xhr.status===200){
		let result=xhr.responseText;//=>获取响应主体中的内容
	}
};
```
> AJAX状态码：描述当前AJAX操作的状态的
> xhr.readyState
>  
> 0：UNSENT 未发送，只要创建一个AJAX对象，默认值就是零
> 1：OPENED 我们已经执行了xhr.open这个操作
> 2：HEADERS_RECEIVED 当前AJAX的请求已经发送，并且已经接收到服务器端返回的响应头信息了
> 3：LOADING 响应主体内容正在返回的路上
> 4：DONE 响应主体内容已经返回到客户端

---
> HTTP网络状态码：记录了当前服务器返回信息的状态 xhr.status
> 200：成功，一个完整的HTTP事务完成（以2开头的状态码一般都是成功）
>  
> 以3开头一般也是成功，只不过服务器端做了很多特殊的处理
> 301：Moved Permanently  永久转移（永久重定向）`一般应用于域名迁移`
> 302：Move temporarily 临时转移（临时重定向，新的HTTP版本中任务307是临时重定向）`一般用于服务器的负载均衡：当前服务器处理不了，我把当前请求临时交给其他的服务器处理（一般图片请求经常出现302，很多公司都有单独的图片服务器）`
> 304：Not Modified 从浏览器缓存中获取数据 `把一些不经常更新的文件或者内容缓存到浏览器中，下一次从缓存中获取，减轻服务器压力，也提高页面加载速度`
>  
> 以4开头的，一般都是失败，而且客户端的问题偏大
> 400：请求参数错误
> 401：无权限访问
> 404：访问地址不存在
>  
> 以5开头的，一般都是失败，而且服务器的问题偏大
> 500：Internal Server Error 未知的服务器错误
> 503：Service Unavailable 服务器超负载
> ...

**`AJAX中其它常用的属性和方法`**
> 面试题：AJAX中总共支持几个方法？
>  
> let xhr=new XMLHttpRequest();
> console.dir(xhr);
>  
> [属性]
> - readyState：存储的是当前AJAX的状态码
> - response / responseText / responseXML ：都是用来接收服务器返回的响应主体中的内容，只是根据服务器返回内容的格式不一样，我们使用不同的属性接收即可
>    +  responseText是最常用的，接收到的结果是字符串格式的（一般服务器返回的数据都是JSON格式字符串）
>    + responseXML偶尔会用到，如果服务器端返回的是XML文档数据，我们需要使用这个属性接收
> - status：记录了服务器端返回的HTTP状态码
> - statusText：对返回状态码的描述
> - timeout：设置当前AJAX请求的超时时间，假设我们设置时间为3000(MS)，从AJAX请求发送开始，3秒后响应主体内容还没有返回，浏览器会把当前AJAX请求任务强制断开
>  
> [方法]
> - abort()：强制中断AJAX请求
> - getAllResponseHeaders()：获取全部的响应头信息（获取的结果是一堆字符串文本）
> - getResponseHeader(key)：获取指定属性名的响应头信息，例如：xhr.getResponseHeader('date') 获取响应头中存储的服务器的时间
> - open()：打开一个URL地址
> - overrideMimeType()：重写数据的MIME类型
> - send()：发送AJAX请求（括号中书写的内容是客户端基于请求主体把信息传递给服务器）
> - setRequestHeader(key,value)：设置请求头信息（可以是设置的自定义请求头信息）
>  
> [事件]
> - onabort：当AJAX被中断请求触发这个事件
> - onreadystatechange：AJAX状态发生改变，会触发这个事件
> - ontimeout：当AJAX请求超时，会触发这个事件
> - ...
```javascript
let xhr = new XMLHttpRequest();
// xhr.setRequestHeader('aaa', 'xxx');//=>Uncaught DOMException: Failed to execute 'setRequestHeader' on 'XMLHttpRequest': The object's state must be OPENED. 设置请求头信息必须在OPEN之后和SEND之前
xhr.open('get', 'temp.xml?_=' + Math.random(), true);
// xhr.setRequestHeader('cookie', '珠峰培训');//=>Uncaught DOMException: Failed to execute 'setRequestHeader' on 'XMLHttpRequest': '珠峰培训' is not a valid HTTP header field value. 设置的请求头内容不是一个有效的值（请求头部的内容中不得出现中文汉字）
xhr.setRequestHeader('aaa', 'xxx');

//=>设置超时
xhr.timeout = 100000;
xhr.ontimeout = ()=> {
    console.log('当前请求已经超时');
    xhr.abort();
};

xhr.onreadystatechange = ()=> {
    let {readyState:state, status}=xhr;

    //=>说明请求数据成功了
    if (!/^(2|3)\d{2}$/.test(status)) return;

    //=>在状态为2的时候就可以获取响应头信息
    if (state === 2) {
        let headerAll = xhr.getAllResponseHeaders(),
            serverDate = xhr.getResponseHeader('date');//=>获取的服务器时间是格林尼治时间(相比于北京时间差了8小时)，通过new Date可以把这个时间转换为北京时间
        console.log(headerAll, new Date(serverDate));
        return;
    }

    //=>在状态为4的时候响应主体内容就已经回来了
    if (state === 4) {
        let valueText = xhr.responseText,//=>获取到的结果一般都是JSON字符串(可以使用JSON.PARSE把其转换为JSON对象)
            valueXML = xhr.responseXML;//=>获取到的结果是XML格式的数据(可以通过XML的一些常规操作获取存储的指定信息)  如果服务器返回的是XML文档,responseText获取的结果是字符串而responseXML获取的是标准XML文档
        console.log(valueText, valueXML);
    }
};
xhr.send('name=zxt&age=28&sex=man');
```

###JS中常用的编码解码方法
> 正常的编码解码（非加密）
> 1、escape / unescape：主要就是把中文汉字进行编码和解码的（一般只有JS语言支持：也经常应用于前端页面通信时候的中文汉字编码）
> ![Alt text](./1513220723614.png)
> 
> 2、encodeURI / decodeURI：基本上所有的编程语言都支持
> ![Alt text](./1513220919779.png)
> 
> 3、encodeURIComponent / decodeURIComponent：和第二种方式非常的类似，区别在于
> ![Alt text](./1513220973796.png)

> 也可以通过加密的方法进行编码解码
> 1、可逆转加密（一般都是团队自己玩的规则）
> 2、不可逆转加密（一般都是基于MD5加密完成的，可能会把MD5加密后的结果二次加密）
> 
> ----
> 需求：我们URL问号传递参数的时候，我们传递的参数值还是一个URL或者包含很多特殊的字符，此时为了不影响主要的URL，我们需要把传递的参数值进行编码。使用encodeURI不能编码一些特殊字符，所以只能使用encodeURLComponent处理
```javascript
let str = 'http://www.baidu.com/?',
	obj = {
		name:'珠峰培训',
		age:9,
		url:'http://www.zhufengpeixun.cn/?lx=1'
	};
//=>把OBJ中的每一项属性名和属性值拼接到URL的末尾（问号传参方式）
for(let key in obj){
	str+=`${key}=${encodeURIComponent(obj[key])}&`;
	//=>不能使用encodeURI必须使用encodeURIComponent，原因是encodeURI不能编码特殊的字符
}
console.log(str.replace(/&$/g,''));

//=>后期获取URL问号参数的时候，我们把获取的值在依次的解码即可
String.prototype.myQueryUrlParameter=function myQueryUrlParameter(){
	let reg=/[?&]([^?&=]+)(?:=([^?&=]*))?/g,
	obj={};
	this.replace(reg,(...arg)=>{
	    let [,key,value]=arg; 		
	    obj[key]=decodeURIComponent(value);//=>此处获取的时候可以进行解码	
	});
	return obj;
}
```

###AJAX中的同步和异步
> AJAX这个任务：发送请求接收到响应主体内容（完成一个完整的HTTP事务）
> xhr.send()：任务开始
> xhr.readyState===4：任务结束
```javascript
let xhr = new XMLHttpRequest();
xhr.open('get', 'temp.json', false);
xhr.onreadystatechange = ()=> {
    console.log(xhr.readyState);
};
xhr.send();
//=>只输出一次结果是4
```
![Alt text](./1513224345022.png)

```javascript
let xhr = new XMLHttpRequest();
xhr.open('get', 'temp.json', false);
xhr.send();//=>[同步]开始发送AJAX请求,开启AJAX任务,在任务没有完成之前,什么事情都做不了(下面绑定事件也做不了) => LOADING => 当readyState===4的时候AJAX任务完成，开始执行下面的操作
//=>readyState===4
xhr.onreadystatechange = ()=> {
    console.log(xhr.readyState);
};
//=>绑定方法之前状态已经为4了,此时AJAX的状态不会在改变成其它值了,所以事件永远不会被触发,一次都没执行方法（使用AJAX同步编程，不要把SEND放在事件监听前，这样我们无法在绑定的方法中获取到响应主体的内容）
```

```javascript
let xhr = new XMLHttpRequest();
xhr.open('get', 'temp.json');
xhr.onreadystatechange = ()=> {
    console.log(xhr.readyState);
};
xhr.send();
//=>输出3次，结果分别是 2 3 4
```
![Alt text](./1513225097180.png)

```javascript
let xhr = new XMLHttpRequest();
xhr.open('get', 'temp.json');
xhr.send();
//=>xhr.readyState===1
xhr.onreadystatechange = ()=> {
    console.log(xhr.readyState);
};
//=>2 3 4
```

```javascript
let xhr = new XMLHttpRequest();
xhr.onreadystatechange = ()=> {
    console.log(xhr.readyState);
};
xhr.open('get', 'temp.json');
xhr.send();
//->1 2 3 4
```

```javascript
let xhr = new XMLHttpRequest();
//=>xhr.readyState===0
xhr.onreadystatechange = ()=> {
    console.log(xhr.readyState);
};
xhr.open('get', 'temp.json', false);
//=>xhr.readyState===1 AJAX特殊处理的一件事：执行OPEN状态变为1,会主动把之前监听的方法执行一次，然后再去执行SEND
xhr.send();
//=>xhr.readyState===4 AJAX任务结束,主任务队列完成
//=>1 4
```

###实战案例：倒计时抢购
```javascript
~function () {
    let box = document.getElementById('box'),
        serverTime = null;

    let fn = ()=> {
        //=>1、计算当前时间和目标时间的差值
        //=>new Date()：获取的是客户端本机时间(会受到客户端自己调整时间的影响),重要的时间参考不能基于这个完成,不管是哪一个客户端都需要基于相同的服务器时间计算
        //=>每间隔1S中,我们需要把第一次获取的服务器时间进行累加
        serverTime = serverTime + 1000;
        let tarTime = new Date('2017/12/14 13:12:20').getTime(),
            spanTime = tarTime - serverTime;

        //=>2、计算差值中包含多少时分秒
        if (spanTime < 0) {
            //=>已经错过了抢购的时间(已经开抢了)
            box.innerHTML = '开抢啦！！';
            clearInterval(autoTimer);
            return;
        }
        let hours = Math.floor(spanTime / (1000 * 60 * 60));
        spanTime -= hours * 3600000;
        let minus = Math.floor(spanTime / (1000 * 60));
        spanTime -= minus * 60000;
        let seconds = Math.floor(spanTime / 1000);
        hours < 10 ? hours = '0' + hours : null;
        minus < 10 ? minus = '0' + minus : null;
        seconds < 10 ? seconds = '0' + seconds : null;
        box.innerHTML = `距离开抢还剩下 ${hours}:${minus}:${seconds}`;
    };
    let autoTimer = setInterval(fn, 1000);

    //=>从服务器端获取服务器时间
    let getServerTime = ()=> {
        let xhr = new XMLHttpRequest();
        xhr.onreadystatechange = ()=> {
            //console.log(xhr.readyState);//=>HEAD请求方式,状态码中没有3(不需要等待响应主体内容)
            if (!/^(2|3)\d{2}$/.test(xhr.status)) return;
            if (xhr.readyState === 2) {
                serverTime = new Date(xhr.getResponseHeader('date')).getTime();
                fn();
            }
        };
        xhr.open('head', 'temp.xml', true);
        xhr.send(null);

        /*
         * 获取服务器时间总会出现时间差的问题：服务器端把时间记录好，到客户端获取到事件有延迟差（服务器返回的时候记录的是10:00，我们客户端获取的时候已经是10:01，但是我们获取的结果依然是10:00，这样就有1分钟时间差）
         *
         * [尽可能的减少时间差，是我们优化的全部目的]
         * 
         * 1、服务器返回的时间在响应头信息中就有，我们只需要获取响应头信息即可，没必要获取响应主体内容，所以请求方式使用 HEAD 即可
         * 2、必须使用异步编程：同步编程我们无法在状态为2或者3的时候做一些处理，而我们获取响应头信息，在状态为2的时候就可以获取了，所以需要使用异步
         * 3、在状态为2的时候就把服务器时间获取到
         * ...
         */
    };
    getServerTime();
}();
```

###AJAX类库的封装
> JQ中的AJAX使用及每一个配置的作用（包含里面的一些细节知识）
```javascript
$.ajax({
	url:'xxx.txt',//=>请求API地址
	method:'get',//=>请求方式GET/POST...在老版本JQ中使用的是type，使用type和method实现的是相同的效果
	dataType:'json',//=>dataType只是我们预设获取结果的类型，不会影响服务器的返回（服务器端一般给我们返回的都是JSON格式字符串）,如果我们预设的是json，那么类库中将把服务器返回的字符串转换为json对象，如果我们预设的是text(默认值)，我们把服务器获取的结果直接拿过来操作即可，我们预设的值还可以是xml等
	cache:false,//=>设置是否清除缓存，只对GET系列请求有作用，默认是TRUE不清缓存，手动设置为FALSE，JQ类库会在请求URL的末尾追加一个随机数来清除缓存
	data:null,//=>我们通过DATA可以把一些信息传递给服务器；GET系列请求会把DATA中的内容拼接在URL的末尾通过问号传参的方式传递给服务器，POST系列请求会把内容放在请求主体中传递给服务器；DATA的值可以设置为两种格式：字符串、对象，如果是字符串，设置的值是什么传递给服务器的就是什么，如果设置的是对象，JQ会把对象变为 xxx=xxx&xxx=xxx 这样的字符串传递给服务器
	async:true,//=>设置同步或者异步，默认是TRUE代表异步，FALSE是同步
	success:function(result){
		//=>当AJAX请求成功 (readyState===4 & status是以2或者3开头的)
		//=>请求成功后JQ会把传递的回调函数执行，并且把获取的结果当做实参传递给回调函数	（result就是我们从服务器端获取的结果）
	},
	error:function(msg){},//=>请求错误触发回调函数
	complate:function(){},//=>不管请求是错误的还是正确的都会触发回调函数（它是完成的意思）
	...
});
```
> 封装属于自己的AJAX类库
> 
> [支持的参数]
> - url
> - method / type
> - data
> - dataType
> - async
> - cache
> - success
> - ...
```javascript
~function () {
    class ajaxClass {
        //=>SEND AJAX
        init() {
            //=>THIS:EXAMPLE
            let xhr = new XMLHttpRequest();
            xhr.onreadystatechange = ()=> {
                if (!/^[23]\d{2}$/.test(xhr.status)) return;
                if (xhr.readyState === 4) {
                    let result = xhr.responseText;
                    //=>DATA-TYPE
                    try {
                        switch (this.dataType.toUpperCase()) {
                            case 'TEXT':
                            case 'HTML':
                                break;
                            case 'JSON':
                                result = JSON.parse(result);
                                break;
                            case 'XML':
                                result = xhr.responseXML;
                        }
                    } catch (e) {
                        
                    }
                    this.success(result);
                }
            };

            //=>DATA
            if (this.data !== null) {
                this.formatData();

                if (this.isGET) {
                    this.url += this.querySymbol() + this.data;
                    this.data = null;
                }
            }

            //=>CACHE
            this.isGET ? this.cacheFn() : null;

            xhr.open(this.method, this.url, this.async);
            xhr.send(this.data);
        }

        //=>CONVERT THE PASSED OBJECT DATA TO STRING DATA
        formatData() {
            //=>THIS:EXAMPLE
            if (Object.prototype.toString.call(this.data) === '[object Object]') {
                let obj = this.data,
                    str = ``;
                for (let key in obj) {
                    if (obj.hasOwnProperty(key)) {
                        str += `${key}=${obj[key]}&`;
                    }
                }
                str = str.replace(/&$/g, '');
                this.data = str;
            }
        }

        cacheFn() {
            //=>THIS:EXAMPLE
            !this.cache ? this.url += `${this.querySymbol()}_=${Math.random()}` : null;
        }

        querySymbol() {
            //=>THIS:EXAMPLE
            return this.url.indexOf('?') > -1 ? '&' : '?';
        }
    }

    //=>INIT PARAMETERS
    window.ajax = function ({
        url = null,
        method = 'GET',
        type = null,
        data = null,
        dataType = 'JSON',
        cache = true,
        async = true,
        success = null
    }={}) {
        let _this = new ajaxClass();
        ['url', 'method', 'data', 'dataType', 'cache', 'async', 'success'].forEach((item)=> {
            if (item === 'method') {
                _this.method = type === null ? method : type;
                return;
            }
            if (item === 'success') {
                _this.success = typeof success === 'function' ? success : new Function();
                return;
            }
            _this[item] = eval(item);
        });
        _this.isGET = /^(GET|DELETE|HEAD)$/i.test(_this.method);
        _this.init();
        return _this;
    };
}();
```

fetch和ajax 的主要区别