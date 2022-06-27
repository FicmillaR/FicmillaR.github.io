> 这是一篇正在编辑中的文章
>
> 目前我的cpp撰写风格还处于照葫芦画瓢期间，我会记录一些没见过的C/C++  风格代码



#  example -1 

```c++
#define REGS_FOREACH(_)  _(X) _(Y)
#define RUN_LOGIC        X1 = !X && Y; \
                         Y1 = !X && !Y;
#define DEFINE(X)        static int X, X##1;
#define UPDATE(X)        X = X##1;
#define PRINT(X)         printf(#X " = %d; ", X);

int main() {
  REGS_FOREACH(DEFINE);
  while (1) { // clock
    RUN_LOGIC;
    REGS_FOREACH(PRINT);
    REGS_FOREACH(UPDATE);
    putchar('\n'); \
    sleep(1);
  }
}
```

解释：

`X##1` :连接符

```c++
#define Conn(x,y) x##y
...
int n = Conn(123,456);
     ==> int n=123456;
char* str = Conn("asdf", "adf");
     ==> char* str = "asdfadf";
```



