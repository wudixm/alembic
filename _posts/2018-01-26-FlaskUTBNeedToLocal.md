---
layout:     post
title:   Flask UTB 视频
category: blog
description:	Flask UTB 视频
---

### 第一个Flask 程序讲解：

1. 第一次创建项目的时候，需要添加Flask 的虚拟环境、添加虚拟环境的时候，一定要选择到python 这个执行文件

2. Flask 程序代码的详细解释：

   ```python
   # -*- coding: utf-8 -*-
   # 从Flask 框架中导入Flask 这个类
   from flask import Flask

   # 初始化一个Flask 对象
   # 需要传递一个参数，固定格式__name__
   # 为什么要写这个name？
   # 1. 方便Flask 框架去寻找资源
   # 2. 方便Flask 插件比如flask-Sqlalchemy 出现错误的时候，好去寻找问题所在的位置
   app = Flask(__name__)

   # @app.route 是一个装饰器
   # @ 开头的，并且在函数的上面，说明是装饰器
   # 这个装饰器的作用是，做一个URL 与视图函数的映射
   # 127.0.0.1:5000/   ->  去请求hello_world 这个函数，并将结果返回给浏览器
   @app.route('/')
   def hello_world():
       return "fdsfdsaf world"

   # 如果当前这个文件是作为入口程序运行，那么就会执行app.run()
   if __name__ == '__main__':
       # app.run() 启动一个应用服务器，来接受用户的请求
       # while true
       #     listen()
       app.run(port=9099,debug=True)
   ```

### 设置debug 模式：

1. 在app.run() 中传入一个关键字参数debug=True，就设置当前项目为debug 模式
2. debug 模式的两大优点
   - 当程序出现问题的时候，可以在页面中看到错误信息和出错的位置
   - 只要修改了项目中的python 文件，程序会自动加载，不需要手动重新启动服务器

### 使用配置文件：

1. 新建一个config.py 文件

2. 在主app 文件中，导入这个文件，并且配置到app 中：

   ```python
   # -*- coding: utf-8 -*-
   import config
   # config 文件内容只有，DEBUG=True，注意，DEBUG 一定要大写
   app.config.from_object(config)
   ```

3. 其它还有许多的其他参数，都是放在这个配置文件中，比如SECREt_KEY，SQLALCHEMY，这些配置文件都是在这个文件中

### URL 传参数

1. 参数的作用：可以在相同的URL，但是指定不同的参数，来加载不同的数据

2. 在Flask 当中如何使用参数：

   ```python
   @app.route('/article/<id>')
   def article(id):
       return u'您请求的参数是 %s' % id
   ```

   - 参数需要放在两个尖括号中
   - 视图函数中需要放和url 中的参数同名的参数

### 反转URL

1. 什么叫做返回URL：从视图函数到url 的转换叫做反转url
2. 反转url 的用处：
   - 在页面重定向的时候，会使用url 反转
   - 在模板中，也会使用url 反转

### 页面跳转和重定向

1. 用处：在用户访问一些需要登录的页面的时候，如果用户没有登录，那么可以让他重定向到登录页面

2. 代码实现：

   ```python
   from flask import Flask, url_for,redirect
   @app.route('/question/<is_login>')
   def question(is_login):
       if is_login == 1:
           return "question page"
       else:
           return redirect(url_for('login'))
   ```

### Flask 渲染Jinja2 模板和传参

1. 如何渲染模板：
   - 模板放在templates 文件夹下
   - 从Flask 中导入render_template 函数
   - 在视图函数中，使用render_template 函数，渲染模板，**注意：只需要填写模板的名字，不需要填写template 文件夹的路径。**
2. 模板传参：
   - 如果只有一个或者少量参数，直接在render_template 函数中添加关键字参数就可以了
   - 如果有多个参数的时候，那么可以先把所有的参数放在字典中，然后在render_template 中使用两个星号，把字典转换成关键参数传递进去，这样的代码更方便管理和使用。
3. 在模板中，如果要使用一个变量，语法是: "{{ params }}"
4. 访问模型中的属性或者是字典，可以通过“{{ params.property}}” 的形式，或者使用“{{params['property']}}”

### if 判断

1. 语法：

   ```python
   {% if user and user.age > 18 %}
       <a href="#">{{ user.username }}</a>
       <a href="#">log out</a>
   {% else %}
       <a href="#">log in</a>
       <a href="#">log out</a>
   {% endif %}
   ```

2. if 的使用，可以和python 中相差无几，and or > < ==  等等


### 过滤器

1. 介绍过滤器：

   - 介绍：过滤器可以处理变量，把原始的变量经过处理后再展示出来

   - 语法：`{{ avatar | default('xxx')}}`

2. default 过滤器：如果当前变量不存在，可以指定默认值

3. length 过滤器：求列表，字符串或者字典或元组的长度


### 继承和block：

1. 继承作用和语法：

   - 作用：可以把一些公共的代码放在父模板中，避免每个模板写同样的代码
   - 语法：

   ```html
   {% extends 'base.html' %}
   ```

2. Block 实现：

   - 作用：可以让子模板实现一些自己的需求，父模板需要定义好
   - 注意点：子模板中的代码必须放在block 块中，才可以显示出来