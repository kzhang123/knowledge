# # XYZ

* 编程语言只是一种工具，更让人兴奋的是，这个工具是怎么造出来的

* 制造和使用工具是人的本性

* ```
          
          active_consumer、active_device、active_device_info、bind、consumer、form、form_device_views、manufacture_device、new_device、share、subscribe、
  ```

# Python

* 从一个文件夹中逐个读取文件名：

* 

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

  * 闭包需要小心使用

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

* [为什么需要多态](https://blog.csdn.net/tushenfengle/article/details/101795925)

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

* 编程总结：

  * 开多个goroutine时，最好用channel进行通信，而不是变量加锁
  
  * 有这样一种使用场景，从channel中取数据，然后批量进行处理，那么channel的使用有两种模式：
  
    * channel一但有数据，就放入一个buffer中，等buffer满了，进行批量处理
    
  * channel中存够一定批量数据，才取到buffer中处理
  
    两者的区别就是，数据流在一个数据批量处理容器中是慢慢存满的，这个缓冲容器应该是channel还是buffer
    
  * 生产者消费者模型中，什么时候开始消费，可以通过click定时（比如一分钟），处理缓冲数据容器，也可以达到一定数量，才进行处理。
  
  * 接口就是方法，实现了接口，就是在各个结构体中实现了接口中的方法。
  
  * 为什么需要接口？它的使用场景在于，结构体差别很大，但操作这些结构体的方法非常一致，所以可以通过方法来驱动结构体，把方法放在优先发展的战略地位
  
  * 如果不同方法具有相同的模式，这些方法需要操作不同的结构体，那么该怎样实现代码复用？
  
* 配环境

  ```bash
  go env -w GOPRIVATE="go.danale.net,dana-tech.com"
  go env -w GOPROXY="https://proxy.go.danale.net,direct"
  go env -w GO111MODULE=on
  ```

  https://www.jianshu.com/p/fb22d50a05f2
  
* [go语言设计与实现](https://draveness.me/golang/docs/part2-foundation/ch04-basic/golang-function-call/)

* [go语言高级编程](https://chai2010.gitbooks.io/advanced-go-programming-book/content/)

* [W3C school](https://www.w3cschool.cn/go/go-tutorial.html)

* [函数方法](https://www.runoob.com/go/go-method.html)

* [time.Tick和time.After](https://www.jianshu.com/p/c9e8aaa13415)

* [runtime.Caller](https://colobu.com/2018/11/03/get-function-name-in-go/)

* tensorflow有go语言的API

* go语言下载包的路径

  * ```go
    import "github.com/tensorflow/tensorflow/tensorflow/go"
    ```

  * 其实是去这个URL下载包：`https://pkg.go.dev/github.com/tensorflow/tensorflow`

* 闭包

  * https://zhuanlan.zhihu.com/p/351428978

* 内嵌结构体初始化

  ```go
  type OutputCommon struct {
  	RequestID string `json:"request_id"`
  	Code      int64  `json:"code"`
  	CodeMsg   string `json:"code_msg"`
  	TraceID   string `json:"trace_id"`
  }
  
  type LoginOutput struct {
  	OutputCommon
  	Body struct {
  		Token string `json:"token"`
  	} `json:"body"`
  }
  
  func main() {
  	a:=LoginOutput {
  		OutputCommon: OutputCommon {RequestID: "good"},   //初始化内嵌结构体
  		Body: struct {									//初始化内嵌匿名结构体
  			Token string `json:"token"` 	//得重复匿名结构体定义，很烦
  		}{
  			Token: "bad",
  		},
  	}
  	fmt.Println(a.RequestID)
  }
  ```

  

* go依赖包下载位置

  * go mod：`$GOPATH/pkg/mod`
  * go get：

* go build

  * `go build -o binary_name ./app/main.go`
  * 依赖包寻找顺序（非go mod）:
    1. $GOROOT/src
    2. $GOPATH/src
  * 如果仅执行`go build`，则当前目录下应当有main module

* [slice](https://zhuanlan.zhihu.com/p/61121325)

  * 初始化方式
    * `var a []int`空slice
    * `b:=a[1:3:6]`b从a中截取，从1到3，容量为`1:6`，即5
    * `b:=[]int{1,2,3}`长度和容量均为3
  * slice是由数组实现的

* 主协程等待其他goroutine的实现方法

  * wait.Group

  * select

    ```go
    func main() {
    	a:=1
    	go func() {
    		time.Sleep(5*time.Second)
    		fmt.Println("go func",a)
    }()
    	select{}
    }
    ```

  * channel

* 时间

  ```go
  	var from int64
  	//2021-03-02 16:27:05.719713526 +0800 CST m=-86399.999965391
  	beginTime := time.Now().Add(-time.Duration(24) * time.Hour) //当前时间回退24小时
  	//1614673625
  	from = beginTime.Unix() // 获取时间戳
  
  	//2021-03-02
  	date := time.Unix(from, 0).UTC().Format("2006-01-02") //只要日期，不要时分秒
  	//2021-03-02 16:27:05 +0800 CST
  	beginTime := time.Unix(from, 0)
  	//1614643200
  	todayStart := time.Date(beginTime.UTC().Year(), beginTime.UTC().Month(), beginTime.UTC().Day(), 0, 0, 0, 0, time.UTC).Unix() 
  	//2021-03-02 08:00:00 +0800 CST
  	DDD:=time.Unix(todayStart,0) //beginTime当天UTC时间0点，即中国时间8点
  ```

  * 日期变动

    ```go
    it.date = time.Now().Add(-time.Duration(it.preload) * time.Hour)
    cursorDate:= it.date.UTC().Format("2006-01-02")
    //日期增加一天
    date= it.date.AddDate(0,0,1)
    ```

    

* [反射](https://www.jianshu.com/p/26a284e69586)[http://c.biancheng.net/golang/reflect/]

  * 反射是指在程序运行期对程序本身进行访问和修改的能力。程序在编译时，变量被转换为内存地址，变量名不会被编译器写入到可执行部分。在运行程序时，程序无法获取自身的信息。

     支持反射的语言可以在程序编译期将变量的反射信息，如字段名称、类型信息、结构体信息等整合到可执行文件中，并给程序提供接口访问反射信息，这样就可以在程序运行期获取类型的反射信息，并且有能力修改它们。

  * 静态语言需要反射，动态语言不需要

  * ```go
    import reflect
    type cat struct{}
    cat_:=cat{}
    typeOf=reflect.TypeOf(cat_)
    fmt.Println(typeOf.Name(),typeOf.Kind())
    ```

  * 

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

* 多态编程范式

  * Task接口中Start在基类BasicTask中实现，Transport在继承类TaskAliyun和TaskHuaweiyun中分别实现

  ```go
  type Task interface {
      Start(options *option.Options, odsConf *ods.Conf)
      Transport()
  }
  type TaskAliyun struct {
      BasicTask
  }
  //Methods:Transport()
  type TaskHuaweiyun struct {
      BasicTask
  }
  //Methods:Transport()
  type BasicTask struct {
      sourceOds    ods.Ods
      targetOds    ods.Ods
      currentDate  string
      timeShiftDay int
  }
  //Methods:Start(opt *option.Options, odsConf *ods.Conf)
  func Run(opt *option.Options) {
  	for _, taskType := range opt.GlobalOpt.TaskType {
  		var mission Task
  		var odsConf ods.Conf
  		switch taskType {
  		case TASK_ALIYUN:
  			mission = &TaskAliyun{}
  			odsConf = opt.OdsOpt.AliyunSourceOds
  		case TASK_HUAWEIYUN:
  			mission = &TaskHuaweiyun{}
  			odsConf = opt.OdsOpt.HuaweiSourceOds
  	
  		mission.Start(opt, &odsConf)
  		mission.Transport()
  	}
  }
  ```

  

* 注意事项

  * `go mod`中的`module bi-relay-statistics`似乎指定了项目的根目录。如果要使用本地的package需要更改
  
  * import添加包时必须以上述`module+package`形式导入。不能只添加包名
  
  * 同一个package内的函数互相引用，不需要加package.func_name
  
  * `强制`package内的函数、结构体如果被外部使用，则需要将函数名、结构体名首字母大写
  
  * `规范`package内的函数、结构体如果没有被外部使用（局部使用），则需要将函数名、结构体名首字母小写，如果是单词拼写，则用大写分割，如`getDeviceId`；package名都小写，如果是单词拼写，则使用下划线分割
  
  * 对外暴露的函数、结构体（即首字母大写的函数等）写注释时，需要遵从这样的规范`//函数名 注释`
  
  * 定义结构体方法时，可以传值，也可以传指针，后者可以改变变量的值
  
  * 一个goroutine内部，代码不一定是顺序执行的
  
    * 运行时，大概有几种错误类型：一是main函数无法看到被修改后的done，因此main的for循环无法正常结束；二是main函数虽然看到了done被修改为true，但是msg依然没有初始化，这将导致错误的输出
  
    ```go
    var msg string
    var done bool = false
    func main() {
        go func() {
            msg = "hello, world"
            done = true
        }()
        for {
            if done {
                println(msg); break
            }
            println("retry...")
        }
    }
    ```
  
* 使用锁同步goroutine

  * 代码中，sync.Mutex必须先Lock然后再Unlock，因为直接Unlock一个Mutex对象会导致panic。代码中，done.Unlock()和第二个done.Lock()分别在不同的Goroutine，它们会强制做一次时间同步。因此最后main函数也可以正常打印msg字符串。

  ```go
  var msg string
  var done sync.Mutex
  func main() {
      done.Lock()							//注意
      go func() {
          msg = "hello, world"
          done.Unlock()					//注意
      }()
      done.Lock()							//注意
      println(msg)
  }
  ```

* [context](https://www.flysnow.org/2017/05/12/go-in-action-go-context.html)

  * https://www.cnblogs.com/yjf512/p/10399190.html

* [闭包示例——较烧脑](https://segmentfault.com/a/1190000018689134)

  * WithNodeID,WithServName即闭包用法，将外部函数的参数作为里面函数的参数，即，返回一个经外部函数参数“装饰”过的函数、

  * 疑惑：直接把nodeID，serverName作为函数packAndCall的参数不好吗？为啥要用闭包，为了炫技吗？

    ```go
    type callOptions struct {
    	nodeID      uint32
    	serviceName string
    	dialTimeOut int
    	callTimeOut int
    	useSSL      bool
    	token       string
    }
    
    type CallOption func(*callOptions)
    
    //WithNodeID 使用节点ID nid
    func WithNodeID(nid uint32) CallOption {
    	return func(o *callOptions) {
    		o.nodeID = nid
    	}
    }
    
    //WithServName 使用服务名
    func WithServName(servName string) CallOption {
    	return func(o *callOptions) {
    		o.serviceName = servName
    	}
    }
    
    //CallWithNodeId 使用context的rpc调用
    func (clis *Clients) CallWithNodeId(serverName string, nodeID uint32, method string, args interface{}, reply interface{}) error {
    	return clis.packAndCall(context.TODO(), method, args, reply, WithNodeID(nodeID), WithServName(serverName))
    }
    
    func (clis *Clients) packAndCall(cxt context.Context, method string, args interface{}, reply interface{}, opts ...CallOption) (err error) {
    
    	var callOpt callOptions
    	for _, opt := range opts {
    		opt(&callOpt)
    	}
    }
    ```

* [`...`用法](https://blog.csdn.net/jeffrey11223/article/details/79166724)

* [panic](https://blog.csdn.net/netdxy/article/details/71339115)

  * 被调函数中触发panic后，如果被调函数中有defer recover代码，则被调函数中panic后的代码不会执行、被调函数中defer会执行、main会全部执行
  * 被调函数中触发panic后，如果被调函数中没有defer recover代码，被调函数中有defer代码，则被调函数中panic后的代码不会执行，被调函数中defer会执行、main中的defer会执行、main中触发panic后的代码不会执行
  * goroutine中触发panic后，如果goroutine中没有defer recover代码，goroutine中有defer代码，则goroutine中panic后的代码不会执行，goroutine中defer代码会被执行，main函数会在goroutine触发panic的时刻全部停止（main中的defer不会执行）
  * 总结：
    * defer语句在当前函数中永远会执行（goroutine中触发panic没有recover，则main中的defer不会执行；被调函数中触发panic没有recover，则main中的defer会执行）；
    
    * recover永远和defer在一起；
    
    * 有了recover，程序就不会输出panic错误；goroutine中的panic会影响main正常执行，即goroutine触发panic后，程序整体退出，main中的defer不会执行（考虑到goroutine和main执行不同步，可能main中的语句先执行完，goroutine才触发panic）
    
    * 实验代码
    
      ```go
      package main
      
      import (
      	"fmt"
      	"sync"
      	"time"
      )
      
      func fullName(firstName *string, lastName *string, wg sync.WaitGroup) {
      	defer fmt.Println("deferred call in goroute")
      	//defer r()
      	if firstName == nil {
      		panic("runtime error: first name cannot be nil")
      	}
      	if lastName == nil {
      		panic("runtime error: last name cannot be nil")
      	}
      	fmt.Printf("%s %s\n", *firstName, *lastName)
      	fmt.Println("returned normally from fullName")
      }
      func r(){
      	recover()
      	fmt.Println("deferred call in r")
      }
      
      func main() {
      	defer fmt.Println("deferred call in main")
      	firstName := "Elon"
      	go fullName(&firstName, nil, wg)
      	time.Sleep(time.Second * 5)
      	fmt.Println("returned normally from main")
      }
      ```
  
* 理解接口的例子

  * 结构体、函数`handler`、非函数`myint`都可以定义方法
  * 只要实现了接口中的方法，就可以通过该接口名，调用不同结构体、函数、非函数中的该方法，即，一个接口，多个调用
  * 函数也可以被强制类型转换`handler(doubler)`

  ```go
  //下面让我们详细看一下例子，其中涉及了函数、函数的方法、结构体方法、接口的使用。
  package main
  
  import (
  	"fmt"
  )
  
  //定义接口
  type adder interface {
  	add(string) int
  }
  
  //定义函数类型
  type handler func (name string) int
  
  //实现函数类型方法
  func (h handler) add(name string) int {
  	return h(name) + 10
  }
  
  //函数参数类型接受实现了adder接口的对象（函数或结构体）
  func process(a adder) {
  	fmt.Println("process:", a.add("taozs"))
  }
  
  //另一个函数定义
  func doubler(name string) int {
  	return len(name) * 2
  }
  
  //非函数类型
  type myint int
  
  //实现了adder接口
  func (i myint) add(name string) int {
  	return len(name) + int(i)
  }
  
  func main() {
  	//注意要成为函数对象必须显式定义handler类型
  	var my handler = func (name string) int {
  		return len(name)
  	}
  
  	//以下是函数或函数方法的调用
  	fmt.Println(my("taozs"))                   //调用函数
  
  	fmt.Println(my.add("taozs")) //调用函数对象的方法
  
  	fmt.Println(handler(doubler).add("taozs")) //doubler函数显式转换成handler函数对象然后调用对象的add方法
  
  	//以下是针对接口adder的调用
  	process(my) //process函数需要adder接口类型参数
  
  	process(handler(doubler)) //因为process接受的参数类型是handler，所以这儿要强制转换
  
  	process(myint(8)) //实现adder接口不仅可以是函数也可以是结构体
  }
  
  #输出为5
  15
  20
  process: 15
  process: 20
  process: 13
  ```

  * wangshilin代码中接口的复杂用法ods

    * ods接口适配了阿里云OSS、华为云OBS、亚马逊S3对象存储，提供了上传数据Upload等方法

      ```go
      //Ods是一个接口，包含的方法如下：
      type Ods interface {
      	// 更新域名对应的IP
      	UpdateIp() (err error)
      	// 对象是否存在
      	IsExist(objectKey string) (ok bool, err error)
      	// 上传数据
      	Upload(objectKey string, data []byte) (err error)
      }
      ```

      

    * 该接口中涉及的方法用统一的一个结构体BaseImpl来实现，即无论OSS还是OBS，都使用BaseImpl定义的方法Upload

      ```go
      // 上传文件
      func (it *BaseImpl) Upload(objectKey string, data []byte) (err error) {
      	statusCode, response, _, err := it.request("Upload", http.MethodPut, data, objectKey, "", "")
      	if err != nil {
      		return
      	}
      	if statusCode != 200 {
      		err = fmt.Errorf("status code is %d, %s", statusCode, string(response))
      	}
      	return
      }
      ```

      

    * Upload实现过程中调用了`BaseImpl.request`，而`BaseImpl.request`又调用了`BaseImpl.impl.request`

      ```go
      func (it *BaseImpl) request(requestMethod, httpMethod string, body []byte, objectKey string, rangeSize string, query string) (statusCode int, data []byte, resp *http.Response, err error) {
      	it.requestInfo.requestInfoLock.Lock()
      	it.requestInfo.totalCount += 1
      	it.requestInfo.httpRequestMap[httpMethod] += 1
      	it.requestInfo.methodRequestMap[requestMethod] += 1
      	it.requestInfo.requestInfoLock.Unlock()
      	var subResource string
      	if requestMethod == "DeleteObjects" {
      		subResource = "delete"
      	}
      	return it.impl.request(httpMethod, body, objectKey, rangeSize, query, subResource)
      }
      ```

      

    * `BaseImpl.impl`是一个接口，接口名为base

      ```go
      type Base interface {
      	// 基础请求函数
      	request(method string, body []byte, objectKey string, rangeSize string, query, subResource string) (statusCode int, data []byte, resp *http.Response, err error)
      }
      ```

      

    ```golang
    func NewByConf(conf *Conf, concurrence int, httpTimeout time.Duration) (ods Ods, err error) {
    	base := BaseImpl{}
    	err = base.unmarshalByConf(conf, concurrence, httpTimeout)
    	if err != nil {
    		return
    	}
    	return newOds(base)
    }
    //核心代码
    func newOds(base BaseImpl) (ods Ods, err error) {
    	switch base.StorageType {
    	case STORAGE_OBS, STORAGE_OSS:
    		cli := &Oss{base}
    		cli.impl = cli						//结构体对象可以作为结构体成员变量
    		ods = cli
    	case STORAGE_S3:
    		cli := &S3{base}					//S3中实现了request方法
    		cli.impl = cli
    		ods = cli
    	default:
    		err = errors.New("wrong storage_type: " + base.StorageType)
    	}
    	return
    }
    
    type S3 struct {
    	BaseImpl
    }
    
    func (it *S3) request(method string, body []byte, objectKey, rangeSize, query, subResource string) (statusCode int, data []byte, resp *http.Response, err error) {}
    
    type BaseImpl struct {
    	Conf
    	Variable
    	impl        Base				//是一个接口，只有request这个方法
    	requestInfo RequestInfo
    }
    ```

    

* [channel](https://www.jianshu.com/p/24ede9e90490)

  * https://segmentfault.com/a/1190000017958702
  * https://blog.csdn.net/paladinosment/article/details/42243303
  * https://www.cnblogs.com/liang1101/p/7285955.html
  * https://blog.csdn.net/sureSand/article/details/79633926
  * 无缓存channel必须在两个协程里使用（或者main和一个协程），如果仅在main函数中使用，只写不读，将会阻塞
  * https://colobu.com/2016/04/14/Golang-Channels/

* [结构体][https://blog.csdn.net/fly910905/article/details/104360402]

* 声明和初始化的区别

  * golang中，slice、map、channel、指针类型声明后需要使用make初始化；其余类型声明即初始化
  
* 编程踩坑

  * 考虑`goroutine`和`main`执行不同步，前者一般慢于后者

    ```go
    #错误：输出均为3
    func main() {
    	for i := 0; i < 3; i++ {
    		go func() {
    			fmt.Println(i)
    		}()
    	}
    	select {
    	}
    }
    #正确：需将变量i作为匿名函数参数传入
    func main() {
    	for i := 0; i < 3; i++ {
    		go func(j int) {
    			fmt.Println(j)
    		}(i)
    	}
    	select {
    	}
    }
  #升级版：避免使用select，而使用sync.WaitGroup{}
    func main() {
    	wg:=sync.WaitGroup{}
    	for i := 0; i < 3; i++ {
    		wg.Add(1)
    		go func(j int) {
    			fmt.Println(j)
    			wg.Done()
    		}(i)
    	}
    	wg.Wait()
    }
    ```
  
    
  
  * len(管道)会随时变化
  
    ```go
    package main
    
    import (
    	"fmt"
    )
    
    func main() {
    	chankai:=make(chan int, 5)
    	chankai<-1
    	chankai<-2
    	chankai<-3
    	for i := 0;i<len(chankai);i++{		//len(chankai)一直变化
  		fmt.Println(<-chankai)
    	}
    }
    //输出为1 2
  //没有3
    ```
  
  * 指针
  
    ```go
    package main
    
    import "fmt"
    //使用st1和st2取结构体内的值，效果一样，即st1.c和st2.c没有差别
    func main() {
    	type st struct {
    		a1 int
    		a2 []int
    		b string
    		c map[string]int
    	}
    	st1:=st{
    		a1: 1,
    		b: "good",
    		c:make(map[string]int),
    	}
    	st1.a2=append(st1.a2,1,2,3)
    	st1.c["xiaoming"]=10
    	st1.c["xiaohua"]=11
    	st2:=&st1
    	st2.a1=99
    	fmt.Println(st1)
    	fmt.Println(st2)
    }
    //输出结果：{99 [1 2 3] good map[xiaohua:11 xiaoming:10]}
  //        &{99 [1 2 3] good map[xiaohua:11 xiaoming:10]}
    ```
  
  * 并发
  
    ```go
    package main
    
    import (
     "fmt"
     "sync"
    )
    
    func main() {
    var m int64 = 0
    
    var wg sync.WaitGroup  //主线程执行完以后，不会等协程，程序就会退出。因此需要这个变量等协程执行完
    go func() {
      wg.Add(1)
      for i := 0; i < 10000000000 ; i++ {
       m++
      }
      wg.Done()
     }()
    
     for i := 0; i < 10000000000 ; i++ {
      m++
     }
    
     wg.Wait()
     fmt.Println(m)
    
    }
    //输出为：10026230153 
    //or 10029343835
    ```
  
    预期输出为20000000000。可见，并发冲突很严重

# Java

* 数据类型

  * Map

    ```java
    Map<Integer, Object> outputMap = new HashMap<>();
    ```

    

  * ArrayList

    ```java
    ArrayList<Recognition> recognitions = new ArrayList<>(numDetectionsOutput);
    ```

    

  * List

    ```java
    private final List<String> labels = new ArrayList<>();
    ```

    

# Shell

* ```bash
  #!/usr/bin/env bash
  #只保留7天的日志文件
  logstore=sz-p2p-relay-dbd-common
  
  target=$(date --utc -d last-day "+%Y-%m-%d")
  del_target=$(date --utc -d "7 days ago" "+%Y-%m-%d")
  cd /media/efs/dana/dbd/bi-relay-statistics/logstore || exit
  cp "${logstore}"-"${target}" /opt/oss/logstore/relay-connection-information/"${target}"
  rm ./*-"${del_target}"
  
  ```

* [Shell脚本中eq，ne，le，ge，lt，gt意义](https://www.jianshu.com/p/f30286cce4d5)

  * -gt：大于

* Shell反引号、$()和${}的区别

  * https://cloud.tencent.com/developer/article/1398436
  * https://blog.csdn.net/K346K346/article/details/51819236

* find

  * 仅列出当前目录及子目录下所有文件`find . -type f`

* 踩坑：

  * 脚本中最好使用绝对路径，否则一定要注意工作路径和脚本所在路径可能不一致的问题。例如：
    1. `/home/kai/try.sh`文件中有命令`find . -type f`
    2. `/home/kai`路径下有文件`try.sh`和`kai.go`
    3. `/home`路径下有文件`zhang.go`
    4. 在`/home/kai`路径下执行`sh try.sh`,执行结果为`./try.sh`和`./kai.go`
    5. 在`/home`路径下执行`sh try.sh`,执行结果为`./kai/try.sh`和`./kai/kai.go`和`./zhang.go`

* sed

  * 注意：sed中如果要使用变量，需要用双引号

  ```bash
  newbiz=pandora
  newapp=pandora-auth-server
  #修改当前目录及子目录下所有文件，sed替换
  sed -i "s/dpush.dpns-gateway/$newbiz.$newapp/g" `find . -type f`
  sed -i "s/dpns-gateway/$newapp/g" `find . -type f`
  sed -i 's/2.1.5/0.2.0/g' `find . -type f`
  #修改当前目录及子目录，sed替换
  for file in `find . -name "*dpns-gateway*"`
  do
  newfile=`echo $file | sed "s/dpns-gateway/$newapp/g"`
  mv $file $newfile
  done
  ```

  

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

  * 其实真正的库文件名是`libflatbuffers.a`和`libtensorflowlite.so`，前缀`lib`被忽略了
  * `-I`指定头文件目录；`-L`指定库文件目录；`-l`指定库文件名

* [主分区、扩展分区、逻辑分区](https://blog.csdn.net/xiexievv/article/details/50525783)

  只能有4个主分区，其中一个可以作为扩展分区，扩展分区可以分为多个逻辑分区
  
* [软硬链接](https://zhuanlan.zhihu.com/p/67366919)

  * Linux通过iNode区分文件和目录
  * 软链接和被链接对象有不同的iNode
  * 硬链接和被链接对象有相同的iNode

* ldd

  * 依赖分析工具
  * https://blog.csdn.net/tf_apologize/article/details/104131254

* [iptable](https://www.jianshu.com/p/ee4ee15d3658)

  * `iptables [-t 表名] 命令选项 ［链名］ ［条件匹配］ ［-j 目标动作或跳转］`
  * `iptables -t nat -A OUTPUT -p tcp -m tcp --dport 20031 -j FERRY_SIDECAR_OUTPUT`
  * 表名包括：filter, nat, mangle, raw
  * 链名包括：INPUT,OUTPUT,FORWARD, PREROUTING, POSTROUTING 

* 查看某进程的资源占用

  1. 获取该进程的pid` ps -ef | grep dana-invoice-service`
  2. 查看资源占用`top -p 8756`

* [/dev/random 与 /dev/urandom 的区别](http://blog.lujun9972.win/blog/2018/02/05/-dev-urandom%E5%92%8C-dev-random%E7%9A%84%E5%8C%BA%E5%88%AB%E6%98%AF%E4%BB%80%E4%B9%88/index.html)

  * 前者是真随机，依赖计算机系统熵值，有可能被挂起
  * 后者是伪随机
  * 一般用后者
  * c：字符设备文件

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
  
  sudo hostnamectl set-hostname masterkai   更改主机名为masterkai
  
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

* datagrip快捷键

  ```bash
  #搜索
  Ctrl+shift+F
  #查找引用，快速找到所有在符号处引用该符号的代码，无论该符号是类，方法，字段，参数还是其他语句的一部分。
  Alt + Shift + 7
  ```

* goland快捷键

  ```bash
  #整理代码
  Ctrl+alt+L
  #返回上一级代码
  alt+左
  #跳转到函数调用处
  Ctrl+B
  ```
  
  

# 工具

* markdown语法

  [菜鸟教程](https://www.runoob.com/markdown/md-link.html)

* [typora](https://sspai.com/post/54912)

  * todo list

    - [x] 看书
    - [ ] 做饭

    ```bash
    - [x] 看书
    - [] 做饭
    ```

  * 左侧阴影

    > 从前有座山，
    >
    > 山里有座庙

    ```bash
    > 从前有座山
    > 山里有座庙
    ```

  * 一个单元格里放两行字

    | 小明 | 小刚         |
    | ---- | ------------ |
    | good | good <br>bad |

    ```bash
    good <br> bad
    ```

  * typora在绘制很长的表格时，可配合wps进行操作

* [jetbrains mono编程字体下载](https://www.jetbrains.com/lp/mono/#support-languages)

  * https://blog.csdn.net/qq_31061615/article/details/104751496

* [typora画图](https://zhuanlan.zhihu.com/p/172635547)

  * https://www.cnblogs.com/codeclock/p/13634272.html
  * https://www.jianshu.com/p/7ddbb7dc8fec
  * https://cloud.tencent.com/developer/article/1334691

* [python的C与C++扩展编程:Cython和pybind11](https://zhuanlan.zhihu.com/p/49737946)

* [datagrip使用指南](https://www.jb51.net/softjc/666676.html)

* goland

  * `shift+alt+左`，回退页面

* vscode

  * 进入/退出全屏:`F11`
  * 切换主题（背景颜色、代码高亮）：`Ctrl+K, Ctrl+T`
  * 使用`Darcula theme`插件

* [markdown画图](https://mermaid-js.github.io/mermaid/#/)

* vps及翻墙

  * https://blog.starryvoid.com/archives/76.html

* [谷歌安装器](https://xiaomifirmware.com/downloads/download-google-installer-3-0-xiaomi/)

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
  #将文件中的所有manufacture_device替换为device，/g 表示一行上的替换所有的匹配），s表示替换
  :%s/manufacture_device/device/g
  #查找device,摁N查找下一个，摁n查找上一个
  :/device
  ```

* [git](https://git-scm.com/book/zh/v2/Git-%E5%88%86%E6%94%AF-%E8%BF%9C%E7%A8%8B%E5%88%86%E6%94%AF)

  ```bash
  Run
  
    git config --global user.email "you@example.com"
    git config --global user.name "Your Name"
  
  to set your account's default identity.
  Omit --global to set the identity only in this repository.
  
  git config --global user.email "kzhang1@mail.ustc.edu.cn"
  git config --global user.name "Plt-张凱"
  #删除本地分支
  git branch -d branchname
  #删除远程分支
  git push origin --delete feat/add_cloud-service_alisz
  #展示所有分支
  git branch -a
  #恢复修改
  git restore
  git checkout .
  #新建并切换分支
  git checkout -b branchname
  #切换分支
  git checkout branchname
  #当本地当前分支落后于远程master分支时
  git rebase master
  git pull --rebase
  
  #变基
  git checkout experiment
  git rebase master
  git checkout master
  git merge experiment
  
  git status                               # 查看文件的状态 
  git add -A                              # 提交所有变化
  git add -u                              # 提交被修改(modified)和被删除(deleted)文件，不包括新文件(new)
  git add .                                 # 提交新文件(new)和被修改(modified)文件，不包括被删除(deleted)文件
  git log
  git branch -u debug origin/debug       # 把本地的debug变为origin/debug的跟踪分支（--set-upstream）
  git tag                                                     # 列出当前的标签
  git tag -a v1.0 -m "xxx"                         # 创建标签
  git tag -d v1.0                                        # 删除标签
  git push origin --tags                            # 推送本地标签到远程仓库
  git push origin :refs/tags/v1.0             # 删除远程仓库标签
  
  ```
  
  * remote，origin区别
    * 一个本地仓库可能跟踪多个远程仓库（虽然一般情况下，只会跟踪一个远程仓库），这个远程叫做remote；多个远程仓库需要区分，所以有一个远程仓库叫origin（自己起名），另一个远程仓库可能叫origin_2
  
* [sed](https://coolshell.cn/articles/9104.html)

  * 处理指定行；用分隔符分隔指定行；打印第几个字段等

* [awk](https://coolshell.cn/articles/9070.html)

  * https://www.ruanyifeng.com/blog/2018/11/awk.html
  * `a`:append; `d`:delete;`s`:switch，替换;`N`:next，处理下一行；
  * 不同的功能会使用不同的语法
  * `-n`仅输出被处理过的行，而不是输出所有行

* grep
  * `grep "good" example.txt| grep "bad" `既要有good也要有bad
  * `grep -E "good|bad|" example.txt`good或者bad
  * `grep "^g"`开头是g
  * `-i`忽略大小写；`-n`显示行号；`-v`反向查找
  
* [wechat](https://appdb.winehq.org/objectManager.php?sClass=version&iId=37758)

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


* 退出git编辑器：摁住ESP，快速摁ZZ两

* cenos下安装`g++`

  * `yum install gcc-c++`

* 设置主机名

  `hostnamectl set-hostname masterkai`

* [linux查看当前目录下文件个数](http://noahsnail.com/2017/02/07/2017-02-07-Linux%E7%BB%9F%E8%AE%A1%E6%96%87%E4%BB%B6%E5%A4%B9%E4%B8%8B%E7%9A%84%E6%96%87%E4%BB%B6%E6%95%B0%E7%9B%AE/)

  ```bash
  #统计当前目录下文件的个数（不包括目录）
  ls -l | grep "^-" | wc -l
  #统计当前目录下文件的个数（包括子目录）
  ls -lR| grep "^-" | wc -l
  #查看某目录下文件夹(目录)的个数（包括子目录）
  ls -lR | grep "^d" | wc -l
  ```

  

* [git删除分支](https://blog.csdn.net/It_BeeCoder/article/details/90229929)

  * `git branch -D feat/relay`

* git tag

  ```bash
  git tag v1.1.1
  git push origin --tags
  #后期加标签
  git log
  git tag -a v1.2 9fceb02
  ```
  
* [安装证书](http://notes.maxwi.com/2017/10/14/certificates-import-linux/)

* 关闭终端后继续运行相应进程https://www.cnblogs.com/baby123/p/6477429.html

  ```bash
  nohup ./test.sh &
  ```

  

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

* dig查询dns解析过程

  ```bash
  dig +trace baidu.com
  ```

  

# 操作系统

* [堆和栈的区别](https://blog.csdn.net/hairetz/article/details/4141043)
  * https://blog.csdn.net/K346K346/article/details/80849966
  * https://www.jianshu.com/p/05b4830a0010
  * [为什么要区分堆和栈](https://blog.csdn.net/a514371309/article/details/77987349)
* 进程和线程
  * 进程是操作系统分配资源的单位
  * 线程是CPU调度和分配的最小单位；多个线程共享进程内的地址空间；线程独立拥有自己的堆栈和局部变量
  * 线程可以在用户态实现，也可以在内核态实现
* 用户级线程模型：
    * 用户线程与内核线程是多对一的映射关系，即，一个进程对应一个内核线程
    * 优点：用户线程切换更开销小，更轻量
    * [缺点](https://segmentfault.com/a/1190000015464889)：当正在运行的用户线程发生io而阻塞时，CPU会被让出，导致该进程内的其他线程也无法使用CPU
  * 内核级线程
* [变量名怎么存储](https://www.zhihu.com/question/34266997)
* [编程语言的可移植性](https://www.cnblogs.com/Braveliu/p/6109057.html)
* [指令集](https://zhuanlan.zhihu.com/p/113157931)
    * 复杂指令集：`X86`和`-X86-64`两种架构，对应操作系统：Windows、macOS、Linux
    * 精简指令集：`arm`架构，对应的操作系统：安卓、ios、Windows phone
    * `X86-64`=`X64`=`AMD64`:均代表`X86`架构的64位；`X86`代表代表`X86`架构的32位
    * 64位处理器可以装32位操作系统；32位处理器不可以装64位系统；高版本兼容低版本
* 寄存器和高速缓存cache
    * 寄存器是CPU的一部分
    * cache

# 算法

* [学习路径](https://www.zhihu.com/question/23148377)

# 数据库

* 资料

  [Ubuntu Linux 上安装和使用开源数据库 PostgreSQL](https://linux.cn/article-11480-1.html) <https://www.jianshu.com/p/f23ae3798fde>

* InfluxDb

  * https://www.jianshu.com/p/d71646c08317

* timescaledb

  * http://www.pgsql.tech/project_303_10000020

* redis

  * https://zhuanlan.zhihu.com/p/78034665

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
  * 插入`inet`类型和`date`类型`"insert into table values (date '2022-01-21',12,'110.77.137.2')"`

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
    
  * 聚合

    * 字段之间的关系可能是“一对多”，如班级和学生，那么应当`group by 班级`而不是`group by 学生`
  * `group by `一定伴随着聚合函数
    
  * 聚合之后就会产生一张新表，如果需要对新表的字段进行筛选，那么就需要`having`语句（为什么不用`where`？因为`where`筛选的是旧表）
    
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

* [`.json`读取为表](https://qa.1r1g.com/sf/ask/2745706771/)

  * `.json`

    ```json
    [{"client_id":"27","company_id":"3","id":"9271","ip":"119.4.252.159","register_type":"2","time":"2015-01-02T05:34:48Z"},
              {"client_id":"27","company_id":"3","id":"9289","ip":"115.55.19.114","register_type":"2","time":"2015-01-02T08:31:53Z"}]
    ```

  * SQL代码

    ```sql
    create table test_consumer_kai_test
    (
        id            bigint,
        company_id    integer,
        client_id     integer,
        time          timestamp with time zone,
        ip            inet,
        register_type smallint
    );
    
    create unlogged table customer_import (doc json);
    copy customer_import from '/home/kai/Desktop/bi-data-backup/temptable/test_consumer_kai/2015_2_1-2015_2_2.json' ;
    insert into test_consumer_kai_test (id, company_id, client_id,time,ip,register_type)
    select p.*
    from customer_import l
      cross join lateral json_populate_recordset(null::test_consumer_kai_test, doc) as p
    ```

    

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
  
  * AWSEU1:法兰克福
  * AWSEU2：爱尔兰，用于判重、router，作用是中心节点
    * 判重：用户注册的手机号、邮箱是否注册过
    * router：各分支节点查router，并缓存在分支节点的router表中。AWSEU2集中存储router表。查询分布式，各接入节点查询router信息时，只需要查询分支节点的router表。
  
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

# 信息安全

* 三要素：保密性（tls）、完整性、可用性(DDOS攻击)
* [CC攻击和DDOS攻击](https://blog.csdn.net/weixin_33717298/article/details/85975018)
  * CC针对应用层，针对网页，此网页会查询数据库，消耗大量资源
  * DDOS（分布式拒绝服务）针对网络层，针对IP，针对服务器
* ssl
  * https://www.jianshu.com/p/3665fbfc2243
* 公私钥作用：
  * 公钥用于加密
  * 私钥用于身份认证
  * 应用场景：
    * 客户端免登录：
      1. 登录gitlab。将个人用ssh-keygen命令生成的公钥上传到gitlab，将免去密码登录
      2. 登录跳板机。将跳板机为个人生成的私钥下载到本地，用`ssh -i *.pem username@hostname`即可免密登录
    * 服务器认证
      * 服务器将CA证书（含服务器公钥）发给客户端，客户端用该公钥加密自己输入的账号、密码，只有拥有该私钥的服务器可以解密
      * 自签发证书只能用于加密通信，但不能解决中间人攻击。因此，需要权威CA签发证书。
* [OCSP](https://www.cnblogs.com/penghuster/p/6895714.html#:~:text=ocsp%EF%BC%88The%20Online%20Certificate,Status%20Protocol%EF%BC%89%E6%98%AF%E4%B8%80%E7%A7%8D%E9%AA%8C%E8%AF%81%E8%AF%81%E4%B9%A6%E7%8A%B6%E6%80%81%E7%9A%84%E4%B8%80%E7%A7%8D%E6%96%B9%E5%BC%8F%EF%BC%8C%E4%B9%9F%E6%98%AFCRL%EF%BC%88certificate%20revocation%20list%EF%BC%89%E8%AF%81%E4%B9%A6%E5%90%8A%E9%94%80%E7%9A%84%E4%B8%80%E7%A7%8D%E6%9B%BF%E4%BB%A3%E6%96%B9%E5%BC%8F%E3%80%82)
  * 用来验证证书是否过期的协议
* 自签证书
  * https://gist.github.com/timest/56b68d4ae77160abae00b4bb28faf7f4
* DH算法
  * Alice和Bob各自拥有私钥，经协商，可以产生共享秘钥
  * 协商过程可以被外界所知，仅私钥不能泄露
* JWT(JSON WEB TOKEN)
  * https://www.jianshu.com/p/576dbf44b2ae

# 工作

* data_warehouse
  * 每日定时执行begin_daily_merge，参数中包含分支节点信息和时间信息
  * 主要步骤包括：import和merge，此外还有clean等
  * import步骤：中心节点branch表中包含了各分支节点的账号密码等信息，中心节点通过dblink函数连接各分支节点，并在分支节点执行import操作。该操作主要从日志中的.csv 文件导入分支节点相应的表
  * merge步骤：函数以init_开头，通过dblink用SQL语句操作分支节点，这些语句的特点是广泛应用了group，count等操作
  * 中心节点每日执行定时任务，执行成功与否记录在表task_status，1代表正在执行，2代表完成。还记录了开始时间，结束时间，节点号
  
  ```mermaid
  graph TD;
      A(begin_daily_merge)-->|"先执行"|B(daily_import_data);
      A(begin_daily_merge)-->|"再执行"|C(daily_merge);
      A(begin_daily_merge)-->|"最后执行"|D(daily_clean);
      B-->|"dblink连接分支节点"|E(start_import);
      E-->F(update_basic_dict);
      E-->G(import_active_consumer);
      E-->Z(...);
  %%    E-->H(import_active_device);
  %%    E-->I(import_bucket_storage_analysis);
  %%    E-->J(import_bucket_cloud_service);
  %%    E-->K(import_relay_log);
  %%    E-->L(import_page_view);
  %%    E-->M(import_vas_bi_event);
      
      C-->N(init_operation);
      C-->O(init_form);
      C-->P(init_subscribe);
      C-->Y(...);
  %%    C-->Q(init_consumer);
  %%    C-->R(init_device);
  %%    C-->S(init_state);
  %%    C-->T(init_retention);
      C-->U(init_active_consumer);
  %%    C-->V(init_active_device);
  %%    C-->W(init_storage);
  %%    C-->X(init_relay);
      
      U-->1(init_active_device_daily_summary);
      U-->2(init_active_device_7_day_summary);
      U-->999(...);
  %%    U-->3(init_active_device_30_day_summary);
  %%    U-->4(init_active_device_over_1_week_in_30_day_summary);
  %%    U-->5(init_active_device_over_2_week_in_30_day_summary);
  %%    U-->6(init_active_device_over_4_week_in_30_day_summary);
      
      1-->|"dblink连接分支节点"|a(table:active_device);
      1-->|"dblink连接分支节点"|b(table:ip_location);
      1-->|"dblink连接分支节点"|c(table:device);
      
  ```
  
  
  
  ```plsql
  ├── branch
  │   ├── biz
  │   │   ├── biz
  │   │   │   ├── function
  │   │   │   │   ├── build_state
  │   │   │   │   │   └── build_state.sql
  │   │   │   │   └── load_data
  │   │   │   │       ├── load_active_consumer.sql
  │   │   │   │       ├── load_active_device.sql
  │   │   │   │       ├── load_bucket_cloud_service.sql
  │   │   │   │       ├── load_bucket_storage_analysis.sql
  │   │   │   │       ├── load_page_view.sql
  │   │   │   │       ├── load_relay_info.sql
  │   │   │   │       ├── load_vas_bi_event.sql
  │   │   │   │       └── start_load.sql
  │   │   │   └── table
  │   │   │       ├── basic.sql
  │   │   │       ├── online_device.sql
  │   │   │       ├── origin_data.sql
  │   │   │       └── state.sql
  │   │   └── dict
  │   │       ├── extension.sql
  │   │       ├── foreign_table.sql
  │   │       ├── function.sql
  │   │       └── table.sql
  │   └── operations
  │       ├── makeup
  │       │   ├── backup.sql
  │       │   ├── ip_location.sql
  │       │   └── sync.sql
  │       └── tools
  │           └── tools.sql
  ├── center
  │   ├── biz
  │   │   ├── bill
  │   │   │   ├── calculate.sql
  │   │   │   ├── load.sql
  │   │   │   └── month-bill.sql
  │   │   ├── dict
  │   │   │   ├── dict_table.sql
  │   │   │   ├── extension.sql
  │   │   │   └── function.sql
  │   │   ├── operation
  │   │   │   ├── calculate.sql
  │   │   │   └── table.sql
  │   │   ├── other
  │   │   │   ├── function
  │   │   │   │   ├── active.sql
  │   │   │   │   ├── consumer.sql
  │   │   │   │   ├── device.sql
  │   │   │   │   ├── form_monthly.sql
  │   │   │   │   ├── form.sql
  │   │   │   │   ├── form_weekly.sql
  │   │   │   │   ├── retention_monthly.sql
  │   │   │   │   ├── retention.sql
  │   │   │   │   ├── retention_weekly.sql
  │   │   │   │   ├── state.sql
  │   │   │   │   └── subscribe.sql
  │   │   │   └── table
  │   │   │       ├── active.sql
  │   │   │       ├── consumer.sql
  │   │   │       ├── device.sql
  │   │   │       ├── form_retention.sql
  │   │   │       ├── form.sql
  │   │   │       ├── retention.sql
  │   │   │       ├── state.sql
  │   │   │       └── subscribe.sql
  │   │   ├── relay
  │   │   │   ├── function
  │   │   │   │   ├── relay_daily.sql
  │   │   │   │   ├── relay_monthly.sql
  │   │   │   │   └── relay_weekly.sql
  │   │   │   └── table
  │   │   │       ├── relay_daily.sql
  │   │   │       ├── relay_monthly.sql
  │   │   │       └── relay_weekly.sql
  │   │   └── storage
  │   │       ├── function
  │   │       │   └── storage_daily.sql
  │   │       └── table
  │   │           └── storage_daily.sql
  │   └── operations
  │       ├── alarm
  │       │   └── alarm.sql
  │       ├── makeup
  │       │   └── center.sql
  │       ├── timer
  │       │   ├── daily_start.sql								#每日定时任务，由此开启
  │       │   ├── monthly_start.sql
  │       │   └── weekly_start.sql
  │       └── tools
  │           └── tools.sql
  ├── doc
  │   └── 部署文档.md
  ├── online-query
  │   └── order-statistics.sql
  ├── README.md
  └── test.sql
  
  ```
  
* gslb---global service load balance

* bi-relay-statistics
  
  * 做了什么
  
    * 每20s拉取阿里云relay日志，先分发，后消费
    * 数据流转路径：一条一条解析拉从阿里云日志服务拉下来的日志，放入`it.consumeChan`管道，另一个goroutine`it.write`监听该管道，一旦有数据，就放入类型为`slice`的`buffer`中，当buffer达到查询阈值时，向数据库集中查询该批buffer的用户信息、设备信息，查到以后，一条一条写入`.csv`文件
  
  * 数据来源：
  
    * 云厂商日志
    
  * `it.client.PullLogs(it.project, it.logstore, shard.ShardID, nextCursor, endCursor, 10)`
  
  * 数据去处：
  
    * /media/efs/dana/dbd/bi-relay-statistics/logstore/
    * 例：`/media/efs/dana/dbd/bi-relay-statistics/logstore/sz-p2p-relay-dbd-common-2021-02-25`
  
  * 直接拉下来的阿里云relay日志中有一个字段`role`，该字段告诉我们，该条日志所属节点是什么，去该节点就可以查到用户信息和设备信息
  
  * 配置文件中有
  
    ```bash
    relay_log:
      project: danale-sz-p2p-relay-log
      logstore:
        - sz-p2p-relay-dbd-common
    ```
  
    该参数是为了拉日志，即，连接该某云厂商的sdk后，从哪里下载日志
  
  * main.go文件中base.Start进入主要部分
  
  * consumer.go文件最重要的两个函数it.write和it.consume，先执行前者，后执行后者，但阅读代码应当先读后者
  
  * consumer.go消费的是云厂商的日志（数据来源），需要填云厂商的用户名密码
  
  * it.consume中，在管道consumeChan中经解析parse等得到除UserClientCode，UserCompanyCode，DeviceProductCode，DeviceCompanyCode这四个字段外的所有数据
  
  * it.write中，从管道consumeChan中读取数据到buffer，并通过查询postgresql数据库（代码中体现为package psql）得到四个字段对应的值（以前通过查询线上数据库，使用package dorm），将完善后的数据写入 /media/efs/dana/dbd/bi-binlog/logstore，后续供data_warehouse读取
  
  *  /media/efs/dana/dbd/bi-relay-statistics/logs是go代码执行产生的日志文件，/media/efs/dana/dbd/bi-relay-statistics/logstore是go代码的目标输出
  
  * /media/efs/dana/dbd/bi-relay-statistics/timer.sh定时将/media/efs/dana/dbd/bi-relay-statistics/logstore中昨天的文件复制到/opt/oss/logstore/relay-connection-information，并将/media/efs/dana/dbd/bi-relay-statistics/logstore目录下15天前的数据删除
  
* bi-relay-log-classify

  * 数据到哪里去
    * `logstore/relay-classified-log/{date}/{node}/{timestamp}.csv`该地址跟程序部署地不一致，在OSS上

* bi-relay-log-consume

  * 数据到哪里去
    * `logstore/sz-p2p-relay-dbd-common-2021-02-25`

* bi-bucket-storage-analysis

  * 做了什么
    
    * 分为两个任务：分析云存储空间大小、分析云服务是否付费
    
  * 数据从哪里来
    
    * ods连接云厂商
    
    * `dana_cloud_info`表和`dana_cloud_order_check`表中查"dana_cloud_info.device_id", "dana_cloud_info.service_id", "dana_cloud_order_check.total_fee", "dana_cloud_info.service_arg"
    
  * 数据到哪里去
  
  * 分析结果存储为`bd/v2/origin-data/bucket-analysis/{data}/{save_site}/{snap|clips|rt|cloud_service}.csv`
    
  * 有用的代码片段
  
    * ```go
      //查表
      product := make([]DictProductJoinDictCompany, 0)
      err = danaSyncOrmCli.Table("dict_product").Join("INNER", "dict_company", "dict_product.company_id = dict_company.id").Find(&product)
      	
      ```
  
    * 


* bi-online-device

  * 做了什么
    * 读取阿里云的设备在离线日志，插入postgresql表
  * 数据来源
    * 使用阿里云SDK读取`github.com/aliyun/aliyun-log-go-sdk`
  * 数据去处
    * postgresql的`online_device`
    * 增加数据`insert into online_device (device_code, ip) values（），（）`
    * 删除数据`delete from online_device where device_code in '000','111'`

* bi-loghub

  * 做了什么

    * 同步阿里云设备上线指标 p2p、用户活跃指标 application，写入logstore

  * 数据来源

    * 阿里云日志：北京时间每日8点定时上报日志、上线日志、下线日志（暂时不懂第一种日志是干啥的）

    * ```bash
      device_log:
        project: danale-sz-p2p-relay-log
        logstore:
          - sz-p2p
          - bj-p2p
      app_log:
        project: danale-openapi
        logstore:
          - application_log
      # 统计间隔, 单位秒
      interval: 10
      ```

    * device从日志中解析出`deviceId, ip`

    * app从日志中解析出`userId, ip`

  * 数据去处

    * `logstore/"+it.logstore+"-"+curDate`
    * 例子：application_log-2021-02-18，  bj-p2p-2021-02-18，  sz-p2p-2021-02-18

* bi-cloud-service-provider-bill

  * 做了什么
    * 从阿里云、华为云、AWS的bucket中，挪取云厂商账单数据，放入阿里云OSS中，供data_warehouse读取
  * 数据来源
    * 阿里云bucket`1825188109189137_BillingItemDetail_%s`,`%s`代表日期
    * 华为云bucket`%s/Danale_PriceFactorBillDetail_%s_1.csv`,`%s`代表日期
    * AWSbucket`billing/dcp-iaas-billing-aws/%s-%s/dcp-iaas-billing-aws-Manifest.json`，该数据下载完需解压
  * 数据去处
    * 阿里云bucket放入OSS`bd/v2/origin-data/bill/aliyun/2006-01-01.csv`
    * 华为云bucket放入OSS`bd/v2/origin-data/bill/huawei/2006-01-01.csv`
    * AWS放入OSS``bd/v2/origin-data/bill/aws/%s-%03d.csv`

* bi-binlog

  * ```go
    //Init方法是公用的，所以要在Common结构体中定义；Run方法是不公用的，所以要在PositionMode和GtidMode结构体中分别定义
    type Mode interface {
    	Init(taskName string, config *internal.Task)
    	Run()
    	Close()
    }
    type Common struct {
    	taskName string
    	can      *canal.Canal
    	save     func()
    	ctx      context.Context
    }
    //正因为Mode接口中的方法有这种区别，所以需要Common结构体
    type PositionMode struct {
    	Common
    }
    
    type GtidMode struct {
    	Common
    }
    func (it *Common) Init(taskName string, config *internal.Task){}
    func (it *PositionMode) Run() {}
    func (it *GtidMode) Run() {}
    ```

  * 做了什么

    * 该代码消费mysql的binlog（描述了mysql中的增删改操作），然后插入postgresql数据仓库

    * 可以记录binlog日志消费到哪里，以便下一次从该处继续消费，记录消费位置的代码片段

      ```go
      set := it.can.SyncedGTIDSet()
      	if err := ioutil.WriteFile(it.taskName+".dat", []byte(set.String()), 0644); err != nil
      ```

      

  * 数据从哪里来

    * mysql产生的binlog
    * 使用工具canal包

  * 数据到哪里去

    * postgresql

  * 程序理解流图

  ```mermaid
  graph TD;
      A(begin_daily_merge)-->|"先执行"|B(daily_import_data);
  ```

* 分支节点各表含义

  * `valid_device`是从`active_device`中选择某段日期，筛选出来的
  * 因此，该有效设备这个概念是依托于具体的指标而存在的，如对于指标“30日超过7天的活跃设备”，那么有效设备指在最近30天内所有的活跃设备
  
  ```plsql
  consumer_code.code=active_consumer.code
  ```
```
  
* 有效设备、激活设备、活跃设备、有效活跃设备
  
    * 激活设备：设备在平台注册，线上表`dana_sync.video_device`有该设备的记录
* 有效设备：设备与APP绑定，线上表`dana_sync.video_user_device_map`
  
  * 活跃设备：即与平台保持心跳的设备
  * 绑定设备：
  
* 查找各虚拟机上部署应用

  ```bash
  sudo su -c '
  touch /home/sreuser/zhangkai
  echo "***************************************************************************************************************" >>/home/sreuser/zhangkai
  echo "                                        呜呜呜，又是一台新机器                                                    " >>/home/sreuser/zhangkai
  echo "***************************************************************************************************************" >>/home/sreuser/zhangkai
  cd /etc/systemd/system
  
  #标记为systemd
  #需排除aliyun.service,AssistDaemon.service,CmsGoAgent.service
  for var in `find . -maxdepth 1 -name "*.service" ! -name "aliyun.service" ! -name "AssistDaemon.service" ! -name "CmsGoAgent.service" |sed 's#.*/##'`
  do
  	cat $var |grep "WorkingDirectory\|ExecStart" >>/home/sreuser/zhangkai
  	echo "*********" >>/home/sreuser/zhangkai
  	#仅对筛选出来的service执行此命令
  	systemctl status $var | grep "Active" >>/home/sreuser/zhangkai
  	echo "####################" >>/home/sreuser/zhangkai
  done
  echo "#########################################################" >>/home/sreuser/zhangkai
  
  #标记为docker
  docker ps -a >> /home/sreuser/zhangkai
  echo "#########################################################" >>/home/sreuser/zhangkai
  #标记为crontab
  crontab -l >> /home/sreuser/zhangkai
  echo "#########################################################" >>/home/sreuser/zhangkai
  '
```

  

* 业务理解

  * 云平台成本：带宽（relay）、存储（bucket）、人脸识别算法和算力（faceR，按照调用次数收费）
* 设备侧的微服务：ip_devce_h,DNS,device_service,dms（为什么设备侧这么多微服务，APP侧只有一个?是不是有历史原因，比如平台和设备侧SDK关系亲密？）
  * 设备固件内预埋DNS服务器地址
* 平台侧微服务：cloud_service_api
  * APP侧微服务：UDH
* API网关：根据不同的功能，可能有不同的网关。比如做协议转换的网关，可以对https进行解密，转为http，这样发到其他服务器的请求就不用解密了
  * ferry是为了跨节点调用，类似于路由器，是一个API网关吗？
* 为什么需要流量治理，两个例子：
    * 灰度发布时，需要把更多或某部分流量转发到部署高版本服务的服务器上
  * 代替服务注册和发现，不需要rpc，通过网格治理，从某个地方发出来的流量，就认为是哪个服务
  * 为什么需要servicemesh：微服务通过rpc进行注册与发现，rpc写在代码中，Go应用有一套rpc，Python应用也有一套rpc，得整两套rpc，很烦。如果通过servicemesh，网格治理，就可以实现代码功能和服务注册发现解耦。
* 集群：即功能的横向扩展：虚拟机集群，redis集群（其实是不同虚拟机上都部署了redis，各redis完全相同）
  * 虚拟机：jms上列出来的就是虚拟机，云厂商把一个机器8核16G，拆成8个1核2G的虚拟机
* k8s: 在100台虚拟机上部署相同的应用，非常费劲，配置文件不一致容易出错。比如，40台配置文件一致，60台配置文件一致，共2份配置文件。通过k8s将应用程序和配置文件打包成2个应用，放在不同的企业空间，可以实现一键部署100台机器。
  * ferry是一个组件，
    * 当发生跨节点调用时，有两种方式。一种是直接通过ip、port调用其他节点的服务，另一种是通过本节点的ferry转发给另一个节点的ferry。
    * 安全性：可以只留ferry服务端口对外，而不是所有的应用都对外
    * 流量监控：所有跨节点调用都在ferry处可查（类似中央集权）
  * 为什么要用阿里云的日志服务sls
  * 虚拟机磁盘一般是40G，而有的应用产生的日志非常大，放不下
    * 非常大的日志文件进行全量搜索非常慢，而阿里云的日志服务可以查的很快，支持SQL搜索等
* etcd用于服务注册与发现
    * 服务注册：应用A将ip、端口、token（此应用自己生成的）注册到etcd
    * 服务发现：应用B从etcd订阅得到应用A的ip、端口、token，然后拿着这些东西直接去调应用A，应用A会对token进行验证
  * falcon-agent和pprof的区别
  * falcon-agent在代码中可以调用bmon等package；将程序运行状态发送到运行在虚拟机的应用falcon-agent，然后发送给中心节点，然后配置告警规则等
    * pprof在代码中可以调用pprof等package；在代码运行时，可以通过ip、端口打开浏览器，观察到应用占用的CPU、内存等，如果发现内存占用率过高，可以查看程序的调用栈，从而定位问题；一旦程序崩溃，则不能再使用
  * 为什么容器需要限制CPU、内存等资源
    
    * 如果不限制，一个程序内存飙升，可能导致整台虚拟机上的所有应用崩溃
  * k8s中的亲和度affinity
    
    * 给不同的node打label，然后给应用的配置文件中添加亲和度（即某种label），这样k8s调度pod时，会优先部署在对应label的node上
  * 就绪指针和存活指针
    * 存活指针：该容器是否在运行
    * 就绪指针：该容器是否可以处理业务
  * falcon-agent-sidercar
  
    * 每个pod中放一个falcon-agent-sidercar；每个node放一个falcon-agent；两者均监听1988端口
  
    * 应用向falcon-agent-sidercar应用的1988端口发出http请求，falcon-agent-sidercar处理该请求，并补充tag和Endpoint信息，发给falcon-plus
    * 因为一个node上可能跑很多pod，这些pod有可能出现相同的应用，所以有可能监控信息被覆盖，所以需要tag区分不同的应用
  * CI，Dockerfile，Makefile
  
    1. 代码推到代码仓库gitlab
    2. `.gitlab-ci.yml`中定义了master打了tag会自动触发CI流程
    3. 按照`.gitlab-ci.yml`定义，先进行对代码进行build，即利用Makefile生成二进制文件
    4. 按照`.gitlab-ci.yml`定义，对二进制文件打包为镜像，利用了Dockerfile
    5. 推到镜像仓库
  * 服务端口
  
    * 容器端口由代码决定，k8s自动发现。其实代码监听某个端口，就是在那个端口建立socket连接，肯定得调用操作系统，所以操作系统是知道该端口的。
    * 服务端口自己填
    * 节点端口k8s自动分配
  * iot-service
    * 架构图：https://gitlab.dana-tech.com/pg-local/iot-service/blob/master/docs/pg_iot.md
  * 业务埋点和程序日志
  
    * 都是为了定位问题或者获得程序执行过程中的信息
    * 业务埋点针对性更强，只针对感兴趣的业务问题
  
    * 业务埋点：大都在设备侧，平台侧很少；通过http report；上报内容包括：从用户点击播放实时视频，到收到视频第一帧所花费的时间；发送到pecker，供分析使用
    * 程序日志：通过`logx.X.infom`等；可能仅保存15天
  * device shadow
  
    * 设备影子：存储了设备属性，放在云端；设备和设备影子需要及时同步
    * 需求来源：对于低功耗设备，每一次查询设备都需要唤醒设备，产生耗电，因此在云端需要设备影子，这样不需要频繁唤醒设备
  * 物联云平台架构
    * 数据节点：redis、mysql
    
    * 接入节点：迪拜、俄罗斯等，设备就近接入，会部署：
      * dana-dns(ddp)
      * device-api：用于注册、登录、业务。平台与设备进行https交互的模块， 初步的接口设计包括：设备注册，设备登录，设备的时区、云服务等信息获取， 设备注册及登录为新增逻辑， 其他逻辑均从device-dms-api继承而来， 此功能模块完善后，即可取消device-dms-api模块， 后续可能也会将policy的逻辑加入到本模块中。（当前dms在policy的下一级，及设备链接由dns直接转到policy， 由policy到dms， device_api兼有两者的功能）
      * conn-policy(ddp)
        * 似乎跟router-policy是一回事，功能之一是分配用户节点，如绑定一个门铃，分配用户节点为32，但似乎不等于数据节点
      * p2p
      * relay
      * natcheck
      
    * 设备上线流程：
      1. 设备内预埋了dana-dns的域名和conn-policy的域名
      2. 通过public-dns，设备知道了dana-dns域名对应的IP
      3. 通过dana-dns，获得device-api的IP
      4. 通过dana-dns，获得conn-policy的IP
      
    * 为什么需要接入节点?
    
      * 就近接入，速度快
      * 隐私政策，不能把俄罗斯的数据发到欧洲
    * 接入节点用户少，因此需要“接入节点”而不是“数据节点”
  * faceR有不同的类型，通过配置文件的不同，来区分不同类型，如类型一向etcd注册faceR1 service，类型二向etcd注册faceR2 service。不同的类型可以设置多个副本
  * face detection有8个副本，每个副本放在一个64核的机器上，通过负载均衡器对外提供服务。
  * udh-api目前功能比较复杂，应当分离出网关功能和业务功能；udh-service用来查数据库
  * snap和clips根据产品型号，由芯片提供。如人形消息，会产生一张snap和一个clips，先存hbase。然后此snap会触发人脸检测，如果检测到人脸，则增加一条人脸消息索引。特殊地，由于门铃的特殊性，人会从门铃侧面转到门铃正面，设置了特殊逻辑，即每2s拍一张snap，因此门铃会有多个snap和一个clips，对于一条人形消息而言。
    * 对于每一种消息，如婴儿哭声、人形消息、人脸消息，snap作为该消息的缩略图，点击该缩略图，会播放对应的clips。因此snap并非完全无用。
  * 监控
    * 滴滴夜莺 n9e
    * open-falcon
  * 深圳节点的postgresql数据库必须通过nginx代理访问（将本机的对16080的访问请求代理到120.79.81.225:16090），而不能直接访问16090端口。是因为16090是ssl端口
  * ferry-sidecar
    * 两个作用
      1. 代替应用进行微服务注册。需求来源：放在容器中的应用不知道自己所在node的ip和端口，所以无法将该信息注册到etcd供其他微服务调用。
      2. 代替应用进行微服务鉴权。需求来源：token是实例级的，即，同一个应用不同的实例具有不同的token，因此，应用实例就变得有状态，即，某些rpc调用“非它不可”。ferry-sidecar代替微服务验证token（具体验证过程是，rpc调用该服务名得到其所有token，看我们要验证的token在不在其中），并将其替换为该node上应用的实际token，这样，即便这个token属于其他实例，ferry-sidecar总将其替换为本pod的真实token，应用就变得无状态，“并非非它不可”。

# 云计算

* [中间件](http://c.biancheng.net/view/3860.html)

* [有状态服务和无状态服务](https://zhuanlan.zhihu.com/p/65762125)
  * 信息保存在请求方是无状态的；信息保存在响应方是无状态的。
  
* Aliyun登录
  * https://signin.aliyun.com/login.htm#/main
  
* nginx
  * https://zhuanlan.zhihu.com/p/54793789
  * 代理：仅开放一个主机ip，即可通过不同端口代理访问多个内网主机。保证了内网安全。
  
* API网关

  * API 网关是 API 管理系统的一部分。API 网关会拦截所有传入的请求，然后通过 API 管理系统（该系统负责处理各种必要的功能）将其发送出去。

    API 网关的工作因实施不同而异。一些常见的功能包括：身份验证、路由、速率限制、计费、监控、分析、策略、警报和安全防护。
    
  * 管控，多渠道统一安全接入
  
  * 当云端业务发生变化时，可能只需要修改该API接口所指向的实例，而端侧不需要改动，仍然调用原来的那个接口

# 云原生

* [K8s中的external-traffic-policy](https://bbs.huaweicloud.com/blogs/158642)
  * Cluster: 流量会发往其他node
  * local：流量仅发给本node
  
* [Kubernetes的三种外部访问方式：NodePort、LoadBalancer 和 Ingress](http://dockone.io/article/4884)
  * NodePort: 所有node都开放一个端口，流量从node端口进入，经service端口转发给对应的Pod（分为Cluster和local）
  * LoadBalance: 流量流向具有独立IP的LoadBalance，然后转向对应pod
  
* 容器可以拥有自己的 **root 文件系统、自己的网络配置、自己的进程空间，甚至自己的用户 ID 空间**

* kubesphere
  * 一个deployment可以创建多个pod，即多个replicas
  * 一个workspace可以创建多个project
  
* 键值对标签的用途

  * node添加标签，pod添加selector，可以实现node亲和性调度
  * pod添加标签，service添加selector，可以将流量转发到对应的pod

* service

  * service建立了容器端口和pod端口之间的映射（一般两个端口是相同的）

  * service有自己虚拟ip
  * 跑在master上
  * 一旦service被创建，node把service name加到自己宿主机的环境变量中，供所有Pod使用
  * 

* 资料

  * 网易云，需求：https://blog.csdn.net/gui951753/article/details/81543545
  
  

# 计算机网络

* 分组交换网中的时延
  * 节点总时延=处理时延+排队时延+传输时延+传播时延
  * 处理时延：思考分组发到哪里去；比特级别的差错检测
  * 排队时延：到达该节点有很多分组，需要一个一个处理
  * 传输时延：分组有长度，由许多比特组成
  * 传播时延：分组需要在电缆上面跑
  * 传播时延是物理层的问题，其余时延是链路层的问题
* [URI、URL](https://zhuanlan.zhihu.com/p/56540212)
* [http协议](https://www.cnblogs.com/ranyonsue/p/5984001.html)
* http中的request_id
  * 一个http请求可能涉及多个子任务，这些子任务都带有相同的request_id，这样查日志就比较方便
  * https://blog.csdn.net/yongwan5637/article/details/90898556
* http中的page和page_size
  * https://crifan.github.io/http_restful_api/website/restful_experience/pagination.html
* ip数据包在路由器之间传递，源ip和目标ip一直不变（除NAT转换外），而源MAC和目标MAC一直在变化
* ARP协议
  * 源主机要给目标主机发消息，只知道目标的ip地址，不知道MAC地址，怎么办？
  * 第一步，看ARP缓存中有没有ip-MAC的映射信息，如果没有，则进行第二步
  * 第二步，ARP广播，向局域网内的所有主机发消息，”我的ip和MAC是++，谁是192.168.1.2，请回答“。如果某台主机是该ip，则向该ip发消息”我是192.168.1.2，我的MAC是++“
  * 第三步，如果目标ip不属于此网段（根据子网掩码，得到该ip的网络号，发现网络号不符），则将消息发给默认网关
  * 为啥该主机知道默认网关的ip地址？DHCP告诉他的。那么，DHCP的ip地址是怎么知道的？请看下一条DHCP
* DHCP
  * 主机有DHCP客户端，某个路由器上部署了DHCP服务器
  * 主机需要动态获取ip地址
    1. 广播，”我要一个ip地址“
    2. 可能会有很多DHCP服务器收到此消息，并七嘴八舌地回复”给你分配这个ip 192.168.1.2“；”给你分配这个ip 192.168.1.3“
    3. DHCP客户端回复某个DHCP服务器”我就要你这个ip了。192.168.1.3“
* [有了 IP 地址，为什么还要用 MAC 地址？_向往美的知乎回答](https://www.zhihu.com/question/21546408)
* [网关和路由器的区别_百哥的回答](https://www.zhihu.com/question/21787311)
  * 网关是一个用于TCP/IP通信的学术概念，路由器是一台具体的设备；路由器可以作为网关设备
  * 网关是一个网段的数据出口，该网段似乎只有一个网关；网关是一个子网的管理员或看门大爷（根据子网掩码和ip地址的“与”运算，得到该ip的网络号，就可以判断是否属于该子网），如果想跟其他子网通信，就需要网关允许；网关工作在传输层及以上
  * 网关是一个软件，可以当网关的东西还真不少，比如防火墙、路由器、三层交换机、电脑、部分MCU、一些存储设备，还有一些设备也很古怪的可以设置成网关，比如视频会议终端、语音网关等。所以说具有路由功能的硬件原则上都可以当网关使唤
  * 路由器的功能：连接不同网段、NAT转换、异构网络互联

# 知识储备

* [danale_gitlab](https://gitlab.dana-tech.com/be/plt-team/plt-biz-team/plt-biz-team-doc/tree/master/biz-group)

* [10分钟看懂Docker和K8S](https://zhuanlan.zhihu.com/p/53260098)

* [迭代器](https://mp.weixin.qq.com/s/Be4tHnR0BY-C__xoPPBjhQ)

* [指针与引用](https://www.cnblogs.com/pythonista/p/11178705.html)

* [google全家桶安装](https://www.zhihu.com/question/24693274)

* [为什么我们需要制品管理](https://xie.infoq.cn/article/f5f72e8a25f5b6581c0a2fb66)

* [APNs推送机制](https://www.jianshu.com/p/958ca3fd3cef)

* [交叉编译详解](https://blog.csdn.net/pengfei240/article/details/52912833)

* [andorid消息推送](https://www.cnblogs.com/Areas/p/5757219.html)

* [API网关](https://www.jianshu.com/p/8cbf97b40c26)

* [正向代理和反向代理的区别](https://cloud.tencent.com/developer/article/1418457)
  
  * 正向代理和反向代理都架设在客户和服务器之间，提供缓存，提高访问速度
  * 正向代理为客户服务，可以访问一些受限制的资源；客户知道目标服务器的IP，但目标服务器不知道客户的IP
  * 反向代理为服务器服务，可以提供负载均衡和安全访问；此时，客户并不知道目标服务器的IP
  
* [MQTT](https://internetofthingsagenda.techtarget.com/definition/MQTT-MQ-Telemetry-Transport)

* [无状态服务和有状态服务](https://blog.csdn.net/xiangxizhishi/article/details/79434749)

    * cookie是无状态服务，用户信息可以保存在客户端
    * session是有状态服务，如http是无状态的，但电子购物时，购物车保存用户信息；用户信息保存在服务器
    * 无状态服务方便水平扩展，如增加多个实例；但有状态得保证一个事务的请求都被发送到相同的实例

* 驼峰命名和蛇形命名

    * camel case：单词之间用下划线连接，如`node_id`
    * snake case: 单词直接用大小写区分，如`nodeId`

* 手动挂载172.19.1.219。[自动挂载](https://www.cnblogs.com/darkknightzh/p/7160792.html)<https://blog.csdn.net/qq_35451572/article/details/79541106>

  ```bash
  mount -t cifs //172.19.1.219/share /mnt/device-share/ -o username=danale-guest,password=danaleguest
  ```

* 配置跳板机，利用[开源堡垒机项目](https://github.com/jumpserver/jumpserver/)

  ```bash
  1. 找文礼申请跳板机账号（文礼会发邮件）；
  2. 点击邮件中的链接，修改密码（https://jms.dana-tech.com/是自签名证书，因此Firefox浏览器打不开，换chrome可以；或者为Linux系统导入Danale证书）
  3. 在网站中绑定二因子验证、上传本机公钥、下载.pem到本机~/.ssh;
  4. 编辑/etc/profile，加入
  alias jms="ssh -p 17927 -i /home/{user_name}/.ssh/{user_name}-jumpserver.pem zhuzhenxuan@jms.dana-tech.com"
  alias jms-sit="ssh -p 18511 -i /home/{user_name}/.ssh/sit-{user_name}-jumpserver.pem sit-zhuzhenxuan@jms.sit.dana-tech.com"
  alias jms-hq="ssh -p 2222 -i /home/danale/.ssh/staff-zhuzhenxuan-jumpserver.pem staff-zhuzhenxuan@jms.ops.haique-tech.com"
  然后source /etc/profile
  5. jms登入跳板机
  ```

  * 进入jms-sit
    * 先执行`source /etc/profile`
    * 再执行`jms-sit`

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

  [线翻译工具](https://tool.lu/crontab/)

  参考资料：
  
  * <https://www.jianshu.com/p/af640f30d034>
  * <https://www.cnblogs.com/linjiqin/p/3178452.html>
  * https://blog.csdn.net/capecape/article/details/78515558
  
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
  * ECS、OSS、RDS、Redis区别
    * ECS用来跑程序，如bi-binlog
    * OSS存储大量文件
    * RDS放mySQL数据库
    * Redis放NoSQL非关系数据库

# 机器学习算法

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

    

* 安装ncnn过程中不知道什么鬼东西（似乎是TPU加速）

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

* [模型可视化工具netron](https://lutzroeder.github.io/netron/)

* onnx转ncnn(两种方法）

  * [一键转换工具](https://convertmodel.com/)

  * 使用onnx2ncnn工具

    ```bash
    ./onnx2ncnn net_sim.onnx
    ```

* 人脸检测Android的demo
  * 模型选择`Ultra-Light-Fast-Generic-Face-Detector-1MB`，图片输入`320*240`

  * 工具：https://developers.google.com/ml-kit/vision/face-detection/android#java

  * 难点

    * 图片预处理：均值、方差、size大小、RGB通道
    * 模型输出结果处理：方框、置信度、label

  * 未知

    * 帧处理和图片转换
    * 模型量化，数据也要量化（数据量化不需要减均值，除以方差）

  * 步骤：
    1. 从摄像头获取图片
    2. 图片预处理
       1. resize
       2. BGR2RGB
       3. 均值、方差处理
    3. 构造人脸检测器
    4. 将预处理图片传入人脸检测器
    5. 人脸检测器invoke，拿到模型输出
    6. 图片后处理
       1. bounding box regression
       2. confidence threshold filter
       3. non-maximum suppression
    7. 得到score和box
    
  * 任务分解

    - [x] 跑通原demo
  
    - [x] 修改build.gradle默认API，从`lib_task_api`改为`lib_interpreter`
  
      * 前者专门用于对象检测，抽象程度更高
      * 后者用`interpreter`实现，当我们用自己的模型时，可以充分进行修改
  
      ```bash
      //build.gradle  
        flavorDimensions "tfliteInference"
        productFlavors {
             // The TFLite inference is built using the TFLite Java interpreter.
             interpreter {
                 getIsDefault().set(true)			//增加此行
                 dimension "tfliteInference"
             }
             // Default: The TFLite inference is built using the TFLite Task library (high-level API).
             taskApi {
      //           getIsDefault().set(true)		//注释此行
                 dimension "tfliteInference"
             }
          }
      或者
          flavorDimensions "tfliteInference"
          productFlavors {
             // The TFLite inference is built using the TFLite Java interpreter.
             interpreter {
      //           getIsDefault().set(true)
                 dimension "tfliteInference"
             }
             // Default: The TFLite inference is built using the TFLite Task library (high-level API).
      //       taskApi {
      ////           getIsDefault().set(true)
      //           dimension "tfliteInference"
      //       }
          }
          
      //settings.gradle
      include ':app', ':lib_interpreter', ':lib_task_api'
      改为
      include ':app', ':lib_interpreter'
      
      //build.gradle(app)
      注释掉dependencies中的 taskApiImplementation project(":lib_task_api")
      ```
      
      
  
    * [ ] 跑模型python脚本
    * [ ] 模型输入需要`320*240`还是`240*320`
    * [ ] 了解安卓开发的基础知识：例如，那么多gradle是干啥的；build之后项目结构都变了；没有main函数，整个项目怎么组织起来
  
  * 原模型
  
    * 输入
      * https://tfhub.dev/tensorflow/lite-model/ssd_mobilenet_v1/1/metadata/2
      * 均值：127.5；方差：127.5
      * `1*300*300*3`
    * 输出
      * location、category、score、number of detections
  
  * ULTRA模型
  
    * 输入
  
      * ```
        fd = UltraLightFaceDetecion(model_path,
                                    input_size=(320, 240), conf_threshold=0.6,
                                    center_variance=0.1, size_variance=0.2,
                                    nms_max_output_size=200, nms_iou_threshold=0.3)
        //https://github.com/Linzaer/Ultra-Light-Fast-Generic-Face-Detector-1MB/tree/master/tflite                    
        ```
  
      * 宽：320，高：240
  
      * 置信度阈值：0.6
  
      * nms_iou_threshold_ = 0.3
  
      * img_resize = cv2.cvtColor(img_resize, cv2.COLOR_BGR2RGB) https://github.com/Linzaer/Ultra-Light-Fast-Generic-Face-Detector-1MB/blob/master/tf/det_image.py 
  
      * ```bash
        #非常确定要做这样的预处理
        img_resize = img_resize - 127.0
            img_resize = img_resize / 128.0
        ```
  
      * preprocess顺序
  
        1. BGR2RGB
        2. resize
        3. mean_normalize
        4. 展成一维`1*240*320*3`
  
    * 输出
  
      * https://github.com/Linzaer/Ultra-Light-Fast-Generic-Face-Detector-1MB/blob/master/ncnn/src/main.cpp
      
      * score，location
      
        * score:`score[0][][1]`:第一维表示图片，由于我们每次处理一张图片，所以第一维是0；第二维表示有几个框对应的分数；第三维看了python和c++代码实现都是选1，不知道为啥
      
        * location:`location[0][][]`：第一维表示图片，由于我们每次处理一张图片，所以第一维是0；第二维表示有几个框；第三维表示框位置
      
      * https://github.com/Linzaer/Ultra-Light-Fast-Generic-Face-Detector-1MB/blob/master/tf/det_image.py
      
      * 输出多个框，遍历每一个框 \# result=[background,face,x1,y1,x2,y2]
      
      * Python处理tflite模型步骤
      
        1. 取出score和boxes
      
           ```python
           #index是python api提供的数据结构，如下所示：
           # details = {
           #         'name': tensor_name,
           #         'index': tensor_index,
           #         'shape': tensor_size,
           #         'shape_signature': tensor_size_signature,
           #         'dtype': tensor_type,
           #         'quantization': tensor_quantization,
           #         'quantization_parameters': {
           #             'scales': tensor_quantization_params[0],
           #             'zero_points': tensor_quantization_params[1],
           #             'quantized_dimension': tensor_quantization_params[2],
           #         },
           #         'sparsity_parameters': tensor_sparsity_params
           #     }
           self._get_boxes_tensor = partial(self._interpreter.get_tensor,
                                                    output_details[0]["index"])
           self._get_scores_tensor = partial(self._interpreter.get_tensor,
                                                     output_details[1]["index"])
               
               # get results
               boxes = self._get_boxes_tensor()[0]
               scores = self._get_scores_tensor()[0]
           ```
        
        2. bounding box regression
        
        3. confidence threshold filter
        
        4. non-maximum suppression
  
  * 未解之谜
  
    * [ ] settings.gradle是怎样决定lib_task_api是否被编译的
    * [ ] 一个模型训练好以后，它的输入图片大小是确定的，还是任意的，还是可以在模型转换过程中被修改
    * [x] Java中的extends和implements
      * extends：继承
      * implements：实现接口
  
  * 查到的小知识
  
    * cv::Scalar(0, 255, 0)表示蓝色框 // a = blue, b = green, c = red表示RGB三个通道
  
    * 图片宽高表示
  
      * ```python
        h, w, _ = img.shape  # 高，宽
        img_resize = cv2.resize(img, (320, 240))  #宽，高
        ```
  
    * nms非极大值抑制
  
      * https://www.cnblogs.com/makefile/p/nms.html
      
    * cv2.resize
  
      * https://blog.csdn.net/jningwei/article/details/76019940
  
    * cv2.retangle
  
      * https://www.jianshu.com/p/0fb7a1077a68
  
    * bounding box
  
      * https://blog.csdn.net/zijin0802034/article/details/77685438
  
  * 可能产生bug的地方
  
    * 图片输入的宽和高
    * 用Java拿到模型输出的方式
  
  * 踩坑记录
  
    * [Android Studio报：Connection timed out: connect. If you are behind an HTTP proxy错误](https://blog.csdn.net/Ryanymk/article/details/116542100)
  
    * [开启开发者模式和USB调试](https://developer.android.com/studio/run/device)
  
    * tensorflow模型输出的检测框代表什么
  
      * 图片坐标原点在左上角
      * 图片大小表述：100 x 200 pixels (height x width)，注意是`高*宽`
      * **`bbox`**: tf.Tensor of type [`tf.float32`](https://www.tensorflow.org/api_docs/python/tf#float32) and shape `[4,]` which contains the normalized coordinates of the bounding box `[ymin, xmin, ymax, xmax]`
      * 参考资料
  
        * https://www.tensorflow.org/api_docs/python/tf/image/draw_bounding_boxes
      * https://www.tensorflow.org/datasets/api_docs/python/tfds/features/BBoxFeature
      
    * g++
    
      * `-I`只会搜索当前目录下的头文件，不会搜索子目录
      * `-L`指定库文件路径
      * `-l`指定库文件，如果库文件直接互相依赖，则被依赖项应放在后边
    
    * generate anchor
    
      ```python
      import numpy as np
      feature_maps = np.array([[40, 30], [20, 15], [10, 8], [5, 4]])
      min_boxes = np.array([[10, 16, 24], [32, 48],
                                          [64, 96], [128, 192, 256]])
      input_size = np.array((320, 240))[:, None]
      anchors = []
      for feature_map_w_h, min_box in zip(feature_maps, min_boxes):
          print('************************************')
          print(feature_map_w_h, min_box)
      
          wh_grid = min_box / input_size
          print(wh_grid)
          wh_grid = np.tile(wh_grid.T, (np.prod(feature_map_w_h), 1))
          print(wh_grid)
      
          xy_grid = np.meshgrid(range(feature_map_w_h[0]),
                                range(feature_map_w_h[1]))
          print(1, xy_grid)
          xy_grid = np.add(xy_grid, 0.5)
          print(2, xy_grid.shape)
          print(xy_grid)
          xy_grid /= feature_map_w_h[..., None, None]
          print(3, xy_grid.shape)
          print(xy_grid)
          xy_grid = np.stack(xy_grid, axis=-1)
          print(4, xy_grid.shape)
          print(xy_grid)
          xy_grid = np.tile(xy_grid, [1, 1, len(min_box)])
          print(5, xy_grid.shape)
          print(xy_grid)
          xy_grid = xy_grid.reshape(-1, 2)
          print(6, xy_grid.shape)
          print(xy_grid)
      
      
          prior = np.concatenate((xy_grid, wh_grid), axis=-1)
          anchors.append(prior)
      
      anchors = np.concatenate(anchors, axis=0)
      anchors = np.clip(anchors, 0.0, 1.0)
      ```
  
* NCNN库

  * 预构建好的：https://github.com/Tencent/ncnn/releases
  * 从源码交叉编译：https://github.com/Tencent/ncnn/wiki/how-to-build#build-for-android

# 临时记录

```plsql
create table if not exists payment_device_summary
(
    date         date    not null, --日期
    node_id      integer not null, --节点编号
    company_id   integer not null, --公司数字编号
    product_id   integer not null, --产品数字编号
    country_code char(2),          --国家代码
    city_name    varchar(80),      --城市名
    coordinates  point,            --位置
    amount       integer not null  -- 新增付费设备数量
);

select (time_bucket(''1d'', cache_payment_device.time))::date date,
                    ' || node_id || ',
                    device.company_id,
                    product_id,
                    country_code,
                    city_name,
                    point(min(coordinates[0]), min(coordinates[1])),
                    count(*)
                from cache_payment_device left join device on cache_payment_device.id = device.id left join consumer on consumer.id = consumer_id left join ip_location on range @> inet_to_box(ip)
                where cache_payment_device.time >= ''' || start_date || '+00''
                and cache_payment_device.time < ''' || end_date + 1 || '+00''
                and device.company_id is not null
                group by 1, 3, 4, 5, 6
                order by 1, 3, 4, 5 ,6

create table if not exists cache_payment_device
(
    id          bigint primary key,   --设备数字编号
    time        timestamptz not null, --初次付费时间
    consumer_id bigint                --用户数字编号
);

create table if not exists device
(
    id          bigint primary key, --设备数字编号
    company_id  integer not null,   --公司数字编号
    product_id  integer not null,   --产品数字编号
    device_type integer not null    --设备类型, 60000,60001,60002,60003,60004,60005
) partition by hash ( id );

create table if not exists consumer
(
    id            bigint primary key,   --用户数字编号
    company_id    integer     not null, --公司数字编号
    client_id     integer     not null, --客户端数字编号
    time          timestamptz not null, --用户注册的时间
    ip            inet,                 --注册时ip地址
    register_type smallint    not null  --注册类型, 0表示平台接口注册, 1表示邮箱,2表示手机,3表示Oauth注册
) partition by hash (id);

create table if not exists ip_location
(
    range        box not null, --ip范围
    country_code char(2),      --国家2位代码
    city_name    varchar(80),  --英文城市名
    coordinates  point         --城市坐标
);

create table if not exists form
(
    time                  timestamptz not null, --收款/退款/已取消/付款失败时间
    order_id              bigint      not null, --订单数字编号
    consumer_id           bigint,               --用户数字编号
    device_id             bigint,               --设备数字编号
    service_id            smallint    not null, --服务数字编号
    service_duration      smallint    not null, --时长,0表示永久
    renewal               boolean     not null, --是否是续费订单
    renewal_type          smallint    not null, --add续费类型，1订阅自动续费，2用户手动续费, 3没有上一单
    currency              char(3)     not null, --货币代码
    pay_channel_id        smallint    not null, --支付通道数字编号
    form_type             smallint    not null, --1收款, 2退款, 3已取消, 4付款失败, 5创建订单
    commission            numeric     not null, --手续费
    amount                numeric     not null, --金额
    free_form_create_type smallint    not null, --1手动开通，2兑换码，3免费赠送,4付费订单
    pre_order_type        smallint    not null  --1没有上一单,2付费,3免费
);

    truncate cache_payment_device;
    insert into cache_payment_device
    select device_id, time, consumer_id
    from form
    where form_type = 1
      and device_id is not null
      and amount > 0
      and time < target + 1
    order by 2
    on conflict do nothing;
```

https://www.jianshu.com/p/05b4830a0010

1. gitlab在浏览器输过一次TFA验证码后，再开一次gitlab标签页后，就不需要输入验证码了

2. etcd可以作为kv存储的中间件，中间件是啥？

3. go的底层

4. [CPU时钟](https://blog.csdn.net/stpeace/article/details/78309668)

5. goroutine调度https://www.cnblogs.com/secondtonone1/p/11803961.html https://learnku.com/articles/35045

6. 通过vpn和代理上外网原理是否一样

7. golang协程之间的切换是不可预测的吗？遵循怎样的规则？

8. runtime.Gosched没有时间的概念，只有出让次数的概念？

9. interface{}用法

   ```go
   func test(a int, b int) interface{} {
   	c:= a+b
   	return c
   }
   ```

   https://www.cnblogs.com/codersaver/p/14359116.html
   
10. 配置文件.yml为什么不会被编译到代码二进制文件中？

11. 闭包和装饰器

12. sleep和时钟中断有什么区别？

13. 为什么vpn需要配置hosts？

14. wg.add和wg.done之间的关系

    ```go
    package main
    
    import (
    	"fmt"
    	"sync"
    )
    
    func main() {
    	var wg sync.WaitGroup
    	wg.Add(1)
    	go func() {
    		wg.Done()
    		fmt.Println("go func")
    		wg.Wait()
    	}()
    	fmt.Println("main done")
    
    }
    ```

15. uint8(byte)和char有什么区别

16. pecker

17. canal用来增量订阅或消费mysql产生的binlog

18. kafka用来publish和subscribe消息，redis

19. 动态语言和静态语言的区别

20. 云监控：https://help.aliyun.com/document_detail/43393.html

21. SDN：https://zhuanlan.zhihu.com/p/38832682

22. http://www.postgres.cn/docs/9.5/routine-vacuuming.html 数据库清理

23. servicemesh，devops,灰度发布

24. ```bash
    "github.com/go-xorm/xorm"
    	_"github.com/lib/pq" #为什么import这一行就解决了问题
    ```

25. ```go
    func GetStartEndTime(Sql *xorm.Engine, tableName,timeCol string) (start, end time.Time, err error) {
    	activeConsumer:= ActiveConsumer{}
    
    	//降序排列
    	has, err := Sql.Table(tableName).Desc(timeCol).Get(&activeConsumer)
    	fmt.Println(activeConsumer)
    	if has && err == nil {
    		end=activeConsumer.Date
    	} else {
    		logx.X.Errorm("cannot get end time", "error", err)
    		return
    	}
    
    	//升序排列
    	has, err = Sql.Table(tableName).Asc(timeCol).Get(&activeConsumer)
    	fmt.Println(activeConsumer)
    	if has && err == nil {
    		start=activeConsumer.Date
    	} else {
    		logx.X.Errorm("cannot get start time", "error", err)
    		return
    	}
    
    	return
    }
    #两次打印fmt.Println(activeConsumer)结果竟然相同
    ```

26. ```go
    type str struct {
    	name string
    }
    
    func Foo(s interface{}){
    	fmt.Println(s)
    }
    
    func main() {
    	sliceStr:=[]str
    	Foo(sliceStr)
    }
    #为何失败
    ```

27. https://www.jianshu.com/p/8cbf97b40c26

28. sidecar

29. `err := internal.Unmarshal("config/global.yml", &tablesConf)`

30. 两个人同时往远程master推代码，冲突怎么解决

31. grpc

32. falcon-agent

33. pprof监听

34. docker创建容器：https://cloud.tencent.com/developer/article/1633272

35. k8s：https://kubernetes.io/zh/docs/concepts/workloads/pods/

36. 什么是容器编排

37. sed https://www.runoob.com/linux/linux-comm-sed.html

38. 后端开发为什么需要鉴权，什么场景下需要鉴权

39. timescaledb和postgresql

40. ```go
    func Run(tablesConf internal.Tables) (err error) {
    	//分别处理各表
    	var wg sync.WaitGroup
    	for _, table := range tablesConf.Tables {
    		wg.Add(1)
    		go func(temp internal.Table) {
    			tableStart := service.TableStart{TableConf: temp}
    			logx.X.Infom("tableStart configuration", "tableStart", tableStart)
    			err=tableStart.Init()
    			if err != nil {
    				logx.X.Error(err)
    				wg.Done()
    				return
    			}
    
    			err = tableStart.StartReload()
    			if err != nil {
    				logx.X.Error(err)
    				wg.Done()
    				return
    			}
    			wg.Done()
    		}(table)
    
    	}
    	wg.Wait()
    
    	return
    }
    ```

41. postgresql中的exception机制和procedure机制 https://blog.csdn.net/qq_31156277/article/details/84931843

42. SSR开发方案

43. 协程池包替换

44. https://github.com/NVIDIA/NeMo

45. https://zhuanlan.zhihu.com/p/72028159

46. https://www.qikqiak.com/k8s-book/docs/32.DaemonSet%20%E4%B8%8E%20StatefulSet.html

47. https://www.kubernetes.org.cn/6935.html

48. 证书

    * https://juejin.cn/post/6844903965499342855
    * https://www.cnblogs.com/f-ck-need-u/p/7113610.html
    
49. https://github.com/wuYin/k8s-in-action

50. https://cloud.tencent.com/developer/article/1004614

51. 容器隧道网络

52. https://github.com/shadowsocks/badvpn

53. draw.io 画图工具

54. 树莓派实验室

55. https://medium.com/@seafoodholdhand/%E6%AF%94%E5%82%B3%E7%B5%B1vpn%E7%BF%BB%E7%89%86%E6%9B%B4%E5%A5%BD%E6%9B%B4%E5%BF%AB%E7%9A%84-shadowsocks-%E5%82%BB%E7%93%9C%E5%BC%8F%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B-24f11c3b1eb3

56. https://www.barretlee.com/blog/2016/08/03/shadowsocks/

57. http://www.yidianzixun.com/article/0IX6DYAi

58. https://www.zhihu.com/question/64860792

59. https://my.oschina.net/u/4414202/blog/3364674

60. ```
    ss://aes-256-cfb:123456@139.198.115.205:50001
    console.log( "ss://" + btoa("aes-256-cfb:123456@139.198.115.205:50001") )
    docker run -e PASSWORD=654321 -p50000:8388 -p50000:8388/udp -d shadowsocks/shadowsocks-libev
    ```

https://www.jianshu.com/p/f1de9f886c3e

https://blog.csdn.net/K346K346/article/details/48877773

https://zhuanlan.zhihu.com/p/48992451

https://www.cnblogs.com/shine-lee/p/10115582.html

https://segmentfault.com/q/1010000002982186

https://www.cnblogs.com/stemon/p/4000468.html

https://cloud.tencent.com/developer/article/1757861

https://aijishu.com/a/1060000000100813

https://zhuanlan.zhihu.com/p/55824651

https://cloud.tencent.com/developer/article/1666793
