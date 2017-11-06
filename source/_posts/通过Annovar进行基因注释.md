# 通过Annovar进行基因注释

## 1. Annovar软件的安装  

### 1.1 下载

通过学校邮箱注册，免费下载。地址：http://www.openbioinformatics.org/annovar/annovar_download_form.php

![1](/home/vickyleexy/Desktop/genesoftware/annovar/1.png)

之后在邮箱里会收到下载链接，下载即可。

### 1.2 安装

通过以下命令进行解压即可：

```
tar xvfz annovar.latest.tar.gz
```

为更加方便使用，将此路径添加到环境变量中，即可直接使用`annotate_variation.pl`

，来代替`perl annotate_variation.pl`进行操作。

环境变量添加如下：

```
vim ~/.bashrc 
```

在文件中添加：

``` 
#annovar
export PATH=$PATH:/home/vickyleexy/gene/annovar
```

保存退出，运行以下命令使其立即生效：

```
source ~/.bashrc
```

## 2. 对人类基因组进行注释

### 2.1 下载数据库

```
annotate_variation.pl -buildver hg19 -downdb -webfrom annovar refGene humandb/
annotate_variation.pl -buildver hg19 -downdb cytoBand humandb/
annotate_variation.pl -buildver hg19 -downdb -webfrom annovar exac03 humandb/
annotate_variation.pl -buildver hg19 -downdb -webfrom annovar avsnp147 humandb/
annotate_variation.pl -buildver hg19 -downdb -webfrom annovar dbnsfp30a humandb/

```

将数据库下载到human文件夹下。

### 2.2 注释

运行`table_annovar.pl`进行注释：

```
table_annovar.pl example/ex1.avinput humandb/ -buildver hg19 -out myanno -remove -protocol refGene,cytoBand,exac03,avsnp147,dbnsfp30a -operation gx,r,f,f,f -nastring . -csvout -polish -xref example/gene_xref.txt
```

基于区域的基因组注释：

```
annotate_variation.pl -regionanno -build hg19 -out sel -dbtype phastConsElements46way base_alt_sel.4070.avinput /home/vickyleexy/humandb
```

![１３](/home/vickyleexy/Desktop/genesoftware/annovar/１３.png)

```
annotate_variation.pl -regionanno -dbtype gff3 -gff3dbfile GCF_000951615.1_common_carp_genome_genomic.gff base_alt_sel.4070.avinput /home/vickyleexy/common_carpdb/
```

![１６](/home/vickyleexy/Desktop/genesoftware/annovar/１６.png)

## 3. 对Annovar中已有的源的物种进行注释

### 3.1 下载物种基因组注释信息



## 4. 对Annovar源中未有的物种进行注释

如果Annovar源中没有现成可用的gene definition file，可以从NCBI、UCSC、Ensemble等网站下载基因组数据，并将GFF3 或GTF 文件转换成 gene definition file。

**以common carp基因组信息为例（ps：注释的是top50PI和前1%的fst的位点）：**

### 4.1 利用下载的.fna和.gff格式的基因组数据

#### 4.1.1 下载common carp 基因组和注释信息

![2](/home/vickyleexy/Desktop/genesoftware/annovar/2.png)

#### 4.1.2 解压文件

```
mkdir common_carpdb                                            #创建新文件夹
gunzip GCF_000951615.1_common_carp_genome_genomic.fna.gz       #解压
gunzip GCF_000951615.1_common_carp_genome_genomic.gff.gz       #解压
```

#### 4.1.3 转换格式 

下载`gff3ToGenePred`或`gtfToGenePred`工具，链接：http://hgdownload.soe.ucsc.edu/admin/exe/linux.x86_64/，推荐使用GTF格式，因为有些GFF3格式文件转换可能不正确，但这里没有GTF格式所以用gff格式。

`gff3ToGenePred`工具说明：

![７](/home/vickyleexy/Desktop/genesoftware/annovar/７.png)

![８](/home/vickyleexy/Desktop/genesoftware/annovar/８.png)

用以下命令进行转换：

```
/home/vickyleexy/gene/gff3ToGenePred -useName GCF_000951615.1_common_carp_genome_genomic.gff ComCarp_refGene.txt
```

![３](/home/vickyleexy/Desktop/genesoftware/annovar/３.png)

生成了`ComCarp_refGene.txt`文件。

若不加参数`-useName`，则最后得到的注释信息显示的是基因编号，而不是基因名称：

```
./gff3ToGenePred /home/vickyleexy/common_carpdb/GCF_000951615.1_common_carp_genome_genomic.gff /home/vickyleexy/common_carpdb/ComCarp_refGene.txt
```

![１５](/home/vickyleexy/Desktop/genesoftware/annovar/１５.png)

#### 4.1.4 用retrieve_seq_from_fasta.pl生成 transcript FASTA file     

```
retrieve_seq_from_fasta.pl -format refGene -seqfile GCF_000951615.1_common_carp_genome_genomic.fna ComCarp_refGene.txt -outfile ComCarp_refGeneMrna.fa
```

![４](/home/vickyleexy/Desktop/genesoftware/annovar/４.png)

![５](/home/vickyleexy/Desktop/genesoftware/annovar/５.png)

生成了`ComCarp_refGeneMrna.fa`文件。

#### 4.1.5 对筛选出来的sel组和con组位点进行注释

**对sel组位点数据文件`base_alt_sel.vcf`进行注释：**

```
table_annovar.pl base_alt_sel.vcf /home/vickyleexy/common_carpdb/ --vcfinput --outfile sel --buildver ComCarp --protocol refGene --operation g
```

![９](/home/vickyleexy/Desktop/genesoftware/annovar/９.png)

生成以下文件：

![１１](/home/vickyleexy/Desktop/genesoftware/annovar/１１.png)

**对con组位点数据文件`base_alt_con.vcf`进行注释：**

```
table_annovar.pl base_alt_con.vcf /home/vickyleexy/common_carpdb/ --vcfinput --outfile con --buildver ComCarp --protocol refGene --operation g
```

![１０](/home/vickyleexy/Desktop/genesoftware/annovar/１０.png)

生成以下文件：

![１２](/home/vickyleexy/Desktop/genesoftware/annovar/１２.png)

#### 4.1.6 对前1%的fst中的位点进行注释

将.vcf格式的文件转换为annovar的输入格式.avinput：

```
convert2annovar.pl -format vcf4 base_alt_filter_001.vcf > base_alt_filter_001.vcf.avinput
```

![１７](/home/vickyleexy/Desktop/genesoftware/annovar/１７.png)

对上面生成的.avinput文件进行注释：

```
annotate_variation.pl -out fst -build ComCarp base_alt_filter_001.vcf.avinput /home/vickyleexy/common_carpdb/
```

![１８](/home/vickyleexy/Desktop/genesoftware/annovar/１８.png)

### 4.2 利用下载的.fa和.gtf格式的基因组数据

![６](/home/vickyleexy/Desktop/genesoftware/annovar/６.png)



参考文档地址：

1. http://doc-openbio.readthedocs.io/projects/annovar/en/latest/
2. http://blog.csdn.net/u013816205/article/details/51262289

http://blog.csdn.net/u013816205/article/details/51262289

http://www.jianshu.com/p/48b5a0972301

http://www.bio-info-trainee.com/641.html

http://www.dxy.cn/bbs/topic/33206128?source=rss

http://annovar.openbioinformatics.org/en/latest/user-guide/startup/

http://blog.sina.com.cn/s/blog_83f77c940102w9rs.html

http://www.dxy.cn/bbs/topic/36491526?from=recommend

https://zhengzexin.com/2016/04/28/annovar-zhu-shi-ruan-jian/#toc_10

https://zhuanlan.zhihu.com/p/28126314

https://www.ncbi.nlm.nih.gov/gquery/?term=common+carp