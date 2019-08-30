# vue + element Upload + axios文件上传

项目开发采用前后端分离，在开发中发现需要开发附件上传功能，遇到的一些问题，做一些总结和记录
整个文件上传逻辑是：
1 前端文件上传前，先使用插件计算文件的唯一MD5值
2 将MD5值上传到服务器，后端进行校验是否存储有相同的MD5值文件
3 如果不存在，上传文件到后端服务器进行存储；如果存在，则前端不在重复提交文件

使用MD5 优势

> 1 前端生成文件唯一MD5,减轻后端服务器压力
> 2 文件的唯一性标志，减小`ftp`服务器存放重复文件概率（重复率为零？待测试）

使用MD5 缺点

> 1 上传一个文件需要2个http请求，如果文件已经存在，则为一次请求（1：上传MD5验证文件是否已经存在 2：不存在则再次上传文件），如果网络环境差，影响操作体验

优化方案： 文件和文件MD5同时传输到后端进行处理

遇到问题

1 前端使用`element Upload` 组件如何自定义上传文件
2 前端如何 `hash` 生成文件 `MD5`
3 前端使用 `axios post` 请求上传文件到服务器
4 后端代码(java)如何接收前端上传的文件

前端架构： `vue` ,`element`, `axios`

后端架构：`java`, `springBoot`

解决步骤1：

配置 Upload 组件 `http-request` 属性 ，自定义http上传

```
:http-request = "customUpload"
```

核心代码代码：

<el-upload
  class="upload-demo"
  ref="upload"
  :multiple="false"
  action="http://127.0.0.1:8080/docmanager/upload"
  :http-request = "customUpload"
  :on-remove="handleRemove"
  :on-change="handleChange"
  :file-list="fileList"
  :auto-upload="true">
  <el-button slot="trigger" size="small" type="primary">选取文件</el-button>
  <el-button style="margin-left: 10px;" size="small" type="success" @click="submitUpload">上传到服务器</el-button>

  <div slot="tip" class="el-upload__tip">只能上传jpg/png文件，且不超过500kb</div>
</el-upload>
customUpload(file) {
  // this.generatorFileMd5(file.file)
  // 自定义上传
  fileUpload(file).then(response => {
    console.log(response)
  })
}

解决步骤2：

使用`hash` 生成 `MD5`

安装依赖包：`spark-md5` [spark-md5](https://github.com/satazor/js-spark-md5)

```
npm install --save spark-md5
```

核心代码：

```
generatorFileMd5(file) {
  /**
    * 生成MD5
    */
  let blobSlice = File.prototype.slice || File.prototype.mozSlice || File.prototype.webkitSlice
    // file = this.files[0],
  let chunkSize = 2097152                           // Read in chunks of 2MB
  let chunks = Math.ceil(file.size / chunkSize)
  let currentChunk = 0
  let spark = new SparkMD5.ArrayBuffer()
  let fileReader = new FileReader()
  fileReader.onload = function (e) {
      // console.log('read chunk nr', currentChunk + 1, 'of', chunks)
      spark.append(e.target.result)                  // Append array buffer
      currentChunk++;

      if (currentChunk < chunks) {
          loadNext();
      } else {
          // console.info('computed hash', spark.end())  // spark.end(): 文件 MD5值生成完成
          console.log(spark.end())
      }
  };

  fileReader.onerror = function () {
      console.warn('fileReader.onerror is error')
  };

  function loadNext() {
      var start = currentChunk * chunkSize,
          end = ((start + chunkSize) >= file.size) ? file.size : start + chunkSize;

      fileReader.readAsArrayBuffer(blobSlice.call(file, start, end))
  }

  loadNext()
}
```

解决步骤3：

使用`axios`上传文件

```
/**
 * 文件上传
 */
export function fileUpload(fileobj) {
  let param = new FormData()
  param.append('files',fileobj.file)
  return request({
    method: 'post',
    url: '/docmanager/upload',
    headers: {'Content-Type':'multipart/form-data'},
    data: param
  })
}
```

解决步骤4：

后端接收前端文件

```
@PostMapping("/upload")
    @JsonBody
    public List<DocForm> upload(MultipartFile[] files) throws Exception {
        List<DocForm> rs = new ArrayList<DocForm>();
        for (MultipartFile file : files) {
            // ... 处理文件储存逻辑
        }
        return rs;
    }
```