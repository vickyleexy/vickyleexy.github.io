# 搭建网站 ---- Django（1）

基于python中的Django框架，进行基本的网站搭建。

ps：基本的python环境已经搭建完毕，且系统为linux。

1. 安装Django。

   直接用pip命令安装即可：

   ``` pip install django``` 

   ![1](/home/vickyleexy/Desktop/webpic/1.png)

   可用以下命令查看帮助。`django-admin.py --help`

   ![2](/home/vickyleexy/Desktop/webpic/2.png)


2. 创建一个Django项目 

   ```django-admin.py startproject mysite```

   ![3](/home/vickyleexy/Desktop/webpic/3.png)

   查看创建的目录中的结构：

   ![9](/home/vickyleexy/Desktop/webpic/9.png)

   其中：

   - 最外层目录mysite/ 是我们在命令中创建的项目名称，它的名字和django无关，随便起名字都可以。
   - manage.py 文件是本项目的管理文件，项目的开启、配置等操作都通过这个文件完成。
   - 内层的 mysite/ 目录是真正的项目目录，里面含有项目中所需要的各种模块等内容。
   - \__init__.py 是引用模块时必备的文件，告诉解释器这个目录可以看成一个python程序包。
   - mysite/settings.py 是配置文件，负责项目的各种配置。
   - mysite/urls.py 是负责处理url相关操作的。

3. 运行服务器 `python ./manage.py runserver`

   ![4](/home/vickyleexy/Desktop/webpic/4.png)

   可以看出缺少点东西，根据提示，运行`python manage.py migrate`

   将manage.py赋予读写权限，以便更加方便调用`chmod 755 manage.py`

   ![6](/home/vickyleexy/Desktop/webpic/6.png)

   ![7](/home/vickyleexy/Desktop/webpic/7.png)

   再次运行服务器:

   ![8](/home/vickyleexy/Desktop/webpic/8.png)

   打开提示中的网址可看到：

   ![5](/home/vickyleexy/Desktop/webpic/5.png)

搞定～