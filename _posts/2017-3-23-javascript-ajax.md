---
layout: post
title: 【Javascript】Ajax
author: juily
---
## 【Javascript】Ajax
-----

大家会习惯用jQuery提供的Ajax方法，但是我们同样可以用原生js去实现Ajax方法。

### 一、jQuery提供的Ajax

{% highlight bash %}
    $.ajax({
        url: '',
        type: '',
        dataType: '',
        data: {

        },
        success: function(){

        },
        error: function(){

        }
    })
{% endhighlight %}

jQuery语法：

jQuery.ajax([settings])

settings 可选，用于配置Ajax请求的键值对集合。

settings 参数参考：[jQuery ajax - ajax()方法](http://www.w3school.com.cn/jquery/ajax_ajax.asp)

### 二、原生js实现Ajax

{% highlight bash %}
    var Ajax = {
        get: function(url, fn){
            var obj = new XMLHttpRequest(); // XMLHttpRequest对象用于在后台与服务器交换数据
            obj.open('GET', url, true);
            obj.onreadystatechange = function(){
                // readyState == 4 说明请求已完成
                if (obj.readyState == 4 && (obj.status == 200 || obj.status == 304)){
                    fn.call(this, obj.responseText); // 从服务器获得数据
                }
            }
            obj.send(null);
        },
        post: function(url, data, fn){
            var obj = new XMLHttpRequest();
            obj.setRequestHeader('Content-type', 'application/x-www-form-urlencoded'); // 发送信息至服务器时内容编码
            obj.onreadystatechange = function(){
                if(obj.readyState == 4 && (obj.status == 200 || obj.status == 304)){ // 304 未修改
                    fn.call(this, obj.responseText);
                }
            }
            obj.send(data);
        }
    }
{% endhighlight %}

以上：

1、open(method, url, async) 方法需要三个参数

（1） method 定义发送请求所使用的方法（'GET'，'POST'）。与POST相比，GET更简单更快，在大部分情况下都可以使用，但是，在一下情况，使用POST请求是更好的选择。

* 无法使用缓存文件（更新服务器上的文件或数据库）

* 像服务器发送大量数据（POST没有数据限制）

* 发送包含未知字符的用户输入时，POST比GET更稳定也更可靠

（2）url 规定服务器端脚本的URL（该文件可以是任何类型的文件，比如.txt、.xml，或者是服务器脚本文件，比如.asp、.php(在传回响应前，能够在服务器上执行任务)）。

（3）async 规定是否对请求进行异步的处理（true异步，false同步）。

2、sead() 方法可将请求送往服务器

3、onreadystatechange 属性存有处理服务器响应的函数

4、readyState 属性存有服务器响应的状态信息。每当readyState改变时，onreadystatechange函数就会被执行。

### 三、上面是一个简单的Ajax实现，下面还有一个较为完整的原生js实现的Ajax，类似jQuery的Ajax：

{% highlight bash %}

    function ajax(){

        var ajaxData = {
            type: arguments[0].type || 'GET',
            url: arguments[0].url || '',
            async: arguments[0].async || 'true',
            data: arguments[0].data || null,
            dataType: arguments[0].dataType || 'text',
            contentType: arguments[0].contentType || 'application/x-www-form-urlencoded',
            beforeSend: arguments[0].beforeSend || function(){},
            success: arguments[0].success || function(){}
            error: arguments[0].error || function(){}
        }

        ajaxData.beforeSend();

        var xhr = createxmlHttpRequest();

        xhr.responseType = ajaxData.dataType;

        xhr.open(ajaxData.type, ajaxData.url, ajaxData.async);

        xhr.setRequestHeader('Content-type', ajaxData.contentType);

        xhr.send(convertData(ajaxData.data));

        xhr.onreadystatechange = function(){
            if(xhr.readyState == 4){
                if(xhr.status == 200){
                    ajaxData.success(xhr.response);
                }else{
                    ajaxData.error();
                }
            }
        }

    }

    function createxmlHttpRequest(){
        if(window.ActiveXObject){
            return new ActiveXObject('Microsoft.XMLHTTP');
        }else{
            return new XMLHttpRequest();
        }
    }

    function convertData(){
        if(typeof data === 'object'){
            var convertResult = '';
            for(var c in data){
                convertResult += c + '=' + data[c] + '&';
            }
            convertResult = convertResult.substring(0, convertResult.length - 1);
            return convertResult
        }else{
            return data;
        }
    }

{% endhighlight %}

使用格式类似jQuery的Ajax

{% highlight bash %}
    ajax({
        type: 'POST',
        url: 'ajax.php',
        dataType: 'json',
        data: {
            'val1': 'abc',
            'val2': 123,
            'val3': '456'
        },
        success: function(msg){
            console.log(msg);
        },
        error: function(){
            console.log('error');
        }
    })
{% endhighlight %}
