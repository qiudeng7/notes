# django笔记

教程：[Python Django Tutorial for Beginners - YouTube](https://www.youtube.com/watch?v=rHux0gMZ3Eg)，后面好像要付费，b站有[完整版](https://www.bilibili.com/video/BV1eX4y1f7Pz/)

## 基础

- 执行 `django-admin startproject demo . ` ，以当前目录作为demo项目的根目录，创建项目。

- 后面用`python manage.py`代替`djanggo-admin`进行项目级的操作，`python manage.py runserver [port]` 初始化并启动web服务器，端口默认8000。

- `python manage.py startapp playground` 创建一个即插即用的应用模块，然后到主应用的settings.INSTALLED_APPS里添加这个刚刚的`name`
  - 默认的`sessions` app 可以注释掉，过时了。

- 处理请求，实现的效果是访问`playground/hello`，服务端返回`HelloWorld`

  - 在`playground`新建一个`urls.py`文件用于绑定接口和处理接口的函数。

    ```python
    from django.urls import path
    from . import views
    
    # 当访问playground/hello的时候，把请求交给views.hello处理
    urlpatterns= [
        path('hello/',views.hello)
    ]
    ```

  - 去`playground.views`里写一个hello函数来处理playground/hello的请求

    ```python
    from django.http import HttpResponse
    
    # Create your views here.
    # view在django中的意思是request handler
    
    # 定义一个处理请求的函数
    # 将在urls.py中把具体的请求资源路径和这个函数联系起来
    def hello(request):
        return HttpResponse('Hello World!')
    ```

  - 在`demo.urls`里加载playground里的接口，

    ```python
    from django.contrib import admin
    from django.urls import path,include
    
    urlpatterns = [
        path('admin/', admin.site.urls),
        # 把所有playground下的请求都交给playground app下的urls模块，在那继续寻找接口
        path('playground/',include('playground.urls'))
    ]
    ```

- 服务端渲染

  - 在`playground`下新建`templates`目录，`templates`下新建hello.html，随便写点标签

  - 把处理请求的函数改成这样，再打开刚刚的接口就变了

    ```python
    def hello(request):
        # return HttpResponse('Hello World!')
        
        # 自动查找`./templates/hello.html`
        return render(request,'hello.html')
    ```

  - 还可以给页面传参，有一些模板语法，就不记了，用不上。

## debug

- vscode调试django [跳转到一小时三十七分处](https://www.bilibili.com/video/BV1eX4y1f7Pz/) 

- django debug toolbar

  - `pip install django-debug-toolbar`

  - 在`demo.settings`：

    - `INSTALLED_APPS`里添加`demo_toolbar`
    - `MIDDLEWARE`（中间件）里添加`debug_toolbar.middleware.DebugToolbarMiddleware`
    - 添加internal ip字段

      ```python
      INTERNAL_IPS = [
          '127.0.0.1'
      ]
      ```

  - 在`demo.urls.urlpatterns`里添加`path('__debug__/',include(debug_toolbar.urls))`

    ```python
    from django.contrib import admin
    from django.urls import path,include
    import debug_toolbar
    
    urlpatterns = [
        path('admin/', admin.site.urls),
        # 读取playground的接口
        path('playground/',include('playground.urls')),
        path('__debug__/',include(debug_toolbar.urls))
    ]
    ```

  - 修改html，当有body标签的时候，debug toolbar就会显示了

## model

- 一个继承`django.db.models.Model` 的class可以当做数据库中的实体，它的属性就是各种类型的数据库字段。所有字段类型的参考：[Model field reference | Django documentation | Django (djangoproject.com)](https://docs.djangoproject.com/en/5.0/ref/models/fields/)

  实体类一般放在`app.models`里

- 新建一个app然后创建model

  - `python manage.py startapp store`

  - 在`demo.settings.INSTALLED_APPS`里添加`store` app

  - 编辑`store.models`，示例如下，字段约束写在参数里。

    ```python
    from django.db import models
    
    # Create your models here.
    
    class Product(models.Model):
        title = models.CharField(max_length=255)
        description = models.TextField()
        # 定义price为六位数字，小数有两位，这两个参数是必须的
        price = models.DecimalField(max_digits=6,decimal_places=2)
    
    class Customer(models.Model):
        frist_name = models.CharField(max_length=255)
        last_name = models.CharField(max_length=255)
        email = models.EmailField(unique=True) # unique
        phone = models.CharField(max_length=255)
        birth_date = models.DateField(null=True) # nullable
    ```

  - 如果没写primary_key的话django会自动生成

- 枚举字段

  ```python
  class Customer(models.Model):
      MEMBER_ALPHA = 'A'
      MEMBER_BRAVO = 'B'
      MEMBER_CHARLIE = 'C'
      MEMBER_DENO = 'D'
      
      MEMERSHIP_CHOICES = [
          # 前一项是数据库实际存储值，后一项是人类可读值,这样可以减少占用
          (MEMBER_ALPHA,'Alpha'),
          (MEMBER_BRAVO,'Bravo'),
          (MEMBER_CHARLIE,'Charlie'),
          (MEMBER_DENO,'Deno')   
      ]
      membership = models.CharField(max_length=1,choices=MEMERSHIP_CHOICES,default=MEMBER_BRAVO)
  ```

- 实体联系：一对一

  - OneToOneField是对外键的封装

  ```python
  class Address(models.Model):
      street = models.CharField(max_length=255)
      city = models.CharField(max_length=255)
      
      # 和Customer建立一对一联系，当对应的Customer被删除时，整个Address对象也会被删除
      # 或者on_delete可以设置为models.SET_NULL，Customer被删除时Address会保留，但是customer字段会被设置成null
      customer = models.OneToOneField(to=Customer,on_delete=models.CASCADE,primary_key=True)
  ```

  这里会对Address实体创建外键cutomer，连接到Customer的主键（假设是customer_id），并且customer同时是Address的主键。

  on_delete有这些选项，分别表示当删除Customer时：

  - `models.CASCADE`（在sql中叫做级联）：删除整个Address。对于todo list中的分组，可能需要这种关系。
  - `models.SET_NULL`：把Address的customer字段设置为null，对于博客中的标签和文章可能需要这种关系。
  - `models.SET_DEFAULT`：把Address的customer字段设置为默认值。对于博客中的分类可能需要这种关系，默认值可以是未分类。
  - `models.PROTECT`：不允许删除Customer，必须先删除Address。

- 一对多：

  - 直接使用外键

  ```python
  cutomer = models.ForeignKey(Customer, on_delete=models.CASCADE)
  ```

- 多对多

  ```Python
  class Promotion(models.Model):
      description = models.CharField(max_length=255)
      # Product下的ManyToMany会自动在这里生成一个products字段
      
  
  
  class Product(models.Model):
      title = models.CharField(max_length=255)
      description = models.TextField()
      # 定义price为六位数字，小数有两位，这两个参数是必须的
      price = models.DecimalField(max_digits=6,decimal_places=2)
      promotions = models.ManyToManyField(Promotion,related_name='products')
  ```

  实际生成的sql可能是这样：

  ```sql
  -- 创建 Promotion 表
  CREATE TABLE "promotion" (
      "id" integer NOT NULL PRIMARY KEY AUTOINCREMENT,
      "description" varchar(255) NOT NULL
  );
  
  -- 创建 Product 表
  CREATE TABLE "product" (
      "id" integer NOT NULL PRIMARY KEY AUTOINCREMENT,
      "title" varchar(255) NOT NULL,
      "description" text NOT NULL,
      "price" decimal(6, 2) NOT NULL  -- 注意：decimal(6, 2) 表示最多6位数字，其中最多2位是小数
  );
  
  -- 由于是多对多关系，Django 会创建一个额外的关联表来存储 Product 和 Promotion 之间的关系
  CREATE TABLE "product_promotion" (
      "id" integer NOT NULL PRIMARY KEY AUTOINCREMENT,
      "product_id" integer NOT NULL,
      "promotion_id" integer NOT NULL,
      FOREIGN KEY ("product_id") REFERENCES "product" ("id") ON DELETE CASCADE,
      FOREIGN KEY ("promotion_id") REFERENCES "promotion" ("id") ON DELETE CASCADE
  );
  ```

  - 如果不写realted_name的话，Promotion下的字段会是prouction_set，还是不建议写related_name，保持默认即可。

###  migration

- migration相当于一个数据库结构的快照，可以通过`python manage.py makemigrations`生成当前数据库结构的一个快照，以后有变动的话可以再用这个指令生成新的migration，新的migration不会保存所有的信息，只会相对上一次migration记录结构变动。
- 可以重命名某个migration，但是也要修改依赖，比如003_A可能依赖002_B，把002_B.py改名为002_AB之后，要在003_A里修改依赖。
- `python manage.py migrate`，把migration转化为sql，并应用到数据库。

### metadata

- 通过metadata设置表index，order，table_name
  - https://www.bilibili.com/video/BV1eX4y1f7Pz?p=12