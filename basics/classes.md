# 클래스

D언어는 Java나 C++처럼 클래스(class)와 인터페이스(interface)를 제공합니다.

모든 `class` 자료형은 [`Object`](https://dlang.org/phobos/object.html)를 상속받습니다.

    class Foo { } // inherits from Object
    class Bar: Foo { } // Bar is a Foo too

D언어의 클래스는 `new`문으로 힙(heap)메모리를 사용하는 보통의 예 입니다:

    auto bar = new Bar;

클래스 객체는 항상 참조 타입이며 `struct`와 다르게 값으로의 복사가 이루어지지 않습니다.

    Bar bar = foo; // bar points to foo

쓰레기 수집기는 객체에 더 이상 참조가 존재하지 않을 때 메모리를 초기화 합니다.
The garbage collector will make sure the memory is freed
when no references to an object exist anymore.

#### 상속

If a member function of a base class is overridden, the keyword
`override` must be used to indicate that. This prevents unintentional
overriding of functions.

    class Bar: Foo {
        override functionFromFoo() {}
    }

D언의 경우 클래스는 단 하나의 클래스만 상속 받을 수 있습니다.

#### Final and abstract member functions

A function can be marked `final` in a base class to disallow overriding
it. A function can be declared as `abstract` to force base classes to override
it. A whole class can be declared as `abstract` to make sure
that it isn't instantiated. To access the base class,
use the special keyword `super`.

### In-depth

- [Classes in _Programming in D_](http://ddili.org/ders/d.en/class.html)
- [Inheritance in _Programming in D_](http://ddili.org/ders/d.en/inheritance.html)
- [Object class in _Programming in D_](http://ddili.org/ders/d.en/object.html)
- [Classes in D spec](https://dlang.org/spec/class.html)

## {SourceCode}

```d
import std.stdio;

/*
Fancy type which can be used for
anything...
*/
class Any {
    // protected is just seen by inheriting
    // classes
    protected string type;

    this(string type) {
        this.type = type;
    }

    // public is implicit by the way
    final string getType() {
        return type;
    }

    // This needs to be implemented!
    abstract string convertToString();
}

class Integer: Any {
    // just seen by Integer
    private {
        int number;
    }

    // constructor
    this(int number) {
        // call base class constructor
        super("integer");
        this.number = number;
    }

    // This is implicit. And another way
    // to specify the protection level
    public:

    override string convertToString() {
        import std.conv: to;
        // The swiss army knife of conversion.
        return to!string(number);
    }
}

class Float: Any {
    private float number;

    this(float number) {
        super("float");
        this.number = number;
    }

    override string convertToString() {
        import std.string: format;
        // We want to control precision
        return format("%.1f", number);
    }
}

void main()
{
    Any[] anys = [
        new Integer(10),
        new Float(3.1415f)
        ];

    foreach (any; anys) {
        writeln("any's type = ", any.getType());
        writeln("Content = ",
            any.convertToString());
    }
}
```
