
## CORS (Cross-Origin Resource Sharing)
  理解:
    * 通过在服务器端设置响应头，把发起跨域的原始域名添加到Access-Control-Allow-Origin 即可
    * 主流谷歌和火狐均支持，ie版本要高于ie10才行
    * 使用CORS可以帮助我们快速实现跨域访问，只需在服务端进行授权即可，无需在前端添加额外设置，比传统的JSONP跨域更安全和便捷。
    * 请求头信息由浏览器检测到跨域自动添加，无需过多干预，重点放在Response headers，它可以帮助我们在服务器进行跨域授权,例如允许哪些原始域可放行，是否需要携带Cookie信息等。

## 基本流程（简单请求）
### Request Headers (请求头)
  origin: 表示跨域请求的原始域 (ps: 请求来自哪个源（协议 + 域名 + 端口）)
  Access-Control-Request-Method： 表示跨域`请求的方式`。（如GET/POST）
  Access-Control-Request-Headers： 表示跨域请求的`请求头信息`。
### Response headers (响应头)
  Access-Control-Allow-Origin: 表示允许`哪些原始域`进行跨域访问。（字符数组）
  Access-Control-Allow-Credentials: 表示是否允许客户端`获取用户凭据`。（布尔类型）
    使用场景：
      例如现在从浏览器发起跨域请求，并且要附带Cookie信息给服务器。则必须具备两个条件
      1. 浏览器端： 发送AJAX请求前需设置通信对象XHR的withCredentials 属性为true
        ```javascript
          var xhr = new XMLHttpRequest();
          xhr.withCredentials = true;
        ```
      2. 服务器端： 设置Access-Control-Allow-Credentials为true
      (ps: 两个条件缺一不可，否则即使服务器同意发送Cookie，浏览器也无法获取。简单说:值为true,发送cookie)
      [正确姿势](https://images2017.cnblogs.com/blog/280044/201711/280044-20171114101549531-30113250.png)
      
  Access-Control-Allow-Methods: 表示跨域`请求的方式`的允许范围。（例如只授权GET/POST）
  Access-Control-Allow-Headers: 表示跨域`请求的头部`的允许范围
  Access-Control-Expose-Headers: 表示`暴露哪些头部信息`，并提供给客户端
    标准头部信息字段:
      Cache-Control,Content-Language,Content-Type,Expires,Last-Modified,Pragma
      ps: 
        XMLHttpRequest对象的getResponseHeader()方法只能拿到以上6个基本字段
        基于安全考虑，如果没有设置额外的暴露，跨域的通信对象XMLHttpRequest只能获取标准的头部信息
  Access-Control-Max-Age: 表示预检请求 [Preflight Request] 的`最大缓存时间`。

### 简单请求demo
```javascript
  // 请求头
  GET /cors HTTP/1.1
  Origin: http://api.bob.com
  Host: api.alice.com
  Accept-Language: en-US
  Connection: keep-alive
  User-Agent: Mozilla/5.0...
  // 响应
  Access-Control-Allow-Origin: http://api.bob.com
  Access-Control-Allow-Credentials: true
  Access-Control-Expose-Headers: FooBar // getResponseHeader('FooBar')可以返回FooBar字段的值。
  Content-Type: text/html; charset=utf-8
```

----------------------------

## 非简单请求

非简单请求: 比如请求方法是PUT或DELETE，或者Content-Type字段的类型是application/json。

  预检请求:
    * 正式通信之前，增加一次HTTP查询请求，称为"预检"请求(preflight), 要求服务器确认可以这样请求。
    * 预检"请求用的请求方法是OPTIONS，表示这个请求是用来询问的
  
    * 头信息里面，关键字段是Origin，表示请求来自哪个源, "预检"请求的头信息包括两个特殊字段。

  两个特殊字段: 
  Access-Control-Request-Method: 用来列出浏览器的CORS请求会用到哪些`HTTP方法`
  Access-Control-Request-Headers: 该字段是一个逗号分隔的字符串，指定浏览器CORS请求会`额外发送的头信息`字段

  预检请求的回应:
    1. 服务器收到请求,检查了Origin、Access-Control-Request-Method和Access-Control-Request-Headers字段以后，确认允许跨源请求，就可以做出回应
      Access-Control-Allow-Methods: 
        值是逗号分隔的一个字符串，表明服务器支持的所有跨域请求的方法
        返回的也是所有支持的方法，避免多次"预检"请求。
      Access-Control-Allow-Headers: 
        如果浏览器请求包括Access-Control-Request-Headers字段,则Access-Control-Allow-Headers字段是必需的
        也是一个逗号分隔的字符串，表明服务器支持的所有头信息字段，不限于浏览器在"预检"中请求的字段。
      Access-Control-Allow-Credentials: 该字段与简单请求时的含义相同。
      Access-Control-Max-Age: 
        指定本次预检请求的有效期，单位为秒
        (有效期是20天（1728000秒），即允许缓存该条回应1728000秒（即20天），在此期间，不用发出另一条预检请求。)

    2. 服务器否否定了"预检"请求，会返回一个正常的HTTP回应，但是没有任何CORS相关的头信息字段 -> 这时，浏览器就会认定，服务器不同意预检请求，因此触发一个错误，被XMLHttpRequest对象的onerror回调函数捕获。控制台会打印出如下的报错信息。
  ```javascript
    XMLHttpRequest cannot load http://api.alice.com.
    Origin http://api.bob.com is not allowed by Access-Control-Allow-Origin.
  ```
### 非简单请求demo

```javascript

// 非简单请求
  var url = 'http://api.alice.com/cors';
  var xhr = new XMLHttpRequest();
  xhr.open('PUT', url, true); 
  xhr.setRequestHeader('X-Custom-Header', 'value');
  xhr.send();

// 预请求http头部信息
  OPTIONS /cors HTTP/1.1
  Origin: http://api.bob.com
  Access-Control-Request-Method: PUT
  Access-Control-Request-Headers: X-Custom-Header
  Host: api.alice.com
  Accept-Language: en-US
  Connection: keep-alive
  User-Agent: Mozilla/5.0...

// 预请求-服务器回应
  HTTP/1.1 200 OK
  Date: Mon, 01 Dec 2008 01:15:39 GMT
  Server: Apache/2.0.61 (Unix)
  Access-Control-Allow-Origin: http://api.bob.com
  Access-Control-Allow-Methods: GET, POST, PUT
  Access-Control-Allow-Headers: X-Custom-Header
  Content-Type: text/html; charset=utf-8
  Content-Encoding: gzip
  Content-Length: 0
  Keep-Alive: timeout=2, max=100
  Connection: Keep-Alive
  Content-Type: text/plain

// 服务器回应的其他CORS相关字段
  Access-Control-Allow-Methods: GET, POST, PUT
  Access-Control-Allow-Headers: X-Custom-Header
  Access-Control-Allow-Credentials: true
  Access-Control-Max-Age: 1728000

```


### 浏览器正常请求回应

  理解:
    * 服务器通过了"预检"请求，以后每次浏览器正常的CORS请求，会有一个Origin头信息字段
  

```javascript
// "预检"请求之后，浏览器的正常CORS请求:
  PUT /cors HTTP/1.1
  Origin: http://api.bob.com  //ps: 浏览器自动添加Origin字段
  Host: api.alice.com
  X-Custom-Header: value
  Accept-Language: en-US
  Connection: keep-alive
  User-Agent: Mozilla/5.0...

// 服务器正常的回应

  Access-Control-Allow-Origin: http://api.bob.com
  Content-Type: text/html; charset=utf-8


```


## 参考资料

[阮一峰跨域资源共享 CORS 详解](http://www.ruanyifeng.com/blog/2016/04/cors.html)
[沧海一滴](https://www.cnblogs.com/softidea/p/5496719.html)
