# 메모리

D는 시스템 프로그래밍 언어로서 프로그래머가 직접 메모리를 다루는 것을 허용합니다.
그러나 직접 메모리를 관리하는 것은 에러가 발생하기 쉬워서 D는 *쓰레기 수집기(GC)*를
기본적으로 사용하여 프로그램 상에서 더 이상 사용하지 않는 메모리를 초기화 시켜줍니다.
D is a system programming language and thus allows you to manually
manage. However manual memory management is very error-prone and thus
D uses a *garbage collector* per default to free unused memory.

D언어는 C언어처럼,`T*`이라는 포인터 자료형을 제공합니다:
D provides pointer types `T*` like in C:

    int a;
    int* b = &a; // a의 주소를 가지는 b
    auto c = &a; // c는 int* 이며 a의 주소를 가짐

A new memory block on the heap is allocated using the
`new` expression, which returns a pointer to the managed
memory:

    int* a = new int;

As soon as the memory referenced by `a` isn't referenced anymore
through any variable in the program, the garbage collector
will free its memory.

D언어는 `@system`, `@trusted`, `@safe`라는 3가지 함수를 위한 보안레벨을 가지고 있습니다.
D has three different security levels for functions: `@system`, `@trusted`, and `@safe`.
Unless specified otherwise, the default is `@system`.
`@safe` is a subset of D that prevents memory bugs by design.
`@safe` code can only call other `@safe` or `@trusted` functions.
Moreover explicit pointer arithmetic is forbidden in `@safe` Code:

    void main() @safe {
        int a = 5;
        int* p = &a;
        int* c = p + 5; // 에러
    }

`@trusted` functions are manually verified functions and allow to bridge the
world between SafeD and the underlying dirty low-level world.

### 더 자세하게
### In-depth

* [SafeD](https://dlang.org/safed.html)

## {SourceCode}

```d
import std.stdio;

void safeFun() @safe
{
    writeln("Hello World");
    // allocating memory with the GC is safe too
    int* p = new int;
}

void unsafeFun()
{
    int* p = new int;
    int* fiddling = p + 5;
}

void main()
{
    safeFun();
    unsafeFun();
}
```
