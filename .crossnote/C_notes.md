# `<center><font face="仿宋">`C Notebook `</font></center>`

## **`<font face="仿宋">`1. basic knowlege `</font>`**

### 布尔运算符:

- `!` 逻辑非
- `&&` 逻辑与
- `||` 逻辑或

---

### 条件

- if/else

  ```c
  if(condition){
      //do something；
  }else{
      //do something；
  }
  ```

  if 语句默认控制其后的一条语句（此时不需要大括号）`if(condition) statemnetT else statementF`
  而当想要控制其后的多条语句的时候需要使用大括号。
  else默认跟随最近的if语句，若想要控制其他if语句的话需要使用大括号。

  ```c
  if(condition1){
      //do something；   
  }
  else if(condition2){
      //do something；
  }
  else{
      //do something；
  }
  ```

- switch-case

  ```c
      switch(condition){
          case 1:
              //do something;
              break;
          case 2:
              //do something;
              break;
          default:
              //do something;
              break;
      }
  ```

  case 标签的表达式必须是整数常量表达式，并且其值必须在编译时确定
  *合法的标签表达式*

  ```c
  switch (x) {
  case 42:      // 直接使用整数常量
      break;
  case 'a':     // 字符常量，等价于 ASCII 值 97
      break;
  case true:    // 布尔值，true 等价于 1
      break;
  case 5 + 3:   // 计算结果是 8，编译时可确定
      break;
  }
  ```

  *不合法*

  ```c
  int y = 10;
  switch (x) {
  case y:       // ❌ 错误：y 不是编译时常量
      break;
  case someFunction(): // ❌ 错误：函数返回值无法在编译时确定
      break;
  case x + 1:   // ❌ 错误：x 不是编译时常量
      break;
  }
  ```

  可以使用const,constexpr使case合法

  ```c
  constexpr int Y = 10; // 编译时已知的常量
  switch (x) {
  case Y:  // ✅ 合法，因为 Y 是编译时常量
      break;
  }
  ```

  注：switch语句中，满足condition对应的结果的case标签(eg:condition为x-y,正好此时结果为1则执行case 1),当不遇到break就会顺着case标签一直执行下去，直到遇到break或者switch语句结束。

---

### fuction

- void 可以不需要return 但是返回其他类型的函数一定要有return

- 最好的做法是  函数先声明再写main函数再定义

当然在简单时可以不声明直接定义再写main函数

eg:
```c
#include <stdio.h>
int max(int x,int y);  //声明

int main(){
    int x = 1;
    int y = 2;
    int z = max(x,y); 
}                       //nain函数

int max(int x,int y){
    if(x>y){
        return x;
    } 
}                       //定义
```
- 变量
  - 局部变量
    - static 变量（静态局部变量）
      ```c
      int max(int x,int y){
          static int z = 0;
          z++;
          if(x>y){
              return x;
          }
      } 
      ```
      static 变量存储在数据段(data segment)贯穿程序生命周期
      每次调用函数该值都不会被重置，会保留上一次的值
    - 普通局部变量
      普通局部变量存储在栈中，每次调用函数该值都会被重置 
  - 全局变量
    - 全局变量存储在数据段(data segment)贯穿程序生命周期
    - 全局变量可以在任何函数中使用
    - 全局变量可以在任何函数中修改
    ```c
    #include <stdio.h>
    int score = 0;  // 全局变量

    void add_score(int points) {
        score += points;
    }

    void show_score() {
        printf("Current score: %d\n", score);
    }

    int main() {
        add_score(10);
        show_score();  // 输出：Current score: 10
        add_score(20);
        show_score();  // 输出：Current score: 30
        return 0;
    }
    ```
- Only-Once function
  ```c
  #include <stdio.h>

  void start_game() {
      static int called = 0;  // `static` 变量在函数作用域内存储，但不会重置
      if (called) {
          printf("Error: You cannot start the game twice!\n");
          return;
      }

      called = 1;  // 标记为已调用
      printf("Game started!\n");
      // 其他游戏初始化逻辑...
  }

  int main() {
      start_game();  // ✅ 正常执行
      start_game();  // ❌ 第二次调用，报错

      return 0;
  }
  ```







--- 

### 速记

`x++`或 `++x` 等价于 `x = x + 1`
`x--` 或 `--x` 等价于 `x = x - 1`
`x *= y + 3` 等价于 `x = x * (y + 3)`

*注* `x++`先使用x的值，然后再递增x的值。`++x`先递增x的值，然后再使用x的值。
eg:

```c
int x = 1;
printf("%d\n",x++);        (print 1 再将x的值加1使新x的值为2)
int y = x++;               (先将x的值赋给y，y=2,再将x的值加1使新x的值为3)
```

---

### 循环

- while

  ```c
      while(condition){
          //do something；
      }
  ```

  注可以用 `while(n--)`来控制循环次数并可获得n的值，循环n次(直到n=0)最终n的值为-1
- do/while 循环

  ```c
      do{
          //do something；
      }while(condition);
  ```

  注：do/while语句中，先执行do中的语句，再判断condition是否满足条件，若满足则继续执行do中的语句，直到condition不满足条件为止。
- for循环

  ```c
      for(initialization;condition;increment){
          //do something；
      }
  ```

  注：for循环中，先执行initialization中的语句，再判断condition是否满足条件，若满足则执行do中的语句，再执行increment中的语句，直到condition不满足条件为止。
- continue

  ```c
      for(initialization;condition;increment){
          if(condition){
              continue;
          }
          //do something；
      }
  ```

  注：continue语句中，当condition满足条件时，continue语句会使程序跳过当前循环中的剩余语句，先进行increment中的语句进行递增递减等操作，然后继续下一次循环。而在while中，continue语句会使程序跳过当前循环中的剩余语句，直接进行condition的判断。

---

### 基础数据类型

- | type      | size                             | interpretation        | example           | signedness |
  | --------- | -------------------------------- | --------------------- | ----------------- | ---------- |
  | char      | 1 byte (8 bits)                  | one ASCII character   | 'a'               | id         |
  | int       | 4 bytes (32 bits)  规定至少16bit | integer               | 123               | signed     |
  | unsigned  | 至少 2 bytes(16 bits)            |                       |                   |            |
  | short     | 至少 2 bytes(16 bits)           |                       |                   | signed     |
  | long      | 至少 4 bytes(32 bits)            |                       |                   | signed     |
  | long long | 至少 8 bytes(64 bits)            |                       |                   | signed     |
  | float     | 4 bytes (32 bits)                | floating-point number | 3.141592          | signed     |
  | double    | 8 bytes (64 bits)                | floating-point number | 3.141592653589793 | signed     |

- ![](/屏幕截图%202025-03-03%20181253.png)

- 整数类型的取值范围：
  1. 有符号整数类型的取值范围：

- uint_32_t : 无符号32位整数类型
  uint_64_t : 无符号64位整数类型
  使具有平台可移植性。因为在windows上unsigned int 默认为32位而在linux上unsigned int 默认为64位。 
  可以使用PRIu32_t来表示无符号32位整数类型的格式化输出。
- char

  - 字符型数据是用单引号括起来的单个字符，如'a'、'b'、'c'等,这表示一个字符的字面量，而它们在内存中以ASCII码的形式存储。（ASCII为8bits）
  - 字符型数据可以进行算术运算，如加减乘除等。
  - ASCII码：https://en.cppreference.com/w/c/language/ascii
  - 

  ```c
    char char_to_uppercase(char c) {
        if (c >= 'a' && c <= 'z') {
            return c - （'a' - 'A'）;
        }
        return c;
    }
  ```

  *不要用 `c - 32` 要用 `c - （'a' - 'A'）`使之方便可读。
  对字符间的运算就是对ASCII码运算，如果用%C输出则为对应字符，如果用%d输出则为对应的ASCII码。
  c语言中没有ord()的内置函数。
  如果将字符与整数直接运算那得到的是将ASCII码转化为整数后的运算结果。
- int

  - unsigned int将32位全部用于表示一个正数或零。
    可以表达从0到4294967295(2^32-1)的 2^32个整数。
  - int
    是有符号的，可以表示从-2147483648到2147483647(2^31-1  正的包含了0)的2^32个整数。最有效位（最左边的位）是符号位，0表示正数，1表示负数。
  - 二进制的补码

    1. 补码的基本概念：
       对于一个整数，其 补码 是它的二进制表示形式的一种编码方式，用于表示正数、负数和零。具体规则如下：
       * 正数的补码与其原码相同。
       * 负数的补码是取其绝对值的二进制表示，按位取反，再加1。
       * 零的补码是全零。
    2. 补码的作用：
       * 简化硬件设计。在补码系统中，加减法的运算可以统一为加法。负数的减法可以转换为加上一个负数的补码，例如：5 - 3 等价于 5 + (-3)。-3 的补码是 11111101，因此可以直接用二进制加法器完成减法。
       * 统一零的表示。在补码表示中，零只有一种形式（全零），避免了原码中“+0”和“-0”的问题。
       * 方便表示和处理负数。在补码表示中，最高位（符号位）仍然表示正负，但它与原码不同，补码在数学运算上更自然。例如：若最高位为 1，则是负数；若最高位为 0，则是正数。`</font>`
  - short int :指位数小于等于int的整数类型。通常情况下，short int的位数为16位（2字节）。long int(等价long 默认为signed long) :指位数大于等于int的整数类型。
- float & double
  **默认使用double类型**

  - float 是单精度浮点数，占用4个字节（32位），可以表示小数点后6位或7位的精度。| 1bit   | 8bits  | 23bits |
    | ------ | ------ | ------ |
    | 符号位 | 指数位 | 尾数位 |
  - double 是双精度浮点数，占用8个字节（64位），可以表示小数点后15位或16位的精度。| 1bit   | 11bits | 52bits |
    | ------ | ------ | ------ |
    | 符号位 | 指数位 | 尾数位 |
- boolean 布尔类型

  - 用到bool需要包含头文件 `#include <stdbool.h>`
  - bool 类型的值只能是 true 或 false。true = 1，false = 0。
- printing redux

  - 格式说明符（Format Specifier）

    | 格式说明符 | 打印内容                                                                        |
    | ---------- | ------------------------------------------------------------------------------- |
    | `%c`     | 单个字符（single character）                                                    |
    | `%d`     | 十进制整数（decimal integer）                                                   |
    | `%u`     | 无符号十进制数（unsigned decimal number）                                       |
    | `%o`     | 八进制数（octal number）                                                        |
    | `%x`     | 无符号十六进制数（unsigned hexadecimal number）                                 |
    | `%f`     | 十进制浮点数（decimal floating point）                                          |
    | `%e`     | 科学计数法（scientific notation）                                               |
    | `%g`     | 选择 `%e` 或 `%f` 中较短的表示方式（picks the shorter of `%e` or `%f`） |
    | `%s`     | 字符串（打印直到 `'\0'`）（string, prints chars until `'\0'`）              |
    | `%%`     | 打印 `%` 符号（prints the `%` character）                                   |
    | `%hd`    | short int                                                                       |
    | `%ld`    | long int                                                                        |
    | `%hu`    | unsigned short int                                                              |
    | `%lf`    | double   (float 是 `%f`)                                                      |
    | `%Lf`    | long double                                                                     |
  - 十进制格式化（Decimal Formatting）

    | 格式说明符 | 打印内容                                                                |
    | ---------- | ----------------------------------------------------------------------- |
    | `%f`     | 默认：显示 6 位小数（default: shows 6 decimal places）                  |
    | `%.nf`   | 显示 n 位小数（shows n decimal places）                                 |
    | `%mf`    | 最小宽度为 m 的数字（prints with minimum width m）                      |
    | `%m.nf`  | 显示 n 位小数，并且最小宽度为 m（n decimal places and minimum width m） |
  - 转义序列（Escape Sequence）
    查询网址：https://en.cppreference.com/w/c/language/escape

    | 转义序列 | 打印内容                            |
    | -------- | ----------------------------------- |
    | `\n`   | 换行（newline）                     |
    | `\t`   | 制表符（tab）                       |
    | `\\`   | 打印反斜杠（prints a backslash）    |
    | `\'`   | 打印单引号（prints a single quote） |
    | `\"`   | 打印双引号（prints a double quote） |
  - 运算符
    ![img](/屏幕截图%202025-02-24%20082155.png)
    operator precedence 运算符优先级：
    {unary + -} > {* / %} > {+ -}
  - 类型和格式化输出
    eg:

    ```c
       char letter='G';
       printf("The letter is %d\n",letter);
    ```

    我们将会发现因为我们格式化输出的并不是%c而是%d所以输出的并不是G而是71（ASCII码以十进制整数形式输出）。
  - 表达式有类型
    表达式的类型由表达式中使用的操作数的类型决定。例如，表达式 3 + 4.0 的类型是 double，因为 3 是 int 类型，4.0 是 double 类型。
    如果需要，常量的类型可以通过使用字母后缀来修改（U 表示无符号常量，L 表示 long 常量，f 表示 float 常量）： 3.14f 是浮点类型的常量，99999999999L 是长 int 类型的常量。
  - **类型转换**
    隐式：

    ```c
    int nHrs = 40;
    int nDays = 7;
    float avg = nHrs/nDays;
    ```

    将先进行整数除法，得到40/7--->5,然后将结果转换为浮点数5.0。
    显式：

    ```c
    int nHrs = 40;
    int nDays = 7;
    float avg = nHrs/(float)nDays;//将除数转化位浮点数更佳
    ```

    此时另一个操作数nHrs被自动提升为浮点数，然后再进行除法运算。

    - **除法**分为整数除法和浮点数除法。整数除法会直接将结果向零截断 eg:3 / -2 = -1 (本来结果为-1.5直接去除小数部分)
    - **取余** `a%b` 要求a和b都是整数。
      - 余数的符号与被除数（a）的符号相同。
        - 当 a 为正时（无论 b 正负），余数为正。
        - 当 a 为负时（无论 b 正负），余数为负。
      - 公式关系：a=(a/b)×b+(a%b)
        - 这种规定使得无论除数 b 的符号如何变化，最终余数的符号都只取决于被除数 a
  - 溢出和下溢
    当一个数值超出了它的类型所能表示的范围时，就会发生溢出。例如，short int 类型的最大值是 0111111111111111（32767），如果将它加1，就会得到 1000000000000000（-32768），这就是下溢。将较大的类型赋值给较小的类型也会导致溢出。如将int赋值给short int 类型，将会只取int的低16位。
    有符号整数溢出是未定义的行为，而无符号整数溢出是定义的行为。
    无符号整数溢出遵循模运算规则，即将结果限制在 [0, 2^n - 1] 的范围内，其中 n 是无符号整数的位数。当结果超出范围时取值为结果对 2^n 取模
    如加法溢出：结果超过最大值的时候会产生回绕，即结果对 2^n 取模，从0开始计数。
    如减法溢出：结果低于最小值的时候会产生回绕，即结果对 2^n 取模，从最大值开始计数。

    ```c
    int a = 1000000;
    long long b = a*a;  // 溢出
    ```
    这个时候可以使用1LL来修改

    ```c
    int a = 1000000;
    long long b = 1LL*a*a;
    ```

    或者使用强制类型转换

- **整数常量**（Integer Literals）是指在程序中直接表示的整数值。在编程语言中，整数常量可以是十进制、二进制、八进制或十六进制的数字，具体表示方式依赖于语言的语法规则。
  - 整数常量的类型和表示方式：
    1. **十进制整数**：
       - 默认情况下，整数常量以十进制的形式表示，例如：
         ```c
         int a = 123;
         ```
       - 这里的 `123` 是一个十进制整数。

    2. **二进制整数**（某些语言支持）：
       - 以 `0b` 或 `0B` 开头表示二进制数，例如：
         ```python
         a = 0b1010  # 二进制数 1010，等于十进制的 10
         ```
       - 在 C 和 C++ 中，可以通过编译器扩展支持二进制常量（如 `0b` 前缀），但标准 C 语言不原生支持二进制。

    3. **八进制整数**：
       - 以 `0` 开头表示八进制数，例如：
         ```c
         int a = 075;  // 八进制的 75，相当于十进制的 61
         ```

    4. **十六进制整数**：
       - 以 `0x` 或 `0X` 开头表示十六进制数，例如：
         ```c
         int a = 0x1A;  // 十六进制的 1A，相当于十进制的 26
         ```
  - 整数常量的范围：
    - 整数常量的大小受限于所用的数据类型（如 `int`, `long`, `short` 等）。
    - 在 C 语言中，可以通过在常量后加 `L` 或 `LL` 来表示长整型常量，如：
      ```c
      long a = 123L;
      long long b = 123LL;
      ```
    通过这些整数常量，程序员可以直接在代码中使用特定的整数值，而不需要用变量来存储这些值。



  

---

### operator

#### 求值顺序与未定义行为（Undefined Behavior）

在 C 和 C++ 语言中，如果两个表达式 `A` 和 `B` 的求值顺序是**未指定的（unspecified behavior）**，并且：

1. **两个表达式都修改同一个对象的值**，或
2. **一个表达式修改对象，而另一个表达式使用该对象的值**

那么**程序的行为是未定义的（undefined behavior, UB）**。

- **示例 1：`i = ++i + i++;`**

```cpp
int i = 1;
i = ++i + i++;
```

**为什么是 UB？**

- `++i` 先自增 `i`，并返回新值（`2`）。
- `i++` 先读取当前 `i` 的值（`2`），再自增 `i`（`3`）。
- `+` 操作数的求值顺序未指定，因此 `++i` 和 `i++` 的执行顺序无法确定。

可能的执行顺序：

1. `++i` 执行，使 `i = 2`，然后 `i++` 读取 `i = 2`，再自增 `i = 3`。
2. `i++` 先执行，`i = 1` 先参与计算，再自增 `i = 2`，然后 `++i` 使 `i = 3`。

**结论**：因为 `i` 在一次表达式中既被读取又被修改，且求值顺序未定义，导致**未定义行为（UB）**。

---

- **示例 2：`i = i++ + 1;`**

```cpp
int i = 1;
i = i++ + 1;
```

**为什么是 UB？**

- `i++` 读取 `i` 的值（`1`），然后 `i` 自增（`2`）。
- `+ 1` 使用 `i++` 的结果（`1`），计算 `1 + 1 = 2`。
- **但是！** `i++` 需要在表达式完成后更新 `i`，而 `=` 也要赋值 `2` 给 `i`，两个操作都在修改 `i`，但求值顺序未定义。

**结论**：`i++` 可能在 `=` 之前或之后生效，导致未定义行为。

---

- **示例 3：`printf("%d, %d\n", i, i++);`**

```cpp
int i = 1;
printf("%d, %d\n", i, i++);
```

**为什么是 UB？**

- `printf` 的参数求值顺序未定义，因此：
  - `i` 可能先被读取（`1`），然后 `i++` 读取 `i = 1`，再修改 `i = 2`。
  - 或者 `i++` 先执行，`i` 先变成 `2`，然后 `i` 被读取。
- 结果可能是 `1, 1`，也可能是 `1, 2`，或者程序崩溃。

**结论**：因为 `i` 既被读取又被修改，而求值顺序未定义，导致**未定义行为**。

---

**如何避免未定义行为**

- **1. 拆分表达式**
  **不要在一个表达式中既修改又读取同一个变量**：

```cpp
int i = 1;
int temp = i;  // 先保存 i 的值
i++;
printf("%d, %d\n", temp, i);  // 现在行为是确定的
```

这样可以保证 `i` 的值是可预测的。

---

- **2. 避免自增/自减与赋值混用**
  ❌ **错误写法（UB）：**

  ```cpp
  i = i++ + 1;  
  ```

  ✅ **正确写法：**

  ```cpp
  i++;
  i = i + 1;
  ```
- **3. 使用临时变量**
  如果需要同时使用和修改变量，**使用临时变量保存原始值**：

  ```cpp
  int old_i = i;
  i++;
  printf("%d, %d\n", old_i, i);
  ```

**总结**

1. **未定义行为（UB）**发生在：

- 同时修改和读取同一个变量，但求值顺序未定义的情况下。
- 例如 `i = i++ + 1;` 或 `printf("%d, %d\n", i, i++);`。

2. **避免 UB 的方法**：

- **拆分表达式**，让赋值和自增/自减分开执行。
- **使用临时变量**，确保先存储旧值再修改变量。
- **避免在同一表达式中既读取又修改变量**。

未定义行为可能导致难以调试的错误，所以在编写 C/C++ 代码时，尽量避免依赖**未指定的求值顺序**！

#### operator

- **?:**
  `condition ? expr1 : expr2;`
  Use it to replace some simple and short if-else statement
- Comparison operators in C cannot be chained.
  eg: a < b < c is interpreted as (a < b) < c (due to left-associativity), which compare (a < b) first, whose result is either 0 or 1 , and then compare 0 < c or 1 < c .To test , use a < b && b < c
- Precedence: ! > comparison operators > &&  ||
- **=** :is right-associative

```c
int a = 0, b = 1, c = 2;
a = b = c;  // interpreted as a = (b = c)
            // Both a and b are assigned with 2.  
```

### 非数字数据类型

- string
  - 字符串是一个字符序列，它以一个称为空结束符的特殊字符结束，空结束符可以用"\0"（读作 "反斜线零"）字面表示，它标志着字符串的结束。一个字符串由内存中第一个字符的位置来表示，每个 8 位字符都会被读取，直到检测到"\0 "为止

---

- **struct**   自定义数据类型1
  [![pEAqHjf.png](https://s21.ax1x.com/2025/01/23/pEAqHjf.png)](https://imgse.com/i/pEAqHjf)
  *rect_tag是一个结构体的标签，由 `struct rect_tag {...}`定义g该结构体，再经过 `typedef struct rect_tag rect_t;` 定义了一个新的类型名称 rect_t*      下面是俩个比较方便的定义方式：

  ```c
  typedef struct rect_tag {
      int x;
      int y;
      int width;
      int height;
  } rect_t;
  ```

  ```c
  typedef struct {
      int x;
      int y;
      int width;
      int height;
  } rect_t;
  ```

  - typedef  类型定义的用途
    类型定义不止可以简化结构体的使用（令结构体使用时不用每次都要写struct   eg：struct rect_tag myrect），还可以定义新的类型名称，使代码更加清晰易读易更改。eg：

    ```c
    typedef unsigned int rgb_t;   // 定义一个新的类型名称表示无符号整数
    rgb_t getRedForPixel(int x, int y);
    rgb_t getGreenForPixel(int x, int y);
    rgb_t getBlueForPixel(int x, int y);

    int main(viod){
        red = getRedForPixel(0, 0);
        green = getGreenForPixel(0, 0);
        blue = getBlueForPixel(0, 0);
    }
    ```

    如此当我想要把这些类型都改为char时只需要将rgb_t改为char即可。
    即变为 `typedef char rgb_t;`

---

- **enum**   自定义数据类型2
  **方便与开关switch语句结合使用**
  当你有一种数据类型，其中有一组值，你想用它们的概念名称来标注（而不是使用原始数字），而且特定的数值并不重要（只要它们是不同的），或者它们是按顺序自然出现的。`<u>`表示一个名称代表一个常量的惯例是将名称写成大写 `</u>`
  例如，在 2011 年之前，美国国土安全部一直在使用一种以颜色编码的恐怖主义威胁咨询表，用于在包括主要机场在内的不同地点加强或放松安保。从绿色到红色，按严重程度由高到低分为五个威胁等级。下面是代码展示：

  ```c
  enum threat_level_t {
      LOW,        //对应数字0
      GUARDED,    //对应数字1
      ELEVATED,   //对应数字2
      HIGH,       //对应数字3
      SEVERE      //对应数字4
  };
  void printThreat(enum threat_level_t threat){
      switch(threat) {
          case LOW:
          printf("Green/Low.\n"); 
          break;
          case GUARDED:
          printf("Blue/Guarded.\n"); 
          break;
          case ELEVATED:
          printf("Yellow/Elevated.\n"); 
          break;
          case HIGH:
          printf("Orange/High.\n"); 
          break;
          case SEVERE:
          printf("Red/Severe.\n");
          break;
      } 
  }
  void printShoes(enum threat_level_t currThreat) {
      if (currThreat >= ELEVATED) {
          printf("Please take off your shoes.\n”);
      }
      else {
          printf("Please leave your shoes on.\n”);
      }
  }

  int main(void) {
      enum threat_level_t myThreat = HIGH;
      printf("Current threat level is:\n");
      printThreat(myThreat);
      printShoes(myThreat);
  return 0;
  }
  ```

  枚举类型的另一个例子是，如果我们想编写一个程序，定期引用一小部分水果：葡萄、苹果、桔子、香蕉和梨。假设我们想用数字来表示其中的每一种水果（因为我们经常在水果本身上使用开关语句等构造），但我们并不关心每一种水果是用哪个数字来表示的。我们可以创建一个枚举类型 enum fruit_t {GRAPE, APPLE,...};然后在整个代码中使用这些常量。

## **`<font face="仿宋">`2.Writing Running and Fixing Code in C `</font>`**

### git 的使用

- git书网址：https://git-scm.com/book/en/v2
- **ls** (list) 列出当前目录中的文件和文件夹

  - **ls -l**  (list long) 列出当前目录中的文件和文件夹的详细信息，包括文件大小、权限、所有者、最后修改时间等信息
  - **ls -a**  (list all) 列出当前目录中的所有文件和文件夹，包括以点(.)开头的隐藏文件 <font size=2>*(以点开头的文件通常是系统文件或配置文件在通常情况下会被隐藏，除非你明确地想要查看它们)*`</font>`
  - 在任何目录中，`.` 代表当前目录本身（因此 `cd .` 什么也不做——它会将你切换到你已经所在的目录）。这个名称在你需要明确指定当前目录中的某些内容时非常有用（例如 `./myCommand`）。`./ `是一种明确指定当前目录的方式，可以确保系统在当前目录中查找并执行文件。
    这样做避免了与系统中已存在的命令同名文件产生冲突，并且让操作系统知道你要运行的是当前目录中的文件或脚本。
  - 而 `..` 代表当前目录的父目录，也就是当前目录所在的目录。使用 **`cd ..`** 会将你带到目录层级中的“上一层”目录。唯一的例外是根目录中的 `..`，它指回根目录本身，因为在根目录无法再“向上”进入任何更高层级的目录。
  - **man ls**  (man list) 列出 ls 命令的手册页，其中包含了 ls 命令的详细用法和选项。
- **cd** (change directory) 切换到指定的目录
- **emacs 文件名** 使用 emacs 编辑器打开指定的文件

  - **c-x c-f**  (ctrl + x & ctrl + f) 可在 Emacs 中用于打开文件
  - 再输入想要创建的文件名 eg:`hello.txt`  再hit `enter`键即可创建文件
  - **c-x 2**  (ctrl + x & 2) 可在 Emacs 中用于分割窗口，形成俩个上下水平分割的窗口。
  - **c-x b**  (ctrl + x & b) 后Emacs会提示输入文件名称以将该文件打开或者会默认为最近使用的文件
  - **c-x o**  (ctrl + x & o) 可在 Emacs 中用于在俩个窗口之间切换
  - **c-x 1**  (ctrl + x & 1) 可在 Emacs 中用于关闭所有窗口，只剩下一个当前窗口
  - **c-x c-s**  (ctrl + x & ctrl + s) 可在 Emacs 中用于保存文件
  - **c-z**  (ctrl + z) 可在 Emacs 中用于挂起程序,将Emacs 切换到后台运行，前台显示的是shell命令行。
- **git add 文件名** 将文件添加到暂存区
- **git commit -m "提交信息"** 将文件从暂存区提交到仓库。
  `git commit` 用于提交更改，`-m` 选项用于添加提交信息后面加上的带引号的内容用于说明提交的更改的目的等信息。
- **git push** 将文件从本地仓库推送到远程仓库。
- **fg** 将后台挂起的程序切换到前台运行。
- grade the assignment, get the feedback

  - **git pull** 拉取远程仓库的最新代码到本地仓库。
  - 整个获取成绩和反馈的命令展示

    ```
    c-z //挂起程序
    grade
    git pull
    fg
    ```

    接着就可以在emacs中查看grade.txt 文件了。(`c-x c-f grade.txt`)
- **c-x c-c**  (ctrl + x & ctrl + c) 可在 Emacs 中用于退出 Emacs 编辑器。
- **git rm 文件名** 删除文件
- **mkdir** 创建文件夹
- **rmdir** 删除文件夹 注：该目录下必须为空
- **rm -r 文件夹名** 删除文件夹 注：该目录下可以不为空
- **rm 文件名** 删除文件

---

- **显示文件**
  - **cat** (concatenate) 查看文件内容

    - **cat 文件名** 查看文件内容
    - **cat**默认行为：将输入直接打印到屏幕。当你只运行 cat 时，它等待你输入内容（标准输入），然后立即将输入的内容打印到屏幕（标准输出）eg：
      ```
      $ cat   
      hello
      ```

    则cat指令会将"hello"立即将其打印出来。这种行为持续到你按下 Ctrl+D，表示输入结束。- **cat < file.txt** 你可以通过文件重定向将 cat 的输入来源改为某个文件将文件 file.txt 的内容作为 cat 的标准输入。cat 会将 file.txt 的内容打印到屏幕。

    - **cat > newfile.txt** 你可以通过文件重定向将 cat 的输出重定向到某个文件。cat 会将输出保存到 newfile.txt 中。而不会显示在屏幕上。按下 Ctrl+D 结束输入，完成文件保存。
    - **cat | 加指令** 结合管道操作。
      eg：`cat | grep hello`   `cat file.txt | grep hello`
      cat 将输入内容传递给 grep，grep hello 只过滤出包含 hello 的行。
      cat 将 file.txt 的内容传递给 grep，grep hello 只过滤出 file.txt 中包含 hello 的行。
    - **cat file1.txt > file2.txt** 从文件读取并重定向到另一个文件。将一个文件的内容通过 cat 复制到另一个文件。file1.txt 的内容被读取，cat 的输出被重定向到 file2.txt。*如果 file2.txt 已经存在，会被覆盖*。
    - | 命令 | 功能                 | 用法                                                                                                          |
      | ---- | -------------------- | ------------------------------------------------------------------------------------------------------------- |
      | more | 按屏查看文件内容     | 只能向前翻页space（空格键）可以显示下一屏，enter向下滚动一行，q退出                                           |
      | less | 更灵活的文件查看工具 | 支持向前（b）和向后（space）翻页，用方向键向上向下滚动一行，支持搜索（/pattern 上 ?pattern 下），大文件友好。 |
      | head | 查看文件前几行       | 默认显示前 10 行，可指定行数 `head -n 5 file.txt`                                                           |
      | tail | 查看文件后几行       | 默认显示最后 10 行，可动态跟踪文件变化 `tail -f file.txt`                                                   |

---

- 移动复制删除
  - **mv** (move) 移动文件或文件夹
    - `mv 文件名 新文件名` 重命名文件
    - `mv 文件名 文件夹名` 将文件移动到文件夹中
    - `mv 文件夹名 新文件夹名` 重命名文件夹
    - `mv 文件夹名 文件夹名` 将文件夹移动到文件夹中
    - `mv 文件1 文件2 文件3 文件夹名` 将文件1，文件2，文件3移动到文件夹中
  - **cp** (copy) 复制文件或文件夹
    - `cp 文件名 新文件名` 复制文件并重命名
    - `cp 文件名 文件夹名` 将文件复制到文件夹中
    - `cp 文件夹名 新文件夹名` 复制文件夹并重命名
    - `cp 文件夹名 文件夹名` 将文件夹复制到文件夹中
  - **rm** (remove) 删除文件或文件夹
    - `rm 文件名` 删除文件
    - `rm 文件名1 文件名2 文件名3` 删除多个文件
    - `rm -r 文件夹名` 删除文件夹（递归删除目录及其内容，删除该目录及其所有文件和子目录）
    - `rm -rf 文件夹名` 强制删除文件夹
    - `rm -i 文件名` 删除文件前提示
    - `rm -f 文件名` 强制删除文件
    - `rmdir 文件夹名` 删除空文件夹
- 模式扩展glob
  在 Unix/Linux 中，**模式扩展**（又叫 **glob**）允许你使用简洁的方式选择匹配特定模式的多个文件。常见的模式字符包括 `*`、`?` 和 `[...]`，它们使得处理多个文件变得更加高效。这里将详细讲解如何使用这些模式字符以及如何按字面意思使用这些特殊字符。
  - 1. `*`（星号）— 匹配任意数量的字符

    - `*` 可以匹配任意数量的字符。
    - 例如，`rm *~` 会删除当前目录下所有以 `~` 结尾的文件。
  - 2. `?`（问号）— 匹配单个字符

    - `?` 可以匹配文件名中的任意单个字符。
    - 例如，`rm file?.txt` 会删除所有以 `file` 开头并且后面有一个字符、以 `.txt` 结尾的文件。
  - 3. `[...]`（方括号）— 匹配字符集

    - `[...]` 用于匹配方括号内的任何单个字符。可以指定一个字符集，匹配这些字符中的任何一个。
  - 4. `[!...]`（排除字符集）— 排除特定字符

    - `[!...]` 可以用来排除字符集中的字符。它表示匹配不在方括号内的字符。
  - 5. 有时候，你可能希望按字面意思使用这些特殊字符（例如 `*`、`?`）。此时，可以使  用 **转义字符**（`\`）来阻止其特殊功能，使其作为普通字符处理。如果你有一个名为 `*` 的文件，在没有转义的情况下，使用 `rm *` 会删除所有文件。为了删除名为 `*` 的文件，你需要使用转义字符 `\` 来按字面意思处理 `*`。

---

- **emacs**
  - **c-s** 搜索
  - **c-l** 换页
  - **c-r** 向后搜索
  - **c-x u** 撤销更改
  - **c-space** 划定要编辑的区域
  - **c-a** 移动到行首
  - **c-e** 移动到行尾
  - **c-k** 剪切从光标到行尾的内容
  - **c-w** 先用c-space划定区域，然后剪切从光标到划定区域结尾的内容
  - **m-w** （alt + w） or **Esc + w** 复制从光标到划定区域结尾的内容
  - **c-y** 粘贴之前剪切下来的东西
  - **m-y** （alt + y） or **Esc + y** 粘贴之前剪切下来的东西
  - **Esc/** 自动补全
  - **c-x**  打开键盘宏，对这一部分进行更改，再用**c-x**关闭键盘宏，再**c-x + e**执行宏

---

### 编译

- **预处理**

  - #include <stdio.h> 标准输入输出头文件
- **gcc编译**
  **`gcc -o hello hello.c`**
  **`gcc -o hello -Wall -Werror -pedantic -std=gnu99 hello.c `**

  - `gcc` 是 GNU 编译器集合（GNU Compiler Collection）的命令，用于编译 C 程序。它将 C 源代码转换为可执行文件，默认情况下会生成一个名为 `a.out` 的可执行文件，除非你使用 `-o` 选项指定输出文件的名称。
  - **`-o hello`**
    `-o` 选项用于指定输出文件的名称。在这个例子中，编译后的可执行文件将命名为 `hello`。如果不使用 `-o` 选项，默认的可执行文件名称是 `a.out`。
  - **`-Wall`**
    `-Wall` 选项启用编译器的所有常见警告信息。`Wall` 是 "Warning All" 的缩写，它让编译器在编译时尽可能报告所有警告，这有助于发现潜在的错误和不规范的代码。比如，未使用的变量、可能的未初始化变量等。
  - **`-Werror`**
    `-Werror` 选项将所有的警告视为错误。这意味着，即使编译器仅仅报告一个警告，也会导致编译失败。这个选项常用于开发过程中确保代码的高质量，因为它强制开发者修复所有警告，避免潜在问题的积累。
  - **`-pedantic`**
    `-pedantic` 选项会启用严格的标准检查，确保代码完全符合 C 标准（如 C89、C99 等）。如果代码使用了编译器的扩展（比如 GNU 特有的语法或行为），该选项会发出警告。使用 `-pedantic` 可以帮助确保代码的可移植性，因为它让编译器对非标准行为更为敏感。
  - **`-std=gnu99`**
    `-std=gnu99` 选项指定使用 **GNU 99** 标准来编译程序。GNU 99 是基于 C99 标准的，添加了一些 GNU 扩展和特性。例如，GNU 99 允许在函数内部定义变量的同时进行初始化，而 C99 标准允许此行为，但 GNU 99 标准在此基础上扩展了一些额外的功能。如果你不指定这个选项，编译器会使用默认的标准（可能会根据编译器和操作系统的不同有所变化）。例如，C89 标准不允许在代码块中间声明变量，但 C99 标准允许在代码的任何地方声明变量。
  - **`./hello`**
    `./hello` 是命令行中运行可执行文件的命令。假设 `gcc` 编译成功并生成了 `hello` 这个可执行文件，你可以通过 `./hello` 来运行它。
    `./` 表示当前目录，因为当前目录通常不在默认的可执行文件搜索路径中，所以你需要明确指定可执行文件的路径。

  **可以把二进制的文件删掉直接 `rm hello`**
- Help(man 页)

  - 输入 `man man`即可查看man的用法。
  - `man -k 关键字` 搜索关键字相关的man页。
  - 在 Unix/Linux 系统中，man 命令分为多个章节，每个章节包含不同类型的信息。常见的章节有：
    第 1 章：用户命令（shell 命令、常见程序等）。
    第 2 章：系统调用（内核提供的功能）。
    第 3 章：库函数（例如 C 标准库函数）。
    第 4 章：特殊文件（如设备文件）。
    第 5 章：文件格式与协议（如 /etc/passwd）。
    第 6 章：游戏和娱乐（如游戏程序）。
    第 7 章：宏和惯例（如系统特性、命名约定等）。
    第 8 章：系统管理命令（管理员使用的命令）。
    eg 我想知道printf的用法：`man -S3 printf`
    -S 选项用于指定要搜索的章节。
    大多数情形不会指定章节直接使用 `man printf`即可。这将会按照默认的搜索顺序搜索手册页。
    输入 `man man`即可查看man的用法。
- **diff**

  - 要对比一个转化为二进制的文件和你自己的c源代码文件，你可以先将二进制文件(eg: ./square) 的输出重定向到一个文件中，然后再使用diff命令比较这个文件和c源代码文件。
    ```
    ./square > output.txt
    diff output.txt square.c
    ```

  当俩文件相同时，diff命令会显示没有任何差异，无其他输出。

  - `diff -y file1 file2`  命令可以将两个文件并排显示，以便你可以逐行比较它们。

---

**目标文件(.o)**

1. **什么是目标文件（Object File）？**

   - 目标文件是源代码文件编译后的中间文件。它包含了编译器将源代码（比如 `.c` 文件）转换为机器代码后生成的内容，但还不是完整的可执行程序。
   - **源文件**（.c 文件）包含程序的源码，编译器将这些代码转换为机器代码。
   - **目标文件**（.o 文件）包含编译后的机器代码，但它只是一个 **不完整** 的文件，因为它可能依赖于其他目标文件中的函数或变量。例如，你可能在 `xyz.c` 中调用了库函数或者其他源文件中的函数，而这些函数在目标文件中没有定义，它们可能会在链接（linking）阶段被解决。

   目标文件本身不能直接运行，它还需要与其他目标文件进行链接，生成完整的可执行文件。
2. **为什么要生成目标文件？**

   - 生成目标文件的目的是让程序的编译过程更加高效，尤其是在大型程序中：
   - **编译源文件成目标文件**：将每个源文件单独编译成目标文件后，你可以更灵活地管理代码。如果某个源文件的代码发生变化，只需要重新编译这个文件生成新的目标文件，而不需要重新编译所有文件。

     例如，如果你有多个源文件 `file1.c`、`file2.c`、`file3.c`，并且你只修改了 `file1.c` 中的代码，你只需要重新编译 `file1.c` 为新的目标文件 `file1.o`，而其他文件不需要重新编译。
   - **避免重复编译**：在大型项目中，源代码可能非常庞大，有时包含数万甚至数十万行代码。如果每次修改源代码后都重新编译所有文件，编译过程可能非常耗时。通过只编译修改过的文件，节省了大量时间。
3. **如何生成目标文件？**

   - 你可以使用 `gcc` 命令将 `.c` 文件编译成 `.o` 目标文件。加上 `-c` 选项，可以告诉 `gcc` 停止编译，只生成目标文件，不执行链接操作。
   - **例子 1**：使用 `-c` 编译源文件为目标文件：

     ```bash
     gcc -c xyz.c
     ```

     这条命令将 `xyz.c` 编译成 `xyz.o` 目标文件。注意，这里没有执行链接，因此不会生成最终的可执行文件。
   - **例子 2**：如果你想为目标文件指定不同的名称，可以使用 `-o` 选项：

     ```bash
     gcc -c xyz.c -o awesomeName.o
     ```

     这条命令将 `xyz.c` 编译成 `awesomeName.o`，而不是默认的 `xyz.o`。
4. **目标文件与链接**

   生成目标文件后，还需要将这些目标文件 **链接**（link）成一个完整的可执行文件。这是通过链接器（linker）来完成的，链接器会将所有目标文件合并并解决其中的依赖关系。例如，链接器会把 `xyz.o` 和其他目标文件（比如 `file1.o`、`file2.o` 等）合并成一个可执行文件。

   - **编译阶段**：编译器将源文件编译成目标文件（.o 文件），这时的文件是机器可执行代码，但没有完全连接成程序。
   - **链接阶段**：链接器将目标文件结合起来，生成最终的可执行文件（比如 `a.out` 或 `program.exe`）。
5. **为什么停止在目标文件很重要？**

   对于大型项目，特别是当程序包含多个源文件时，编译和链接的分开处理非常重要：

   - **分割代码**：将程序分割成多个源文件有助于代码的组织与管理。每个源文件可以包含一个独立的功能模块，通过头文件（`.h`）进行接口声明，源文件之间通过引用这些接口来进行协作。
   - **提高编译效率**：在大型程序中，重新编译所有文件需要很长时间。通过 **只编译修改过的文件**，你能大幅缩短编译时间。

   例如，你修改了 `file1.c`，只需重新编译 `file1.c` 为 `file1.o`，然后和其他文件一起链接成最终的可执行文件，而不必重新编译 `file2.c`、`file3.c` 等。

   对比一下：

   - 如果你 **不使用目标文件**，每次修改一个文件都需要重新编译整个项目，可能需要很长时间。
   - 使用目标文件后，只需要重新编译修改过的文件，节省了大量时间。
6. **优化**

   在启用了优化选项（如 `-O2`、`-O3`）的情况下，编译过程可能会变得更慢，因为编译器会对代码进行更复杂的优化。这时，重新编译某个文件和重新编译整个项目的时间差异会非常显著。

   - **重新编译一个文件**：你只需要重新编译修改过的文件，节省大量时间。
   - **重新编译所有文件**：如果你不使用目标文件而每次都重新编译所有源文件，尤其是启用了优化的情况下，编译时间会显著增加。
7. **总结**：

   通过将代码拆分为多个源文件，每个文件单独编译成目标文件，可以大大提高编译效率。修改某个文件后只需重新编译该文件，而无需重新编译所有文件。最终，通过链接将所有目标文件合并成一个完整的可执行程序，这使得开发大型程序时能够节省大量的时间。

---

### MAKE(tool)

- 在emacs中进行 c-x c-v 就可以查看makefile的内容
- **makefile**制作

  ```
  myProgram: oneFile.o anotherFile.o
  gcc -o myProgram oneFile.o anotherFile.o
  oneFile.o: oneFile.c oneHeader.h someHeader.h
  gcc -std=gnu99 -pedantic -Wall -c oneFile.c
  anotherFile.o: anotherFile.c anotherHeader.h someHeader.h
  gcc -std=gnu99 -pedantic -Wall -c anotherFile.c
  ```

  这段文本是一个 **Makefile** 的示例，它定义了如何使用 `make` 工具来编译和链接 C 程序。让我来解释一下每个部分的作用：

  1. **目标和依赖关系**

     `myProgram: oneFile.o anotherFile.o`

     ```make
     myProgram: oneFile.o anotherFile.o
     ```

  - **目标（Target）**：`myProgram` 是构建的最终目标，它是要生成的可执行文件。
  - **依赖关系（Dependencies）**：`myProgram` 依赖于 `oneFile.o` 和 `anotherFile.o` 这两个目标文件（也叫目标对象文件）。这意味着，如果 `oneFile.o` 或 `anotherFile.o` 有变化，`myProgram` 就需要重新链接。

    `oneFile.o: oneFile.c oneHeader.h someHeader.h`

    ```make
    oneFile.o: oneFile.c oneHeader.h someHeader.h
    ```
  - **目标（Target）**：`oneFile.o` 是一个目标文件，它是通过编译源文件 `oneFile.c` 得到的。
  - **依赖关系（Dependencies）**：`oneFile.o` 依赖于 `oneFile.c` 和两个头文件 `oneHeader.h` 和 `someHeader.h`。如果这些源文件或头文件有任何修改，`oneFile.o` 就需要重新编译。

    `anotherFile.o: anotherFile.c anotherHeader.h someHeader.h`

    ```make
    anotherFile.o: anotherFile.c anotherHeader.h someHeader.h
    ```
  - **目标（Target）**：`anotherFile.o` 是另一个目标文件，它通过编译 `anotherFile.c` 得到。
  - **依赖关系（Dependencies）**：`anotherFile.o` 依赖于 `anotherFile.c` 和两个头文件 `anotherHeader.h` 和 `someHeader.h`。如果这些源文件或头文件发生变化，`anotherFile.o` 也需要重新编译。

  2. **构建命令**

  **`gcc -o myProgram oneFile.o anotherFile.o`**
  ``make gcc -o myProgram oneFile.o anotherFile.o ``

  - 这是 `make` 在构建 `myProgram` 时执行的命令。它使用 `gcc`（GNU 编译器）将 `oneFile.o` 和 `anotherFile.o` 链接成一个可执行文件 `myProgram`。
  - `-o myProgram` 指定了输出文件的名称，即最终生成的可执行文件是 `myProgram`。

  **`gcc -std=gnu99 -pedantic -Wall -c oneFile.c`**

  ```make
  gcc -std=gnu99 -pedantic -Wall -c oneFile.c
  ```

  - 这是编译 `oneFile.c` 的命令，生成目标文件 `oneFile.o`。
  - `-std=gnu99`：指定使用 GNU 99 标准来编译 C 程序。
  - `-pedantic`：启用编译器的严格模式，会对不符合标准的代码发出警告。
  - `-Wall`：启用所有常见的编译器警告，帮助开发者发现潜在的问题。
  - `-c`：表示编译源代码文件 `oneFile.c`，但不进行链接，只生成目标文件 `oneFile.o`。

  ** `gcc -std=gnu99 -pedantic -Wall -c anotherFile.c`
  ``make gcc -std=gnu99 -pedantic -Wall -c anotherFile.c ``

  - 这是编译 `anotherFile.c` 的命令，生成目标文件 `anotherFile.o`，命令和上面的编译命令基本相同，只是处理不同的源文件。

  3. **总结**
     整个 Makefile 的流程是：

     当你运行 make 时，它会检查 myProgram 是否需要更新。myProgram 依赖于 oneFile.o 和 anotherFile.o，如果这两个目标文件中的任何一个发生了改变，make 会重新编译它们。
     oneFile.o 依赖于 oneFile.c 和 oneHeader.h 等文件，如果这些源文件或头文件有变化，make 会重新执行编译命令 gcc -c oneFile.c。
     anotherFile.o 依赖于 anotherFile.c 和 anotherHeader.h 等文件，如果这些文件有变化，make 会重新编译 anotherFile.o。
     最终，如果 oneFile.o 和 anotherFile.o 没有错误，它们将会被链接成 myProgram 可执行文件。
     这个过程使得开发者在修改源代码时，只需要重新编译和链接修改过的部分，而不是整个程序，从而提高了效率。

  - 变量的灵活性：
    CC：可以指定为不同的 C 编译器（如 gcc, clang）。
    CFLAGS：编译选项，默认启用所有警告 (-Wall) 和优化 (-O2)。
    CPPFLAGS：预处理器标志，可用于指定头文件目录（如 -Iinclude）。
    LDFLAGS：链接器标志，适合需要外部库的情况（如 -lm 链接数学库）。
    TARGET_ARCH：指定目标架构，适用于交叉编译（如 -march=armv7）。
  - 默认规则：

    使用 %.o: %.c 模式规则，编译每个 .c 文件为 .o 文件。
    OUTPUT_OPTION 中的 $@ 是当前目标（如 something.o），$< 是第一个先决条件（如 something.c）。
  - 自动化：

    SRCS 和 OBJS 自动处理当前目录中的 .c 文件，无需手动更新源文件列表。
    使用伪目标 .PHONY 避免目标名与文件名冲突。
  - 清理规则：

    删除所有生成的中间文件和可执行文件，便于重新编译。

---

- **clean**
  在 **Makefile** 中，`clean` 是一个常见的目标，用来删除所有中间文件和生成的目标文件，从而使整个项目返回到一个干净的状态。通常，`clean` 目标会删除编译过程中的临时文件（如 `.o` 文件、可执行文件等），以便开发者可以重新构建整个项目。

  常见的 `clean` 目标的示例

  ```makefile
  # 定义源文件和目标文件
  CFLAGS=-std=gnu99 -pedantic -Wall
  OBJ = oneFile.o anotherFile.o
  TARGET = myProgram

  # 默认目标
  all: $(TARGET)

  # 构建目标程序
  $(TARGET): $(OBJ)
      gcc -o $(TARGET) $(OBJ)

  # 编译目标文件
  oneFile.o: oneFile.c oneHeader.h someHeader.h
      gcc $(CFLAGS) -c oneFile.c

  anotherFile.o: anotherFile.c anotherHeader.h someHeader.h
      gcc $(CFLAGS) -c anotherFile.c

  # clean目标，用于删除生成的目标文件和可执行文件
  .PHONY: clean   # .PHONY 申明clean是一个伪目标，不管有没有更改clean，都执行clean
  clean:
      rm -f $(OBJ) $(TARGET)
  ```

  **详细解释**

  1. **目标 `clean`**

     ```makefile
     clean:
         rm -f $(OBJ) $(TARGET)
     ```

     - `clean` 是一个目标，用于清理构建过程中生成的临时文件。
     - `rm -f $(OBJ) $(TARGET)` 通过 `rm` 命令删除目标文件（如 `.o` 文件）和最终的可执行文件（如 `myProgram`）。
     - `-f` 选项强制删除文件，即使文件不存在，也不会产生错误。
  2. **目标文件和依赖**

     - `$(OBJ)`：包含了目标文件的列表，通常是编译过程中生成的 `.o` 文件。
     - `$(TARGET)`：指定最终生成的可执行文件（在这个例子中是 `myProgram`）。
  3. **执行 `make clean`**

     - 当你在命令行中运行 `make clean` 时，`make` 会找到名为 `clean` 的目标并执行它。
     - 它会删除所有的中间文件和可执行文件，帮助你清理项目。

  **为什么需要 `clean` 目标？**

  - **重新构建项目**：有时在修改代码或依赖项后，需要重新构建项目。使用 `make clean` 可以确保删除所有旧的编译文件，防止它们干扰新构建的过程。
  - **节省空间**：临时文件（如 `.o` 文件）占用磁盘空间，通过 `make clean` 删除这些文件可以节省空间，尤其在大型项目中。
  - **防止潜在问题**：有时目标文件或中间文件的版本可能不一致，导致链接错误或不可预测的行为。使用 `clean` 可以避免这种问题。

  **使用 `make clean` 的步骤：**

  1. 在终端运行 `make` 来构建项目。
  2. 如果你需要重新开始或清理项目，运行 `make clean` 删除所有生成的文件。
  3. 重新运行 `make` 来重新构建项目。

  这样，`make clean` 使得项目的管理变得更加灵活和简洁。

==make== 现在不想仔细学到时候有空再来看看吧
https://www.gnu.org/software/make/manual/

```makefile
CC = gcc
CFLAGS = -std=gnu99 -pedantic -Wall -O3
DBGFLAGS = -std=gnu99 -pedantic -Wall -ggdb3 -DDEBUG
SRCS=$(wildcard *.c)
OBJS=$(patsubst %.c,%.o,$(SRCS))
DBGOBJS=$(patsubst %.c,%.dbg.o,$(SRCS))
.PHONY: clean depend all
all: myProgram myProgram-debug
myProgram: $(OBJS)
    gcc -o $@ -O3 $(OBJS)
myProgram-debug: $(DBGOBJS)
    gcc -o $@ -ggdb3 $(DBGOBJS)
%.dbg.o: %.c
    gcc $(DBGFLAGS) -c -o $@ $<
clean:
    rm -f myProgram myProgram-debug *.o *.c~ *.h~
depend:
    makedepend $(SRCS)
    makedepend -a -o .dbg.o  $(SRCS)
# DO NOT DELETE
anotherFile.o: anotherHeader.h someHeader.h
oneFile.o: oneHeader.h someHeader.h
```

这份 `Makefile` 是用来编译 C 程序的，目标是生成两个版本的程序：一个是发布版本 (`myProgram`)，另一个是调试版本 (`myProgram-debug`)。下面是每一部分的详细解释：

- 1. **变量定义部分**:

  - **`CC`**: 定义了使用的编译器，这里使用的是 `gcc`（GNU C 编译器）。
  - **`CFLAGS`**: 编译时使用的标志（flags）。包括：

    - `-std=gnu99`: 使用 GNU99 标准。
    - `-pedantic`: 强制检查所有标准要求，确保代码符合标准。
    - `-Wall`: 打开所有的警告信息。
    - `-O3`: 开启高级优化，通常用于发布版本以提高程序性能。
  - **`DBGFLAGS`**: 调试时使用的编译标志。包括：

    - `-std=gnu99`: 使用 GNU99 标准。
    - `-pedantic` 和 `-Wall`: 和 `CFLAGS` 中一样，保证编译时的严格性。
    - `-ggdb3`: 生成调试信息，以便用 GDB（GNU 调试器）调试程序。
    - `-DDEBUG`: 定义 `DEBUG` 宏，通常用于调试时启用特定的调试代码（例如打印调试信息）。
  - **`SRCS`**: 使用 `wildcard` 函数获取当前目录下所有的 `.c` 源文件。
  - **`OBJS`**: 使用 `patsubst` 函数将所有的 `.c` 文件转化为 `.o`（目标文件），这些文件是用来生成最终的可执行文件的。
  - **`DBGOBJS`**: 和 `OBJS` 类似，不过这里的 `.o` 文件后缀是 `.dbg.o`，用于调试版本。
- 2. **目标部分**:

  - **`all`**: 默认目标，执行时会依次构建 `myProgram` 和 `myProgram-debug`，即发布版本和调试版本。
  - **`myProgram`**: 生成发布版本的可执行文件，使用 `$(OBJS)`（普通的目标文件）并且进行优化 (`-O3`) 编译。
  - **`myProgram-debug`**: 生成调试版本的可执行文件，使用 `$(DBGOBJS)`（调试版的目标文件）并且带有调试信息 (`-ggdb3`)。
  - **`%.dbg.o`**: 这是一个模式规则（Pattern Rule），它指定了如何将 `.c` 源文件编译成带有调试信息的 `.dbg.o` 目标文件。使用 `$(DBGFLAGS)` 编译并生成 `.dbg.o` 文件。
  - **`clean`**: 清理目标，删除生成的程序文件、目标文件和临时文件（比如 `*.o`, `*.c~`, `*.h~` 等），用来清理构建过程中产生的中间文件。
  - **`depend`**: 这个目标使用 `makedepend` 命令来自动生成源文件的依赖关系，确保在编译时，头文件的修改能正确影响到源文件的重新编译。
- 3. **依赖管理部分**:

  - `makedepend` 会分析源文件和头文件的依赖关系，生成自动的依赖规则。比如，如果某个源文件引用了头文件，那么在头文件修改后，相应的源文件会被重新编译。
  - 目标 `depend` 会调用两次 `makedepend`，一次是生成普通的依赖信息，另一次则是为调试版本的目标文件（`.dbg.o`）生成依赖信息。
- 4. **手动添加的依赖**:

  - 在 `# DO NOT DELETE` 后面的部分，手动列出了某些目标文件与头文件的依赖关系。比如：
    - `anotherFile.o` 依赖于 `anotherHeader.h` 和 `someHeader.h`。
    - `oneFile.o` 依赖于 `oneHeader.h` 和 `someHeader.h`。

      这部分的目的是确保在相关头文件变化时，正确地重新编译对应的源文件。

---

## **`<font face="仿宋">`3.Pointers,Arrays,and Recursion `</font>`**

### 指针(Pointers)

#### 指针的定义

- code

  ```c
  main(){
      int x =5;
      int *xPtr;  //int * 表示xPtr是一个指针，指向一个int类型的变量

      xPtr = &x;  //&x表示x的地址，将x的地址赋值给xPtr
      *xPtr = 6;  //*xPtr表示xPtr指向的变量的值，将6赋值给xPtr指向的变量。此处的*表示取消引用。
      printf("x = %d\n",x);
  }
  ```
- 同时定义多个指针
  ```c
  int* p1, p2, p3;   // `p1` is of type `int*`, but `p2` and `p3` are ints.
  int *q1, *q2, *q3; // `q1`, `q2` and `q3` all have the type `int*`.
  int* r1, r2, *r3;  // `r1` and `r3` are of the type `int*`,
                     // while `r2` is an int  
  ```
- & and *
  `&`后必须是一个可以在内存中招到的变量 eg：`&(a+b)`and `&42` 是错误的。

- `++*ptr`是被允许的，但是`*++ptr`是不被允许的。
  先解引用，在对指向的变量进行修改。 

- 在32位的机器上,指针的大小始终是4字节。
- 程序与硬件的交互
  [![pEVKnO0.png](https://s21.ax1x.com/2025/01/28/pEVKnO0.png)](https://imgse.com/i/pEVKnO0)
  [![pEVKmyq.png](https://s21.ax1x.com/2025/01/28/pEVKmyq.png)](https://imgse.com/i/pEVKmyq)

- NULL指针
  在Memery图的底部，代码段下方有一个空白区域，这是程序内存中的无效区域。你可能会问，为什么代码不从地址 0 开始，而要浪费这个空间。原因是程序使用了一个数值为 0 的指针--它有一个特殊的名字 NULL，意思是 "不指向任何东西"。通过不将程序的任何有效部分置于（或接近）地址 0，我们可以确保没有任何东西会被置于地址 0。这就意味着，任何正确初始化的指针，只要真正指向某物，其值都不会是 NULL。
  ```c
  int *ptr = NULL;   //与  int *ptr = 0; 等价

  int m = 1;
  ptr = &m;
  ```
  空指针用于初始化指针，确保指针不会指向无效的内存地址。
  可以将指向空的指针重新赋值为指向其他有效的内存地址，但不可以解引用空指针。
  空指针通常用于表示一个无效的地址或指针。
  eg: `*ptr = 10;` 是错误的，因为ptr是空指针，不能解引用。

- 隐式初始化指针
  - 全局变量和静态局部变量的指针会被初始化为0（NULL）
  - 局部非静态变量的指针会被初始化为随机值，形成野指针，有巨大的风险。这个时候就要对指针进行初始化。
  - 用短路求值（circuit evaluation）（&&）来避免未初始化的指针
    ```c 
    int *ptr = NULL;
    if (ptr != NULL && *ptr == 42) {
    // 只有在 ptr 不为 NULL 时，才会执行 *ptr == 42
    printf("Pointer points to 42\n");
    }
    ```
- 什么时候用指针
  当你需要在调用函数的时候修改函数外的变量时，就需要用指针。
  ```c
  void fun(int *px) {
  *px = 42;
  }
  int main(void) {
  int i = 30;
  fun(&i); // int *px = &i
  printf("%d\n", i); // 42
  } 
  ```
  



---

#### 结构体指针

- code
  ```c
  struct A {
      int x;
  }

  struct A a;   
  a.x = 10;

  struct A *q = &a;   //用结构体指针访问
  (*q).x = 10;        //先解引用，再访问成员

  q->x = 10;          //直接访问成员
  ```
- 复杂嵌套结构
  ```c
  struct B {
  int y;
  };

  struct A {
      int x;
      struct B *b;
  };

  //声明并初始化结构体变量
  struct B b_instance = {20};
  struct A a_instance = {10, &b_instance};
  struct A *q = &a_instance;

  //访问b_instance的成员y
  q->b->y =  30;   //等价于(*q).b->y = 30;
  ```

---

#### 指针到指针

- 声明指针到指针
  [![pEVdWk9.png](https://s21.ax1x.com/2025/01/29/pEVdWk9.png)](https://imgse.com/i/pEVdWk9)
- 解引用指针到指针
  *r  指q
  **r  指q指向的变量p
  ***r  指q指向的变量指向的变量a

#### const

- 定义

  - 定义一个常量，其值不能被修改。

  ```c
  const int x = 3; // 赋值给 x 是非法的  
  ```
- const
  `const T(随便一种type) name = value;`
  必须要在声明时初始化。
  一旦初始化，const变量的值就不能再被修改。
  const变量必须在声明时初始化，否则会导致编译错误。
  const变量的值不能被修改，否则会导致编译错误。

- **指向指针的指针（Pointers to Pointers）**
  我们可以在不同的地方加 `const` 来限制修改权限。

  | **声明**                | **能修改 `**p` 吗？** | **能修改 `*p` 吗？** | **能修改 `p` 吗？** |
  | ----------------------------- | ----------------------------- | ---------------------------- | --------------------------- |
  | `int **p`                   | ✅**是**                | ✅**是**               | ✅**是**              |
  | `const int **p`             | ❌**否**                | ✅**是**               | ✅**是**              |
  | `int *const *p`             | ✅**是**                | ❌**否**               | ✅**是**              |
  | `int **const p`             | ✅**是**                | ✅**是**               | ❌**否**              |
  | `const int *const *p`       | ❌**否**                | ❌**否**               | ✅**是**              |
  | `const int **const p`       | ❌**否**                | ✅**是**               | ❌**否**              |
  | `int *const *const p`       | ✅**是**                | ❌**否**               | ❌**否**              |
  | `const int *const *const p` | ❌**否**                | ❌**否**               | ❌**否**              |
- `const` 只会影响 **通过该变量** 访问数据的方式，而不会影响数据本身的修改权限。

  - ✅ **示例 1（合法）**

    ```c
    int x = 3;
    const int *p = &x;  // `p` 不能修改 `x`，但 `x` 本身可以被修改
    x = 4;  // ✅ 允许
    ```

    虽然 `p` 不能修改 `*p`，但 `x` 不是 `const`，所以 `x` 的值可以直接修改。
  - ❌ **示例 2（非法）**

    ```c
    int x = 3;
    int *const p = &x;  // `p` 不能修改 `x`，但 `x` 本身可以被修改
    *p = 4;  // ❌ 错误：尝试修改 `x`
    ```

    错误信息：

    ```
    initialization discards 'const' qualifier from pointer target type
    ```

    问题在于 `y` 是 `const`，但 `q` 是一个普通 `int *` 指针，它允许修改 `*q`，这就违背了 `y` 的 `const` 限定。
- **总结（Summary）**

  1. `const int *p`  → **指针指向的值不能变，但指针本身可以变**。
  2. `int *const p`  → **指针本身不能变，但指针指向的值可以变**。
  3. `const int *const p`  → **指针和指针指向的值都不能变**。
  4. `const` 只影响**通过该变量修改数据的方式**，但数据本身仍可能通过其他方式修改。
     使用 `const` 可以提高代码的**安全性**和**可读性**，避免意外修改数据。

#### Aliasing(别名)

- 定义
  在讨论指针（Pointer）时，我们已经暗示了可能会有多个名称指向同一个内存位置，但尚未明确讨论这个现象。例如，在下图中，我们有四个不同的名字指向变量 `a` 所占据的内存空间：

  - `a`
  - `*p`
  - `**q`
  - `***r`

  当多个不同的名称（变量或表达式）指向同一个内存地址时，我们称这些名称**互为别名（Alias each other）**，也就是说，我们可以说 `*p` 是 `**q` 的别名。
- 不同类型的别名（Aliasing with Different Types）
  让我们分析**底层的存储**和**类型转换**的区别：

  - **(1) 类型转换（Type Casting）**

    ```cpp
    int x = (int) f;
    ```

    这个操作告诉编译器：

    > “请帮我把 `f` 这个 `float` 变量转换成 `int`。”
    >

    编译器会生成**额外的指令**，执行浮点到整数的转换，最终得到 `3`。
  - **(2) 底层存储**

    ```cpp
    float f = 3.14;
    int * p = (int *) &f;
    printf("%d\n", *p);
    ```

    这个操作告诉编译器：

    > “我不管 `f` 是什么类型，就把它的原始位模式按 `int` 方式读取。”
    >

    在计算机的存储中，**`float` 类型的 3.14** 实际存储的**二进制表示**是：

    ```
    0100 0000 0100 1000 1111 0101 1100 0011
    ```

    这个二进制值的**十六进制**形式是：

    ```
    0x4048F5C3
    ```

    如果把它**按整数（int）解析**，转换成十进制后就是：

    ```
    1078523331
    ```

    因此，程序打印 `1078523331`，而不是 `3`！
- **(3) 避免错误的指针转换**

  ```cpp
  float f = 3.14;
  int * p = (int *) &f;  // ❌ 通常是错误做法！
  printf("%d\n", *p);
  ```

  如果想转换类型，应该使用正确的转换方法，例如：

  ```cpp
  int x = (int) f;  // ✅ 正确做法
  ```

  这样会让编译器正确处理浮点到整数的转换，而不会错误地解释底层的位模式。

  ---

#### Pointer Arithmetic

- 定义
  指针的算术运算指的是**指针移动**的操作，通常用于**遍历数组**或**在内存中移动指针**。指针的算术运算涉及**指针的类型**和**指针所指向的类型**。

  例如，考虑以下代码：

  ```c
  int x = 4;
  float y = 4.0;
  int *ptr = &x;

  x = x + 1;   // 对整数变量 x 进行加法运算
  y = y + 1;   // 对浮点数变量 y 进行加法运算
  ptr = ptr + 1;   // 对指针变量 ptr 进行加法运算
  ```

  - `y = y + 1;`
  - `y` 是**浮点数（Floating Point Number）**，需要**先将 `1` 转换为 `1.0`，然后执行浮点数加法（Floating Point Addition）**，`y` 变成 `5.0`。
  - `ptr = ptr + 1;`
  - `ptr` 是**指针（Pointer）**，执行**指针运算（Pointer Arithmetic）**，它的值**并不会简单地加 `1`，而是会跳到下一个 `int` 类型的存储位置**。
- 指针的类型决定了指针**移动的步长**。例如，`int *` 类型的指针每次移动 `sizeof(int)` 个字节，而 `float *` 类型的指针每次移动 `sizeof(float)` 个字节。

  ```
  ptr = ptr + (1 * sizeof(int));  // 通常等价于 ptr = ptr + 4;
  ```

  如果 `sizeof(int) == 4`（即 `int` 类型占 4 个字节），那么 `ptr` 的值会增加 **4**，指向内存中**下一个整数的位置**。

- 指针的减法
  只能将同一个数组间的指针相减，得到两个指针之间的元素个数。
  不同数组间的指针相减是未定义行为。  



- 在 C 语言中，**指向 `void` 类型（`void *`）的指针不能进行加法或减法运算

  ```cpp
  void *ptr;
  ptr = ptr + 1;  // ❌ 编译错误！
  ```

  这是因为 `void *` 代表**不确定类型的指针（Pointer to unknown type）**，编译器无法确定“一个 `void` 类型的数据占多少字节”，所以无法计算 `ptr + 1` 应该移动多少字节。

  如果你确实需要对 `void *` 指针执行指针运算，必须先转换为**已知类型的指针**：

  ```cpp
  ptr = (char *)ptr + 1;  // ✅ 以字节（Byte）为单位移动
  ```

  这里，我们将 `ptr` 转换为 `char *`，然后再加 `1`，意味着指针会向前移动 `1` 个字节。
- **指针运算的风险：指针越界（Pointer Out-of-Bounds）**
  上面的指针运算在**数组（Array）**这样的**连续内存块**上是安全的，但如果在**单独的变量上进行指针加法或减法**，可能会导致**指针越界（Out-of-Bounds）**，造成**未定义行为（Undefined Behavior, UB）**。

  来看下面这段代码：

  ```cpp
  int x = 4;
  int *ptr = &x;
  ptr = ptr + 1;  // ❌ 这里发生了未定义行为！
  ```

  - `ptr` **最初指向 `x`，但是 `x` 并不是一个数组**，它在内存中是**孤立的**。
  - `ptr + 1` 会让 `ptr` 指向未知的内存位置，可能会指向：
  - `y` 变量的地址（如果 `y` 正好在 `x` 之后）。
  - 栈上的其他数据，比如函数的返回地址（可能导致崩溃）。
  - 甚至 `ptr` 本身的存储位置（可能引发奇怪的错误）。

  **这意味着 `ptr` 不再指向一个合法的 `int` 变量！如果你尝试 `*ptr = 3;`，程序可能崩溃，或者出现无法预测的错误。**
- **(2) 解决方案**
  如果你想让 `ptr` 进行加减运算，**应确保它指向一个数组（Array），而不是一个孤立的变量**：

  ```cpp
  int arr[3] = {10, 20, 30};
  int *ptr = arr;  // 指向数组的起始地址
  ptr = ptr + 1;   // ✅ 现在 ptr 指向 arr[1]
  printf("%d\n", *ptr);  // 输出 20
  ```

  这里 `ptr + 1` 是合法的，因为 `arr` 是一个连续的内存块。
- **总结**
  **(1) 指针加法（Pointer Addition）**
- `ptr + 1` 让 `ptr` 向前移动 **1 个元素的大小**（以 `sizeof(T)` 计算）。
- `ptr + N` 让 `ptr` 向前跳 `N` 个 `T` 类型的存储单元。

**(2) 指针不能随意运算**

- **`void *` 指针不能直接进行指针运算**（因为 `void` 没有确定的大小）。
- **对孤立变量进行 `ptr + 1` 可能导致未定义行为**，应该确保指针指向**数组（Array）**或**有效的内存块**。

**(3) 避免未定义行为**

- 只有当指针仍然指向有效的对象时，才可以对其进行解引用（`*ptr`）。
- **越界指针（Out-of-Bounds Pointer）** 可能导致程序崩溃或不可预测的错误。

**🔹 牢记：指针运算很强大，但必须确保它们指向的是**有效的内存地址**，否则会导致不可预测的错误！**

---

### 数组(Arrays)

#### 数组声明和初始化

- 定义
  - eg: `int myArray[4];` myArray是指向数组的4个方框中的第一个方框的指针，不是一个l值，我们不能改变它的指向。即myArray是一个常量指针
  - arr[i]等价于i[arr]   `因为arr[i]=*(arr+i)=*(i+arr)=i[arr]`
  - 数组的类型  是元素类型和数组大小的组合  eg`int myArray[4]`的类型是`int[4]`
- 声明和初始化数组
  ```c
  int myArray[4];               //声明一个包含4个int类型元素的数组 
  int myArray[4] = {1,2,3,4};   //声明并初始化一个包含4个int类型元素的数组
  int myArray[] = {1,2,3,4};    //声明并初始化一个包含4个int类型元素的数组，可以在声明的时候省略数组大小
  int myArray[4] = {0};         //声明并初始化一个包含4个int类型元素的数组，全部初始化为0
  int myArray[4] = {1}          //声明并初始化一个包含4个int类型元素的数组，只有第一个元素被初始化为1，其余元素被初始化为0
  int myArray[4] = {1,2};       //在初始化器中提供少于数组大小的值时，剩余的元素将被初始化为0
  int e[10] = {[0] = 2, 3, 5, [7] = 7, 11, [4] = 13};
  //[0] = 2 → e[0] = 2    ; 3 没有指定索引，填充到下一个位置 → e[1] = 3  ; 5 继续填充 → e[2] = 5  ; [7] = 7 → e[7] = 7   ; 11 没有指定索引，填充到下一个位置 → e[8] = 11  ; [4] = 13 → e[4] = 13
  ```
  - 声明的时候，对于全局数组，会默认将所有元素初始化为0； 对于静态局部变量数组，会默认将所有元素初始化为0； 对于局部变量数组，不会默认将所有元素初始化为0，会是随机垃圾值。
  - 指针数组与指向数组的指针的表示差别
    ```c
    int *p[4];    //p是一个数组，数组的元素是int*类型的指针
    int (*p)[4];  //p是一个指针，指向一个类型为int[4]的数组，数组的元素是int类型的
    ```

  
  
  - 结构化初始化
    ```c
    int myArray[5] = {0};
    int myArray[5] = {[2] = 10};//{0,0,10,0,0}
    ```
    如果给定了初始化器中的某个元素，那么它前面的元素将被初始化为0。给定的元素个数小于数组的大小，那么剩余的元素将被初始化为0。

- 初始化结构体
  ```c
  typedef struct A {
      int x;
      int y;
      int z;
  }point
  point p = {{1},{2},{3}};        //声明并初始化一个包含3个struct A类型元素的数组
  point p = { .x=1, .y=2, .z=3};  //这种声明方式更稳固可以防止后续在结构体里添加新的元素导致错误
  point p = {.x=1,.z=3};          //在初始化器中提供少于数组大小的值时，剩余的元素将被初始化为0
  ```
- 初始化一个结构体数组
  ```c
  point myPoints[] = {{.x =3, .y=4,.z=5},
                      {.x=1,.y=2},
                      {.x=3,.y=4}};      //声明并初始化一个包含3个struct A类型元素的数组
  ```

#### 将数组作为参数传递

| 方式                   | 代码示例                                          | 适用场景                             |
| ---------------------- | ------------------------------------------------- | ------------------------------------ |
| **指针传递**     | `void func(int *arr, int size)`                 | **推荐，适用于可变大小数组**   |
| **数组语法**     | `void func(int arr[], int size)`                | **等价于指针传递，代码更直观** |
| **指针偏移**     | `func(arr + 2, size - 2)`                       | **传递子数组**                 |
| **固定大小数组** | `void func(int (*arr)[5])`                      | **仅适用于固定大小数组**       |
| **结构体封装**   | `typedef struct {int *arr; int size;} Wrapper;` | **封装数组和大小，提高可读性** |
| **二维数组**     | `void func(int arr[][3], int rows)`             | **必须指定列数**               |


`int function(int a[10]){}`这里的长度并不能限定传入的数组的长度。
所以我们可以用`int function(int *a, int size){}`来限定传入的数组的长度。
eg
```c
void print(int *a, int n) {
for (int i = 0; i < n; ++i)
printf("%d\n", a[i]); // Look at this!
}
```
- 数组不能作为函数的返回值
  因为数组是一个局部变量，当函数返回时，数组的内存会被释放，返回的指针将指向无效的内存。
  所以我们可以用传递指针的方式来返回数组。
  eg
  ```c
  //将数组a中为奇数的元素反序后生成b数组
  int copy_odd_reversed(int *from, int n, int *to) {
  int cnt = 0;
  for (int i = n - 1; i >= 0; --i)
  if (from[i] % 2 == 1)
  to[cnt++] = from[i]; // What does this mean?
  return cnt;
  }
  int main(void) {
  int a[5] = {1, 2, 3, 5, 6}, b[5];
  int b_length = copy_odd_reversed(a, 5, b); // b_length == 3.
  // Now `a` is unchanged, and the values in `b` are {5, 3, 1}.
  } 
  ```
#### 悬而未决的指针

不要返回指向局部数组的指针（如函数内部的数组），因为数组存储在栈中，函数返回后会被销毁，导致悬空指针（Dangling Pointer）。

#### size_t & sizeof

size_t是一个无符号整数类型，通常用于表示数组的大小。
sizeof是一个运算符，用于计算一个对象的大小（以字节为单位）。sizeof(int)也可计算为int类型的大小。
`size_t size = sizeof(arr);`

### 数组与指针的转换
- 在 C 语言中，数组和指针之间有着密切的关系。当我们写 `p = &a[0]` 时，其中 `a` 是一个类型为 `T[N]` 的数组，`p` 是一个指针。

  - `p + i` 相当于 `&a[i]`。
  - `*(p + i)` 相当于 `a[i]`。

  这些是数组和指针之间常见的转换规则。我们可以通过这种方式访问数组的元素。

- **数组到指针的隐式转换**
  - 数组名本身是指向数组第一个元素的指针。所以，`p = &a[0]` 可以简化为 `p = a`。
  - 也就是说，**数组名 `a` 实际上就是指向数组第一个元素的指针**。这样，`p = a` 和 `p = &a[0]` 是等效的。

- **`*a` 等价于 `a[0]`**
  - `*a` 相当于访问数组的第一个元素，`a[0]`。两者的作用是一样的。
    - `*a` 表示取 `a` 所指向的位置的值，也就是数组的第一个元素。
    - `a[0]` 直接表示数组的第一个元素。
- eg
```c
 int a[10];
 bool find(int value) {
 for (int *p = a; p < a + 10; ++p)
 if (*p == value)
 return true;
 return false;
 }
 ``` 
---


### 字符串

#### 可变字符串

[![pEmBKC4.png](https://s21.ax1x.com/2025/02/08/pEmBKC4.png)](https://imgse.com/i/pEmBKC4)
`char str[] = "Heloo World\n";`等价于 `char str[] = {'H','e','l','l','o','\0'};`

#### 字符串相等

字符串的初始化是一个指向第一个字符的指针，所以比较两个字符串相等的 `str1==str2`是比较两个指针是否指向内存中的同一个位置。
而想要比较两个字符串的内容是否相等，需要使用 `strcmp(str1,str2)`函数(stringcompare)（区分大小写）`strcasecmp(str1,str2)`（不区分大小写）。

#### 字符串复制

`strncpy(dest,src,n)`将src(source)的前n个字符复制到dest(destination)中。
如果src的长度小于n，则dest的剩余部分将用空字符\0填充。
如果src的长度大于或等于n，则复制的结尾将不会有\0，需要手动添加 `dest[n] = '\0';`。

#### 将字符串转化为整数

字符串不能通过显式或隐式转换为整数 `char *str = "123"; int x = str;`是错误的
用解引用的方式 `char *str = "123"; int x = *str;`只可以将字符串的第一个字符转化为整数，并不能实现整个字符串转化为整数。
`atoi(str)`(**ASCII to Integer**)函数可以将ASCII字符串转化为整数如果遇到非数字字符，会停止转换并返回已转换的部分。
`strtol` (**string to long**) 是 C 语言标准库中的一个函数（位于 `<stdlib.h>` 头文件），用于将 **字符串转换为长整型 (`long`)**，并提供更好的错误检测机制。
**`strtol` 语法**

```c
long int strtol(const char *str, char **endptr, int base);
```

**参数说明**

- `str`：要转换的 C 字符串（以 `\0` 结尾）。
- `endptr`：如果不为 `NULL`，它会指向解析 **第一个无效字符** 的位置。
- `base`：转换的进制，常用值：

  - `10`（十进制）
  - `16`（十六进制，字符串必须以 `0x` 或 `0X` 开头）
  - `8`（八进制，字符串可选以 `0` 开头）
  - `0`（自动检测进制，如 `0x` 开头按十六进制，`0` 开头按八进制，否则按十进制）
    **`strtol` vs. `atoi`**| 特性       | `atoi`               | `strtol`                    |
    | ---------- | ---------------------- | ----------------------------- |
    | 进制支持   | 仅支持 10 进制         | 支持 2-36 进制（可自动检测）  |
    | 错误处理   | 不能检测错误           | 可检测转换失败（`endptr`）  |
    | 数值范围   | `int`                | `long`（通常范围更大）      |
    | 空字符处理 | 遇到非数字字符直接返回 | `endptr` 可指示转换停止位置 |
    | 推荐使用   | ❌ 不安全              | ✅ 更安全                     |
- `strtol("1234abc", &end, 10);` 解析 `1234` 为 `long` 型数值。
- `end` 指向 `"abc"`，表示转换在此停止。

**`manstring`**可以查看更多关于字符串的函数信息

### 多维数组

#### 声明和初始化

```c
double arr[3][4] = {
    {1.0, 2.0, 3.0, 4.0},
    {5.0, 6.0, 7.0, 8.0},
    {9.0, 10.0, 11.0, 12.0}
};
```

只能省略第一维的大小
arr[0]是一个指向第一行数组的指针等价于arr
arr[0][0]是第一行数组的第一个元素。&arr[0][0]等价于arr

#### 指针数组

[![pEmyMo6.png](https://s21.ax1x.com/2025/02/08/pEmyMo6.png)](https://imgse.com/i/pEmyMo6)

- myMatrix[i]是一个l值可以改变指向的位置，而直接定义一个二维数组就无法改变指向的位置。且用指针数组可以使得每一行的长度不同。

#### 字符串数组

`const char *words[] = {"Hello", "World", "!", NULL};`习惯以NULL结尾，标记一个字符串数组的结束，这样可以在while循环中遍历数组而无需知道数组的大小（设置遇到NULL就停止）

#### 函数指针

```c
typedef int (*int_function)(int);

void dotoAll(int *data, int size, int_function f){
    for(int i = 0; i < size; i++){
        data[i] = f(data[i]);
    }
}

int inc(int x) {
  return x + 1;
}

int square(int x) {
  return x * x;
}

int main() {
  int array1[] = {1, 2, 3};
  int array2[] = {4, 5, 6};
  int n1 = 3, n2 = 3;

  // 使用函数指针传递不同的操作
  doToAll(array1, n1, inc);    // 对 array1 的每个元素执行 inc 操作
  doToAll(array2, n2, square); // 对 array2 的每个元素执行 square 操作
}
```

函数名本身就是一个指向函数的指针
