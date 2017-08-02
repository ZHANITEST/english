# 델리게이트

### 인수로서의 함수

함수는 다른 함수의 인수(매개 값)로도 쓸 수 있습니다:

    void doSomething(int function(int, int) doer) {
        // 전달된 함수 호출
        doer(5,5);
    }

    doSomething(add); // `add`라는 전역함수(글로벌 함수)를 호출함
                      // add는 반드시 두 개의 int 인수가 필요

`doer`는 다른 평범한 함수들처럼 사용할 수 있습니다.

### 지역함수(로컬함수)와 문맥
### Local functions with context

방금 본 예제는 전역함수 포인터인 `function`타입을 사용했습니다.
멤버 함수나 지역 함수는 참조 형태이며 `delegate`가 쓰입니다.
함수 포인터는 추가적으로 함수를 *둘러 싼* 문맥에 대한 정보를 가집니다.
다른 언어들에서 이것을 **클로저(closure)**로 부릅니다.
예를 들어 `delegate`는 클래스의 멤버함수와 클래스 객체에서 포인터를 가리킵니다.
`delegate`는 중첩 함수에 포함된, 둘러싼 문맥을 참조하는 대신 생성됩니다.
그러나 D 컴파일러는 메모리 안전을 위해 자동으로 힙메모리에 문맥을 복사하고,
델리게이트는 이 힙메모리 영역을 참조합니다.

The above example uses the `function` type which is
a pointer to a global function. As soon as a member
function or a local function is referenced, `delegate`'s
have to be used. It's a function pointer
that additionally contains information about its
context - or *enclosure*, thus also called **closure**
in other languages. For example a `delegate`
that points to a member function of a class also includes
the pointer to the class object. A `delegate` created by
a nested function includes a link to the enclosing context
instead. However, the D compiler may automatically make a copy of
the context on the heap if it is necessary for memory safety -
then a delegate will link to this heap area.

    void foo() {
        void local() {
            writeln("local");
        }
        auto f = &local; // f의 타입은 delegate()
    }

The same function `doSomething` taking a `delegate`
would look like this:

    void doSomething(int delegate(int,int) doer);

`델리게이트(delegate)`와 `함수(function)` 객체는 섞일 수 없지만,
표준라이브러리 중에 [`std.functional.toDelegate`](https://dlang.org/phobos/std_functional.html#.toDelegate)를 사용해
`함수(function)`를 `델리게이트(delegate)`로 변환할 수는 있습니다.

### 익명함수 & 람다

As functions can be saved as variables and passed to other functions,
it is laborious to give them an own name and to define them. Hence D allows
nameless functions and one-line _lambdas_.

    auto f = (int lhs, int rhs) {
        return lhs + rhs;
    };
    auto f = (int lhs, int rhs) => lhs + rhs; // Lambda - internally converted to the above

It is also possible to pass-in strings as template argument to functional parts
of D's standard library. For example they offer a convenient way
to define a folding (aka reducer):

    [1, 2, 3].reduce!`a + b`; // 6

String functions are only possible for _one or two_ arguments and then use `a`
as first and `b` as second argument.

### In-depth

- [Delegate specification](https://dlang.org/spec/function.html#closures)

## {SourceCode}

```d
import std.stdio;

enum IntOps {
    add = 0,
    sub = 1,
    mul = 2,
    div = 3
}

/**
Provides a math calculuation
Params:
    op = selected math operation
Returns: delegate which does a math operation
*/
auto getMathOperation(IntOps op)
{
    // Define 4 lambda functions for
    // 4 different mathematical operations
    auto add = (int lhs, int rhs) => lhs + rhs;
    auto sub = (int lhs, int rhs) => lhs - rhs;
    auto mul = (int lhs, int rhs) => lhs * rhs;
    auto div = (int lhs, int rhs) => lhs / rhs;

    // we can ensure that the switch covers
    // all cases
    final switch (op) {
        case IntOps.add:
            return add;
        case IntOps.sub:
            return sub;
        case IntOps.mul:
            return mul;
        case IntOps.div:
            return div;
    }
}

void main()
{
    int a = 10;
    int b = 5;

    auto func = getMathOperation(IntOps.add);
    writeln("The type of func is ",
        typeof(func).stringof, "!");

    // run the delegate func which does all the
    // real work for us!
    writeln("result: ", func(a, b));
}
```
