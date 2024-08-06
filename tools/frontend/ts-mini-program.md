# ts-mini-program
- 微信小程序, [文档](https://developers.weixin.qq.com/miniprogram/dev/framework/) | [小程序后台](https://mp.weixin.qq.com/) 
- uniapp, [搭建环境和发布](https://uniapp.dcloud.net.cn/quickstart-cli.html) | [文件目录](https://uniapp.dcloud.net.cn/tutorial/project.html) | 
- typescript
- uv-ui, [文档](https://www.uvui.cn/components/intro.html)
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
	1. 打开[微信开发者工具](https://developers.weixin.qq.com/miniprogram/dev/devtools/download.html)，设置里开启service mode
	2. 项目根目录执行`npm run xxxxxx`，选项比较多建议把自己用的改短一点
	3. 执行之后uniapp会自动把uniapp+vue项目自动编译成目标小程序项目结构
	4. 根据uniapp的提示，在微信开发者工具里导入项目的`./dist/dev/mp-weixin`，这是uniapp项目编译成的小程序项目，小程序再模拟运行它。
		- [小程序后台](https://mp.weixin.qq.com/wxopen/waregister?action=step1)获取appid

## 使用uv-ui
参考 [官方文档](https://www.uvui.cn/components/install.html)
```bash
npm i @climblee/uv-ui
```
修改`src/pages.json`的`easycom`，这个文件是uniapp专门用来添加vue工具链的。
```json
// pages.json
{
    "easycom": {
		"autoscan": true,
		"custom": {
			"^uv-(.*)": "@climblee/uv-ui/components/uv-$1/uv-$1.vue"
		}
	},
    // 其他内容
    pages:[
        // ...
    ]
}
```
项目根目录添加一个vue.config.ts，内容如下，作用是让uv-ui能被uniapp一起编译。
```ts
 module.exports = {
    transpileDependencies: ['@climblee/uv-ui']
 }
```