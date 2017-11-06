# 转录组分析流程及软件操作（基于linux）

## 一. 检测原始数据的质量

### 1.所用软件

**fastqc**（http://www.bioinformatics.babraham.ac.uk/projects/fastqc/）

### 2. fastqc安装

**2.1 软件下载**

http://www.bioinformatics.babraham.ac.uk/projects/download.html#fastqc

**2.2  安装**

2.2.1 解压缩

2.2.2 更改目录下fastqc的权限并创建软连接即可。

```
- cd ~gene/FastQC
- chmod 755 fastqc
- sudo ln -s ~/gene/FastQC/fastqc /usr/local/bin/fastqc
```

![1](/home/vickyleexy/Desktop/genesoftware/fastqc/1.png)

**2.3 操作**

可查看fastqc版本：

```
- fastqc -version
```

对序列进行质控：

```
- fastqc /media/vickyleexy/LENOVO_USB_HDD/Rawdata/* -o /media/vickyleexy/LENOVO_USB_HDD/fastqc_result
```

fastqc 要质控的序列数据 -o 输出结果的目录（不会自动生成，需手工创建）

![2](/home/vickyleexy/Desktop/genesoftware/fastqc/2.png)

对结果进行查看：

刚才存放结果的文件夹中，生成了两种格式的报告，一种是html，一种是zip格式。html格式的报告直接双击即可在浏览器中打开。

![3](/home/vickyleexy/Desktop/genesoftware/fastqc/3.png)

参考：

1. http://www.bioinformatics.babraham.ac.uk/projects/fastqc/INSTALL.txt
2. https://www.plob.org/article/5987.html

## 二. 去除测序数据中的接头和低质量的reads

### 1. 所用软件

**fastx**（http://hannonlab.cshl.edu/fastx_toolkit/）

或 **cutadapt**

或 **Trimmomatic**（http://www.usadellab.org/cms/?page=trimmomatic）

### 2. fastx安装

**2.1 下载** 

http://hannonlab.cshl.edu/fastx_toolkit/download.html

**2.2 安装前需要安装的条件**

```
- sudo apt-get install gcc g++ pkg-config wget
```

但经过测试，发现电脑上之前已经安装过gcc 、g++ 和pkg-config，所以只安装wget即可。

![1](/home/vickyleexy/Desktop/genesoftware/fastx/1.png)

![2](/home/vickyleexy/Desktop/genesoftware/fastx/2.png)

![3](/home/vickyleexy/Desktop/genesoftware/fastx/3.png)

![4](/home/vickyleexy/Desktop/genesoftware/fastx/4.png)

**2.3 Install libgtextutils**

下载并解压后

```
- cd libgtextutils-0.6
- ./configure
- make
- sudo make install
- cd ..
```

![10](/home/vickyleexy/Desktop/genesoftware/fastx/10.png)

* 但到make这一步的时候报错

  ![7](/home/vickyleexy/Desktop/genesoftware/fastx/7.png)

  报错解决参考：

1. https://stackoverflow.com/questions/38659115/make-fails-with-error-cannot-convert-stdistream-aka-stdbasic-istreamchar?answertab=active#tab-top

2. https://gcc.gnu.org/gcc-6/porting_to.html

   **解决：**

   ![9](/home/vickyleexy/Desktop/genesoftware/fastx/9.png)

   可以看出是GCC编译器的版本改变的问题。

   1. 定位报错文件的位置：/home/vickyleexy/gene/libgtextutils-0.6.1/src/gtextutils文件夹下的text_line_reader.cpp

   2. 将最后的输出中 `return input_stream` 改为`return (bool)input_stream ` 即可。

      ![6](/home/vickyleexy/Desktop/genesoftware/fastx/6.png)

   重新编译make一下，通过。

   ![8](/home/vickyleexy/Desktop/genesoftware/fastx/8.png)

继续进行安装：

![11](/home/vickyleexy/Desktop/genesoftware/fastx/11.png)

![12](/home/vickyleexy/Desktop/genesoftware/fastx/12.png)

**2.4 安装fastx-toolkit**

解压压缩包之后：

```
- cd fastx_toolkit-0.0.13.2
- ./configure
- make
- sudo make install
```

![13](/home/vickyleexy/Desktop/genesoftware/fastx/13.png)

![14](/home/vickyleexy/Desktop/genesoftware/fastx/14.png)

![15](/home/vickyleexy/Desktop/genesoftware/fastx/15.png)

**2.5 添加环境变量：**

```
To run the fastx programs, you'll need to tell ubuntu about 
 // about the new shared library in /usr/local/lib.
 //
 // Method one:
 //   add /usr/local/lib to LD_LIBRARY_PATH environment variable
 $ export LD_LIBRARY_PATH=/usr/local/lib:$LD_LIBRARY_PATH
 //
 // Method two:
 // run 'ldconfig' to update the shared libraries cache
 $ sudo ldconfig
```

![16](/home/vickyleexy/Desktop/genesoftware/fastx/16.png)

**2.6 check**

![17](/home/vickyleexy/Desktop/genesoftware/fastx/17.png)

参考：

1. http://hannonlab.cshl.edu/fastx_toolkit/install_ubuntu.txt
2. 使用参考：http://blog.sciencenet.cn/blog-1509670-848270.html


### 3. 安装cutadapt

可直接通过pip安装

```
- sudo pip install cutadapt
- which cutadapt #查看安装路径
- cutadapt --help #查看使用说明和功能参数
```

![1](/home/vickyleexy/Desktop/genesoftware/cutadapt/1.png)

![2](/home/vickyleexy/Desktop/genesoftware/cutadapt/2.png)

一般使用方法：

```
cutadapt [-a|-b|-g] input_reads.[fasta|fastq] > output_reads.[fasta|fastq]
```

-a     移除reads 3‘接头序列
-g     移除reads 5‘接头序列
-e     最大允许的错误率
-m    除去小于一定数值的序列
-M    除去大于一定长度的序列
-q     reads移去接头之前，3‘或5‘末端质量截取

### 4. 安装Trimmomatic

**4.1 下载：**

http://www.usadellab.org/cms/?page=trimmomatic

解压缩后可直接使用。

**4.2 使用**

可参考：https://www.bbsmax.com/A/kjdw7BpBdN/

## 三. 转录组数据与参考基因组序列比对（有参比对）

### 1. 所用软件

TopHat2（http://ccb.jhu.edu/software/tophat/index.shtml）

### 2. 软件安装

Tophat2 依赖以下几个软件，所以在安装TopHat2之前要先安装[*Boost* C++ Libraries](http://www.boost.org/)，SAM tools和bowtie2

### 2.1 安装Boost 

**方法一**

**2.1.1 下载**

http://www.boost.org/users/download/

**2.1.2 安装**

**解压压缩包**后

```
- cd boost_1_64_0
```

然后**运行bootstrap.sh脚本并设置相关参数**： 

```
./bootstrap.sh –with-libraries=all –with-toolset=gcc 
```

–with-libraries指定编译哪些boost库，all的话就是全部编译，只想编译部分库的话就把库的名称写上，之间用 , 号分隔即可，可指定的库有以下几种： 

| 库名              | 说明                  |
| --------------- | ------------------- |
| atomic          |                     |
| chrono          |                     |
| context         |                     |
| coroutine       |                     |
| date_time       |                     |
| exception       |                     |
| filesystem      |                     |
| graph           | 图组件和算法              |
| graph_parallel  |                     |
| iostreams       |                     |
| locale          |                     |
| log             |                     |
| math            |                     |
| mpi             | 用模板实现的元编程框架         |
| program_options |                     |
| Python          | 把C++类和函数映射到python之中 |
| random          |                     |
| regex           | 正则表达式库              |
| serialization   |                     |
| signals         |                     |
| system          |                     |
| test            |                     |
| thread          | 可移植的C++多线程库         |
| timer           |                     |
| wave            |                     |

–with-toolset指定编译时使用哪种编译器，[Linux](http://lib.csdn.net/base/linux)下使用gcc即可，如果系统中安装了多个版本的gcc，在这里可以指定gcc的版本，比如–with-toolset=gcc-6.3.0

![1](/home/vickyleexy/Desktop/genesoftware/tophat2/boost/1.png)

**注意：** 
如果是需要支持mpi，在执行了./bootstrap.sh后，需要修改文件`project-config.jam` ，添加 
`using mpi` ; 
当然，首先保证已经安装了mpich2或openmpi。然后在第2行执行： 

``` 
./bjam –with-mpi
```

**编译boost：**

```
./b2（或bjam） toolset=gcc 
```

![2](/home/vickyleexy/Desktop/genesoftware/tophat2/boost/2.png)

这一步等待时间较长。

可用`echo $?`命令查看是否执行成功，输出0则代表成功。

![3](/home/vickyleexy/Desktop/genesoftware/tophat2/boost/3.png)

**安装boost:** 

```
./b2 （或bjam）install –prefix= /home/boost/boostSDK 
```

`–prefix=/home/boost/boostSDK`用来指定boost的安装目录，不加此参数的话默认的头文件在`/usr/local/include/boost`目录下，库文件在`/usr/local/lib/`目录下。（ps: 我安装在默认路径下）

![4](/home/vickyleexy/Desktop/genesoftware/tophat2/boost/4.png)

然后做以下的两步：（默认路径就不用做了）

1. 在/usr/include/下生成一个boost库的include文件夹连接：

```
ln -s /home/boost/boostSDK/include/boost   /usr/include/boost 
```

2. 在/usr/lib/ 下生成所有boost编译出的lib库文件的对应连接 切换到stage目录下,执行

```
find $PWD/lib/*.* -type f -exec ln -s {} /usr/lib/ \;
       (普通用户执行： sudo find $PWD/lib/*.* -type f -exec ln -s {} /usr/lib/ \;)
```

**刷新链接库**

```
sudo ldconfig
```

**在使用时要注意家链接库：** 

例如：

```
g++ main.cpp -g -o main -lboost_thread
```

**测试是否安装成功：**

在linux下任意目录下创建boosttest.cpp

```
#include<iostream>
#include<boost/lexical_cast.hpp>
int main()
{
   int a = boost::lexical_cast<int>("123456");
   std::cout << a <<std::endl;
   return 0;
}
```

这是一个字符串转化为整数的简单程序。

运行命令:

```
g++ boosttest.cpp -o boosttest
./boosttest
```

将得到输出结果为：123456
代表boost安装成功。

![5](/home/vickyleexy/Desktop/genesoftware/tophat2/boost/5.png)

**参考：**

1. http://blog.csdn.net/u011573853/article/details/52682256
2. http://blog.csdn.net/happyyear1/article/details/50970742
3. http://blog.chinaunix.net/uid-12226757-id-3427282.html

**方法二**

```
apt-cache search boost       #查看libboost库的名字
sudo apt-get install libboost-all-dev  
```

### 2.2 安装SAMtools

**2.2.1 下载：**

http://www.htslib.org/download/

**2.2.2 安装**

**安装libncurses5：**

```
- sudo apt-get install libncurses5 libncurses5-dev
```

![1](/home/vickyleexy/Desktop/genesoftware/tophat2/samtools/1.png)

**安装zlib：**

```
- sudo apt-get install zlib1g-dev
```

![2](/home/vickyleexy/Desktop/genesoftware/tophat2/samtools/2.png)

**安装libcurl：**

```
- sudo apt-get install libcurl4-gnutls-dev
```

![3](/home/vickyleexy/Desktop/genesoftware/tophat2/samtools/3.png)

**安装SSL：**

```
- sudo apt-get install libssl-dev
```

![4](/home/vickyleexy/Desktop/genesoftware/tophat2/samtools/4.png)

**安装SAMTools（没错，才开始进入正式步骤）：**

将第一步下载下来的压缩包解压

**进入解压后的目录：**

```
- cd samtools-1.5
```

**配置安装程序：**

```
./configure –enable-plugins–enable-libcurl prefix=/home/vickyleexy/samtools
```

但**报错**了，如图所示：

![5](/home/vickyleexy/Desktop/genesoftware/tophat2/samtools/5.png)

![6](/home/vickyleexy/Desktop/genesoftware/tophat2/samtools/6.png)

从图中可以看出，应该是`libbz2-dev` 这个库没有安装，所以安装一下这个库：

```
- sudo apt-get install libbz2-dev
```

![7](/home/vickyleexy/Desktop/genesoftware/tophat2/samtools/7.png)

**重新配置文件：**

![8](/home/vickyleexy/Desktop/genesoftware/tophat2/samtools/8.png)

测试是否成功：`echo $?`

![9](/home/vickyleexy/Desktop/genesoftware/tophat2/samtools/9.png)

**用make来安装：**

```
- make all all-htslib
- make install install-htslib
```

![10](/home/vickyleexy/Desktop/genesoftware/tophat2/samtools/10.png)

![11](/home/vickyleexy/Desktop/genesoftware/tophat2/samtools/11.png)

**在.bashrc中配置SAMTools的路径：**

```
vim ~/.bashrc
```

![13](/home/vickyleexy/Desktop/genesoftware/tophat2/samtools/13.png)

**用vim在.bashrc中输入：**

```
# added by SAMTools
export PATH= 你的samtools的路径/samtools/bin:$PATH
```

![16](/home/vickyleexy/Desktop/genesoftware/tophat2/samtools/16.png)

**重启Terminal或运行以下命令以刷新.bashrc：**

```
source ~/.bashrc
```

![14](/home/vickyleexy/Desktop/genesoftware/tophat2/samtools/14.png)

**check：**

```
- samtools --version
- samtools view
```

![15](/home/vickyleexy/Desktop/genesoftware/tophat2/samtools/15.png)

参考：

1. http://m.010lm.com/roll/2016/0620/2343389.html

### 2.3 安装Bowtie2

**2.3.1 下载已经编译好的版本**

https://sourceforge.net/projects/bowtie-bio/files/bowtie2/

**2.3.2 安装**

将下载下来的文件解压缩，并将路径添加到环境变量中即可使用。

添加环境变量：

```
- vim ~/.bashrc
```

```
#bowtie2
export PATH=/home/vickyleexy/gene/bowtie2-2.3.2:$PATH
```

![1](/home/vickyleexy/Desktop/genesoftware/tophat2/bowtie2/1.png)

并输入`source ~/.bashrc `刷新。

![2](/home/vickyleexy/Desktop/genesoftware/tophat2/bowtie2/2.png)

**check：**

```
bowtie2 --version
```

![3](/home/vickyleexy/Desktop/genesoftware/tophat2/bowtie2/3.png)

### 2.4 安装TopHat2

**2.4.1 下载**

http://ccb.jhu.edu/software/tophat/index.shtml

**2.4.2 安装**

下载编译好的tophat-2.1.1.Linux_x86_64 直接解压缩，然后将路径添加到环境变量中即可。

```
- vim ~/.bashrc 
```

```
#TopHat2
export PATH=$PATH:/home/vickyleexy/gene/tophat-2.1.1.Linux_x86_64
```

![1](/home/vickyleexy/Desktop/genesoftware/tophat2/tophat2/1.png)

然后刷新一下：

```
source ~/.bashrc
```

**check：**

```
tophat2 --version
```

![2](/home/vickyleexy/Desktop/genesoftware/tophat2/tophat2/2.png)

至此，tophat2的安装已经完成了。为了检测tophat2的安装是否完全正确，可以用tophat2官网提供的test数据做检验。tophat官网：[http://ccb.jhu.edu/software/tophat/tutorial.shtml](http://ccb.jhu.edu/software/tophat/tutorial.shtml)

```
tar zxvf test_data.tar.gz
cd test_data
tophat -r 20 test_ref reads_1.fq reads_2.fq
```

If TopHat ran successfully, you should see some lines of output, like this:

```
[Mon May  4 11:07:23 2009] Beginning TopHat run (v1.1.1)
-----------------------------------------------
[Mon May  4 11:07:23 2009] Preparing output location ./tophat_out/
[Mon May  4 11:07:23 2009] Checking for Bowtie index files
[Mon May  4 11:07:23 2009] Checking for reference FASTA file
[Mon May  4 11:07:23 2009] Checking for Bowtie
	Bowtie version:		 0.9.9.1
[Mon May  4 11:07:23 2009] Checking reads
	seed length:	 75bp
	format:		 fastq
	quality scale:	 phred
	Splitting reads into 3 segments
[Mon May  4 11:07:23 2009] Mapping reads against test_ref with Bowtie
[Mon May  4 11:07:24 2009] Mapping reads against test_ref with Bowtie
[Mon May  4 11:07:24 2009] Mapping reads against test_ref with Bowtie
	Splitting reads into 3 segments
[Mon May  4 11:07:24 2009] Mapping reads against test_ref with Bowtie
[Mon May  4 11:07:24 2009] Mapping reads against test_ref with Bowtie
[Mon May  4 11:07:24 2009] Mapping reads against test_ref with Bowtie
[Mon May  4 11:07:24 2009] Searching for junctions via coverage islands
[Mon May  4 11:07:24 2009] Searching for junctions via mate-pair closures
[Mon May  4 11:07:24 2009] Retrieving sequences for splices
[Mon May  4 11:07:24 2009] Indexing splices
[Mon May  4 11:07:24 2009] Mapping reads against segment_juncs with Bowtie
[Mon May  4 11:07:24 2009] Mapping reads against segment_juncs with Bowtie
[Mon May  4 11:07:24 2009] Mapping reads against segment_juncs with Bowtie
[Mon May  4 11:07:24 2009] Joining segment hits
[Mon May  4 11:07:24 2009] Mapping reads against segment_juncs with Bowtie
[Mon May  4 11:07:24 2009] Mapping reads against segment_juncs with Bowtie
[Mon May  4 11:07:24 2009] Mapping reads against segment_juncs with Bowtie
[Mon May  4 11:07:24 2009] Joining segment hits
[Mon May  4 11:07:24 2009] Reporting output tracks
-----------------------------------------------
Run complete [00:00:00 elapsed]
```

In the directory `tophat_out` should be a file `junctions.bed`. This file should contain a pair of junctions, on the reference sequence "test_chromosome".

在tophat_out目录下可以找到文件junctions.bed。 这就是说明，你的tophat安装正确。

**参考：**

1. http://blog.sina.com.cn/s/blog_751bd9440102v2rr.html
2. http://blog.sina.com.cn/s/blog_8808cae20101amqp.html
3. http://blog.sina.com.cn/s/blog_751bd9440102v6mq.html

## 四. 转录组数据与参考基因组序列比对（无参比对）

### 1. 所用软件

**Trinity**（https://github.com/trinityrnaseq/trinityrnaseq/wiki）

### 2. 安装Trinity

**2.1 下载：**

https://github.com/trinityrnaseq/trinityrnaseq/releases****

**2.2 安装**

将压缩包解压后

```
- cd trinityrnaseq-Trinity-v2.4.0
- make
```

仅需要在安装目录下进行make即可。该命令编译了由C++编写的Inchworm和Chrysalis，而 使用Java编写的Butterfly则不需要编译，可以直接使用。

![1](/home/vickyleexy/Desktop/genesoftware/trinity/1.png)

中间出现了很多警告，但最后用`echo $?` 检查，执行成功。

![2](/home/vickyleexy/Desktop/genesoftware/trinity/2.png)

为能在终端直接使用此软件，将其路径添加到环境变量中。

```
- vim ~/.bashrc
```

```
#Trinity
export PATH=$PATH:/home/vickyleexy/gene/trinityrnaseq-Trinity-v2.4.0
```

![3](/home/vickyleexy/Desktop/genesoftware/trinity/3.png)

然后用`source ~/.bashrc` 刷新一下。

在终端测试一下：成功。

![4](/home/vickyleexy/Desktop/genesoftware/trinity/4.png)

**2.3 使用**

2.3.1 直接运行安装目录下的程序Trinity.pl来使用该软件，不带参数则给出使用帮助。其典型用法为：

```
Trinity.pl --seqType fq --JM 50G --left reads_1.fq  --right reads_2.fq --CPU 6
```

2.3.2 Trinity参数

```
必须的参数：
--seqType     reads的类型：(cfa, cfq, fa, or fq)
--JM          jellyfish使用多少G内存用来进行k-mer的计算，包含‘G’这个字符
--left        左边的reads的文件名
--rigth       右边的reads的文件名
--single      不成对的reads的文件名

可选参数：

Misc：
--SS_lib_type        reads的方向。成对的reads: RF or FR; 不成对的reads: F or R。在数据具有链特异                      性的时候，设置此参数，则正义和反义转录子能得到区分。默认情况下，不设置此参数，                        reads被当作非链特异性处理。FR: 匹配时，read1在5'端上游, 和前导链一致, read2在                      3'下游, 和前导链反向互补. 或者read2在上游, read1在下游反向互补; RF: read1在                      5'端上游, 和前导链反向互补, read2在3'端下游, 和前导链一致;
--output             输出结果文件夹。默认情况下生成trinity_out_dir文件夹并将输出结果保存到此文件夹                      中。
--CPU                使用的CPU线程数，默认为2
--min_contig_length  报告出的最短的contig长度。默认为200
--jaccard_clip       如果两个转录子之间有UTR区重叠，则这两个转录子很有可能在de novo组装的时候被拼接                      成一条序列，称为融合转录子(Fusion Transcript)。如果有fastq格式的paired                          reads，并尽可能减少此类组装错误，则选用此参数。值得说明的是：
                     1. 适合于基因在基因组比较稠密，转录子经常在UTR区域重叠的物种，比如真菌基因组。而                         对于脊椎动物和植物，则不推荐使用此参数; 
                     2. 要求fastq格式的paired reads文件(文件中reads名分别以/1和/2结尾，以利于软件                         识别)，同时还需要安装bowtie软件用于reads的比对; 
                     3. 单独使用具有链特异性的RNA-seq数据的时候，能极大地减少UTR重叠区很小的融合转录                         子; 
                     4. 此选项耗费运算，若没必要，则不用此参数。
--prep               仅仅准备一些文件(利于I/O）并在kmer计算前停止程序运行
--no_cleanup         保留所有的中间输入文件
--full_cleanup       仅保留Trinity fasta文件，并重命名成${output_dir}.
Trinity.fasta
--cite               显示Trinity文献引证和一些参与的软件工具
--version            报告Trinity版本并推出

Inchworm 和 K-mer 计算相关选项：
--min_kmer_cov      使用Inchworm来计算K-mer数量时候，设置的Kmer的最小值。默认为1
--inchworm_cpu      Inchworm使用的CPU线程数，默认为6和--CPU设置的值中的小值。

Chrysalis相关选项：
--max_reads_per_graph   在一个Bruijn图中锚定的最大的reads数目，默认为200000
--no_run_chrysalis      运行Inchworm完毕，在运行chrysalis之前停止运行Trinity
--no_run_quantifygraph  在平行化运算quantifygrahp前停止运行Trinity

Butterfly相关选项：
--bfly_opts                    Butterfly额外的参数
--max_number_of_paths_per_node 从node A -> B,最多允许多少条路径。默认为10
--group_pairs_distance         最大插入片读长度，默认为500
--path_reinforcement_distance  延长转录子路径时候，reads间最小的重叠碱基数。默认PE:75; SE：25
--no_triplet_lock              不锁定triplet-supported nodes
--bflyHeapSpaceMax             运行Butterfly时java最大的堆积空间，默认为20G
--bflyHeapSpaceInit            java初始的堆积空间，默认为1G
--bflyGCThreads                java进行无用信息的整理时使用的线程数，默认由java来决定
--bflyCPU                      运行Butterfly时使用的CPU线程数，默认为2
--bflyCalculateCPU             计算Butterfly所运行的CPU线程数，由公式 80% * max_memory /                                      maxbflyHeapSpaceMax 得到
--no_run_butterfly             在Chrysalis运行完毕后，停止运行Butterfly

Grid-computing选项：
--grid_computing_module        选定Perl模块，在/Users/bhaas/SVN/trinityrnaseq/trunk                                        /PerlLibAdaptors/。
```

具体用法可详见参考链接1.

参考：

1. http://www.chenlianfu.com/?p=688

## 五. 组装

### 1. 所用软件

**cufflinks**（http://cole-trapnell-lab.github.io/cufflinks/）

### 2. 安装cufflinks

**2.1 下载编译好的版本：**cufflinks-2.2.1.Linux_x86_64.tar.gz

http://cole-trapnell-lab.github.io/cufflinks/install/

**2.2 安装**

```
- cd cufflinks-2.2.1.Linux_x86_64
- sudo cp cufflinks cuffdiff cuffcompare  /usr/local/bin
```

![1](/home/vickyleexy/Desktop/genesoftware/cufflinks/1.png)

check：

```
- cufflinks -v
```

![2](/home/vickyleexy/Desktop/genesoftware/cufflinks/2.png)

参考：

1. http://blog.sina.com.cn/s/blog_751bd9440102v4lq.html
2. 使用参考：http://blog.sina.com.cn/s/blog_1319a10ee0102vf49.html
3. 使用参考：http://blog.sciencenet.cn/home.php?mod=space&uid=635619&do=blog&id=884213




其他转录组参考链接：

1. http://blog.sciencenet.cn/home.php?mod=space&uid=635619&do=blog&id=884213
2. http://blog.sciencenet.cn/blog-1509670-848266.html
3. https://www.plob.org/article/9756.html

