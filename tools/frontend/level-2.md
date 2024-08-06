# level-2
- uniapp
- typescript
大部分配置文件和用法之类的都可以参考模板或者问AI要示例。
## 新建uniapp项目
1. node 版本 18+、20+
2. 安装
```bash
# js
npx degit dcloudio/uni-preset-vue#vite my-vue3-project

# ts
npx degit dcloudio/uni-preset-vue#vite-ts my-vue3-project
```
3. degit是一个脚手架，井号用于定位github分支，不是注释。
4. 如果下载失败可以从gitee获取模板：[js](https://gitee.com/dcloud/uni-preset-vue/repository/archive/vite.zip) | [ts](https://gitee.com/dcloud/uni-preset-vue/repository/archive/vite-ts.zip)
5. 测试运行模板
	1. 打开微信开发者工具，设置里开启service mode
	2. 项目根目录执行`npm run xxxxxx`，选项比较多建议把自己用的改短一点
	3. 执行之后uniapp会自动把uniapp+vue项目自动编译成目标小程序项目结构
	4. 根据uniapp的提示，在微信开发者工具里导入项目的`./dist/dev/mp-weixin`，这是uniapp项目编译成的小程序项目，小程序再模拟运行它。
## 微信平台
1. [下载微信开发者工具](https://developers.weixin.qq.com/miniprogram/dev/devtools/download.html)
2. [小程序后台](https://mp.weixin.qq.com/wxopen/waregister?action=step1)
打开设置，service mode那一页几个选项全部打开，作用是让uni-app使用微信开发工具的接口。

