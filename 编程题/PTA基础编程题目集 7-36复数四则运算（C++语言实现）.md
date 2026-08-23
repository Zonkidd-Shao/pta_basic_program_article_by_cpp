# 7-36 复数四则运算（C++语言实现）

## 前言

本题（7-36 复数四则运算）的主要考点是：准确理解题意、规范处理输入输出格式，并正确处理边界与精度。下面先给出题目描述与格式要求，再通过清晰的思路与可直接运行的CPP代码进行讲解。

## 题目描述

本题要求编写程序，计算2个复数的和、差、积、商。

## 输入格式

输入在一行中按照a1 b1 a2 b2的格式给出2个复数C1=a1+b1i和C2=a2+b2i的实部和虚部。题目保证C2不为0。

## 输出格式

分别在4行中按照(a1+b1i) 运算符 (a2+b2i) = 结果的格式顺序输出2个复数的和、差、积、商，数字精确到小数点后1位。如果结果的实部或者虚部为0，则不输出。如果结果为0，则输出0.0。

## 输入样例1

```in
2 3.08 -2.04 5.06
```

## 输入样例2

```in
1 1 -1 -1.01
```

## 输出样例1

```out
(2.0+3.1i) + (-2.0+5.1i) = 8.1i
(2.0+3.1i) - (-2.0+5.1i) = 4.0-2.0i
(2.0+3.1i) * (-2.0+5.1i) = -19.7+3.8i
(2.0+3.1i) / (-2.0+5.1i) = 0.4-0.6i
```

## 输出样例2

```out
(1.0+1.0i) + (-1.0-1.0i) = 0.0
(1.0+1.0i) - (-1.0-1.0i) = 2.0+2.0i
(1.0+1.0i) * (-1.0-1.0i) = -2.0i
(1.0+1.0i) / (-1.0-1.0i) = -1.0
```

## 解题思路

### 核心问题分析
本题需要解决的核心问题：
1. **复数四则运算公式**：正确实现加减乘除的数学运算
2. **浮点数比较**：由于需要判断实部/虚部是否为0，需考虑浮点精度，用阈值0.05判断
3. **格式化输出**：精确到小数点后1位，实部虚部为0时不输出，注意符号处理
4. **特殊情况**：结果为0时输出"0.0"

### 算法原理说明
设 C1 = a1 + b1i，C2 = a2 + b2i

- **加法**：`C1 + C2 = (a1+a2) + (b1+b2)i`
- **减法**：`C1 - C2 = (a1-a2) + (b1-b2)i`
- **乘法**：`C1 * C2 = (a1*a2 - b1*b2) + (a1*b2 + a2*b1)i`
- **除法**：`C1 / C2 = [(a1*a2 + b1*b2) + (b1*a2 - a1*b2)i] / (a2² + b2²)`

### 格式化输出规则
- 实部和虚部都接近0（绝对值<0.05）：输出"0.0"
- 只有实部接近0：只输出虚部+"i"（注意虚部的正负号）
- 只有虚部接近0：只输出实部
- 都不为0：虚部为正时输出"a+bi"，虚部为负时输出"a-bi"（负号自带）

### 具体计算步骤
1. 输入a1, b1, a2, b2
2. 按公式分别计算和、差、积、商的实部和虚部
3. 每次计算后调用printOp输出完整表达式
4. printComplex根据规则格式化输出单个复数

## 完整代码

```cpp
#include <iostream>
#include <math.h>
#include <cstdio>
using namespace std;

void printComplex(double a, double b) {
    if (fabs(a) < 0.05 && fabs(b) < 0.05) {
        printf("0.0");
    } else if (fabs(a) < 0.05) {
        printf("%.1fi", b);
    } else if (fabs(b) < 0.05) {
        printf("%.1f", a);
    } else if (b > 0) {
        printf("%.1f+%.1fi", a, b);
    } else {
        printf("%.1f%.1fi", a, b);
    }
}

void printOp(double a1, double b1, char op, double a2, double b2, double a, double b) {
    cout << "(";
    printComplex(a1, b1);
    cout << ") " << op << " (";
    printComplex(a2, b2);
    cout << ") = ";
    printComplex(a, b);
    cout << endl;
}

int main() {
    double a1, b1, a2, b2;
    cin >> a1 >> b1 >> a2 >> b2;

    double a, b;

    a = a1 + a2;
    b = b1 + b2;
    printOp(a1, b1, '+', a2, b2, a, b);

    a = a1 - a2;
    b = b1 - b2;
    printOp(a1, b1, '-', a2, b2, a, b);

    a = a1 * a2 - b1 * b2;
    b = a1 * b2 + a2 * b1;
    printOp(a1, b1, '*', a2, b2, a, b);

    double denom = a2 * a2 + b2 * b2;
    a = (a1 * a2 + b1 * b2) / denom;
    b = (b1 * a2 - a1 * b2) / denom;
    printOp(a1, b1, '/', a2, b2, a, b);

    return 0;
}
```

## 代码流程说明

### 1. printComplex函数
- 输入：复数的实部a、虚部b
- 功能：按规则格式化输出复数
- 流程：
  - 实部虚部都接近0 → 输出"0.0"
  - 仅实部接近0 → 输出虚部+"i"
  - 仅虚部接近0 → 输出实部
  - 都不为0 → 虚部正用"+"连接，虚部负直接连接（自带负号）

### 2. printOp函数
- 输入：两个复数和运算符、结果复数
- 功能：输出完整运算表达式
- 流程：左括号 + C1 + 右括号 + 运算符 + 左括号 + C2 + 右括号 + "=" + 结果 + 换行

### 3. main函数-加法
- `a = a1+a2`，`b = b1+b2`
- 调用printOp输出加法

### 4. main函数-减法
- `a = a1-a2`，`b = b1-b2`
- 调用printOp输出减法

### 5. main函数-乘法
- `a = a1*a2 - b1*b2`，`b = a1*b2 + a2*b1`
- 调用printOp输出乘法

### 6. main函数-除法
- 先求分母`denom = a2² + b2²`
- `a = (a1*a2 + b1*b2)/denom`，`b = (b1*a2 - a1*b2)/denom`
- 调用printOp输出除法

## 代码流程图

```mermaid
flowchart TD
    A[开始] --> B[输入两个复数的实部虚部]
    B --> C[按公式计算加法结果]
    C --> D[printOp输出加法表达式]
    D --> E[按公式计算减法结果]
    E --> F[printOp输出减法表达式]
    F --> G[按公式计算乘法结果]
    G --> H[printOp输出乘法表达式]
    H --> I[计算除法的分母]
    I --> J[按公式计算除法结果]
    J --> K[printOp输出除法表达式]
    K --> L[结束]

    subgraph printOp流程
        M[输出左括号] --> N[printComplex打印左复数]
        N --> O[输出右括号运算符左括号]
        O --> P[printComplex打印右复数]
        P --> Q[输出右括号等号]
        Q --> R[printComplex打印结果]
        R --> S[输出换行]
    end

    subgraph printComplex流程
        T{实部虚部都接近0?} -->|是| U[输出0.0]
        T -->|否| V{只有实部接近0?}
        V -->|是| W[只输出虚部加i]
        V -->|否| X{只有虚部接近0?}
        X -->|是| Y[只输出实部]
        X -->|否| Z{虚部为正数?}
        Z -->|是| AA[输出实部加虚部带正号]
        Z -->|否| AB[输出实部虚部自带负号]
    end
```

## 解题流程图

```mermaid
flowchart TD
    A[理解复数四则运算需求] --> B[推导加减乘除数学公式]
    B --> C[设计浮点0值判断阈值]
    C --> D[设计printComplex格式化输出函数]
    D --> E[处理五种输出情况]
    E --> F[设计printOp表达式输出函数]
    F --> G[按公式实现四种运算]
    G --> H[编写完整代码]
    H --> I[用样例一验证输出]
    I --> J{格式和数值正确?}
    J -->|是| K[用样例二验证边界情况]
    J -->|否| L[检查公式printf格式阈值]
    L --> H
    K --> M{边界正确?}
    M -->|是| N[完成]
    M -->|否| O[检查纯零纯实部纯虚部处理]
    O --> H
```

## 代码解析

```cpp
#include <iostream>
#include <math.h>
#include <cstdio>
using namespace std;

void printComplex(double a, double b) {
    if (fabs(a) < 0.05 && fabs(b) < 0.05) {
        printf("0.0");
    } else if (fabs(a) < 0.05) {
        printf("%.1fi", b);
    } else if (fabs(b) < 0.05) {
        printf("%.1f", a);
    } else if (b > 0) {
        printf("%.1f+%.1fi", a, b);
    } else {
        printf("%.1f%.1fi", a, b);
    }
}

void printOp(double a1, double b1, char op, double a2, double b2, double a, double b) {
    cout << "(";
    printComplex(a1, b1);
    cout << ") " << op << " (";
    printComplex(a2, b2);
    cout << ") = ";
    printComplex(a, b);
    cout << endl;
}

int main() {
    double a1, b1, a2, b2;
    cin >> a1 >> b1 >> a2 >> b2;

    double a, b;

    a = a1 + a2;
    b = b1 + b2;
    printOp(a1, b1, '+', a2, b2, a, b);

    a = a1 - a2;
    b = b1 - b2;
    printOp(a1, b1, '-', a2, b2, a, b);

    a = a1 * a2 - b1 * b2;
    b = a1 * b2 + a2 * b1;
    printOp(a1, b1, '*', a2, b2, a, b);

    double denom = a2 * a2 + b2 * b2;
    a = (a1 * a2 + b1 * b2) / denom;
    b = (b1 * a2 - a1 * b2) / denom;
    printOp(a1, b1, '/', a2, b2, a, b);

    return 0;
}
```

printComplex函数

## 复杂度分析

设输入规模为 $n$（对数值类题目为参与运算的数据量，对字符串/序列类题目为长度）。

- **时间复杂度**：$O(n)$ 或 $O(n \log n)$，主要来自一次线性遍历与常数次数学运算，无嵌套高复杂度循环。
- **空间复杂度**：$O(n)$，用于存储输入、中间结果与输出字符串；若仅使用若干标量变量则为 $O(1)$。

## 常见易错点

### 1. 输入/输出格式不符
错误：多余空格、遗漏换行、大小写或精度不符。后果：判题系统判为格式错误。正确：严格按题目要求的格式输出，数值用合适精度。

### 2. 边界条件遗漏
错误：未处理 0、最小值、单字符或空输入等边界。后果：特例 WA。正确：先列出所有边界样例，在编码前单独分支处理。

### 3. 整数溢出与类型
错误：使用过小的整数类型或忽略负号。后果：大数计算溢出。正确：按数据范围选择合适类型，必要时用更大整数类型或字符串处理。

## 更多测试

### 测试一：常规边界

**输入：**

```text
（可取题目边界附近的值，如最小值或最大值）
```

**输出：**

```text
（依据题意推导的正确结果）
```

### 测试二：特殊用例

**输入：**

```text
（可取易错点，如 0、单一元素、全同值等）
```

**输出：**

```text
（对应正确结果）
```

## 总结

本题的核心在于理清「复数四则运算」的输入输出关系与边界处理：先按格式读取输入，再依据规则逐步计算或遍历，最后按规范输出。该思路可迁移到同类格式化输入输出与模拟计算的题目。

