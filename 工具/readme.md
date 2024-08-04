# 工具

这里记录一些介绍文字比较少的小工具，也当做备忘录用。字数多的写在同级目录下的其它文档里。

## 生成目录结构树

我说的目录结构树是这种东西:
```
react-app
├── config
│   └── webpack.config.js
└── components
    ├── Modal
    ├── Form
    └── Table
```

- `https://devtool.tech/tree` 可以把markdown无序列表转换成目录结构树
- `https://dir.yardtea.cc/` 可以直接拖拽目录进去，生成文件树，还可以配置树的深度。

## ssh客户端

- `terminus` 收费，界面好看，跨端同步，
- `windterm` 功能强大，界面美观程度中上等，但是我没培养出使用习惯

现在用的还是最原始的powershell里的ssh指令，因为不需要那么频繁的远程连接了，
在虚拟机和容器里调试完就直接在远程服务器部署了，不需要对远程服务器做很多操作。

## 画图
- `http://draw.io` 这个我用的最久，功能很强，但是操作比较繁琐
- `https://excalidraw.com/` 还行，似乎不能同时打开多个页面这一点比较麻烦
- `https://www.tldraw.com/` 最近新发现的，兼顾功能性和方便，也能同时编辑多个页面，可以共享。
- `https://revezone.com` 整合了markdown、excalidraw和tldraw，有点Obsidian的既视感，总担心他收费严重，而且目前bug不少。

每每用到html5的canvas，我都会产生一股自己写一个画图软件的冲动，因为我对这些画图软件总是不满意。

`processon` 倒是很好用，就是收费贵了些，到处让人充钱的界面，做的跟百度网盘似的，看着很不舒服。