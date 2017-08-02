# 함수

`main()`함수를 전에 한번 소개해드렸었습니다 - 모든 D 프로그램의 시작점이지요. 함수는 값을 돌려주거나(return) - `void`로 선언했다면 아무것도 돌려주지 않으며 - 임의의 매개변수(인수)를 입력받기도 합니다:

    int add(int lhs, int rhs) {
        return lhs + rhs;
    }

### `auto`형 반환

반환될 자료형(타입)이 `auto`로 정의된 경우 D컴파일러는 반환해 줄 타입을 알아서
추측하여 반환합니다. 이 때문에 다중`return`에서는 반환될 값들을 모두 만족할 수 있는 단 한가지 타입으로 선택됩니다.

    auto add(int lhs, int rhs) { // `int`형으로 반환
        return lhs + rhs;
    }

    auto lessOrEqual(int lhs, int rhs) { // `double`형으로 반환
        if (lhs <= rhs)
            return 0;
        else
            return 1.0;
    }

### 기본 매개변수(인수)

함수는 기본 매개변수(인수)를 선택적으로 사용할 수 있습니다.
이 방법으로 인수를 장황하게 작성해야 할 일을 피할 수 있습니다.

    void plot(string msg, string color = "red") {
        ...
    }
    plot("D rocks");
    plot("D rocks", "blue");

Once a default argument has been specified all following arguments
must be default arguments too.

### 지역 함수

Functions might even be declared inside others functions where they may be
used locally and aren't visible to the outside world.
These function can even have access to objects local to
the parent's scope:

    void fun() {
        int local = 10;
        int fun_secret() {
            local++; // that's legal
        }
        ...

Such nested functions are called delegates and they will be explained in more depth
[soon](basics/delegates).

### 더 자세하게

- [Functions in _Programming in D_](http://ddili.org/ders/d.en/functions.html)
- [Function parameters in _Programming in D_](http://ddili.org/ders/d.en/function_parameters.html)
- [Function specification](https://dlang.org/spec/function.html)

## {SourceCode}

```d
import std.stdio;
import std.random;

void randomCalculator()
{
    // Define 4 local functions for
    // 4 different mathematical operations
    auto add(int lhs, int rhs) {
        return lhs + rhs;
    }
    auto sub(int lhs, int rhs) {
        return lhs - rhs;
    }
    auto mul(int lhs, int rhs) {
        return lhs * rhs;
    }
    auto div(int lhs, int rhs) {
        return lhs / rhs;
    }

    int a = 10;
    int b = 5;

    // uniform generates a number between START
    // and END, whereas END is NOT inclusive.
    // Depending on the result we call one of
    // the math operations.
    switch (uniform(0, 4)) {
        case 0:
            writeln(add(a, b));
            break;
        case 1:
            writeln(sub(a, b));
            break;
        case 2:
            writeln(mul(a, b));
            break;
        case 3:
            writeln(div(a, b));
            break;
        default:
            // special code which marks
            // UNREACHABLE code
            assert(0);
    }
}

void main()
{
    randomCalculator();
    // add(), sub(), mul() and div()
    // are NOT visible outside of their scope
    static assert(!__traits(compiles,
                            add(1, 2)));
}

```
