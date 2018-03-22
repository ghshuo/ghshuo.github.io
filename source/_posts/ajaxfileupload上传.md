---
title: ajaxfileupload上传
date: 2018-03-20 12:19:56
tags: [前端, JavaScript]
categories: 
        - 前端
        - JavaScript
---


### 前言
  在工作中使用了Jquery的ajaxFileUpload的图片上传插件，还有工作中遇到的问题，接下来问大家介绍下这个使用方法

### 引入文件

```js
<script type="text/javascript" src="../js/jquery-1.11.3.js"></script>
<script type="text/javascript" src="../js/ajaxfileupload.js"></script>
```
### html

我在这里是使用的bootstrap的模态框
```html
<div class="modal fade" id="upFile" tabindex="-1" role="dialog" aria-labelledby="myModalLabel">
    <div class="modal-dialog" style="max-width: 600px;">
        <div class="modal-content">
            <div class="modal-header">
                <h4 class="modal-title" style="color:#31708F">流量产品图标上传</h4>
            </div>
            <div class="modal-body" style="padding:20px 100px">
                <div>
                    请选择文件:
                    <input class="fileinput" type="file" id="Filedata" name="Filedata" style="display: inline-block;">
                </div>
            </div>
            <div class="modal-footer">
                <button name="fileuploadbtn" id="fileuploadbutton" class="btn btn-success btn-sm" onclick="return fileForm_Validator(this)">上传</button>
                <button data-dismiss="modal" class="btn btn-default" type="button">关闭</button>
            </div>
        </div>
    </div>
</div>

```
### js方法

```js
function fileForm_Validator(o){
    var filedata = $("#Filedata").val();
    // 获得截取文件路径后的文件名称
    filedata = filedata.substr(filedata.lastIndexOf("\\")+1);
    var str = filedata;
    // 正则判断是否是图片的格式
    var sear = new RegExp('.+(.JPEG|.jpeg|.JPG|.jpg|.GIF|.gif|.BMP|.bmp|.PNG|.png)$'); 
    if($('#Filedata').val() == ''||$('#Filedata').val() == 'undefined'||$('#Filedata').val() == undefined){
        alert('无可上传文件');
    }else if(sear.test(str)) {
        //把文件名赋值给要提交的内容
        $("#accnbrs").val(filedata);
        $.ajaxFileUpload({
            url:'/cyt/flowProduct/uploadIcon',   //后台方法的路径
            type: "POST",
            secureuri: false, //是否需要安全协议，一般设置为false
            fileElementId: 'Filedata', //文件上传域的ID
            dataType: 'json',
            data:{'filedata':filedata},
            success: function (data){
                if(data.result==1){
                    alert('上传成功');
                }else{
                    alert('上传失败');
                }
            }
        })
    }else{
        alert('请检查上传文件类型是否为图片格式');
        return false;
    }
}

```
### 遇到的问题

上传成功之后返回的数据部署json格式 而是字符串下面需要转换一下。谷歌返回的数据不一样需要特殊处理一下 
```js

success: function (data){
        var data =data;
        // 判断是否是谷哥浏览器  对data进行截取
        if(navigator.userAgent.indexOf("Chrome") > -1){
            data= data.match(/\{[^\}]+\}/)[0]; // 直截取{}中的内容
        }
        dataJson = JSON.parse(data);
        if(dataJson.result==1){
            $('#upFile').modal('hide');
            $("#iconUrl").val(dataJson.resultMsg);
            alert('上传成功');
        }else{
            alert(dataJson.resultMsg);
        }
    }
```            