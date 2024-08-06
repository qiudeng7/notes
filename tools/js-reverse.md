# js逆向
教程: [bilibili 志远二期](https://www.bilibili.com/video/BV1Kh411r7uR)

2024年8月5日，看到十一集的30分钟处。

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

## 加密方式和编码

这里需要掌握的能力是判断加密类型，需要了解各种密文的特征，比如他们的位数，以及熟悉'123456'的密文，填表单的时候可以填123456，可以直接通过密文判断加密方式。

如果是密码的话，可能123456会不通过校验，可以用其他密码的加密。

1. 取盐校验
    - MD5 16位、32位、64位
        - md5的32位，中间有一段就包括了16位的密文
2. 对称加密(只用一个密码进行加密和解密)
    - AES, DES, 3DES
3. 非对称加密(公钥加密,私钥解密,一个明文可以生成多个密文，多个密文能够解密出同一个明文)
    - RSA

### 常见加密结果：
以下密文为16进制，通常也都是16进制。

| 名称   | 位数 | '123456' 的加密结果                                          |
| ------ | ---- | ------------------------------------------------------------ |
| md5    | 16位 | 49ba59abbe56e057                                             |
| md5    | 32位 | e10adc3949ba59abbe56e057f20f883e                             |
| sha1   | 40位 | 7c4a8d09ca3762af61e59520943dc26494f8941b                     |
| sha256 | 64位 | 8d969eef6ecad3c29a3a629280e686cf0c3f5d5a86aff3ca12020c923adc6c92 |


### 编码

通常用16进制和base64 

- 16进制，如果包含比F大的字母就不是16进制，可能是base64编码
- base64 
    - 浏览器环境中用`btoa`函数进行base64编码，`atob`进行解码
    - base64组成只包括大小写字母、数字、加号和等于号，末尾常有等于号

    | 原文 | base64 |
    | ---- | ------ |
    | btoa('123456') | 'MTIzNDU2' |
    | btoa('123456a') | 'MTIzNDU2YQ==' |
    | btoa('123456ab') | 'MTIzNDU2YWI=' |
    | btoa('123456abc') | 'MTIzNDU2YWJj' |
    | btoa('123456abcd') | 'MTIzNDU2YWJjZA==' |
    | btoa('123456abcde') | 'MTIzNDU2YWJjZGU=' |

## 抓包之后

### 搜索思路一
抓包之后可以在sources页面按ctrl+shift+f进行全局搜索，
搜索的关键字可以是payload的参数名称，或者url接口名

比如xhr传了一个password参数，内容是密文的，如果要搜索password，一个思路是找password的赋值，那么可以搜索这些关键字:
1. .passoword= 
2. ['password']= 
3. ["password"]=

一般格式化之前赋值运算符是没有空格的

也可以找formdata的创建，其他参数的赋值

### 搜索思路二
搜索`0123456789`、`abcedf`，因为所有加密算法都是需要填充值的，如果是没有魔改过的md5算法默认填充值就是这些，所以生成的就是16进制。

这样搜索可以直接搜索到加密算法内部

或者可以搜索常见算法内部的魔法值，比如常规md5算法的一个magic number是`1732584193`


