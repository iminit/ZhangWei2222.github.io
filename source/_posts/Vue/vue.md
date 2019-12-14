---
title: vue搭环境
date: 2018-08-01 10:11:47
categories:
- Vue
tags:
comments: false
---

# 搭建一个简单的vue环境

## 1.安装vue-cli

在要项目的根目录中

    npm install vue-cli -g

## 2.实例化

在要项目的根目录中

    vue init webpack <项目名字>

会有很多选项，按需填写

因为要上传到git上，所以

## 3.下载包

进入项目文件夹

    npm install
    npm run dev

# 上传git

在文件夹中

    git init
    git status
    
    git remote add origin https://gitee.com/fox9916/demo.git
    //fatal: remote origin already exists.
    //解决方法git remote remove origin
    
    git pull --rebase origin master
    git add . 添加目录下所有发生改变的文件
    git commit -m '注释信息'
    git push -u origin master 本地仓库代码提交至远程仓库
    (git push -u origin master -f )


# 引入scss

    npm install sass-loader node-sass --save-dev

# 引入element

    npm i element-ui -S

* 如何局部覆盖element-ui的默认样式

在需要更改的组件里新增一个style标签【重点：不要加scoped】，然后直接获取class设置样式就可以咯，class自己去浏览器里右键审查元素可得到。

建议：在获取到的样式里加上能限制范围的父层选择器，不然就变成全局样式咯。


    <style>
      .ruzhu-mess .el-form-item__label{
        font-size:16px!important;
      }
      .zhuanye-mess .el-form-item__label,.zhuanye-mess .color-t{
        font-size:16px!important;
        color:#606266;
      }
    </style>
    <style lang="scss" scoped>
    
      @import '../../../assets/scss/pages/organizationUserCenter1.scss';
    
    </style>


# 引入pug

## 1.安装pug和jade依赖

    #安装支持pug依赖
    npm install pug pug-loader pug-filters -D
    #安装支持jade依赖
    npm install jade jade-loader -D

## 2.配置pug和jade

    打开webpack.base.conf，在module的rules节点下添加如下配置：
    {
          test: /\.jade$/,
          loader: "jade"
    },
    {
          test: /\.pug$/,
          loader: 'pug'
    },

# 安装、使用vuex

## 1.安装

npm install vuex --save

## 2.配置

 main.js :

    import vuex from 'vuex'
    Vue.use(vuex);
    var store = new vuex.Store({//store对象
        state:{
            show:false
        }
    })

在实例化 Vue对象时加入 store 对象 :

    new Vue({
      el: '#app',
      router,
      store,//使用store
      template: '<App/>',
      components: { App }
    })

完成到这一步 , 上述例子中的 $store.state.show 就可以使用了。

## 3.在vue中使用

    import { mapState } from "vuex";
    
    computed: {
        ...mapState(["activeTab"])
      },

# 导出excel表格数据

## 1.安装

    npm install -S file-saver
    npm install -S xlsx
    npm install -D script-loader

## 2.放文件

在项目中创建一个文件夹（比如vendor，一般是在src目录下创建）

把Blob.js和 Export2Excel.js这两个文件夹放到新建的文件夹内

## 3.在页面中使用

    <template>
    <el-button style='margin-bottom:20px;' type="primary" icon="document" @click="handleDownload" :loading="downloadLoading">导出excel</el-button>
        <el-table:data="tableData" style="width: 100%">
          <el-table-column prop="date" label="日期"width="180">
          </el-table-column>
          <el-table-column prop="name"  label="姓名"  width="180">
          </el-table-column>
          <el-table-column  prop="address" label="地址">
          </el-table-column>
        </el-table>
      </template>
    
      handleDownload() {
            this.downloadLoading = true
            require.ensure([], () => {
              const { export_json_to_excel } = require('@/vendor/Export2Excel')
              const tHeader = ['日期', '姓名', '地址']
              const filterVal = ['date', 'name', 'address']
              const list = this.tableData
              const data = this.formatJson(filterVal, list)
              export_json_to_excel(tHeader, data, '列表excel')
              this.downloadLoading = false
            })
          },
          formatJson(filterVal, jsonData) {
            return jsonData.map(v => filterVal.map(j => v[j]))
          }

## 4.注意问题

const { export_json_to_excel } = require('@/vendor/Export2Excel')这里的@/vendor/Export2Excel路径问题
关键是看build目录下的webpack.base.conf.js配置文件的alias

需要修改两个地方

* Export2Excel.js

      require('script-loader!@/vendor/Blob');

* handleDownload()

      const { export_json_to_excel } = require("@/vendor/Export2Excel");

# 数据可视化

    npm i v-charts echarts -S