* hash值
	* 将任意长度的二进制字符串通过一定的算法映射成一个固定长度的较小二进制字符串，这个字符串就是对应的hash值，主要特点就是唯一的，不可逆的。
* 前端路由的hash值(#)----->angular
	* hash通常应用在spa单页面应用中。因为通过不同的hash值映射的url来是的浏览器添加一条不同的url历史记录。
	* 通过浏览器的pushstate、replaceState来操作，请求不同的浏览器记录达到请求不同的页面的效果
	* H5中提供的两个操作hash值得API来操作hash值
	* window.location.hash读取#值
	* window.onhashchange = func 监听hash改变