1.编写递归程序时，需要牢记以下四条法则：  
  1. 基准情形：基准情形下，无需递归就可以解出；例：f(x)=f(x-1)+2 f(0)=0; 则x=0为基准情形  
  2. 不断推进：对于需要递归求解的情形，每一次递归调用都必须要使状况朝向一种基准情形推进；  
  3. 设计法则：假设所有的递归调用都能运行；  
  4. 合成效益法则：在求解一个问题的同一实例时，切勿在不同的递归调用中做重复性的工作。  
2.如果一个算法用常数时间(O(1))将问题的大小消减为其一部分(通常为1/2)，那么算法就是O(logN)。如果只减少了常数时间，那么就是O(N)的。  
3.ANSI C规定NULL为零。  
4.

