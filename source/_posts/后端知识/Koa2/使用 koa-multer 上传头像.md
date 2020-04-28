---
title: 使用 koa-multer 上传头像
date: 2020-04-28 10:34:37
categories:
- Koa2
comments: false
---

上传头像的原理是：
1. 使用 koa-multer 上传图片到服务器
2. 上传成功，返回图片名
3. 前端根据返回的图片名从服务器获取图片，达到实时上传的效果

<!-- more -->

### 安装中间件

```bash
npm install koa-multer --save
```



### 后端

```typescript
const multer = require('koa-multer');
const path = require('path')

// 文件上传 配置
const storage = multer.diskStorage({
    // 文件保存路径
    destination: function (req, file, cb) {
        cb(null, path.join(__dirname, '../../public/userUploads')) // 服务器的存放地址
    },
    // 修改文件名称
    // 上传的文件默认没有后缀名，需要手动加上后缀名；为了命名不重复，使用时间戳转为16进制作为文件命名
    filename: function (req, file, cb) {
        let type = file.originalname.split('.')[1] // 以点分割成数组，数组的最后一项就是后缀名
        cb(null, `${file.fieldname}-${Date.now().toString(16)}.${type}`)
    }
})
// //文件上传限制，指定限制可以帮助保护站点免受拒绝服务(DoS)攻击
// const limits = {
//     fields: 10, // 非文件字段的数量
//     fileSize: 500 * 1024, // 文件大小 单位 b
//    files: 1 // 文件数量
//}
// 加载配置
//const upload = multer({ storage, limits })
const upload = multer({ storage })

// 路由
// 由于这里只上传一个文件，所以使用 single 方法，single 方法接受一个字符串，这个字符串为上传文件的字段名，另外上传多文件可以使用 array、fileds
router.post('/api/userImage', checkToken, upload.single('avatar'), async (ctx, next) => {
    // 在路由中，可通过 ctx.file 获取上传完毕的文件信息，多文件上传可通过 ctx.files 获取
    let user = {
        id: ctx.req.body.id,
        filename: ctx.req.file.filename
    }

    await userModel.uploadUserAvator(user).then((res) => {
        global.log.trace("[userImage] 头像上传成功!" + ctx.req.file.filename);
        ctx.body = {
            code: 0,
            msg: '上传成功!',
            filename: ctx.req.file.filename // 返回文件名
        }
    }).catch((err) => {
        global.log.error("[userImage] 头像上传失败!" + err);
        ctx.body = {
            code: -1,
            msg: err,
            data: []
        }
    })
})
```




### 前端

```js
let self = this;

let params = new FormData(); // 创建一个form对象,必须是form对象否则后端接受不到数据
params.append("avatar", file.file); // append 向form表单添加数据
// 添加请求头  通过form添加的图片和文件的格式必须是multipart/form-data
params.append("id", self.user["id"]);

let config = {
    headers: { "Content-Type": "multipart/form-data" } // 重要！
};

const res = await userImageAPI(params, config);
try {
    console.log("上传头像信息" + JSON.stringify(res.data));
    if (res.data.code === 0) {
        Toast.success(res.data.msg);
        window.location.reload();
    } else if (res.data.code === 104) {
        Toast.fail(res.data.msg);
    }
} catch (error) {
    Toast.fail("上传失败");
    console.log("上传失败" + error);
}
```



### 参考文章

[node koa2图片上传的实现](https://blog.csdn.net/ziwoods/article/details/72822730)

[Koa - 使用koa-multer上传文件（上传限制、错误处理）](https://www.cnblogs.com/chanwahfung/p/11443089.html)