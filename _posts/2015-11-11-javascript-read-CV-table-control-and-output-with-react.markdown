
代码如下
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title></title>
    <link rel="stylesheet" href="../../fonts/iconfont.css"/>
    <link rel="stylesheet" href="../../css/2_monitor/add_group.css"/>

    <script src="../../js/lib/underscore.js"></script>
</head>
<style>
    table{
        width: 100%;
        border-collapse: collapse;
        text-align: left;
    }

    td textarea,th{

        padding: 5px 10px;
    }
    td,th{
        border: 1px solid #ccc;
    }
    td{
        position: relative;
        padding: 0;

        max-width: 35vw;
    }
    td>div{
        display: block;
        word-break: break-all;
        visibility: hidden;
        padding: 5px 10px;
        font-size: 13px;
        line-height: 20px;
        font-family: 'Microsoft Yahei', '微软雅黑', "Helvetica Neue", Helvetica, Arial, sans-serif;
    }
    td textarea{
        display: block;
        -ms-word-break: break-all;
        word-break: break-all;
        font-size: 13px;
        resize: none;
        left: 0;
        top: 0;
        line-height: 20px;
        position: absolute;
        box-sizing: border-box;
        width: 100%;
        height: 100%;
        border:none;
        outline:none;
        font-family: 'Microsoft Yahei', '微软雅黑', "Helvetica Neue", Helvetica, Arial, sans-serif;
        background-color: transparent;
    }
    td textarea:focus{
        outline: 2px solid #000;
    }
    td textarea.error{
        outline: 2px solid red;
    }
    .download-wrapper{
        text-align: center;
        padding: 8px 0;
    }
    .download-wrapper a{
        display: inline-block;
        padding: 8px 15px;
        border-radius: 3px;
        color: #666;
        font-size: 13px;
        text-decoration: none;
        border: 1px solid #aaa;
        cursor: pointer;
    }
    .download-wrapper a:active{
        color: #000;
        background: #eee;
        box-shadow: 0 0 7px #ccc;
    }

</style>
<body>

<div class="g-wrap" >
    <div class="g-page-title">
        <span class="text-blue iconfont" > &#xe65a;</span>
        <span class="text">推广计划</span>
        <span class="text">&gt;</span>
        <span class="text">监测单元</span>
        <span class="text">&gt;</span>
        <span class="text">监测单元</span>
    </div>
    <div class="g-wrapper page" id="container">
    </div>
</div>


<form>
    <input type="file" id="ff"/>

</form>
<textarea name="" id="test" cols="80" rows="30" ></textarea>

</body>
<script src="../../js/lib/react.js"></script>
<script type="text/html">
    var ff = document.getElementById('ff');

    ff.addEventListener('change', function () {
        alert('!');
        var file = ff.files[0];
        console.log(file);
        loadUpdatedFile('UTF-8',file,function(fileStream){
            var arr = fileStream.split('\n');
            console.log(arr);
//            document.getElementById('text')
        });
    });


    function loadUpdatedFile(code,fileObj, cb) {
        var reader = new FileReader();
        reader.onload = function (e) {
            var file = reader.result;
            if(file.indexOf('�')!= -1){
                loadUpdatedFile('GBK',fileObj)
            }else{
                cb && cb.call(null, file);
                console.log(reader.result);
            }
        };
        reader.readAsText(fileObj,code);
    }

</script>
<script src="../../dist/components/monitor__add_group.js"></script>
<script>
</script>
</html>
```
