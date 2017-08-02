# Imports and modules

D언어로 작성한 간단한 hello world 프로그램에도 `import`는 필요합니다.
`import`문은 **모듈(module)**로 부터 모든 함수와 타입들을 사용할 수 있게 해줍니다.
For a simple hello world program in D, `import`s are needed.
The `import` statement makes all public functions
and types from the given **module** available.

[Phobos](https://dlang.org/phobos/)라 부르는 표준라이브러리는
`std` **패키지(package)** 하위에 존재하며 그 모듈들은
`import std.MODULE`를 입력하여 가져다 쓸 수 있습니다.
The standard library, called [Phobos](https://dlang.org/phobos/),
is located under the **package** `std`
and its modules are referenced through `import std.MODULE`.

또한 `import`문은 모듈의 기호를 선택할 수도 있습니다:
The `import` statement can also be used to selectively
import certain symbols of a module:

    import std.stdio: writeln, writefln;

선택적인 모듈반입(import)은 
Selective imports can be used to improve the readability by making
it obvious where a symbol comes from and also as a way to
prevent clashing of symbols with the same name from different modules.

`import`문을 꼭 소스파일의 맨 상단애 작성해야만 하는 건 아닙니다.
지역적인(locally) 함수에서도 작성해도 되는 등 모든 범위에서 작성해셔도 좋습니다.

## {SourceCode}

```d
void main()
{
    import std.stdio;
    // or import std.stdio: writeln;
    writeln("Hello World!");
}
```
