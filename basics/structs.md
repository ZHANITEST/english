# 구조체

D언어에서 두 가지 이상의 다른 타입을 정의 하는 방법 중 하나로
`구조체`를 사용하는 방법이 있습니다:

    struct Person {
        int age;
        int height;
        float ageXHeight;
    }

`구조체`는 항상 스택메모리(`new`는 사용하지 않죠)에 생성되며
할당되거나 함수 호출의 매개변수(인수)로 전달될 때 **값으로서** 복사됩니다.

    auto p = Person(30, 180, 3.1415);
    auto t = p; // 복사

`구조체` 타입으로 새 객체를 만들 때 그 멤버들은 초기화 되어
When a new object of a `struct` type is created its members can be initialized
in the order they are defined in the `struct`. A custom constructor can be defined through
a `this(...)` member function. If needed to avoid name conflicts, the current instance
can be explicitly accessed with `this`:

    struct Person {
        this(int age, int height) {
            this.age = age;
            this.height = height;
            this.ageXHeight = cast(float)age * height;
        }
            ...

    Person p(30, 180); // 초기화
    p = Person(30, 180);  // 새 인스턴스로 할당

`구조체`는 멤버 함수의 모든 수를 포함합니다.
기본적으로 `public`로 설정되며 밖에서도 접근이 가능합니다.
이와 달리 `private`는 `구조체` 안에 있는 다른 멤버 함수에서만 이거나 같은 모듈의 다른 코드에서 호출 가능합니다.

    struct Person {
        void doStuff() {
            ...
        private void privateStuff() {
            ...

    p.doStuff(); // doStuff 메서드 호출
    p.privateStuff(); // 불가능

### 상수 멤버 함수

멤버 함수가 `const`로 선언된 경우 그 멤버에 관한 모든 수정을 허락하지 않습니다.
이 기능은 컴파일러가 강제적으로 막아섭니다.
멤버 함수를 `const`로 만들었을 땐 `const`나 `immutable` 객체로 호출할 수 있을 뿐만 아니라,
호출자에게 멤버 함수가 객체의 상태를 절대로 변경하지 않음을 보장합니다.

If a member function is declared with `const`, it won't be allowed
to modify any of its members. This is enforced by the compiler.
Making a member function `const` makes it callable on any `const`
or `immutable` object, but also guarantee callers that
the member function will never change the state of the object.

### 정적 멤버 함수

멤버 함수가 `static`으로 선언됬을 땐 `Person.myStatic()`처럼 인스턴스화 할 필요없이 호출할 수 있습니다.
그러나 `static` 멤버가 아닌 경우는 불가능 합니다.
`static` 멤버 함수는 

  A `static` member function can be used you to work to give access to all instances of a
`struct`, rather than the current instance, or when the
member function must be usable by callers that don't have an instance
available.  For example, Singleton's (only one instance is allowed)
use `static`.

### 상속

구조체가 다른 구조체로부터 상속할 수 없다는 점은 알아두세요.
나중에 보시게 될 클래스(Classes)부분을 보시면 아시겠지만 클래스 타입만 상속이 가능합니다.
다만 `alias this`나 `mixins`을 통해서 쉽게 다형성 상속이 가능합니다.

Note that a `struct` can't inherit from another `struct`.
Hierachies of types can only be built using classes,
which we will see in a future section.
However with `alias this` or `mixins` one can easily achieve
polymorphic inheritance.

### 더 자세하게

- [_Programming in D_의 구조체 부분](http://ddili.org/ders/d.en/struct.html)
- [구조체 명세서](https://dlang.org/spec/struct.html)

### 실습

Given the `struct Vector3` implement the following functions and make
the example application run successfully:

* `length()` - returns the vector's length
* `dot(Vector3)` - returns the dot product of two vectors
* `toString()` - returns a string representation of this vector.
  The function [`std.string.format`](https://dlang.org/phobos/std_format.html)
  returns a string using `printf`-like syntax:
  `format("MyInt = %d", myInt)`. Strings will be explained in detailed in a later
  section.

## {SourceCode:incomplete}

```d
struct Vector3 {
    double x;
    double y;
    double z;

    double length() const {
        import std.math: sqrt;
        return 0.0;
    }

    // rhs will be copied
    double dot(Vector3 rhs) const {
        return 0.0;
    }

    /**
    Returns: representation of the string in the
    special format. The output is restricted to
    a precision of one!
    "x: 0.0 y: 0.0 z: 0.0"
    */
    string toString() const {
        import std.string: format;
        // 힌트: refer to the documentation of
        // std.format to see how to influence
        // output for floating point numbers.
        return format("");
    }
}

void main() {
    auto vec1 = Vector3(10, 0, 0);
    Vector3 vec2;
    vec2.x = 0;
    vec2.y = 20;
    vec2.z = 0;

    // 멤버함수에 매개변수가 없을 시,
    // the calling braces () may be omitted
    assert(vec1.length == 10);
    assert(vec2.length == 20);

    // Test the functionality for dot product
    assert(vec1.dot(vec2) == 0);

    // 1 * 1 + 2 * 1 + 3 * 1
    auto vec3 = Vector3(1, 2, 3);
    assert(vec3.dot(Vector3(1, 1, 1) == 6);

    // 1 * 3 + 2 * 2 + 3 * 1
    assert(vec3.dot(Vector3(3, 2, 1) == 10);

    // Thanks to toString() we can now just
    // output our vector's with writeln
    import std.stdio: writeln, writefln;
    writeln("vec1 = ", vec1);
    writefln("vec2 = %s", vec2);

    // Check the string representation
    assert(vec1.toString() ==
        "x: 10.0 y: 0.0 z: 0.0");
    assert(vec2.toString() ==
        "x: 0.0 y: 20.0 z: 0.0");
}
```
