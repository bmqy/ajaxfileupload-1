# ajaxfileupload

基于 ajaxfileupload.js 文件的增强版 ajaxfileupload.js 的再修改

## 问题及修改
执行成功后，始终指向 error 方法处理，无法执行 sucess 方法

    * 原因：`dataType` 为 `json` 时，若返回 `data` 为 `json` 格式的字符串时，会出现问题

    * 解决：将 `uploadHttpData` 方法中 if(type == "json") 里的data返回parseJSON；

        ```javascript
        uploadHttpData: function (r, type) {
            var data = !type;
            ...
            if (type == "json") {
                data = r.responseText;// 去掉前面的 var
                //var rx = new RegExp("<pre.*?>(.*?)</pre>", "i");
                //var am = rx.exec(data);
                //data = (am) ? am[1] : "";// 去掉前面的 var
                //eval("data = " + data);// 返回 json 对象，注释掉可返回 json 格式的字符串
                data = jQuery.parseJSON(data); //直接将data解析成json
            }
            ...
            return data;
        }
        ```

## 使用及建议

1. 前端交互部分

    ```javascript
    function ajaxFileUpload() {
        // 开始上传
        $.ajaxFileUpload({
            url: "http://www.example.com",
            secureuri: false,// 是否启用安全提交，默认为 false
            type: "POST",
            fileElementId: "fileId",// input[type=file] 的 id
            dataType: "json",// 返回值类型
            data: $("form").serializeJSON(),// 添加参数，无参数时注释掉，请预先调用“jquery.serializejson.min.js”
            success: function (data, status) {
                // console.log(data);
            },
            error: function (data, status, e) {
                // console.log(data);
            }
        });
    }
    ```

2. 后端接收与返回

    <font color=red>一定注意后端json数据的返回，必须返回json字符串</font>

## 参考
[基于 ajaxfileupload.js 文件的增强版 ajaxfileupload.js](https://github.com/gaojr/ajaxfileupload)
