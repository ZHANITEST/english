# Foreach문

{{#img-right}}dman-teacher-foreach.jpg{{/img-right}}

D언어의 `foreach` 반복문은 열거형을 읽기 쉽게 만들어주며
에러를 줄여주도록 해주는 기능입니다.

### 열거형 요소
`int[]`타입의 `arr`배열은 `foreach`반복문을 사용해 요소를 하나씩 꺼내줍니다:

    foreach (int e; arr) {
        writeln(e);
    }

`foreach`반복문의 첫번째 필드는 반복문에서 열거할 변수 이름을 선언합니다.
타입은 자동으로 포함되므로 따로 지정해주지 않아도 됩니다:

    foreach (e; arr) {
        // typeof(e)은 int
        writeln(e);
    }

두번째 필드는 반드시 배열-이거나 **range**라 부르는, 열거가능한 객체가 와야만 합니다.
이에 대한 내용은 다음 섹션에서 소개해드리겠습니다.

### 침조로서 접근
### Access by reference

요소는 배열(array)이나 범위(range)가 열거되는 동안에 복사됩니다.

Elements will be copied from the array or range during iteration.
This is acceptable for basic types, but might be a problem for
large types. To prevent copying or to enable *in-place
*mutation, `ref` can be used:

    foreach (ref e; arr) {
        e = 10; // overwrite value
    }

### Reverse iteration with `foreach_reverse`

A collection can be iterated in reverse order with
`foreach_reverse`:

    foreach_reverse (e; [1, 2, 3]) {
        writeln(e);
    }
    // 3 2 1

### In-depth

- [`foreach` in _Programming in D_](http://ddili.org/ders/d.en/foreach.html)
- [`foreach` with Structs and Classes  _Programming in D_](http://ddili.org/ders/d.en/foreach_opapply.html)
- [`foreach` specification](https://dlang.org/spec/statement.html#ForeachStatement)

## {SourceCode}

```d
import std.stdio;

void main() {
    auto arr = [ [5, 15], // 20
          [2, 3, 2, 3], // 10
          [3, 6, 2, 9] ]; // 20

    // Iterate through array in reverse order
    import std.range: retro;
    foreach (row; retro(arr))
    {
        double accumulator = 0.0;
        foreach (c; row)
            accumulator += c;

        writeln("The average of ", row,
            " = ", accumulator / row.length);
    }
}
```
