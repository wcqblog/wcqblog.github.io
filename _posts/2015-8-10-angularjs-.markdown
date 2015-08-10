---
layout: post
title:  "angularjs的指令directive 2 多文件上传"
date:   2015-08-10 23:35:00
categories: Angular directive FileReader 坑...
tags: Webfrontend 
image: /assets/article_images/2015-01-25-webgl-shader/plexus.jpg
---






###使用angularjs构建多文件上传组件

今天下午处理了一下angular多文件上传的问题~
总结了几个点要注意的

1. ` ng-change`只能监听`.value`，对于`input[type='file']`无能为力
所以需要在`link`函数中找到`input`元素，并且绑定onChange事件
2. 动态显示上传的文件信息，但是因为无法监听到`input[type='file']`的值，所以双向数据绑定的`ng-model`指令也不管用.... 所以每当你的处理完成之后，需要使用`$apply`来手动更新数据
3. FileReader 来拉去本地数据得到文件信息
4. 设置ajax的时候不要设置`contentType` ,也不要让jQuery处理发送的数据~

#####模板

```html
	<script type="text/ng-template" id="templates/admin/uploader.html">
    <div class="my-uploader">
        <div class="upload-wrapper">
            <input type="file" id="poster" ng-model="uploadFile" multiple />
            <button type="button" class="btn btn-info upload-btn">上传图片</button>
        </div>
        <div class="upload-content" ng-click="getFileInfo()">
            <span ng-if="filesInfo.length == 0">
                还没有文件在上传
            </span>
        </div>


    </div>
    <div class="row upload-result">
        <div class="row img-preview"  ng-repeat="f in filesInfo">
            <div class="row" >
                <div class="info" >
                    <img class="thumbnail" src="{{f.dataURL}}" alt="加载中" ng-if="f.type.indexOf('image') > -1"/>
                    <span class="thumnail"  ng-if="f.type.indexOf('image') == -1"> 该文件类型为{{f.type}},不能像图片一样预览哟 </span>
                </div>
                <div class="info">
                    <div>文件名:</div>
                    <span>{{f.name}}</span>
                </div>
                <div class="info">
                    <div>大小:</div>
                    <span>{{f.size}}</span>
                </div>
                <div class="info">
                    <div>类型:</div>
                    <span>{{f.type}}</span>
                </div>
            </div>
        </div>
    </div>

</script>
```


#####指令

```javascript
app.directive('uploader', function () {
    return {
        restrict: 'E',
        transclude: true,
        scope: {},
        templateUrl: 'templates/admin/uploader.html',
        link: function ($scope, $element) {
            console.log($element.find('input[type="file"]').eq(0));
            $element.find('input[type="file"]').eq(0).on('change', function () {
                $scope.getFileInfo();
            });

        },
        controller: function ($scope, $element, $http, $location) {
            $scope.info = "尚未上传文件";
            $scope.uploadFile = null;
            $scope.filesInfo = [];
            var sendData = function () {
                var files = new FormData();
                for (var i in $scope.filesInfo) {
                    var item = $scope.filesInfo[i];
                    files.append(item.name, item.obj);
                    var uploadURL = '/users/pics/';
                }
                var fs = new FormData($element.find("form").get(0));
                $.ajax({
                    url: "/users/pics/",
                    type: "POST",
                    data: files,
                    processData: false,  //不处理数据
                    contentType: false //不设置contentType
                }).success(function(){
                    alert("success");
                });
            };
            $scope.getFileInfo = function () {
                function loadUpdatedFile(index) {
                    var reader = new FileReader();
                    reader.onload = function (e) {
                        $scope.$apply(function () {
                            $scope.filesInfo[index].dataURL = reader.result;
                            console.log(reader.result);
                        });

                    };
                    reader.readAsDataURL($scope.uploadFile[index]);
                };

                $scope.uploadFile = $element.find('input').get(0).files;

                for (var i = 0; i < $scope.uploadFile.length; i++) {
                    var f_obj = $scope.uploadFile[i];
                    var f_info_obj = {
                        "name": f_obj.name,
                        "size": f_obj.size / 1000 + 'Kb',
                        "type": f_obj.type,
                        "formData_name": 'file_' + i,
                        obj: f_obj
                    };
                    $scope.filesInfo.push(f_info_obj);
                    loadUpdatedFile(i);
                }
                console.log($scope.filesInfo);
                sendData();
            };
        }
    }
});
```

样式表
```scss
	$upload-btn-width: 90px;
$upload-btn-height: 37px;
$upload-btn-bg-color: #5bc0de;
.my-uploader{
  overflow: hidden;
  width: 100%;
  height: auto;
  .upload-wrapper{
    display: block;
    float: left;
    width: $upload-btn-width;
    height: $upload-btn-height;
    cursor: pointer;
    .upload-btn{
      display: block;
      position: absolute;
      margin-left: 15px;
      top:0;
      left: 0;
    }
    input[type='file'] {
      display: block;
      position: absolute;
      padding: 20px;
      opacity: 0;
      top:0;
      left: 0;
      z-index: 200;
      width: inherit;
      height: inherit;
      box-sizing: border-box;
      cursor: pointer;

    }
    &:hover {
      .upload-btn{
        background-color: darken($upload-btn-bg-color, 20);
      }
    }

  }
  .upload-content{
    display: block;
    float: left;
    line-height: $upload-btn-height;
    margin-left: 15px;

  }


}
.upload-result{
  margin-top:50px;
  .img-preview{
    margin: 10px 0;
    .info{
      float: left;
      &:first-child{
        width: 25%;
      }
      &:nth-child(2),
      &:nth-child(3){
        width: 28%;
      }
      &:last-child {
        width: 15%;
      }
      span{
        word-break: break-all;
      }
    }
    .thumbnail{
      width: 100px;
    }
  }
}



```
