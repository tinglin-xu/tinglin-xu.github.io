---
layout:     post
title:      DropzoneJS 图片上传
subtitle:   DropzoneJS 图片上传
date:       2019-08-05
author:     BY
header-img: img/post-bg-2015.jpg
catalog: true
tags:
    - DropzoneJS
---
## DropzoneJS 图片上传

### 官网url

https://www.dropzonejs.com/#dropzone-methods

### html

```swift

<form action="/file-upload"
      class="dropzone"
      id="my-awesome-dropzone">
</form>

<div class="layui-form-item">
        <label class="layui-form-label">尾部背景图上传</label>
        <div class="layui-inline">
            <div id="uploadFooterImg" class="dropzone"></div>
        </div>
        <input type="hidden" name="footer_img" id="footer_img" >
        <button type="button" class="layui-btn layui-btn-disabled" id="upload_footer_img_btn">开始上传</button>
</div>

```

### js
```swift

<script>
var myDropzone = new Dropzone("div#uploadFooterImg", { url: "/file/post"});
</script>

$("#dropz").dropzone({
        url: "index.php", //必须填写
        method:"post",  //也可用put
        paramName:"Filedata", //默认为file
        maxFiles:10,//一次性上传的文件数量上限
        maxFilesize: 20, //MB
        acceptedFiles: ".jpg,.gif,.png", //上传的类型
        previewsContainer:"#adds", //显示的容器
        parallelUploads: 3,
        dictMaxFilesExceeded: "您最多只能上传10个文件！",
        dictResponseError: '文件上传失败!',
        dictInvalidFileType: "你不能上传该类型文件,文件类型只能是*.jpg,*.gif,*.png。",
        dictFallbackMessage:"浏览器不受支持",
        dictFileTooBig:"文件过大上传文件最大支持.",
        previewTemplate: document.querySelector('#preview-template').innerHTML,//设置显示模板
        init:function(){
            this.on("addedfile", function(file) { 
            //上传文件时触发的事件
            });
            this.on("queuecomplete",function(file) {
                //上传完成后触发的方法
            });
            this.on("removedfile",function(file){
                //删除文件时触发的方法
                 });
            });
        }
 
    });


```

### 上传图片显示
```swift

//uploadObj 对象
//name  图片名
//url   图片的url
//btnId 上传按钮id
function uploadImgShow(uploadObj, name, url, btnId)
{
    if(!name){
        return false;
    }
    var mockFile = { name: name, accepted:true };
    uploadObj.emit("addedfile", mockFile);
    uploadObj.emit("thumbnail", mockFile, url);
    uploadObj.emit("complete", mockFile);
    uploadObj.options.maxFiles = uploadObj.options.maxFiles - 1;
    $('#'+btnId).addClass('layui-btn-disabled');
}

```

### 其他
file.xhr.response.name  上传成功后的文件名

file.accepted   true则已上传 false 未上传

如果您已autoProcessQueue禁用，则需要给.processQueue() 自己打电话。如果要显示文件并让用户单击“接受”按钮以实际上载文件，这将非常有用。
