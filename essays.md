1.在存在(!=EOF)判断条件时，应该使用int类型的变量去接收getchar()输入，因为EOF是int类型的。
2.在使用缓冲输入时，用getchar()接收键盘输入，会将换行符也接收到缓冲区域中，在下次调用该区域时换行符会被提取出来，造成程序运行结果出问题，所以需要跳过换行符。
例：
int main(){
  int ch;
  ch=getchar();
  while(ch!=EOF)
  {  
    while(ch!='/n')
      printf("hi");
    ch=getchar();
  }
 }
    
