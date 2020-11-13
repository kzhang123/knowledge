# 语法

## python

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


## 数据库

* [Ubuntu Linux 上安装和使用开源数据库 PostgreSQL](https://linux.cn/article-11480-1.html) <https://www.jianshu.com/p/f23ae3798fde>

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


# 技巧

* VS Code和Spyder等IDE内，多行代码缩进,先选中代码，然后快捷键`shift+tab`，多行代码后退`tab`

# 工具

* markdown语法

  [菜鸟教程](https://www.runoob.com/markdown/md-link.html)

* [python的C与C++扩展编程:Cython和pybind11](https://zhuanlan.zhihu.com/p/49737946)

* 

# 知识储备

* [danale_gitlab](https://gitlab.dana-tech.com/be/plt-team/plt-biz-team/plt-biz-team-doc/tree/master/biz-group)
* [10分钟看懂Docker和K8S](https://zhuanlan.zhihu.com/p/53260098)
* [迭代器](https://mp.weixin.qq.com/s/Be4tHnR0BY-C__xoPPBjhQ)
* [指针与引用](https://www.cnblogs.com/pythonista/p/11178705.html)
* [为什么我们需要制品管理](https://xie.infoq.cn/article/f5f72e8a25f5b6581c0a2fb66)
* [APNs推送机制](https://www.jianshu.com/p/958ca3fd3cef)

# 临时记录

