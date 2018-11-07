---
title: "C2Dlang: pointer to const pointer"
date: 2018-10-01 
categories: [dlang]
tags: [aaa]
draft: false
---



> 이 글은 C를 기준으로 Dlang을 비교/설명함으로써 C 언어 사용자가 Dlang을 쉽게 이해할 수 있도록 하기 위해 작성되었습니다.  



제목처럼 C에서 "ponter"와 "const"의 조합을 Dlang에서는 어떻게 사용하는 지, 어떤 차이점이 있는지를 정리하고자 합니다. 

아시겠지만 C에서 const 키워드가 어떻게 사용되는지를 먼저 간단히 요약하면  

- read & write 가 모두 가능한 변수를 read only로 만들기 위한 방법이고  

- "#define" 을 대체하는 수단으로 사용될 수 있고 

  ```c
  #define MY_DEFINE1			1	// MY_DEFINE can be any of integer/float/double 	
  const float MY_CONST =		1; 	// MY_COSNT must be an float 
  ```



  함수 파라메터에 사용할 때는 해당 파라메터가 함수내에서 변경하지 않음을 명시할 수 있습니다 

  ```C
  void sub(const char* ptr)	// announce sub() never change contents of "ptr"
  {   ptr[0] = 0;				// sub) will not use this kind of operations
  }
  void main()
  {   char*   ptr = "my_data";
      sub(ptr);
  }
  ```



## const 키워드를 선언하는 방법의 차이 

그럼 본론으로 들어가서 C와 D에서의 "const" 선언 방법을 정리하면 아래와 같고 개인적으로는 Dlang의 const 선언 방법이 조금 더 의미 전달이 명확하다고 생각합니다  

| in C               | not allowed operation | in D                 |
| ------------------ | --------------------- | -------------------- |
| const int value    | value = 100;          | const (int) value    |
| const char* ptr    | *ptr = 0;             | const (char) * ptr   |
| char* const ptr    | ptr = NULL;           | const (char*) ptr    |
| const char** ptr   | **ptr = 0;            | const (char) ** ptr  |
| char * const * ptr | *ptr = 0;             | const (char *) * ptr |
| char ** const ptr  | ptr = NULL;           | const (char **) ptr  |



## const가 적용되는 범위의 차이  

또 다른 차이는 const 정의 후 read-only로 취급되는 범위가 아래와 같이 C보다 좀 더 넓어집니다.  

- in C 

|                    | ptr = NULL  | *ptr = NULL | **ptr = 0   |
| ------------------ | ----------- | ----------- | ----------- |
| char ** const ptr  | now allowed |             |             |
| char * const * ptr |             | not allowed |             |
| const char ** ptr  |             |             | not allowed |

- in D

|                      | ptr = NULL  | *ptr = NULL | **ptr = 0   |
| -------------------- | ----------- | ----------- | ----------- |
| const (char **) ptr  | not allowed | not allowed | not allowed |
| const (char*) * ptr  |             | not allowed | not allowed |
| const (char) ** ptr; |             |             | not allowed |



위와 같은 read-only가 작용되는 커버리지에 대한 확인은 아래와 같은 C와 D 코드를 컴파일해 보면 확인할 수 있습니다. 

```C
// in C 
#include <stdio.h>

void main()
{
    {   char**          x;
        x = NULL;
        *x = NULL;
        **x = 'c';
    }
    {   char**const     x;
        x = NULL;       // ERROR
        *x = NULL;
        **x = 'c';
    }
    {   char* const *   x;
        x = NULL;
        *x = NULL;      // ERROR
        **x = 'c';
    }
    {   const char**    x;
        x = NULL;
        *x = NULL;
        **x = 'c';      // ERROR
    }
}
```

```C
// in D 
void main()
{
    {   char**          x;
        x = null;
        *x = null;
        **x = 'c';
    }
    {   const (char**)  x;  // in C : char** const x;
        x = null;           // ERROR
        *x = null;          // ERROR
        **x = 'c';          // ERROR
    }
    {   const(char*)*   x;  // in C : char* const * x;
        x = null;
        *x = null;          // ERROR
        **x = 'c';          // ERROR
    }
    {   const (char)**  x;  // in C : const char**    x;
        x = null;
        *x = null;
        **x = 'c';          // ERROR
    }
}
```



---

내용 중 오류 또는 추가되어야 할 내용을 알려주시면 감사하겠습니다.   

이 저작물은 [CC 저작자표시-비영리-변경금지 4.0 국제 라이선스](http://creativecommons.org/licenses/by-nc-nd/4.0/)에 따라 이용할 수 있습니다. 
