## 变量
- 公式
    
        变量=变量名+值+数据类型
        
- 3中使用方式
    
        1. 指定变量类型，声明后若不赋值，使用默认值
           var name string 
        2. 根据自行判断变量类型
           var name = 10.11
        3. 省略var,注意：左侧的变量不应该是声明过的
           name := "tom" (var name string  name="tom")
## 基本数据类型
- 整形
        
        int8 (-127~128)
- 浮点型

- 字符类型
    
        var c1 byte = 'a'
        fmt.Println("c1=".c1)  //97
        fmt.Printf("c1=%c",c1) //a
        
    注意：Println()中必须用双引号

- 布尔型
        
        占一个字节
        字节的长度：unsafe.sizeof()

- 字符串型
        
        go语言的字符串都是utf8 
        go中的字符串是不可变的
        go中字符串的拼接方式是：+

- 数据类型的转换

        Golang 中数 据类型不能自动转换。
    
        var n2 int8 = int8(i) //前后类型必须一致才能转换
        
        案例：
        	//错误的案例
        	// n2 = n1+20
        	// n3 = n1+32
        	//类型转换后正确
        	n2 = int64(n1)+20
        	n3 = int8(n1)+32
        	
- 基本数据类型和string的转换

    - 基本数据类型转string(在 Go 中字符串是不可变的)
            
           (1)fmt.Sprintf("%参数",表达式 )
           
            案例：
            //整形
            var str string
            str = fmt.Sprintf("%d",n1)
            fmt.Printf("str type %T str=%v\n",str,str)
            //浮点型
            str = fmt.Sprintf("%f",n2)
            fmt.Printf("str type %T str=%v\n",str,str)
            //布尔
            str = fmt.Sprintf("%t",n3)
            fmt.Printf("str type %T str=%q\n",str,str)
            //char
            str = fmt.Sprintf("%c",n4)
            fmt.Printf("str type %T str=%q\n",str,str)
           
            (2)使用strconv包的函数
            
            //整形
            str = strconv.FormatInt(int64(n1),10)
            fmt.Printf("str type %T str=%v\n",str,str)
            //浮点型
            str = strconv.FormatFloat(n2,'f',10,64)
            fmt.Printf("str type %T str=%v\n",str,str)
            //布尔
            str = strconv.FormatBool(n3)
            fmt.Printf("str type %T str=%q\n",str,str)
            
            var num5 int = 456
            str = strconv.Itoa(num5)
            fmt.Printf("str type %T str=%q\n",str,str)
            
    - string 转基本数据类型
            
            package main
            
            import(
            	"fmt"
            	"strconv"
            )
            
            func main(){
                //转bool
            	var str string = "true"
            	var b bool
            	//ParseBool会返回2个值（value bool,err error）
            	b,_ = strconv.ParseBool(str)
            	fmt.Printf("b type %T b=%v\n",b,b)
            
                //转int
            	var str2 string = "23312312"
            	var n1 int
            	var n2 int64
            	n2,_ = strconv.ParseInt(str2,10,64) //转换为64
            	n1 = int(n2)
            	fmt.Printf("n1 type %T n1=%v\n",n1,n1)
            	fmt.Printf("n2 type %T n2=%v\n",n2,n2)
            
                //转string
            	var str3 string = "23.232"
            	var n3 float64
            	n3,_ = strconv.ParseFloat(str3,64)
            	fmt.Printf("n3 type %T n3=%v\n",n3,n3)
            }
            注意：因返回的都是int64或float64，如果需要得到int34或float34需要转换int34(value)
            总结：
                string转基本类型：
                    b,_ = strconv.ParseBool(str)
                    n2,_ = strconv.ParseInt(str2,10,64) //转换为64
                    n3,_ = strconv.ParseFloat(str3,64)
                基本类型转string:
                    FormatBool(bool) string
                    FormatFloat(float64,'f',10,64)//'f'格式，10表示小数位保留10位，64表示float64
                    FormatInt(int64,10)
                    
- 指针
        
        指针类型，指针变量存的是一个地址，这个地址指向的空间存的才是值
        比如:var ptr *int = &num
        
        案例：
            var i int = 8
        	var ptr *int = &i 
        	fmt.Println("ptr=%v\n",ptr)  //本身值为地址，取得是i的地址
        	fmt.Println("ptr=%v\n",&ptr) //ptr的值
        	fmt.Println("ptr=%v\n",*ptr) //取得是i的值
        
        指针的使用细节
        1) 值类型，都有对应的指针类型， 形式为 *数据类型，比如 int 的对应的指针就是 *int, float32 对应的指针类型就是 *float32, 依次类推。
        2) 值类型包括:基本数据类型 int 系列, float 系列, bool, string 、数组和结构体 struct

- 值类型和引用类型
    
        * 内存 分为栈区和堆区
        
        * 值类型：变量直接存储值，通常存在栈区
        
        * 引用类型：变量存储的是一个地址，这个地址对应的空间才真正存储数据(值)，内存通常在堆 上分配，当没有任何变量引用这个地址时，该地址对应的数据空间就成为一个垃圾，由 GC 来回收

- 标识符的基本使用
        
        定义：对各种变量、方法、函数等的命名时使用的字符序列被成为标识符
        
        _ 代表空标识符（只是不能单独使用做变量）
        _ 仅能被作为占位符使用，不能作为标识符使用
        
        golong中没有public,private,protect
        一个包如果使用另一个包的变量，是通过变量名的大小写进行控制。ActionVar
        如果变量名，函数名，常量名首字母大写，就可以被其他包访问

- 标识符命名注意事项

        1) 包名 : 保持 package 的名字和目录保持一致，尽量采取有意义的包名，简短，有意义，不要和 标准库不要冲突 fmt
        
        2) 变量名、函数名、常量名 : 采用驼峰法

        3) 如果变量名、函数名、常量名首字母大写，则可以被其他的包访问;如果首字母小写，则只能
        在本包中使用 ( 注:可以简单的理解成，首字母大写是公开的，首字母小写是私有的) ,在 golang 没有 public , private 等关键字。


















