---
title: 小程序开发总结(一)
date: 2018-06-25 14:38:29
tags:
---
新入职公司后，一直在参与几个小程序项目的开发，在开发的过程中也遇到了困扰和问题，今天就来总结归纳一下，也希望可以为新入门的朋友带一些帮助，避免入坑。
### 1.路由跳转问题
小程序提供五种路由跳转方式，每种有不同的功能，要仔细阅读文档，根据需求适当使用
1.若要跳转的目标路由页面是tabBar配置，则只能使用wx.switchTab方法，且路径后不能携带参数。
2.若正常跳转使用wx.navigateTo，保留当前页面，跳转到应用内的某个页面，路径后可带参数
3.若要关闭当前页面跳转到应用内的某个页面则可使用wx.redirectTo，路径后可带参数，注意，此时不能返回到上一页面，因上一级页面栈被销毁
4.小程序现默认路由嵌套最多十层，注意需求逻辑，不要造成循环，避免路由跳转失效

### 2.带参返回上一页几种方法
有时候我们会有这种需求，有AB页面,从A跳转到B填写完信息后再跳回A，并携带数据。
#### 方法一
`把当前页面数据放入本地缓存（ wx.setStorage（wx.setStorageSync），上一个页面再从缓存中取出（wx.getStorage（wx.getStorageSync））同时退出登录时要清除缓存，此方法不建议，下面还有更多方法`
#### 方法二
```
在当前页设置上一页的data,例如
  var pages = getCurrentPages();             //  获取页面栈
  var currPage = pages[pages.length - 1];    // 当前页面
  var prevPage = pages[pages.length - 2];   // 上一个页面
  prevPage.setData({                        // 设置上一页的数据
   mydata: {name:'json', age:10}            // 假数据
  })
```
#### 方法三
直接调用方法名来更新数据
```
页面A
Page({
     data: {
        name: ''
     },
     ...
     ,
     //更新name
     changeData: function(name){
        this.setData({
            name: name
        })
     }
})

```
```
页面B,假设有一个文本框用于输入姓名,点击返回按钮后更新页面A的name
Page({
    //此方法用于文本框输入回调
    inputTyping: function (e) {
        //获取页面栈
        var pages = getCurrentPages();
        if(pages.length > 1){
            //上一个页面实例对象
            var prePage = pages[pages.length - 2];
            //关键在这里
            prePage.changeData(e.detail.value)
        }
    }
})
```
#### 方法四
在app.js中设置全局变量，当前页赋值，上一页取之，建议避免使用，尤其多人开发
```
globalData: {
    userInfo: null,
  }
```
### 3.小程序项目包大小限制
#### 一.小程序包现默认大小为2M,超出将无法上传，下面说一下对小程序项目包大小的一些处理
```
1. 项目中，图片的资源是影响项目包大小的一个主要原因，所以一定要进行压缩处理！一定要进行压缩处理！一定要进行压缩处理！
2. 逻辑层，对公共部分进行抽离拆分，封装组件，减少代码量，下一章节记录下小程序组件的使用。
3. 样式文件尽量复用！样式文件尽量复用！样式文件尽量复用！
```
#### 二.进行分包加载处理，[<font color=#0099ff>传送门</font>](https://developers.weixin.qq.com/miniprogram/dev/framework/subpackages.html)
```
分包的好处是将小程序划分成不同的子包，在构建时打包成不同的分包，用户在使用时按需进行加载
增加项目包体积 整个小程序所有分包大小不超过 8M  单个分包/主包大小不能超过 2M
```
<font color=#FF0000>但是注意:</font>首页的 TAB 页面必须在 app（主包）内，并且遵循官方给出的引用原则，我们可根据项目需求 拆分成不同的功能模块，进行分包。
项目中已经进行分包，现未发现问题，如有问题，及时更新哈。