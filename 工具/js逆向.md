# js逆向

## 断点类型
- dom断点
    - 直接对某个元素右键，可以监听这个元素的dom树变化、attribute变化或者删除
- dom事件断点
    - 选中某个元素，elements页面右侧默认是styles，切换到Event Listeners，可以查看到这个元素的所有事件监听器，也会显示对应代码位置。可以去监听器里面下断点，触发事件之后就会断住。
- 所有事件断点
    - 找到sources标签页右侧面板的`Event Listener Breakpoints`，可以断住所有此类事件
- xhr断点
    - sources标签页右侧面板的`XHR/fetch breakpoints`，点击右侧加号可以添加断点，内容是要匹配的URL，匹配到的xhr和fetch会被断住
- 常规断点
    - 跟其他debug一样在代码行断点
    - 浏览器执行到`debugger;`语句会自动断点
    - exception断点