const fs = require("fs");
const path = "./";

var qiniu = require("qiniu");


//需要填写你的 Access Key 和 Secret Key
qiniu.conf.ACCESS_KEY = 'xxx';
qiniu.conf.SECRET_KEY = 'xxx';

//要上传的空间
bucket = 'hexo';


//构建上传策略函数
function uptoken(bucket, key) {
  var putPolicy = new qiniu.rs.PutPolicy(bucket+":"+key);
  return putPolicy.token();
}



//构造上传函数
function uploadFile(uptoken, key, localFile) {
    var extra = new qiniu.io.PutExtra();
    qiniu.io.putFile(uptoken, key, localFile, extra, function(err, ret) {
      if(!err) {
        // 上传成功， 处理返回值
        console.log('upload success : ',ret.hash, ret.key);
      } else {
        // 上传失败， 处理返回代码
        console.log(err);
      }
  });
}

/**
 * 读取文件后缀名称，并转化成小写
 * @param file_name
 * @returns
 */
function getFilenameSuffix(file_name) {
  if(file_name=='.DS_Store'){
    return '.DS_Store';
  }
    if (file_name == null || file_name.length == 0)
        return null;
    var result = /\.[^\.]+/.exec(file_name);
    return result == null ? null : (result + "").toLowerCase();
}


fs.readdir(path, function (err, files) {
    if (err) {
        return;
    }
    var arr = [];
    (function iterator(index) {
        if (index == files.length) {
            return;
        }

        fs.stat(path + "/" + files[index], function (err, stats) {
            if (err) {
                return;
            }
            if (stats.isFile()) {
              var suffix = getFilenameSuffix(files[index]);
              if(!(suffix=='.js'|| suffix == '.DS_Store')){
                //要上传文件的本地路径
                filePath = './'+files[index];
                console.log(files[index]);
                //上传到七牛后保存的文件名
                key = files[index];
                //生成上传 Token
                token = uptoken(bucket, key);

                uploadFile(token, key, filePath);
                arr.push(files[index]);
            }
            iterator(index + 1);
              }
        })
    }(0));
});
