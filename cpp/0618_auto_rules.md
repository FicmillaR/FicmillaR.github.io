# auto 关键字推导规则
1. auto声明的变量 值初始化 不是指针或者引用 则忽略const和vloatile限定符

   ```c++
   const int i = 5;
   auto ii = i;//ii类型为int
   auto &iii = i;//iii类型为cont int&
   ```

   可以这样理解：直接优化成`auto ii = 5` 了

   

2. auto声明的变量初始化 目标是引用 忽略引用属性

   ```c++
   int i = 5;
   int& j = i;
   auto jj = j;//jj类型为int
   ```

   可以这样理解，引用即别名 所有针对`j`的操作其实是在操作`i`

   

3. 



