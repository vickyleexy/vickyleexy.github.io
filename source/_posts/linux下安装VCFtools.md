

# linux下安装VCFtools & FastQTL

## **一. 安装VCFtools**

### 1. 下载它的最新版本

https://github.com/vcftools/vcftools

### 2. 安装

2.1 解压：

```
tar -xvf vcfools.0.X.XX.tar.gz
```

2.2 安装 tabix package，其实也就是samtools，因为之前已经安装过了，在这里就不重复安装了。

2.3 将这个模块`Vcf.pm module`加到环境路径中

```
export PERL5LIB=/path/to/your/vcftools-directory/src/perl/
```

![2](/home/vickyleexy/Desktop/genesoftware/vcftools/2.png)

![1](/home/vickyleexy/Desktop/genesoftware/vcftools/1.png)

2.4 编译安装

将路径切换到vcftools下，并执行：

```
cd vcftools/ 
./autogen.sh 
./configure	
make 
make install
```

![3](/home/vickyleexy/Desktop/genesoftware/vcftools/3.png)

![4](/home/vickyleexy/Desktop/genesoftware/vcftools/4.png)

![5](/home/vickyleexy/Desktop/genesoftware/vcftools/5.png)

![6](/home/vickyleexy/Desktop/genesoftware/vcftools/6.png)



## **二. 安装FastQTL**

### 1.下载linux版本（此处用的源码版本）

http://fastqtl.sourceforge.net/

第一个应该是一个已经编译好的版本，但以防不能用（虽然试了下也确实报错了），那我们在这里就重新编译安装。ps：在下载的压缩包中有个叫 "INSTALL" 的文件中其实有具体的安装过程。

### 2.进行安装 

#### 2.1 安装**The standalone Rmath library:**

其实在deepin中可以通过apt-get安装，但在之后的FastQTL中有一些路径的问题比较麻烦，所以在这里我们用压缩包的方式进行安装。

**2.1.1** 下载压缩包，在这里最新的版本是3.4.1，所以我们下载的是[R-3.4.1.tar.gz](https://mirrors.tuna.tsinghua.edu.cn/CRAN/src/base/R-3/R-3.4.1.tar.gz)

链接：https://mirrors.tuna.tsinghua.edu.cn/CRAN/

**2.1.2**  解压压缩包：

```
tar xzvf R-3.4.1.tar.gz
```

**2.1.3** 进入文件夹

```
cd R-3.4.1
```

**2.1.4**  配置编译文件

```
./configure
```

**2.1.5**  进入 `R math library `文件夹

```
cd src/nmath/standalone
```

**2.1.6** 编译此文件夹中 的代码

```
make
```

**2.1.7** 返回到R-3.4.1文件夹中

```
cd ../..
```

**2.1.8** 记住此文件夹的路径，后面的fastqtl中的编译文件中需要修改一下

```
RMATH=$(pwd)
我的是：RMATH=~/gene/R-3.4.1/src
```

**2.1.9** 将Makefile文件中RMATH的路径修改一下：

```
#PLEASE SPECIFY THE R path here where you built the R math library standalone 
RMATH=~/gene/R-3.4.1/src         #修改此处
```

* 在deepin中也可以通过apt-get直接安装**The standalone Rmath library**：

  ```
  sudo apt-get install r-mathlib 
  ```

  但这样有一个问题，要用`whereis Rmath` 定位一下Rmath，还要用`find / -name libRmath.a`搜一下libRmath.a 在哪个文件夹下，从而更改Makefile文件中的路径，感觉比较麻烦。

  ​

#### 2.2 安装**The Boost C++ libraries**

第一种方法：（手工安装）

(1)下载[boost_1_65_0.tar.bz2](http://dl.bintray.com/boostorg/release/1.65.0/source/boost_1_65_0.tar.bz2)

链接：http://www.boost.org/users/download/

(2) Unzip the package:				

```
tar xzvf boost165_0.tar.bz2
```

(3) Go in the folder:				

```
cd boost165_0
```

(4) Install required packages:		

```
./bootstrap.sh --prefix=/home/olivier/Desktop/myInstall --with-libraries=iostreams,program_options
```

第二种方法：（自动安装，推荐）

（1）搜索仓库中关于boost的安装包

```
apt-cache search boost
```

![5](/home/vickyleexy/Desktop/genesoftware/fastqtl/5.png)

（2）安装以下，即可安装所有的依赖包和插件

```
sudo apt-get install libboost-all-dev
```

![7](/home/vickyleexy/Desktop/genesoftware/fastqtl/7.png)

之前直接安装的`libboost-dev`，然后开始不知道为啥编译的时候一直出现`未定义`这个问题，应该是`iostreams`这个包没有安装的问题，所以还是直接全安装得了……

![1](/home/vickyleexy/Desktop/genesoftware/fastqtl/1.png)

#### 2.3 安装**GSL**

```
sudo apt-get install libgsl0-dev
```

![6](/home/vickyleexy/Desktop/genesoftware/fastqtl/6.png)

### 2.4 安装zlib

下载：[US (zlib.net)](http://www.zlib.net/zlib-1.2.11.tar.gz) ([GPG signature](http://www.zlib.net/zlib-1.2.11.tar.gz.asc))

解压缩后进入文件夹，然后

```
./configure
make
sudo make install
```

**2.5** **编译 FASTQTL**

```
cd FastQTL
make cleanall && make
./bin/fastQTL.static --help      #验证
```

![2](/home/vickyleexy/Desktop/genesoftware/fastqtl/2.png)

![3](/home/vickyleexy/Desktop/genesoftware/fastqtl/3.png)

![4](/home/vickyleexy/Desktop/genesoftware/fastqtl/4.png)