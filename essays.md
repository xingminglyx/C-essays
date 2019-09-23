    1.在存在(!=EOF)判断条件时，应该使用int类型的变量去接收getchar()输入，因为EOF是int类型的。
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
    17.
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
