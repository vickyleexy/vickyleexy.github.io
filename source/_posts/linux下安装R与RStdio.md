# linux下安装R与RStudio

## 1. 安装R

#### 1.1 最简单安装方法，如果不介意版本的话

在linux下可直接通过命令行安装:(但不一定是最新版)

```
- sudo apt-get update
- sudo apt-get install r-base r-base-dev
```

![1](/home/vickyleexy/Desktop/genesoftware/R/1.png)

**check：**

![3](/home/vickyleexy/Desktop/genesoftware/R/3.png)

#### 1.2 在这里更想更新源，安装最新版本：so以下

打开安装源列表：

```
- sudo gedit /etc/apt/sources.list 
```

![10](/home/vickyleexy/Desktop/genesoftware/R/10.png)

将以下安装源添加到打开的安装源列表中。

```
- deb http://mirrors.tuna.tsinghua.edu.cn/CRAN/bin/linux/debian stretch-cran34/
```

![4](/home/vickyleexy/Desktop/genesoftware/R/4.png)

可以从此链接https://cran.r-project.org/ 中Supported branches部分，查看更新源的格式等信息。

```
deb http://<favourite-cran-mirror>/bin/linux/debian stretch-cran34/
```

然后终端输入以下命令进行更新：

```
- sudo apt-get update
```

however……报错如下，没有公钥，无法验证数字签名：

![5](/home/vickyleexy/Desktop/genesoftware/R/5.png)

CRAN中存储的debain包需要通过密钥FCAE2A0E115C3D8A （上面报错中缺少的公钥）进行签名验证，运行以下命令添加密钥到debain系统，在终端输入：

```
- sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys FCAE2A0E115C3D8A 
```

然后再使用`sudo apt-get update`，进行更新，通过：

![7](/home/vickyleexy/Desktop/genesoftware/R/7.png)

最近进行安装R：

```
- sudo apt-get install r-base r-base-dev
```

![8](/home/vickyleexy/Desktop/genesoftware/R/8.png)

check一下，可以看出从刚才的版本3.3变成了3.4：

![9](/home/vickyleexy/Desktop/genesoftware/R/9.png)

**参考：**

1. https://mirrors.tuna.tsinghua.edu.cn/CRAN/
2. http://blog.sina.com.cn/s/blog_ab08623c0101d4uw.html
3. http://jingyan.baidu.com/article/e8cdb32b3526f837052badea.html

## 2. 安装RStudio

#### 2.1 下载：

https://www.rstudio.com/products/rstudio/download/

#### 2.2 安装：

```
- sudo apt-get install gdebi-core
- sudo apt-get install libapparmor1
- cd gene                            #放deb安装文件的文件夹
- sudo gdebi rstudio-xenial-1.0.153-amd64.deb
```

![1](/home/vickyleexy/Desktop/genesoftware/rstdio/1.png)

![2](/home/vickyleexy/Desktop/genesoftware/rstdio/2.png)

![3](/home/vickyleexy/Desktop/genesoftware/rstdio/3.png)

经过上面的几条命令，RStudio应该已经安装好了，下面可以访问试一下。

输入一下命令进行安装测试：

```
- sudo rstudio verify-installation
```

![4](/home/vickyleexy/Desktop/genesoftware/rstdio/4.png)

![5](/home/vickyleexy/Desktop/genesoftware/rstdio/5.png)

