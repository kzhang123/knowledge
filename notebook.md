# 语法

## python

```python
import tensorflow
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


* 模板的声明或定义只能在全局，命名空间或类范围内进行。即不能在局部范围，函数内进行，比如不能在main函数中声明或定义一个模板

* [C语言中文网 C++ STL（标准模板库）](http://c.biancheng.net/cplus/80/)

* 目前的c++标准，支持VLA，也就是可以通过变量来定义数组的大小

  ```c++
  int size = 10;
  int arr[size] = {0};
  ```


* [C++读取numpy数据二进制文件](https://blog.csdn.net/guyuealian/article/details/106422400)
* [C++ double和float（浮点类型）详解](http://c.biancheng.net/view/1321.html)

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

  

# 工具

* markdown语法

  [菜鸟教程](https://www.runoob.com/markdown/md-link.html)

* [python的C与C++扩展编程:Cython和pybind11](https://zhuanlan.zhihu.com/p/49737946)

* [Ubuntu Linux 上安装和使用开源数据库 PostgreSQL](https://linux.cn/article-11480-1.html) <https://www.jianshu.com/p/f23ae3798fde>

# 知识储备

* [danale_gitlab](https://gitlab.dana-tech.com/be/plt-team/plt-biz-team/plt-biz-team-doc/tree/master/biz-group)

* [10分钟看懂Docker和K8S](https://zhuanlan.zhihu.com/p/53260098)

# 临时记录

* [安装opencv](https://www.cnblogs.com/hichens/p/12665897.html)

* # [tensorflow C++接口调用目标检测pb模型代码](https://www.cnblogs.com/cnugis/p/11506767.html)



https://xie.infoq.cn/article/f5f72e8a25f5b6581c0a2fb66

* [c++使用opencv读取图像进tensorflow做预测](https://blog.csdn.net/luoyexuge/article/details/82852023)

```python
[[[0.4089657 0.39028615 0.53447574 0.5700223 ] 
[0.8558948 0.42989227 1.0045267 0.606625 ]
 [0.3998442 0.37940985 0.5982684 0.56084234] 
[0.20891714 0.35454977 0.3605047 0.5166982 ] 
[0.97804457 0.5113069 0.99955636 0.5798416 ] 
[0.94513017 0.9079508 1.004485 1.0062561 ] 
[0.22192839 0.40005988 0.32978496 0.5397108 ]
[0.9770811 0.00604879 1.0007473 0.05647356] 
[0.97379166 0.48825654 1.0009657 0.5576541 ] 
[0.3319717 0.343028 0.553981 0.66085917]]] 
[[0. 0. 0. 0. 0. 0. 0. 0. 0. 0.]] 
[[0.9743335 0.13643828 0.04565468 0.03972378 0.02167872 0.02157673 0.01734331 0.01599815 0.01430091 0.01419213]] 

[[[ 0.10837875 0.28216088 0.34203565 0.53766775] [ 0.12449323 0.26207277 0.4400407 0.55033517] [ 0.19303468 0.31320673 0.3617011 0.52730924] [ 0.1292868 0.2677885 0.29452762 0.4225408 ] [ 0.8573876 0.285033 1.0003952 0.437113 ] [ 0.03426938 0.02839385 0.30346435 0.42340326] [ 0.9468248 0.36853987 1.0088819 0.46592146] [-0.00595287 0.08207253 0.20091504 0.3554146 ] [ 0.8631652 0.34014785 0.98004836 0.42484927] [ 0.00297058 0.6797067 0.01890697 0.7074748 ]]] 
[[0. 0. 0. 0. 0. 0. 0. 0. 0. 0.]] 
[[0.98810923 0.06335232 0.04161656 0.02315471 0.01933593 0.01608488 0.01454628 0.01356906 0.01081046 0.01032504]]
```

```python
1228800 bytes read
initialize:in index:0,out index:333
initialize:in index:32550,out index:334
initialize:in index:-1123142752,out index:335
initialize:in index:32550,out index:336
initialize:in index:4,out index:4
initialize:in index:24,out index:24
output_boxes:
0.770867	0.972092	0.797686	0.996002	
0.386049	0.954847	0.445958	0.992366	
0.737775	0.955044	0.780851	0.988773	
0.405795	0.954481	0.466828	0.991603	
0.044561	0.062726	1.046411	0.997394	
0.785745	0.968809	0.816579	0.992401	
0.757035	0.956488	0.802702	0.990582	
0.716339	0.955704	0.762092	0.991250	
0.427741	0.953840	0.485911	0.992787	
-0.009695	0.860698	0.047746	1.110604	
0.166888	0.928744	end boxes
output_classes:
0.000000	0.000000	0.000000	0.000000	
0.000000	0.000000	0.000000	0.000000	
0.000000	0.000000	1.077390	0.585216	
end classes
output_scores:
0.063332	0.043061	0.041831	0.036001	
0.034555	0.031156	0.029072	0.025913	
0.023157	0.023011	0.424863	0.233844	
end scores

```



