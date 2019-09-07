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
    7.
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
