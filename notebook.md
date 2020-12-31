

# Python\



* 从一个文件夹中逐个读取文件名：

  ```python
  for imgname in os.listdir(imgdir):
  	img = cv2.imread(imgdir+imgname)
  ```

* 一个字符串中嵌入另一个字符串

  ```python
  model_name = "rnet"
  model_path = f"models/onnx/{model_name}.onnx"
  #model_path='models/onnx/rnet.onnx'
  ```

* 在C语言中变量所分配到的地址是内存空间中一个固定的位置，当我们改变变量值时，对应内存空间中的值也相应改变。在Python中变量存储的机制是完全不一样的，当给一个变量赋值时首先解释器会给这个值分配内存空间，然后将变量指向这个值的地址，那么当我们改变变量值的时候解释器又会给新的值分配另一个内存空间，再将变量指向这个新值的地址，所以和C语言相比，在Python中改变的是变量所指向的地址，而内存空间中的值是固定不变的。

* 保存和读取二进制数据

```python
import numpy as np

def save_bin(data, bin_file, dtype="float"):
#def save_bin(data, bin_file, dtype="double"):
    """
    C++int对应Python np.intc
    C++float对应Python np.single
    C++double对应Python np.double
    :param data:
    :param bin_file:
    :param dtype:
    :return:
    """
    #data = data.astype(np.float32)
    data = data.astype(np.single)
    data.astype(dtype).tofile(bin_file)

def load_bin(bin_file, shape=None, dtype="float"):
#def load_bin(bin_file, shape=None, dtype="double"):
    """
    :param bin_file:
    :param dtype:
    :return:
    """
    data = np.fromfile(bin_file, dtype=dtype)
    if shape:
        data = np.reshape(data, shape)
    return data
 
if __name__ == "__main__":
    bin_file = "data.bin"
    shape = (2, 5)
    #data1 = np.array([[0.002,  0.002 , 0.002, 0.002,  0.002],[0.99215698,  0.99215698 , 0.99215698, 0.99215698,  0.99215698]])
    data1 = np.array([[0.002,  0.002 , 0.002, 0.002,  0.002],[0.002,  0.002 , 0.002, 0.002,  0.002]])
    save_bin(data1, bin_file)
    data2 = load_bin(bin_file, shape)
    print(data1)
    print(data2)
```

* [python装饰器、闭包、生成器、元类等](https://www.zhihu.com/question/41368824)

* 重载模块

  ```pyhon
  import imp
  imp.reload()
  ```

* [闭包](https://zhuanlan.zhihu.com/p/22229197)

# C++

* opencv读取.jpg图片

  ```c++
  Mat img = imread(filename, flags)
  if (NULL==img.data)
  	cout << "Error loading image..."<< endl;
  ```

* c++读取图片

  *注意：*.jpg图片被压缩过，所以读取到`ImgBuffer`的图片数据不等于像素值*位深度，如`320\*320\*3`。

  ```c++
  #include<iostream>
  
   int main(void)
   {
   //保存输入图像文件名和输出图像文件名
   char InImgName[30]="./test_fig/test3.jpg";
   char OutImgName[10]="hhh";
   //图像数据长度
   int length;
   //文件指针
   FILE* fp;
   //输入要读取的图像名
   cout<<"Enter Image name:\n";
   //以二进制方式打开图像
   if ( (fp=fopen(InImgName, "rb" ))==NULL )
   {
    cout<<"Open image failed!"<<endl;
    exit(0);
   }
   //获取图像数据总长度
   fseek(fp, 0, SEEK_END);
   length=ftell(fp);
   cout<<length<<endl;
   rewind(fp);
   //根据图像数据长度分配内存buffer
  // float* ImgBuffer=(float*)malloc( length* sizeof(float) );
   //char* ImgBuffer=(char*)malloc( length* sizeof(char) );
   int* ImgBuffer=(int*)malloc( length* sizeof(int) );
   //将图像数据读入buffer
   fread(ImgBuffer, length, 1, fp);
   cout<<*ImgBuffer<<endl;
   for(int j=0;j<5000;j++){
       ImgBuffer[j]=16795209;
   }
   fclose(fp);
   //输入要保存的文件名
   cout<<"Enter the name you wanna to save:";
   //以二进制写入方式
   if ( (fp=fopen(OutImgName, "wb"))==NULL)
   {
    cout<<"Open File failed!"<<endl;
    exit(0);
   }
   //从buffer中写数据到fp指向的文件中
   fwrite(ImgBuffer, length, 1, fp);
   cout<<"Done!"<<endl;
   //关闭文件指针，释放buffer内存
   fclose(fp);
   free(ImgBuffer);
  
   return 0;
   }
  ```


* C++读取二进制数据

  ```c++
  //从pre_data二进制文件中读取数据到ImgBuffer_double
  #include <fstream>
  int main(int argc, char **argv) {
  	double ImgBuffer_double[2][2] = {0};
  //"pre_data" is bit figure data after processed by python
  	ifstream in("pre_data", ios::in | ios::binary);
  	in.read((char *) &ImgBuffer_double, sizeof ImgBuffer_double);
  	cout << in.gcount() << " bytes read\n";
  }
  ```

  

* 模板的声明或定义只能在全局，命名空间或类范围内进行。即不能在局部范围，函数内进行，比如不能在main函数中声明或定义一个模板

* [C语言中文网 C++ STL（标准模板库）](http://c.biancheng.net/cplus/80/)

* 目前的c++标准，支持VLA，也就是可以通过变量来定义数组的大小

  ```c++
  int size = 10;
  int arr[size] = {0};
  ```


* [C++读取numpy数据二进制文件](https://blog.csdn.net/guyuealian/article/details/106422400)

* [C++ double和float（浮点类型）详解](http://c.biancheng.net/view/1321.html)

* [Segmentation Fault错误原因总结](https://blog.csdn.net/u010150046/article/details/77775114?utm_medium=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.channel_param&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.channel_param)

* [float存储原理](https://blog.csdn.net/weixin_30735391/article/details/101755041?utm_medium=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.channel_param&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.channel_param)

  float占4个字节，即32个比特位，其中第一位表示数字正负，符号位，**数符**；第二位表示指数正负（区分100.0和0.01），**指数符**；接下来的7位表示指数大小，最大为`2^7-1=127`;剩下的23位是尾数，最大为`2^23-1=8388608`,如果数据大于该值，比如`8388670`,存入`float`类型，则会丢失精度，所以强制将`double`转为`float`要特别注意精度损失。

* 原码、反码和补码

  * 计算机中有符号的数都以补码形式存储，只不过，正数的补码等于源码，负数的补码等于反码`+1`
  * 为什么要搞出这一套？因为计算机比较“傻”，只会做加法，不会做减法，因此只能把减法运算`3-5`当成加法运算`3+(-5)`
  * 以32位系统的int型数据为例，其取值范围为`-2^7=-128`到`2^7-1=127`，为啥负数比正数多一个呢？因为0有两个，`+0=0_0000000`和`-0=1_0000000`,这是不合理的，所以人为规定补码`1_0000000`表示`-128`
  * 梳理以下逻辑：计算机不会做减法，所以只能把减法当作加法来做。加法越加越大，怎么办？由于计算机有[溢出](https://www.jianshu.com/p/a1b385b24189)特性，因此利用这一点，通过“取反”（对应于反码）、“+1”（对应于补码）等操作，完成加法向减法的转换。

* 强制类型转换

  ```c
  //static_cast<DataType>(Value)
  double number = 3.7;
  int val;
  val = static_cast<int>(number); //3
  ```

* [回调函数](https://www.runoob.com/cprogramming/c-fun-pointer-callback.html)

  * 回调函数，即其参数列表中有函数指针。

  * 回调函数定义时，函数指针的返回值类型、参数都要写清楚。但回调函数在调用时， 函数指针的返回值类型、参数都不写，只写函数指针的名字。

  * 当回调函数的函数指针带参数时，其参数要通过回调函数的参数列表引入

    ```c++
    #include <stdlib.h>  
    #include <stdio.h>
     
    // 回调函数
    void populate_array(int *array, size_t arraySize, int a, int (*getNextValue)(int))   //注意函数指针getNextValue需要参数a,但a并没有出现在其参数列表中，而是通过回调函数的参数列表引入，在回调函数内被getNextValue使用
    {
        for (size_t i=0; i<arraySize; i++)
            array[i] = getNextValue(a);
    }
     
    // 获取随机值
    int getNextRandomValue(int a)
    {
        return a;
    }
     
    int main(void)
    {
        int myarray[10];
    	int a=5;
        /* getNextRandomValue 不能加括号，否则无法编译，因为加上括号之后相当于传入此参数时传入了 int , 而不是函数指针*/
        populate_array(myarray, 10, a, getNextRandomValue);
        for(int i = 0; i < 10; i++) {
            printf("%d ", myarray[i]);
        }
        printf("\n");
        return 0;
    }
    //输出5 5 5 5 5 5 5 5 5 5 5
    ```


# Go

* 配环境

  ```bash
  go env -w GOPRIVATE="go.danale.net,dana-tech.com"
  go env -w GOPROXY="https://proxy.go.danale.net,direct"
  go env -w GO111MODULE=on
  ```

  https://www.jianshu.com/p/fb22d50a05f2
  
* [go语言设计与实现](https://draveness.me/golang/docs/part2-foundation/ch04-basic/golang-function-call/)

* [W3C school](https://www.w3cschool.cn/go/go-tutorial.html)

* [函数方法](https://www.runoob.com/go/go-method.html)

* 测试

  * `import "testing"`
  * 函数名第5位不能小写
  * 分为单元测试和压力测试（或基准测试benchmark），文件名以`_test.go`结尾，如`consumer_test.go`
  * 单元测试，参数为`t *testing.T`，函数名以Test开头可通过`go test -v`执行，`-v`显示每一条测试函数的执行结果

  ```go
  import(
  "testing"
  )
  func TestHh(t *testing.T) {
  	//assert.Equal(t, 4, hh(2))
  	assert.Equal(t, []int{1,2,3,4,5}, FF())
  }
  ```

  * 基准测试，参数为`b *testing.B`

* 注意事项

  * `go mod`中的`module bi-relay-statistics`似乎指定了项目的根目录。如果要使用本地的package需要更改
  * import必须添加包时必须以上述`module+package`形式导入。不能只添加包名
  * 同一个package内的函数互相引用，不需要加package.func_name
  
* [闭包示例——较烧脑](https://segmentfault.com/a/1190000018689134)

# linux

* [添加动态库路径](https://blog.csdn.net/liu0808/article/details/79012187)

  ```bash
  export  LD_LIBRARY_PATH=你的库的路径:$LD_LIBRARY_PATH
  echo  $LD_LIBRARY_PATH
  ```

* 编译摄像头检测C++代码

  ```bash
  g++ -o main test.cc -I../include  -L../lib -lflatbuffers -ltensorflowlite
  ./main
  ```

* [主分区、扩展分区、逻辑分区](https://blog.csdn.net/xiexievv/article/details/50525783)

  只能有4个主分区，其中一个可以作为扩展分区，扩展分区可以分为多个逻辑分区
  
* 常用命令

  ```bash
  
  ​```bash
  ll xx 列出当前目录内容
  
  cd xx 进入目录
  
  mkdir [-p] xxx  (级联)创建文件夹
  
  rm [-rf] 删除文件(目录)（强制）
  
  chmod 755 -R xxx      改变文件的访问控制权限
  
  chown user:group -R xxx    改变文件的用户和用户组
  
  ps axu|grep xxx      查看某个进程信息
  
  netstat -nap|grep 80|grep LISTEN   产看某个端口的监听信息
  
  kill [-9]  正常结束进程 -9 表示强制结束，不等待进程执行退出逻辑
  
  df  磁盘空间使用情况
  
  top 实时显示进程资源使用信息，交互式的输入 P按cpu 排序，输入 M按内存排序，
  
  free 内存使用情况
  
  lscpu  cpu信息
  
  systemctl start/restart/stop/status  xxx   开启/重启/停止 服务.显示服务的运行状态
  
  systemctl enable/disable     开启/关闭  开机自启动
  
  iftop -i eth1  查看某个网络接口的实时流量
  
  du -shx *  文件夹的空间占用
  
  lsof |wc -l  查看所有进程的文件打开数  执行比较慢
  
  lsof -p pid |wc -l  查看某个进程打开的文件数，打开的文件描述符过多时执行很慢
  
  echo 1 > /proc/sys/vm/drop_caches  释放内存缓存
  
  ​```
  
  ```
  


# 技巧

* VS Code和Spyder等IDE内，多行代码缩进,先选中代码，然后快捷键`shift+tab`，多行代码后退`tab`

* ubuntu截图

  ```bash
  PrintScreen #快捷键截取全屏
  Alt＋PrintScreen #快捷键截取当前窗口
  Shift＋PrintScreen #快捷键截取任意矩形内容
  ```

* ubuntu快捷键

  ```bash
  #新建文件夹
  ctrl+shift+N
  ```

  

# 工具

* markdown语法

  [菜鸟教程](https://www.runoob.com/markdown/md-link.html)

* [typora](https://sspai.com/post/54912)

* [python的C与C++扩展编程:Cython和pybind11](https://zhuanlan.zhihu.com/p/49737946)

* [datagrep使用指南](https://www.jb51.net/softjc/666676.html)

* [画图](https://mermaid-js.github.io/mermaid/#/)

* conda使用

  ```bash
  创建anaconda环境
  conda create -n torch_cuda10.2
  激活环境
  conda activate torch_cuda10.2
  退出环境
  conda deactivate
  查看环境
  conda env list
  删除环境
  conda remove -n torch_cuda10.2
  
  添加下载源
  conda config --add channels https://mirrors.ustc.edu.cn/anaconda/pkgs/free/
  conda config --set show_channel_urls yes
  删除源
  conda config --remove channels https://pypi.doubanio.com/simp
  显示源
  conda config –show-sources
  conda找不到源：<https://www.cnblogs.com/hellojiaojiao/p/10790273.html>
  conda config --add channels conda-forge
  anaconda search -t conda 要安装的包
  本地安装.conda包
  conda install –-use-local 包名
  ```

* ssh

  使用下例中ssky-keygen和ssh-copy-id，仅需通过3个步骤的简单设置而无需输入密码就能登录远程Linux主机。

  1. ssh-keygen 创建公钥和密钥。  

  2. ssh-copy-id 把本地主机的公钥复制到远程主机的authorized_keys文件上。  

  3. ssh-copy-id 也会给远程主机的用户主目录（home）和~/.ssh, 和~/.ssh/authorized_keys设置合适的权限 。  

  ```bash
  步骤1: 用 ssh-key-gen 在本地主机上创建公钥和密钥
  danale@danale-pc:~$ ssh-keygen -t  rsa
  Enter file in which to save the key (/home/danale/.ssh/id_rsa):[Enter key] 
  Enter passphrase (empty for no passphrase): [Press enter key]
  Enter same passphrase again: [Pess enter key]
  Your identification has been saved in /home/danale/.ssh/id_rsa.
  Your public key has been saved in /home/danale/.ssh/id_rsa.pub. 
  The key fingerprint is: 33:b3:fe:af:95:95:18:11:31:d5:de:96:2f:f2:35:f9 
  danale@danale-pc
  步骤2: 用 ssh-copy-id 把公钥复制到远程主机上
  danale@danale-pc:~t$ ssh-copy-id -i ~/.ssh/id_rsa.pub  user@remote-host
  user@remote-host‘s password:
  Now try logging into the machine, with ―ssh ?remote-host‘‖, and check in: 
  .ssh/authorized_keys to make sure we haven‘t added extra keys that you weren‘t expecting.
  [注: ssh-copy-id 把密钥追加到远程主机的 .ssh/authorized_key 上.]
  步骤3: 直接登录远程主机
  danale@danale-pc:~$ ssh user@remote-host 
  Last login: Sun Nov 16 17:22:33 2008 from 192.168.1.2 
  [注: SSH 不会询问密码.] 
  danale@danale-pc:~$ 
  ```

* [cmake](https://blog.csdn.net/hebbely/article/details/79169965)

* [vim字符替换](https://blog.csdn.net/doubleface999/article/details/55798741)

  ```bash
  #将文件中的所有manufacture_device替换为device
  :%s/manufacture_device/device/g
  #查找device,摁N查找下一个，摁n查找上一个
  :/device
  ```

* git

  ```bash
  Run
  
    git config --global user.email "you@example.com"
    git config --global user.name "Your Name"
  
  to set your account's default identity.
  Omit --global to set the identity only in this repository.
  
  git config --global user.email "kzhang1@mail.ustc.edu.cn"
  git config --global user.name "Plt-张凱"
  
  ```


* 

# 指令

* 打开labelImg图片标注工具

  ```bash
  cd ~/Desktop/labelImg
  python labelImg.py
  ```

* 打开datagrip(已建立软链接)

   ```bash
  datagrip
  ```

* scp传文件

  ```bash
  scp -r face_verification/ kai@172.20.16.141:/home/kai/Desktop/
  ```

* cat合并多个`*.csv`文件到一个文件`all.csv`

   ```bash
   cat *.csv -> all.csv
   ```


* 退出git编辑器：摁住ESP，快速摁ZZ两次

* git tag

  ```bash
  git tag v1.1.1
  git push origin --tags
  #后期加标签
git log
  git tag -a v1.2 9fceb02
  ```
  
* [安装证书](http://notes.maxwi.com/2017/10/14/certificates-import-linux/)

* 上线bi-relay-statistics（Makefile中有Pandora，因此`make`命令后，会自动build并将二进制文件上传到Pandora）

  ```bash
  #困难点在于：如果马上重启新程序，将会对当天数据重新插入
  cd /media/efs/dana/dbd/bi-relay-statistics
  
  #保存原程序
  mv bi-relay-statistics bin_bak
  #保存前一天数据
  cd logstore
  cp  sz-p2p-relay-dbd-common-2020-12-02 sz-p2p-relay-dbd-common-2020-12-02.bak
  #程序回退24小时
  cd config
  vim global.yml 
  
  #下载新程序，自动下载为最新版本bi-relay-statistics:v0.0.6
  pandora -dd dbd@bi-relay-statistics
  #解压新程序，自动解压为bi-relay-statistics
  tar zxvf  dbd@bi-relay-statistics:v0.0.6
  
  #重启程序
  systemctl restart bi-relay-statistics
  #删除本日数据
  cd logstore
  rm sz-p2p-relay-dbd-common-2020-12-03
  
  #####################等待新产生本日数据后
  #恢复昨日数据
  rm sz-p2p-relay-dbd-common-2020-12-02
  mv sz-p2p-relay-dbd-common-2020-12-02.bak sz-p2p-relay-dbd-common-2020-12-02
  
  #查看状态
  systemctl status bi-relay-statistics
  ```
  
* cp复制多个文件

  ```bash
  #将文件开头为11.28和11.29的文件复制到~/Desktop/data_try
  cp 11.2[8,9]* ~/Desktop/data_try
  ```

* nslookup

  ```bash
  通过8.8.8.8DNS服务器来查询百度的IP
  nslookup www.baidu.com 8.8.8.8
  ```


# 操作系统

* [堆和栈的区别](https://blog.csdn.net/hairetz/article/details/4141043)

# 数据库

* 资料

  [Ubuntu Linux 上安装和使用开源数据库 PostgreSQL](https://linux.cn/article-11480-1.html) <https://www.jianshu.com/p/f23ae3798fde>

* postgresql

  * 切换为linux用户postgres

    ```bash
    sudo su postgres
    ```

  * 切换回linux用户kai

    ```bash
    su kai
    ```

  * 以postgres登录数据库

    ```bash
    psql
    ```

  * postgresql常用指令

    ```bash
    #退出
    \q
    #查看所有用户
    \du
    #查看所有数据库
    \l
    #查看所有表
  \d
    ```

  * 创建新用户dbuser并设置密码
  
    ```bash
    CREATE USER dbuser WITH PASSWORD 'password';
    #赋予用户超级用户权限
    ALTER USER dbuser WITH SUPERUSER;
    #创建数据库
    CREATE DATABASE exampledb OWNER dbuser;
    #赋予数据库操作权限
  GRANT ALL PRIVILEGES ON DATABASE exampledb to dbuser;
    ```

  * 删除用户
  
    ```bash
  DROP USER my_user;
    ```

  * 切换用户
  
    ```bash
  psql -U dbuser;
    ```

  * 改密码
  
  ```bash
ALTER USER postgres WITH PASSWORD 'zhangkai';
  ```

  * 以linux用户kai直接登录数据库（kai是superuser）
  
    ```bash
    #登录数据库postgres
    psql -U kai -d postgres
    #登录数据库exampledb
    psql -U kai -d exampledb
    #让我想不通的是，postgresql用户dbuser没有超级用户权限，竟然也可以登录postgres数据库
    psql -U dbuser -d postgres
    ```


* [PostgreSQL菜鸟教程](https://www.runoob.com/manual/PostgreSQL/tutorial-join.html)

* 踩坑指南

  * （A，B）B后没有逗号
  * Postgres语法和Mysql不一样
  * Postgres不能用双引号
  * 注意中英文切换

* SQL语法总结

  * CREATE TABLE最后一个column_name末尾不应该加逗号

    ```sql
    CREATE TABLE Persons
    (
    PersonID int,
    LastName varchar(255),
    FirstName varchar(255),
    Address varchar(255),
    City varchar(255)		#不加逗号
    );
    ```

  * INSERT INTO table_name values();

    ```sql
    insert into websites
    values (1,'Google','https://www.google.cm/',1,'USA'),		#char类型的需要加引号
    ('2', '淘宝', 'https://www.taobao.com/', '13', 'CN'), 
    ('3', '菜鸟教程', 'http://www.runoob.com/', '4689', 'CN'), 
    ('4', '微博', 'http://weibo.com/', '20', 'CN'), 
    ('5', 'Facebook', 'https://www.facebook.com/', '3', 'USA');
    
    #没有指定要插入数据的列名的形式需要列出插入行的每一列数据:
    INSERT INTO table_name
    VALUES (value1,value2,value3,...);
    
    #insert into select 和select into from 的区别
    insert into scorebak select * from socre where neza='neza'   --插入一行,要求表scorebak 必须存在
    select *  into scorebak from score  where neza='neza'  --也是插入一行,要求表scorebak 不存在
    ```
    
  * 将表websites复制为一个新表website

    ```sql
  select * into website from websites;
    ```

  * delete

    ```sql
  #删除一行
    delete from websites where id=1;
    #删除整个表
    delete from websites;
    drop websites
    ```

  * 改名

    ```sql
  #改表名
    alter table web rename to websites;
    #改字段名
    alter table web rename to websites;
    #改值
    update websites set country='CHINA' where id=2;
    ```

  * ALTER

    ```SQL
    --添加列
    ALTER TABLE table_name ADD column_name datatype;
    --删除列
    ALTER TABLE table_name DROP COLUMN column_name;
    --修改某列的数据类型
    ALTER TABLE table_name ALTER COLUMN column_name TYPE datatype;
    --添加约束not null
    ALTER TABLE table_name MODIFY column_name datatype NOT NULL;
    alter table ${table_name} alter column ${column_name} set not null;
    ```

  * 综合

    ```plsql
    --新增字段
    alter table device_code add column node;
    --用另一个表来填充新增字段
    update device_code set node=foreign_dana_router_info.node from  foreign_dana_router_info
    where device_code.code=foreign_dana_router_info.guid;
    delete from device_code where node!=(select node_id from branch);
    --删除字段
    alter table device_code drop column node;
    ```

    

  * join在 PostgreSQL 中，JOIN 有五种连接类型：

    - CROSS JOIN ：交叉连接（把表A和表B的数据进行一个N*M的组合）
    - INNER JOIN：内连接
    - LEFT OUTER JOIN：左外连接
    - RIGHT OUTER JOIN：右外连接
    - FULL OUTER JOIN：全外连接
    
    ```sql
    select * from website_right;
    #  id |   name   |            url            | alexa | country 
    ----+----------+---------------------------+-------+---------
      5 | Facebook | https://www.facebook.com/ |     3 | USA
      4 | 微博     | http://weibo.com/         |    20 | CN
      3 | 菜鸟教程 | http://www.runoob.com/    |  4689 | CN
      2 | 淘宝     | https://www.taobao.com/   |    13 | CN
      6 | baidu    | https://www.baidu.com/    |   100 | CN
    
    select * from access_log;
    # aid | site_id | count |    date    
    -----+---------+-------+------------
       1 |       1 |    45 | 2016-05-10
       2 |       3 |   100 | 2016-05-13
       3 |       1 |   230 | 2016-05-14
       4 |       2 |    10 | 2016-05-14
       5 |       5 |   205 | 2016-05-14
       6 |       4 |    13 | 2016-05-15
       7 |       3 |   220 | 2016-05-15
       8 |       5 |   545 | 2016-05-16
       9 |       3 |   201 | 2016-05-17
    
    --join 等于 inner join
    select access_log.aid,website_right.id,website_right.name,website_right.url,access_log.count,access_log.date from website_right join access_log on website_right.id=access_log.site_id;
    #  aid | id |   name   |            url            | count |    date    
    -----+----+----------+---------------------------+-------+------------
       2 |  3 | 菜鸟教程 | http://www.runoob.com/    |   100 | 2016-05-13
       4 |  2 | 淘宝     | https://www.taobao.com/   |    10 | 2016-05-14
       5 |  5 | Facebook | https://www.facebook.com/ |   205 | 2016-05-14
       6 |  4 | 微博     | http://weibo.com/         |    13 | 2016-05-15
       7 |  3 | 菜鸟教程 | http://www.runoob.com/    |   220 | 2016-05-15
       8 |  5 | Facebook | https://www.facebook.com/ |   545 | 2016-05-16
       9 |  3 | 菜鸟教程 | http://www.runoob.com/    |   201 | 2016-05-17
    
    --inner join
    select access_log.aid,website_right.id,website_right.name,website_right.url,access_log.count,access_log.date from website_right inner join access_log on website_right.id=access_log.site_id;
    # aid | id |   name   |            url            | count |    date    
    -----+----+----------+---------------------------+-------+------------
       2 |  3 | 菜鸟教程 | http://www.runoob.com/    |   100 | 2016-05-13
       4 |  2 | 淘宝     | https://www.taobao.com/   |    10 | 2016-05-14
       5 |  5 | Facebook | https://www.facebook.com/ |   205 | 2016-05-14
       6 |  4 | 微博     | http://weibo.com/         |    13 | 2016-05-15
       7 |  3 | 菜鸟教程 | http://www.runoob.com/    |   220 | 2016-05-15
       8 |  5 | Facebook | https://www.facebook.com/ |   545 | 2016-05-16
       9 |  3 | 菜鸟教程 | http://www.runoob.com/    |   201 | 2016-05-17
    
    --left join 等于 left outer join
    select access_log.aid,website_right.id,website_right.name,website_right.url,access_log.count,access_log.date from website_right left join access_log on website_right.id=access_log.site_id;
    #  aid | id |   name   |            url            | count |    date    
    -----+----+----------+---------------------------+-------+------------
       2 |  3 | 菜鸟教程 | http://www.runoob.com/    |   100 | 2016-05-13
       4 |  2 | 淘宝     | https://www.taobao.com/   |    10 | 2016-05-14
       5 |  5 | Facebook | https://www.facebook.com/ |   205 | 2016-05-14
       6 |  4 | 微博     | http://weibo.com/         |    13 | 2016-05-15
       7 |  3 | 菜鸟教程 | http://www.runoob.com/    |   220 | 2016-05-15
       8 |  5 | Facebook | https://www.facebook.com/ |   545 | 2016-05-16
       9 |  3 | 菜鸟教程 | http://www.runoob.com/    |   201 | 2016-05-17
         |  6 | baidu    | https://www.baidu.com/    |       | 
    
    --right join
    select access_log.aid,website_right.id,website_right.name,website_right.url,access_log.count,access_log.date from website_right right join access_log on website_right.id=access_log.site_id;
    aid | id |   name   |            url            | count |    date    
    -----+----+----------+---------------------------+-------+------------
       1 |    |          |                           |    45 | 2016-05-10				#id没1
       2 |  3 | 菜鸟教程 | http://www.runoob.com/    |   100 | 2016-05-13
       3 |    |          |                           |   230 | 2016-05-14
       4 |  2 | 淘宝     | https://www.taobao.com/   |    10 | 2016-05-14
       5 |  5 | Facebook | https://www.facebook.com/ |   205 | 2016-05-14
       6 |  4 | 微博     | http://weibo.com/         |    13 | 2016-05-15
       7 |  3 | 菜鸟教程 | http://www.runoob.com/    |   220 | 2016-05-15
       8 |  5 | Facebook | https://www.facebook.com/ |   545 | 2016-05-16
       9 |  3 | 菜鸟教程 | http://www.runoob.com/    |   201 | 2016-05-17
       
    --SELECT access_log.site_id
    select access_log.aid,access_log.site_id,website_right.name,website_right.url,access_log.count,access_log.date from website_right right join access_log on website_right.id=access_log.site_id;
    # aid | site_id |   name   |            url            | count |    date    
      -----+---------+----------+---------------------------+-------+------------
         1 |       1 |          |                           |    45 | 2016-05-10				#site_id有1
         2 |       3 | 菜鸟教程 | http://www.runoob.com/    |   100 | 2016-05-13
         3 |       1 |          |                           |   230 | 2016-05-14
         4 |       2 | 淘宝     | https://www.taobao.com/   |    10 | 2016-05-14
         5 |       5 | Facebook | https://www.facebook.com/ |   205 | 2016-05-14
         6 |       4 | 微博     | http://weibo.com/         |    13 | 2016-05-15
         7 |       3 | 菜鸟教程 | http://www.runoob.com/    |   220 | 2016-05-15
         8 |       5 | Facebook | https://www.facebook.com/ |   545 | 2016-05-16
         9 |       3 | 菜鸟教程 | http://www.runoob.com/    |   201 | 2016-05-17
    
    
      --from A left join B 等于 from B right join A
      select access_log.aid,website_right.id,website_right.name,website_right.url,access_log.count,access_log.date from access_log right join website_right on website_right.id=access_log.site_id;
        # aid | id |   name   |            url            | count |    date    
      -----+----+----------+---------------------------+-------+------------
         2 |  3 | 菜鸟教程 | http://www.runoob.com/    |   100 | 2016-05-13
         4 |  2 | 淘宝     | https://www.taobao.com/   |    10 | 2016-05-14
         5 |  5 | Facebook | https://www.facebook.com/ |   205 | 2016-05-14
         6 |  4 | 微博     | http://weibo.com/         |    13 | 2016-05-15
         7 |  3 | 菜鸟教程 | http://www.runoob.com/    |   220 | 2016-05-15
         8 |  5 | Facebook | https://www.facebook.com/ |   545 | 2016-05-16
         9 |  3 | 菜鸟教程 | http://www.runoob.com/    |   201 | 2016-05-17
           |  6 | baidu    | https://www.baidu.com/    |       | 
    ```

* 主键（约束）

  等于非空约束`not null`加唯一约束`UNIQUE`

* 索引

​       唯一索引会创建唯一约束；唯一约束不会创建唯一索引

* 不太懂的语句，先记录下来

  ```plsql
  select create_hypertable('manufacture_device', 'time', create_default_indexes=> false,
                           chunk_time_interval => interval '30 day');
  create index idx_manufacture_device_uid on manufacture_device (uid);
  create unique index idx_manufacture_device_uid_sn on manufacture_device(time, uid, sn);
  
  alter function import_manufacture(date) owner to postgres;
  
  select time + '-8 hour' as time, --utc								#?????????????????????????????????
             uid,
             sn,
             batch_number,
             country,
             province,
             city,
             store,
             channel
      from temp_manufacture_device
      
   execute format('copy temp_manufacture_device from ''%s'' with csv', manufacture_path);
   
   什么时候用call，什么时候用select
   
   execute、call、perform有什么区别
   
   create or replace procedure daily_clean(node smallint) as
  $$
  declare
      addr text;
  begin
      -- 获取数据库地址
      select format('host=%s port=%s user=%s password=%s dbname=%s', host, port, username, password, database)
      from branch
      where branch.node_id = $1
      into addr;
      perform * from dblink(addr, 'call start_clean()') as result(col text);				#???????????????????????
  end;
  $$
    language plpgsql;
      
      select target, node_id, *   					#1. target哪里来的 2. *和target同时存在？
      from dblink(addr,
                  ' select company_id,
                      product_id,
                      country_code,
                      city_name,
                      point(min(coordinates[0]), min(coordinates[1])),
                      count(distinct device_id)
                  from active_device left join ip_location on range @> inet_to_box(ip),      #？？？？？？
                      device
                  where active_device.date = ''' || target || '+00''					#引号
                      and active_device.device_id = device.id
                  group by 1, 2, 3, 4
                  order by 1, 2, 3, 4;
                  ') as result (company_id integer,
                                product_id integer,
                                country_code char(2),
                                city_name varchar(80),
                                coordinates point,
                                amount integer
  
  start_ts bigint =extract(epoch from start_date);                              #####？？？？？？？？？？？？？？
                                
  insert into table_name_1 with table_name_2 as (select columns from table_name_3) select columns from 
  ```

* branch.node

  | SZ   | HK | EU   | US   | BJ   | GZ   |
  | ---- | ---- | ---- | ---- | ---- | ---- |
  |1|2|4|3|22|32|


* 联表删除

  ```plsql
  #using实现
  DELETE FROM films USING producers
    WHERE producer_id = producers.id AND producers.name = 'foo';
  #子查询实现
  DELETE FROM films
    WHERE producer_id IN (SELECT id FROM producers WHERE name = 'foo');
  ```

* Xorm

  ```plsql
  var ints []int64
  err := engine.Table("user").Cols("id").Find(&ints)
  
  https://gobook.io/read/gitea.com/xorm/manual-zh-CN/chapter-05/4.find.html
  ```

* 疑问

  ```go
  变量申明两者对比
  func GetDeviceInfo(deviceIDArr []string) (infos map[string]*DeviceInfo, err error) {
  	infos = make(map[string]*DeviceInfo, 0)
  和
      func GetProductInfo(productID int64) (productCode, coreCode string, err error) {
          	productCode = product.ProductCode
  	coreCode = company.CoreCode
          
          
  ```

  

# 计算机网络

* 分组交换网中的时延
  * 节点总时延=处理时延+排队时延+传输时延+传播时延
  * 处理时延：思考分组发到哪里去；比特级别的差错检测
  * 排队时延：到达该节点有很多分组，需要一个一个处理
  * 传输时延：分组有长度，由许多比特组成
  * 传播时延：分组需要在电缆上面跑
  * 传播时延是物理层的问题，其余时延是链路层的问题

# 知识储备

* [danale_gitlab](https://gitlab.dana-tech.com/be/plt-team/plt-biz-team/plt-biz-team-doc/tree/master/biz-group)

* [10分钟看懂Docker和K8S](https://zhuanlan.zhihu.com/p/53260098)

* [迭代器](https://mp.weixin.qq.com/s/Be4tHnR0BY-C__xoPPBjhQ)

* [指针与引用](https://www.cnblogs.com/pythonista/p/11178705.html)

* [为什么我们需要制品管理](https://xie.infoq.cn/article/f5f72e8a25f5b6581c0a2fb66)

* [APNs推送机制](https://www.jianshu.com/p/958ca3fd3cef)

* [交叉编译详解](https://blog.csdn.net/pengfei240/article/details/52912833)

* [正向代理和反向代理的区别](https://cloud.tencent.com/developer/article/1418457)
  * 正向代理和反向代理都架设在客户和服务器之间，提供缓存，提高访问速度
  * 正向代理为客户服务，可以访问一些受限制的资源；客户知道目标服务器的IP，但目标服务器不知道客户的IP
  * 反向代理为服务器服务，可以提供负载均衡和安全访问；此时，客户并不知道目标服务器的IP
  
* [MQTT](https://internetofthingsagenda.techtarget.com/definition/MQTT-MQ-Telemetry-Transport)

* 手动挂载172.19.1.219。[自动挂载](https://www.cnblogs.com/darkknightzh/p/7160792.html)<https://blog.csdn.net/qq_35451572/article/details/79541106>

  ```bash
  mount -t cifs //172.19.1.219/share /mnt/device-share/ -o username=danale-guest,password=danaleguest
  ```

* 配置跳板机

  ```bash
  1. 找文礼申请跳板机账号（文礼会发邮件）；
  2. 点击邮件中的链接，修改密码（https://jms.dana-tech.com/是自签名证书，因此Firefox浏览器打不开，换chrome可以；或者为Linux系统导入Danale证书）
  3. 在网站中绑定二因子验证、上传本机公钥、下载.pem到本机~/.ssh;
  4. 编辑/etc/profile，加入
  alias jms="ssh -p 17927 -i /home/{user_name}/.ssh/{user_name}-jumpserver.pem zhuzhenxuan@jms.dana-tech.com"
  alias jms-sit="ssh -p 18511 sit-zhuzhenxuan@jms.sit.dana-tech.com"
  alias jms-hq="ssh -p 2222 -i /home/danale/.ssh/staff-zhuzhenxuan-jumpserver.pem staff-zhuzhenxuan@jms.ops.haique-tech.com"
  然后source /etc/profile
  5. jms登入跳板机
  ```

* cron表达式

  典型的cron表达式如下（使用空格连接）：

  * `秒` `分` `时` `每月第几天` `月` `每周第几天` `年` 

  用例如下：

  ```bash
  #2020年每月1日6时15分30秒定时执行/media/efs/dana/dbd/bi-manufacture/timer.sh
  30 15 6 1 * ? 2020 /media/efs/dana/dbd/bi-manufacture/timer.sh
  #2020年每月周日6时15分30秒定时执行/media/efs/dana/dbd/bi-manufacture/timer.sh
  30 15 6 ? * 1 2020 /media/efs/dana/dbd/bi-manufacture/timer.sh
  ```

  用法：

  * `秒`：取值0-59，允许特殊字符` , - * / `
  * `分`：取值0-59，允许特殊字符` , - * / `
  * `时`：取值0-23，允许特殊字符` , - * / `
  * `每月第几天`：取值1-31，允许特殊字符`, - * ? / L W C `
  *  `月` ：取值1-12，允许特殊字符`, - * / `
  * `每周第几天`：取值1-7，允许特殊字符` , - * ? / L C #  `
  * `年`：取值1970-2099，允许特殊字符` , - * /  `

  特殊字符说明：

  ```bash
  * ：代表整个时间段
  “?”字符：表示不确定的值
  “,”字符：指定数个值
  “-”字符：指定一个值的范围
  “/”字符：指定一个值的增加幅度。n/m表示从n开始，每次增加m
  “L”字符：用在日表示一个月中的最后一天，用在周表示该月最后一个星期X
  “W”字符：指定离给定日期最近的工作日(周一到周五)
  “#”字符：表示该月第几个周X。6#3表示该月第3个周五
  ```

  需要注意：

  * `每周第几天`位置：`1`代表周日、`2`代表周一、`3`代表周二....`7`代表周六
  * `每月第几天`位置和`每周第几天`位置都代表了“天”的含义，因此可能会存在冲突，因此`30 15 6 * * 1 2020`是错误的，应改为`30 15 6 ? * 1 2020`

  参考资料：

  * <https://www.jianshu.com/p/af640f30d034>
  * <https://www.cnblogs.com/linjiqin/p/3178452.html>
  
* 云原生应用部署

  * 业务程序推送到harbor
    1. gitlab新建分支
    2. git clone该分支
    3. 在项目中放入.gitlab-ci.yml、Dockerfile、Makefile（三个文件需要修改）
    4. git push
    5. merge
    6. 新建tag
    7. CI
    8. 在harbor中查看

  * 在helm charts中添加该业务程序的配置文件
    1. git clone helm charts的业务仓库
    2. 新建业务代码目录，如idgen.idgen-pvt-svr
    3. 将@wangkaixiong等人已经做好的配置文件（idgen.iris-wid-pvt-svr/ks-charts-sit-a998-alibj和idgen.iris-wid-pvt-svr/ks-charts-sit-a999-alibj）复制到该目录
    4. 找到该程序部署的位置，打包配置文件（tar zcvf config.ddas.tar.gz config.ddas）、上传到Pandora（pandora -pt zhangkai@config config.ddas.tar.gz）
    5. ks-app-config下有两个目录，一个目录是ferry-sidecar。另一个目录是程序名，如idgen-pvt-svr，在该目录下下载（Pandora -dt zhangkai@config）、解压（tar zxvf config.ddas.tar.gz）
    6. 按要求修改各个文件中的port（dst_port不修改，port任选）、程序名、业务线名、文件名
    7. gitlab新建issue
    8. 本地切换到该issue
    9. git push
    10. merge
    11. CI
  * kubesphere部署（998和999分别部署）

# 算法

* 安装 ncnn

  * 下载源码

    ```bash
    $ git clone https://github.com/Tencent/ncnn.git
    $ cd ncnn
    $ git submodule update --init
    ```

  * [安装ncnn依赖](https://github.com/Tencent/ncnn/wiki/how-to-build#build-for-linux)

    ```bash
    sudo apt install build-essential git cmake libprotobuf-dev protobuf-compiler libvulkan-dev vulkan-utils libopencv-dev
    ```

  * build

    ```bash
    $ cd ncnn
    $ mkdir -p build
    $ cd build
    build$ cmake -DCMAKE_BUILD_TYPE=Release -DNCNN_VULKAN=ON -DNCNN_SYSTEM_GLSLANG=ON -DNCNN_BUILD_EXAMPLES=ON ..
    build$ make -j$(nproc)
    ```

    

* 安装ncnn过程中不知道什么鬼东西

  ```bash
  $ wget https://sdk.lunarg.com/sdk/download/1.2.154.0/linux/vulkansdk-linux-x86_64-1.2.154.0.tar.gz?Human=true -O vulkansdk-linux-x86_64-1.2.154.0.tar.gz
  $ tar -xf vulkansdk-linux-x86_64-1.2.154.0.tar.gz
  $ export VULKAN_SDK=$(pwd)/1.2.154.0/x86_64
  ```

  ********

  

* [pytorch二进制安装](https://pytorch.org/)(大概率会因网络问题失败)

  ```bash
  conda install pytorch torchvision torchaudio cudatoolkit=10.2 -c pytorch
  ```

* [pytorch源码安装](https://github.com/pytorch/pytorch#from-source)

  * 安装依赖

    ```bash
    conda install numpy ninja pyyaml mkl mkl-include setuptools cmake cffi typing_extensions future six requests dataclasses
    ```

  * 下载pytorch源代码

    ```bash
    git clone --recursive https://github.com/pytorch/pytorch
    cd pytorch
    # if you are updating an existing checkout
    git submodule sync
    git submodule update --init --recursive
    ```

  * 安装pytorch

    ```bash
    export CMAKE_PREFIX_PATH=${CONDA_PREFIX:-"$(dirname $(which conda))/../"}
    python setup.py install
    ```

  *****

* .pt转onnx

  ```bash
  conda activate torch_cuda10.2  #该环境下安装了pytorch 1.8.0
  cd pytorch_onnx_ncnn
  python pytorch2onnx.py
  ```

* onnx裁减

  ```bash
  pip3 install onnx-simplifier
  python3 -m onnxsim net.onnx net_sim.onnx
  ```

* onnx转ncnn(两种方法）

  * [一键转换工具](https://convertmodel.com/)

  * 使用onnx2ncnn工具

    ```bash
    ./onnx2ncnn net_sim.onnx
    ```



# 临时记录

```go
//zhangkai try
/*
func GetDeviceInfo(deviceIDArr []string) (infos map[string]*DeviceInfo, err error) {
	infos = make(map[string]*DeviceInfo, 0)
	//where语句应该放在哪里,使用in还是where
	//DeviceId, DeviceId, ProductCode需不需要改为device_id
	Sql.SQL("select device_code.code DeviceId, basic_dict_company_all.core_code CompanyCode, basic_dict_product_all.product_code ProductCode from device_code t1 left join basic_video_device_uid t2 on t1.uid=t2.device_id left join basic_dict_product_all t3 on t2.product_id=t3.id left join basic_dict_company_all t4 on t3.company_id=t4.id").Find(&infos)
	err = DanaSyncDb.In("device_id", deviceIDArr).Find(&devices)
	if err != nil {
		logx.X.Error(err)
		return
	}

	for _, it := range infos {
		var productCode, coreCode string
		productCode, coreCode, err = GetProductInfo(it.ProductId)
		if err != nil {
			logx.X.Error(err)
			return
		}
		infos[it.DeviceId] = &DeviceInfo{
			DeviceId:    it.DeviceId,
			CompanyCode: coreCode,
			ProductCode: productCode,
		}
	}
	return
}
*/

```




