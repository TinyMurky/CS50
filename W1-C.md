# Link
- [課程本體連結](https://cs50.harvard.edu/x/2024/weeks/1/)
- [Lecture 1 筆記連結](https://cs50.harvard.edu/x/2024/notes/1/#lecture-1)

# 前導影片

[![IMAGE ALT TEXT HERE](https://img.youtube.com/vi/U3aXWizDbQ4/0.jpg)](https://www.youtube.com/watch?v=U3aXWizDbQ4)

# 課程內容
## 1. 本地端Compile C

> **GNU編譯器套裝**（英語：**GNU Compiler Collection**，縮寫為**GCC**） 為常見的C Compiler

Ubuntu/mint可以使用以下工具包安裝 gcc和 g++，但一般Ubuntu 已經預設安裝好了
```
sudo apt update && sudo apt install build-essential
```


> Compile 指令

假設我們有一個c file `hello.c` ，可以用以下方法編譯

1.
```
gcc hello.c
```
這種發法會直接輸出 `./a.out`檔案，在Command line中輸入 `./a.out`就可以執行

2.
```
gcc -o hello hello.c
```
加上 `-o` 可以幫產出的binary檔案取名字，像是上面指令會產出 `./hello`檔案

3.
```
gcc -S hello.c
```
s指令可以產出組合語言
## 2. import function from other file
- [How to invoke function from external .c file in C?](https://stackoverflow.com/questions/21260735/how-to-invoke-function-from-external-c-file-in-c)
- [Calling a function from another C file](https://riptutorial.com/c/example/3250/calling-a-function-from-another-c-file)
-  [#ifndef.#define, #endif 的用法](https://www.cnblogs.com/weekend/p/4794007.html)

假設我們有兩個 C 檔案在同一個資料夾：
- `add.c`

1.用 `#include "xxx.c"`
```c
// add.c
int add(int a, int b) {
    return a + b;
}
```

我們可以用 `#include "add.c"` 的方法從 別的file import function，但這不是最佳解。
```c
// main c
#include <stdio.h>

#include "add.c"

int main() {
    int myNumber1;
    int myNumber2;

    printf("Please Enter number1:");
    scanf("%d", &myNumber1);

    printf("Please Enter number2:");
    scanf("%d", &myNumber2);

    int result = add(myNumber1, myNumber2);
    printf("Answer is: %d", result);
    return 0;
}
```

使用 `.c`會遇到以下問題 [stackoverflow.com/a/232710/6028746](http://stackoverflow.com/a/232710/6028746)
>is it recommended? no - .c files compile to .obj files, which are linked together after compilation (by the linker) into the executable (or library), so there is no need to include one .c file in another. What you probably want to do instead is to make a .h file that lists the functions/variables available in the other .c file, and include the .h file


最佳解是把`add.c`改使用 `add.h`，在最上面使用`#ifndef` , `#define`，底端使用`#endif`  ， `ADD_H`這個詞可以用自己喜歡的字

>`#ifndef` , `#define`, `#endif` 很關鍵，是為了避免多重包含，比如如果兩個C文件包含同標頭檔，那麼就會出現問題，所以使用這種方式可以可以有效避免這種情况。
```c
// add.h
 #ifndef ADD_H
 #define ADD_H

int add(int a, int b) {
    return a + b;
}

 #endif // ADD_H
```

就可以正常import了
```c
//main.c
#include <stdlib.h>
#include <stdio.h>
#include "ClasseAusiliaria.h"

// main c
#include <stdio.h>

#include "add.h"

int main() {
    int myNumber1;
    int myNumber2;

    printf("Please Enter number1:");
    scanf("%d", &myNumber1);

    printf("Please Enter number2:");
    scanf("%d", &myNumber2);

    int result = add(myNumber1, myNumber2);
    printf("Answer is: %d", result);
    return 0;
}
```

## 3. header 標頭檔
標頭黨是用 `.h`結尾的檔案，有兩種呼叫方法
1. 存放在 路徑`/usr/include`裡的header可以用 `<>` 的方法呼叫，compiler會找到這個file的位置，ex `#include <stdio.h>`
2. 剩下的用雙引號並輸入路徑，ex: `#include "./header/add.h"`

## 3. Function
c 中的function 如下，function前方的 `int`代表function 回傳的type，function後面括號裡面代鰾要輸入的變數與他們的type，如果沒有可以輸入`void`
```c
int add(int a, int b) {
    return a + b;
}
```

`main()` 是特別的function，是c 程式啟動的入口，推薦在main跑完後return 0代表程式正常，回傳1代表不正常
```c
#include <stdio.h>

int main(void)
{
    printf("hello, world\n");
    return 0;
}
```


注意要被呼叫的fuction要放在呼叫他的function前面，或是向下面一樣先在 呼叫他的function前面init 一次
```c
void meow();

int main(void) {
    int i = 0;

    while (i < 3) {
        meow();
        i++;
    }
    return 0;
}

void meow() {
    printf("Meow!\n");
}
```
## 4. Variable
C 的variable有許多不同的type，每種type都有自己預設在記憶體中使用的byte的長度。


```c
#include <stdio.h>
int main(void) {
    char a = 'a';
    printf("%c\n", a);  // a
    printf("%d\n", a);  // 97
}
```
### 4-1 Variable Type
- [C - Data Types](https://www.tutorialspoint.com/cprogramming/c_data_types.htm)

| Sr.No. | Types & Description                                                                                                                                                            |
| ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 1      | **Basic Types**<br><br>They are arithmetic types and are further classified into: (a) integer types and (b) floating-point types.                                              |
| 2      | **Enumerated types**<br><br>They are again arithmetic types and they are used to define variables that can only assign certain discrete integer values throughout the program. |
| 3      | **The type void**<br><br>The type specifier _void_ indicates that no value is available.                                                                                       |
| 4      | **Derived types**<br><br>They include (a) Pointer types, (b) Array types, (c) Structure types, (d) Union types and (e) Function types.                                         |

#### Basic Types
##### int type
注意c中沒有 string, 只有 char array

|Type|Storage size|Value range|
|---|---|---|
|char|1 byte|-128 to 127 or 0 to 255|
|unsigned char|1 byte|0 to 255|
|signed char|1 byte|-128 to 127|
|int|2 or 4 bytes|-32,768 to 32,767 or -2,147,483,648 to 2,147,483,647|
|unsigned int|2 or 4 bytes|0 to 65,535 or 0 to 4,294,967,295|
|short|2 bytes|-32,768 to 32,767|
|unsigned short|2 bytes|0 to 65,535|
|long|8 bytes|-9223372036854775808 to 9223372036854775807|
|unsigned long|8 bytes|0 to 18446744073709551615|
##### Floating-Point Types
| Type        | Storage size | Value range            | Precision         |
| ----------- | ------------ | ---------------------- | ----------------- |
| float       | 4 byte       | 1.2E-38 to 3.4E+38     | 6 decimal places  |
| double      | 8 byte       | 2.3E-308 to 1.7E+308   | 15 decimal places |
| long double | 10 byte      | 3.4E-4932 to 1.1E+4932 | 19 decimal places |

####  void Type
| Sr.No | Types & Description                                                                                                                                                                                                                                    |
| ----- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 1     | **Function returns as void**<br><br>There are various functions in C that do not return any value or you can say they return **void**. A function with no return value has the return type as **void**. For example, **void exit (int status);**       |
| 2     | **Function arguments as void**<br><br>There are various functions in C which do not accept any parameter. A function with no parameter can accept a void. For example, **int rand(void);**                                                             |
| 3     | **Pointers to void**<br><br>A pointer of type void * represents the address of an object, but not its type. For example, a memory allocation function void ***malloc( size_t size );** returns a pointer to void which can be casted to any data type. |

#### Arrays in C
```c
int marks[5];
```
or
```c
int marks[ ]={50,56,76,67,43};
```

####  Pointers in C
需要注意pointer在不同architecture中的大小可能是不同的
```c
int x;  
int *y;  
y=&x;
```
#### User-defined Data Types in C
- **struct** 
- **union**

struct 的空見大小是內部所有variable 相加大小
```c
struct student   {   
   char name[20];   
   int marks, age;   
};
```

union大小是內部中最大的東西的大小
```c
union ab  {  
   int a;  
   float b;  
};
```

## 5. Conditional

if else if else結構

```c
    if (x < y)
    {
        printf("x is less than y\n");
    }
    else if (x > y)
    {
        printf("x is greater than y\n");
    }
    else
    {
        printf("x is equal to y\n");
    }
```

switch結構
但要小心沒有加上break的部份會fall back到下一個，像是case 10 和 cas 9
```c
    switch(score / 10) { 
        case 10:
        case 9: 
            cout << "得 A" << endl; 
            break; 
        case 8: 
            cout << "得 B" << endl; 
            break; 
        case 7: 
            cout << "得 C" << endl; 
            break; 
        case 6: 
            cout << "得 D" << endl; 
            break; 
        default: 
            cout << "得 E（不及格）" << endl; 
            break;
    } 
```

## 6. For loop
>for

```c
void meow();

int main(void) {
	for (int i = 0; i < 3; i++){
		meow();
	}
    return 0;
}

void meow() {
    printf("Meow!\n");
}
```

> while

如果符合條件就繼續下去
```c
void meow();

int main(void) {
    int i = 0;

    while (i < 3) {
        meow();
        i++;
    }
    return 0;
}

void meow() {
    printf("Meow!\n");
}
```

> do while

先做一次，如果之後符合 while條件就再做一次
```c
int main(void) {
    int readResult;
    unsigned long creditNum;
    do {
        printf("Number: ");
        readResult = scanf("%lu", &creditNum);
    } while (readResult != 1);  // 沒有成功讀取就一直讀

    bool isValid = validCredit(&creditNum);
    if (isValid) {
        // Do stuff
    } else {
        printf("INVALID\n");
    }

    return 0;
}


```

## 7. Argc / argv

C 可以在命令列裡面輸入想要對這個file input的指令，main會把他們接下來之後， 用 argv 接起來：

舉例來說下方這段 `scanf.c`
```c
#include <stdio.h>

int main(int argc, char *argv[]) {
    printf("argc: %d\n", argc);

    printf("argv[0]: %s\n", argv[0]);
    printf("argv[1]: %s\n", argv[1]);
    printf("argv[2]: %s\n", argv[2]);
}
```

compile 成 binary(也可以用 `make scanf`)
```
gcc -o scanf scanf.c
```

接著執行他，並在後面放入aaa, bbb
```
./scanf aaa bbb
```

可以看到下面 argc 呈現 `main()`接到3個值，這三個值存在 argv中，分別是：
- 檔案位置：`./scanf`
- aaa
- bbb

```
argc: 3
argv[0]: ./scanf
argv[1]: aaa
argv[2]: bbb
```

## 8. 備註：`scanf`使用注意事項
scanf有一個特點是會讀取記憶體中殘留的字串，舉例來說，以下有一個輸入int和字串並print出來的

```c
#include <stdio.h>

int main(void) {
    int num;
    char str[10];

    printf("Enter a number: ");
    scanf("%d", &num);
    printf("My Number is: %d\n", num);

    int c;
    while ((c = getchar()) != '\n' && c != EOF)
        ;
    printf("Enter a string: ");
    scanf("%s", str);
    printf("My String is: %s\n", str);
}
```

理想狀況如下，輸入1=> print出 1, 輸入 doller print出doller
```
> ./scanf
Enter a number: 1
My Number is: 1
Enter a string: doller
My String is: doller
```

但如果輸入 `1doller`會發現程式碼不會停下來等你輸入第二次，而是直接結束
這是因為 int 只會讀`1doller`的第一個字`1`，`doller`被留在記憶體裡，下一次scanf 會直接讀到記憶體殘留的`doller`
```
./scanf
Enter a number: 1doller
My Number is: 1
Enter a string: My String is: doller
```

解決方法是加入
```c
    int c;
    while ((c = getchar()) != '\n' && c != EOF)
        ;
```

ex
```c
#include <stdio.h>

int main(void) {
    int num;
    char str[10];

    printf("Enter a number: ");
    scanf("%d", &num);
    printf("My Number is: %d\n", num);

    int c;
    while ((c = getchar()) != '\n' && c != EOF)
        ;
    printf("Enter a string: ");
    scanf("%s", str);
    printf("My String is: %s\n", str);
}
```

他的功能是用while loop把記憶體裡面才於的東西都讀完，直到遇到 `\n`或句尾`EOF`才交給下個 `scanf`，結果如下

```
> ./scanf
Enter a number: 1asdasdasd
My Number is: 1
Enter a string: aaa
My String is: aaa
```
# 作業分享
## 1. Mario.c()
- [Mario](https://cs50.harvard.edu/x/2024/psets/1/mario/more/#mario)
目標，印出畫面如下
```
Height: 4
   #  #
  ##  ##
 ###  ###
####  ####
```

> 我的想法

1. C的string是由char 組成array 且最後一個字要是 `\0`
2. 先使用 do while迴圈幫助user 輸入 0～9的數字
3. 使用nest for loop, 範圍是 0 ～height(不包含height)
	1. 每一行的長度是`[height + i + 4]
	2. h 代表 左半邊預留的空格
	3. 4 拆成 `i+1`和3
	4. 3代表 `空格 空格 \0`
	5. `i+1` 代表右邊的`#`
4. 接著用以下邏輯填入空格
```c
        for(int j = 0; j < h; j++) {
            if (j < h -i -1){
                bar[j] = ' ';
            } else {
                bar[j] = '#';
                bar[j + i + 3] = '#';
            }
        }
```
## 2.  Credit
- [Credit](https://cs50.harvard.edu/x/2024/psets/1/credit/#credit)
目標： 驗證信用卡是否合規，並要辨認是 那一家的credit card

1. Validate Card
	1. 第一步驟，偶數digit加起來
		1. 用一個count紀錄現在是第幾位數
		2. while迴圈，當目標數字target大於0的時候執行
		3. 每次用`%`取出digit， `target /= target`讓target縮一個數字
		4. 如果 count % 2 == 0 就把digit 加入sum（記得10位和各位要拆開加入)
	2. 第二步驟，奇數加起來，用上面同方法取digit和算位數，然後加上sum
2. 分辨是那一張卡：
	1. 通過1辨認之後，用以下方法取出card Number的最前面兩個digit後，就可以用他們和number長度分便是那一張卡
```c
    int count = 0;
    int firstDigit = 0;
    int secondDigit = 0;
    while (target > 0) {
        count += 1;
        secondDigit = firstDigit;
        firstDigit = target % 10;
        target /= 10;
    }

```

## 3. prime

prime我想不太出來，目前我是放入10個 質數先到一個array裡面，當一個數字除了 質數array除了任何一個值數不是1，且以下這個算式為真，就不是植樹

```c
(float)(number / magic[i] == ((float)number / magic[i])))
```

```c
#include <stdbool.h>

bool prime(int number)
{
    // TODO
    int magic[10] = {2, 3, 5, 7, 11, 13, 17, 19, 23, 29};
    for (int i = 0; i < 10; i++) {
        if (number / magic[i] != 1 &&(float)(number / magic[i] == ((float)number / magic[i]))) {
            return false;
        }
    }
    return true;
}

```
