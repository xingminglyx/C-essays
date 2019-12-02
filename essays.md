    1.在存在(!=EOF)判断条件时，应该使用int类型的变量去接收getchar()输入，因为EOF是int类型的。如下代码是错误的：
     char ch;
     ...
     while((ch=getchar())!=EOF)...
        EOF需要的位数比字符型值所能提供的位数要多，这也是getchar()返回一个整型值而不是字符值的原因，然而，
        把getchar的返回值首先储存于ch中将导致它被截短。然后这个被截短的值被提升为整型并与EOF进行比较。当
        这段存在错误的代码在使用有符号字符集的机器上运行时，如果取了一个值为/377的字节时，循环将会终止，
        因为这个值截短再提升之后与EOF相等，等代码在无符号字符集的机器上运行时，这个循环将永远不会终止！
    2.在使用缓冲输入时，用getchar()接收键盘输入，会将换行符也接收到缓冲区域中，在下次调用该区域时换行符会被提取出来，
    造成程序运行结果出问题，所以需要跳过换行符。  
    如下面的fet_first()函数
    #include <stdio.h>
    #include <stdlib.h>
    char choice();
    char get_first();
    int main()
    {
        char ch;
        while((ch=choice())!='q')
        {
            switch(ch)
            {
                case 'a':printf("add\n");break;
                case 's':printf("subtract\n");break;
                case 'm':printf("multiply\n");break;
                case 'd':printf("divide\n");break;
            }
        }
        printf("bye\n");
    }
    char choice(){
        char ch;
        printf("Enter the operation of your choice\n");
        printf("a.add             s.subtract\n");
        printf("m.multiply        d.divide\n");  
        ch=get_first();
        while(ch!='a'&&ch!='s'&&ch!='d'&&ch!='m'&&ch!='q')
        {
            printf("Please input a,s,m,d,q.\n");
            ch=get_first();
        }
        return ch;
    }
    char get_first()
    {
        int ch;
        ch=getchar();
        while(getchar()!='\n')           //跳过换行符
            continue;
        return ch;
    }
    3.在用参数作为调用函数中的for循环次数依据时，最外层循环可以直接代入参数本身，而内层循环则需要额外定义一个变量来使用，
    否则参数被for循环第三个表达式改变值无法进行下一次循环。
    #include <stdio.h>
    #include <stdlib.h>
    void s(char,int,int);
    int main()
    {
        char ch;
        scanf("%c",&ch);
        s(ch,3,3);
    }
    void s(char ch,int a,int b)
    {
        for(b;b>0;b--)
        {
            for(int c=a;c>0;c--)    //防止传入的参数a改变无法进行下一次循环
                printf("%c",ch);
            printf("\n");
        }
    }
    4.转换说明中%lf转换double类型，%f转换float类型。
    #include <stdio.h>
    #include <stdlib.h>
    void s(double *,double *);
    int main()
    {
        double a,b;
        scanf("%lf %lf",&a,&b);
        s(&a,&b);
        printf("%f,%f",a,b);
    }
    void s(double * a,double * b)
    {
        double p=*a;
        double q=*b;
        if(p>q)
            *b=p;
        else
            *a=q;
        return 0;
    }
    5.编写一个对多个变量排序并交换他们的值的数组，可以将指针当作函数参数，然后将指针放入数组使用冒泡排序，再将数组的值赋给指针。
    #include <stdio.h>
    #include <stdlib.h>
    void s(double *,double *,double *);
    int main()
    {
        double a,b,c;
        scanf("%lf %lf %lf",&a,&b,&c);
        s(&a,&b,&c);
        printf("%lf,%lf,%lf",a,b,c);
    }
    void s(double *a,double *b,double *c)
    {
        double temp;
        double list[]={*a,*b,*c};        //将指针放入数组
        for(int i=0;i<2;i++)             //冒泡排序
        {
            for(int j=0;j<2-i;j++)
            {
                if(list[j]>list[j+1])
                {
                    temp=list[j];
                    list[j]=list[j+1];
                    list[j+1]=temp;
                }
            }
        }
        *a=list[0];
        *b=list[1];
        *c=list[2];
    }
    6.用scanf输入字符时可能会出现和2.同样的问题，输入数字就不会，也需要将换行符跳过。
    int main()
    {
        s();
    }
    void s()
    {
        char ch;
        while(scanf("%c",&ch)!=EOF)       //或者使用scanf("%c%*c",&ch)将换行符跳过
        {
            if(ch>='a'&&ch<='z')
            {
                int a=(int)ch-96;
                printf("该字符是字母，在字母表中位置为%d\n",a);
            }
            else if(ch>='A'&&ch<='Z')
            {
                int a=(int)ch-64;
                printf("该字符是字母，在字母表中位置为%d\n",a);
            }
            else if(ch=='\n')        //跳过换行符
                continue;
            else
                printf("该字符不是字母");
        }
    }
    7.千万不要造成数组过界，不然真的不知道结果会是什么鬼样子，还半天找不到错在哪！
    8.传递数组，虽然传递的是首地址地址，但是参数到了函数内，就成了普通指针，不再是数组首地址了，
    所以试图在别的函数中无法得到传递数组的长度。只能先计算好长度后再传过去。
    void false_sort(int a[]){
        int length=sizeof(a)/sizeof(a[0]);
        printf("length=%d\n",length);//输出为1
    }
    9.二维数组作函数参数至少可以分成三种：
    第一种形参为二维数组：
    声明：void function(int a[m][n]);
    void function(int a[][n]);//不论多少维数组，第一维都可省略。第二维不可省略。
    调用：function(a);实参直接写数组名。
    在函数操控元素：
    1. *(a[i] + j)  //代表第 i 行 第 j 列
    2. *(*(a+i) + j) //同上
    3. *((int *)a +i*n +j )//同上，n表示第二维数组长度,即列宽
    不管怎么样，a[i][j]不被允许。也是由编译器的寻址方式决定。
    第二种形参为数组指针：
    声明：void function(int (*a)[n]);不是(int *a[n])(指针数组) ,而是(int (*a)[n])(数组指针);缘由是 [] 的 优先级比 *的大。
    调用：function(a);实参直接写数组名
    在函数操控元素：
    1. *(a[i] + j)  //代表第 i 行 第 j 列
    2. *(*(a+i) + j) //同上
    3. *((int *)a +i*n +j )//同上，n表示第二维数组长度,即列宽
    a[i][j]不被允许。由编译器的寻址方式决定。
    第三种形参为二级指针：
    声明：void function(int **a,int n);n表示第二维数组长度,即列宽。
    调用：function( (int **)a,int n);
    在函数操控元素：
    *((int *)a +i*n +j )只有一种，n表示第二维数组长度,即列宽
    其他不被允许。由编译器的寻址方式决定。
    10.一维数组作为参数时在函数操控数组元素的时候可以直接用‘数组名[]’来调用。
    11.用数组形式和指针形式声明字符串时，字符串储存在静态储存区中，指针指向该字符串的首字符的地址，
    而程序运行时会将字符串拷贝到数组中，此时使用的是动态内存，而数组名指向的是数组首元素的地址，所以两个指向的地址不同。
    指针指向的地址与数组的字符串字面量地址相同，与其数组元素地址不同。
    12.使用输入来给数组元素赋值时，需要先为其分配足够的空间，即定义好数组大小，或者创建一个函数来边读取数据边分配空间。
    13.将字符串数组中的一个元素改为'\0'字符，即可达到缩短字符串长度的效果，因为字符串以'\0'结尾，但数组后面的元素依然存在。  
    14.在条件语句判定中，非零值都为真。  
    15.声明数组将分配储存数据的空间，而声明指针只分配储存一个地址的空间。
    16.块作用域的变量通常具有自动储存期，储存期从块的开始到块的结束，但变长数组的储存期是从声明处到块的末尾。
    外部变量的作用域是从声明处到文件结尾。
    17.只能把枚举值赋予枚举变量，不能把元素的数值直接赋予枚举变量。C中允许enum变量自加自减，但是C++中不允许。
    18.strlen 是一个函数，它用来计算指定字符串 str 的长度，但不包括结束字符（即 null 字符）。
    /*判断一*/
    if(strlen(x)>= strlen(y))
    /*判断二*/
    if(strlen(x)- strlen(y)>= 0)
    从表面上看，上面的两个判断表达式完全相等，但实际情况并非如此。其中，判断表达式一没什么问题，程序也能够完全按照预想的那样工作；
    但判断表达式二的结果就不一样了，它将永远是真，原因很简单，因为函数 strlen 的返回结果是 size_t 类型（即无符号整型），
    而 size_t 类型绝不可能是负的。因此，语句“if(strlen(x)-strlen(y)>=0）”将永远为真。
    19.当整型值转换为float值时，也有可能损失精度，float仅要求6位数字的精度，如果将一个超过6位数字的整型值赋值给一个float型变量时，
    其结果可能只是该整型值的近似值。
    20.在对指针进行间接访问时一定要确保指针已经初始化，如果不知道该初始化成什么地址，可以初始化成NULL，但不要对NULL指针进行间接访问。
    21.将双引号引起的字符串赋给指针，其实是将字符串第一个字符的地址赋给了指针，所以不会报错，而数字不行。
    22.如果打算在指针上执行运算，则不应该使用int (*p)[]=m;（m为一个二维数组）这种声明，因为其没有数组长度，执行运算时，它的
    值将根据空数组的长度进行调整（也就是与零相乘），结果很可能不是我们本来想要的。
    23.可以用一个整型指针指向一个二维数组，即把数组压扁，但不建议使用这种方法。
    24.要处理包含null的数据，无法使用字符串函数，可以使用内存操作函数。
    25.这里有个陷阱：
    typedef struct{
        int a;
        SDA *b;
        int c;
    }SDA;
    这个声明是错误的，因为类型名直到声明的末尾才定义，所以在结构声明的内部它尚未定义，所以b指针无法指向，可以加一个结构标签来声明b。
    如：
    typedef struct A{
        int a;
        struct A *b;
        int c;
    }SDA;
    26.可以在声明中对结构的成员列表进行排列，让那些对边界要求最严格的成员首先出现，最弱的最后出现，这种做法可以最大限度地减少因边界
    对齐而带来的空间损失。如：char类型的必须存储于一个能被4整除的地址，而如果下一个成员是int类型，则必须跳过3个字节到达合适的边界才
    被存储，即必须存储一个能被8整除的地址，这样浪费了很多空间。
    27.可以用指向指针的指针给指针间接访问变量。只有当确实需要时，才应该使用多层访问，不然会使程序庞大，缓慢并且难以维护。
    28.函数名被使用时总是由编译器把它转换为函数指针，所以可以直接赋给函数指针。
    29.字符串常量实际上是个指针，指向第一个字符。可以利用这个省去一些传统循环。
    30.不该在宏定义的尾部加上分号，如果这样做了就会多产生一条空语句，在某些只允许出现一条语句的场合就会出现问题。
31.![image](https://github.com/xingminglyx/JavaWeb/blob/master/images/69.jpg)
    32.如果程序失败，缓冲输出可能不会被是实际写入，这就可能使程序员得到关于错误出现位置的不正确结论，解决办法就是在每个用于调试
    的printf函数之后立即调用fflush，即fflush(stdout);它将迫使缓冲区的数据立即写入。  
    33.如果函数体内没有任何语句，那么该函数就称为存根。
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
