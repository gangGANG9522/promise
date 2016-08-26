## Promise
promise是异步编程的一种方案，比传统的解决方案-回调函数和事件，更加合理。ES6实现了标准化。所谓的promise简单来说就是一个容器，里面保存着某个未来才会结束的事件的结果，从语法上讲，promise是一个对象，从它可以获取异步操作的消息。
### Promise的两个特点
> #####对象的状态不受外部的影响

  promise对象代表一个异步操作，有三种状态，Pending 进行中,Resolved 已完成,Rejected 已失败。只有异步的操作结果可以决定当前是哪一种状态，其他任何操作都无法改变状态。这也是promise的由来，“承诺”。
> #####一旦状态发生改变就不会再变化

  Promise的状态改变只能有两种，从pending到resolved，从pending到rejected。只有这两种情况的发生，状态就凝固了不会再变化了。就算改变已经发生，你再对promise添加回调函数也会立即得到这个结果。

  有了promise对象，就可以将异步的操作以同步的流程表达出来，避免嵌套过深，promise提供统一的接口，使得异步操作更加容易。

  promise也有一些缺陷，首先，无法取消promise，一旦建立就会执行，无法中途取消，其次如果不设置回调函数 ，promise内部将抛出错误，不会反映到外部。第三，当处于pending状态时，无法得知目前是哪个阶段。

> #####基本用法 

		var promise = new Promise(function(resolve,rejected){
		if(/操作成功/){
				resolve(value)
			}
		else{
				//操作失败
				reject(value)
			}
		})

Promise构造函数接受一个函数作为参数，该函数的两个参数分别是resolve和reject。它们是两个函数，由JavaScript引擎提供，不用自己部署。

resolve函数的作用是，将Promise对象的状态从“未完成”变为“成功”（即从Pending变为Resolved），在异步操作成功时调用，并将异步操作的结果，作为参数传递出去；reject函数的作用是，将Promise对象的状态从“未完成”变为“失败”（即从Pending变为Rejected），在异步操作失败时调用，并将异步操作报出的错误，作为参数传递出去。

Promise实例生成以后，可以用then方法分别指定Resolved状态和Reject状态的回调函数。

		promise.then(function(value){//success},function(error){//error})

then方法接受两个函数作为参数，第一个是promise的状态变为resolve时调用，第二个是promise的状态变为reject时调用。其中第二个函数书可选的。

		function time(ms){
				return new Promise(function(resolve,reject){
							setTimeout(resolve,ms)
		
							})
			}

>#####应用

	加载图图片

		function loadImageAsync(url) {
 		 return new Promise(function(resolve, reject) {
   		 var image = new Image();

    		image.onload = function() {
     		 resolve(image);
    		};

    		image.onerror = function() {
    		  reject(new Error('Could not load image at ' + url));
   		 };

   		 image.src = url;
  		});
		}

	
ajax应用

		var getJSON = function(url) {
  		var promise = new Promise(function(resolve, reject){
    	var client = new XMLHttpRequest();
    	client.open("GET", url);
    	client.onreadystatechange = handler;
    	client.responseType = "json";
    	client.setRequestHeader("Accept", "application/json");
    	client.send();

    	function handler() {
      	if (this.readyState !== 4) {
       	 return;
      	}
      	if (this.status === 200) {
      	  resolve(this.response);
     	 } else {
       	 reject(new Error(this.statusText));
     	 }
   	 };
  			});

  			return promise;
			};

			getJSON("/posts.json").then(function(json) {
  			console.log('Contents: ' + json);
			}, function(error) {
  			console.error('出错了', error);
			});
