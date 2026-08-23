# PTA基础编程题目集 6-6求单链表结点的阶乘和（C++语言实现）

## 题目描述

本题要求实现一个函数，求单链表`L`结点的阶乘和。这里默认所有结点的值非负，且题目保证结果在`int`范围内。

### 函数接口定义

```cpp
int FactorialSum( List L );
```

其中单链表`List`的定义如下：

```cpp
typedef struct Node *PtrToNode;
struct Node {
    int Data; /* 存储结点数据 */
    PtrToNode Next; /* 指向下一个结点的指针 */
};
typedef PtrToNode List; /* 定义单链表类型 */
```

### 裁判测试程序样例

```cpp
#include <stdio.h>
#include <stdlib.h>

typedef struct Node *PtrToNode;
struct Node {
    int Data; /* 存储结点数据 */
    PtrToNode Next; /* 指向下一个结点的指针 */
};
typedef PtrToNode List; /* 定义单链表类型 */

int FactorialSum( List L );

int main()
{
    int N, i;
    List L, p;

    scanf("%d", &N);
    L = NULL;
    for ( i=0; i<N; i++ ) {
        p = (List)malloc(sizeof(struct Node));
        scanf("%d", &p->Data);
        p->Next = L;  L = p;
    }
    printf("%d\n", FactorialSum(L));

    return 0;
}

/* 你的代码将被嵌在这里 */
```

### 输入样例

```in
3
5 3 6
```

### 输出样例

```out
846
```

## 解题思路

这道题的核心是**链表遍历累加阶乘**：从链表头 L 开始遍历，只要结点不为空，就对当前结点的 Data 值计算阶乘并累加到 totalsum，然后让 L 指向下一个结点，直到链表遍历完毕。

### 核心问题分析

1. **链表遍历**：从链表头 L 开始，用 while (L != NULL) 循环，结点非空就进入循环体。
2. **阶乘计算**：对当前结点的 Data 值计算阶乘，sum 从 1 开始，i 从 Data 递减到 1 累乘。
3. **累加与移动**：将阶乘累加进 totalsum，再让 L = L->Next 移动到下一个结点。

### 算法原理说明

题目要求求单链表中所有结点值的阶乘之和。思路：从链表头 `L` 开始遍历，只要结点不为空，就对当前结点的 `Data` 值计算阶乘并累加到 `totalsum`，然后让 `L` 指向下一个结点，直到链表遍历完毕，返回总和。

### 具体计算步骤

1. 初始化 totalsum = 0。
2. while (L != NULL) 循环遍历链表：结点非空则进入循环体。
3. 对当前结点值计算阶乘：sum 从 1 开始，for 循环变量 i 从 L->Data 递减到 1，每轮执行 sum = sum * i。
4. 将阶乘累加进 totalsum，指针后移 L = L->Next。
5. 链表遍历完后返回 totalsum。


## 完整代码

```cpp
// 题目：6-6 求单链表结点的阶乘和
// 要求：遍历单链表，求每个结点Data的阶乘之和
//
// 实现原理：
//   while遍历链表，对每个Data用循环计算阶乘后累加
#include <stdio.h>
#include <stdlib.h>

typedef struct Node *PtrToNode;
struct Node {
    int Data; // 存储结点数据
    PtrToNode Next; // 指向下一个结点的指针
};
typedef PtrToNode List; // 定义单链表类型

int FactorialSum(List L);

int main()
{
    int N, i;
    List L, p;
    scanf("%d", &N);
    L = NULL;
    for (i = 0; i < N; i++) {
        p = (List)malloc(sizeof(struct Node));
        scanf("%d", &p->Data);
        p->Next = L;  L = p; // 头插法
    }
    printf("%d\n", FactorialSum(L));
    return 0;
}

/* 遍历链表，逐点计算阶乘并求和 */
int FactorialSum(List L)
{
    int i;
    int sum, totalsum = 0;
    while (L != NULL) {
        sum = 1;
        for (i = L->Data; i >= 1; i--) {
            sum = sum * i; // 计算当前结点阶乘
        }
        totalsum += sum; // 累加
        L = L->Next;
    }
    return totalsum;
}
```

下面仅给出需要嵌入裁判程序（位于 `/* 你的代码将被嵌在这里 */` 或 `/* 请在这里填写答案 */` 处）的函数实现，可直接复制到提交框：

## 代码流程说明

1. 初始化 totalsum = 0。
2. while (L != NULL) 循环遍历链表：结点非空则进入循环体。
3. 对当前结点值计算阶乘：sum 从 1 开始，for 循环变量 i 从 L->Data 递减到 1，每轮执行 sum = sum * i。
4. 将阶乘累加进 totalsum，指针后移 L = L->Next。
5. 链表遍历完后返回 totalsum。

## 代码流程图

```mermaid
flowchart TD
  A["开始"] --> B["totalsum = 0"]
  B --> C{"L != NULL?"}
  C -- "否" --> G["返回 totalsum"]
  C -- "是" --> D["sum = 1, i 从 L->Data 递减到 1"]
  D --> E["totalsum = totalsum + sum"]
  E --> F["L = L->Next"]
  F --> C
  G --> H["结束"]
```

## 解题流程图

```mermaid
flowchart TD
  A["开始"] --> B["读取 N 并建立链表"]
  B --> C["从链表头开始遍历"]
  C --> D{"当前结点是否为空?"}
  D -- "是" --> E["输出阶乘总和"]
  D -- "否" --> F["计算当前结点值的阶乘"]
  F --> G["累加进总和"]
  G --> H["移到下一个结点"]
  H --> C
  E --> I["结束"]
```
