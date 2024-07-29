# Nginx

看的这个教程：[NGINX Tutorial for Beginners (youtube.com)](https://www.youtube.com/watch?v=9t9Mp0BGnyI)，Nginx的发音是 engine X。

我发现老外的教程基本都是 “讲给傻子听” 的

1. Nginx的作用是反向代理，负载均衡，缓存，是整个后台系统的入口，最主要的作用是这两个：
   1. 作为负载均衡器，Nginx把请求发送给空闲服务器
   2. 由Nginx来解密https的加密数据，而不必在系统内部的每个服务器都要添加解密程序。

## 基础

nginx.conf里实际生效的内容叫做directive（命令），命令可以写在特定的上下文环境里。上下文有两个，一个http，定义了与 HTTP 服务相关的配置，包括服务器如何处理 HTTP 请求；一个events，定义了 Nginx 服务器的事件处理机制，它影响着 Nginx 处理连接和请求的效率。

### 静态文件服务器

```

http{
	types{
		# 将css后缀的文件识别为text/css的mime类型
		# 否则css虽然会被浏览器下载 但不会被应用到html
		text/css css
		test/html html
	}
	# 定义一个http服务器
	server{
		listen 80;
		# 这里的相对引用似乎是相对于执行nginx指令的目录，或者nginx所在目录
		# root是 http://ip:prot 会访问到的页面
		root html
	}
}

events{}
```



### 导入mime type

```
http{
	# 导入mime.types文件
	include mime.types;
	
	server{
		listen 80;
		root ../html/index.html
	}
}

events{}
```

### 资源路径

用location定义url中的资源路径，在location里用root定义这个路径对应实际打开路径，不过要注意使用root命令的时候，资源路径如果写的是`/hello`，就必须在`html`目录下真的有一个`hello`目录，root也必须指向这个`hello`。

但是就算不写这个root和location，资源路径直接访问hello也是能访问到的，所以像下面这样写其实写不写都一样，但是location下会有其他命令可以生效，也可以不用root命令，用alias。

```
http{
	include mime.types;
	
	server{
		listen 80;
		root html;
		
		location /hello {
		
			# 会打开./html/hello/index.html
			root hello
		}
	}
}

events{}
```

### alias，tryfiles

- `alias`：比如在location A下写alias B，那么A就是B的一个别名。***这里有个巨坑***，Windows下用alias，后面的路径要用单引号，并且用双反斜杠，像这样`'D:\\workspace\\nginx-demo\\html'`，折腾我好久。
- `try_files`：在location下找不到index.html时，会尝试找try_files设置的文件作为响应，多个候选文件之间用空格间隔，最后一项写`=404`表示404页面。

### location 正则

`~*` 表示后面是一个正则表达式，示例所示的正则会匹配`/count/1`、`/count/2`到`/count/9`的资源路径

```
location ~* /count/[0-9] {

}
```

### 临时重定向

像下面这样写，就可以在访问A路径的之后直接重定向到B路径，而且HTTP method保持不变，也就是GET不会变成POST，307是HTTP状态码，表示临时重定向。但是这样做URL也会变成B
```
location /A {
	return 307 B
}
```

### rewrite

在server上下文可以把一个location重定向到另一个location，效果和上面的307差不多

```
server{
	rewrite A B
}
```

## 负载均衡

// todo

## 实战笔记

```
http{
	# 定义一个名为django的服务器组，后面可以设置把请求转发到这
    upstream django {
        server 0.0.0.0:8000;
    }
    
    server{
        # 表示接收到的所有http请求，里，发给哪些ip的会被本server模块处理
        # 0.0.0.0表示nginx接收到的所有请求都会被本server模块处理
        server_name 0.0.0.0;

        # 客户端请求的最大body大小限制为10MB
        client_max_body_size 10M;
		
		location / {
            # 设置静态文件的根目录
            root /app/dist;
            # $uri：首先尝试查找与请求URI完全匹配的文件。例如，如果用户请求http://www.example.com/about，则Nginx会尝试查找/app/dist/about文件。
            # $uri/：如果上一步没有找到文件，Nginx会尝试查找一个目录。这里的$uri/表示将URI视为目录，并尝试查找目录索引文件。例如，如果用户请求http://www.example.com/about/，Nginx会尝试查找/app/dist/about/目录。
            # /index.html：如果前两个尝试都失败了，Nginx将使用/app/dist/index.html文件作为默认文件来响应请求。这通常用于单页应用（SPA），其中前端路由由JavaScript处理，所有的页面请求最终都映射到index.html。
            try_files $uri $uri/ /index.html;
        }
        
        location /api/ {
            # 把请求发送给之前定义的django服务器组
            proxy_pass http://django;
            
            # 转发给django的请求，附带这些请求头
            proxy_set_header Host $host; # 要请求的服务器的域名和端口号 形如 www.example.com
            proxy_set_header X-Real-IP $remote_addr; # 原始请求的客户端IP
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for; # 形如 123.456.789.0, 192.168.1.1，表示该请求的来源和经过哪些代理
            proxy_set_header X-Forwarded-Proto $scheme; # 用于表示原始请求是http还是https，取值就是'https'或者'http'

            # 收到django的响应之后覆盖默认的响应头，再发送给客户端
            add_header 'Access-Control-Allow-Origin' '*';
            add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';
            add_header 'Access-Control-Allow-Headers' 'Origin, Content-Type, Accept, Authorization';
            add_header 'Access-Control-Allow-Credentials' 'true';
        }
        
    }
    
}
```