# Conda install R with MKL



## 一、install conda（如先前已安装请忽略）

```sh
# install conda（如先前已安装请忽略）
#网站教程：https://docs.conda.io/projects/miniconda/en/latest/index.html#quick-command-line-install
mkdir -p ~/miniconda3
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda3/miniconda.sh
bash ~/miniconda3/miniconda.sh -b -u -p ~/miniconda3
rm -rf ~/miniconda3/miniconda.sh
#After installing, initialize your newly-installed Miniconda. The following commands initialize for bash and zsh shells:（必须！！）
~/miniconda3/bin/conda init bash
~/miniconda3/bin/conda init zsh
```

## 二、添加镜像源

```sh
#添加以下三个镜像源（必须）
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
conda config --show-sources #查看镜像源
```

## 三、#创建R环境

```sh
#创建R环境，链接mkl的blas库
conda create -n Rmkl r-base=4.3.1 "libblas=*=*mkl*" -y
```

## 四、取消直接进入base环境（仅设置一次即可）

```sh
#根据自己需要，新安装conda后，用户前会出现base记号，如(base) [yliao@mu01]$，即自动进入base环境，如果需要依赖conda base环境下的bin和lib，可忽略
#切记是在终端输入以下命令，而不是加载进环境变量！
conda config --set auto_activate_base false
```

## 五、将conda加入环境变量

```sh
#通过vim ~/.bashrc加入.bashrc或.bash_profile中
export PATH="/share/home/yzwl_liaoy/miniconda3/bin:$PATH"
#保存退出后记得source
source .bashrc
```

## 六、检查mkl是否链接成功

```sh
#4 如需查看R是否成功链接上mkl的blas库，使用以下方法
conda activate Rmkl #进入conda的Rmkl环境
R #进入R环境
> sessionInfo() #输入这个函数，如果BLAS出现libmkl_rt.so.2即代表链接成功，如果不是请从第一步开始安装
R version 4.3.2 (2023-10-31)
Platform: x86_64-conda-linux-gnu (64-bit)
Running under: CentOS Linux 7 (Core)

Matrix products: default
BLAS/LAPACK: /share/home/yzwl_liaoy/miniconda3/envs/Rmkl/lib/libmkl_rt.so.2;  LAPACK version 3.10.1

locale:
 [1] LC_CTYPE=en_US.UTF-8       LC_NUMERIC=C              
 [3] LC_TIME=en_US.UTF-8        LC_COLLATE=en_US.UTF-8    
 [5] LC_MONETARY=en_US.UTF-8    LC_MESSAGES=en_US.UTF-8   
 [7] LC_PAPER=en_US.UTF-8       LC_NAME=C                 
 [9] LC_ADDRESS=C               LC_TELEPHONE=C            
[11] LC_MEASUREMENT=en_US.UTF-8 LC_IDENTIFICATION=C       

time zone: Asia/Shanghai
tzcode source: system (glibc)

attached base packages:
[1] stats     graphics  grDevices utils     datasets  methods   base     

loaded via a namespace (and not attached):
[1] compiler_4.3.2


```

## 七、其他说明

```sh
#1 conda相关命令可从网上查询

#2 请将环境变量中的R，或[alias R=]注释或删除掉，以免影响以上安装好的Rmkl环境中R的调用

#3 如果以提交任务方式，须在任务脚本中加入以下命令激活创建conda的R环境，否则将报错
source activate Rmkl

```

