# 배열

D언어에는 **정적인 배열**과 **동적인 배열**로 두 가지 형태의 배열이 존재합니다.
모든 종류의 배열을 조작할 때 항상 경계 점검이 수행되는데 여기서 문제가 일어나면 `RangeError`라는 예외 발생 시킨 후 어플리케이션은 중단됩니다.
대담하게 이 기능을 사용하지 않을 수도 있는데,
컴파일러 플래그인 `-boundschecks=off`를 입력함으로서 성능을 쥐어 짜낼 수 있습니다.

There are two types of Arrays in D: **static** and **dynamic**
arrays. Access to arrays of any kind is always bounds checked -
a failed range check yields a `RangeError` which aborts the application.
The brave can disable this with the compiler flag `-boundschecks=off` to squeeze the last cycles out of their binary.

#### 정적 배열

정적 배열은 함수 안에서 정의될 경우 스택메모리에 저장되거나 스택 외의 곳에 저장됩니다.
정적배열의 길이는 컴파일타임에 알 수 있으며 고정된 길이를 가집니다:

    int[8] arr;

`arr`의 자료형은 `int[8]`입니다.
배열의 길이는 타입에 근접한 표시이며 나중에 C/C++처럼 바뀌지 않습니다.

#### 동적 배열
#### Dynamic arrays

동적 배열은 힙 메모리(heap)에 저장되며 프로그램이 실행되는 중에(runtime) 길이를 늘리거나 줄일 수 있습니다. 동적배열은 `new`구문을 통해 만들어지고 길이는 아래처럼 정해집니다:

    int size = 8; // 런타임(실행시간) 변수
    int[] arr = new int[size];

`arr`의 자료형은 **얇은** `int[]`이며 다음 주제에서 더 자세히 설명되어 있습니다. 다차원 배열은 `auto arr = new int[3][3]` 문법을 사용해 쉽게 만들 수 있습니다.

#### 배열 연산자와 속성

동적배열로 생성된 배열을 연결할 땐 `~`연산자를 사용합니다.
Arrays can be concatenated using the `~` operator which
will create a new dynamic array.

수치(숫자) 연산자는 
Mathematical operations can
be applied to whole arrays using the `c[] = a[] + b[]` syntax,
which for example adds all elements of `a` and `b` so that
`c[0] = a[0] + b[0]`. `c[1] = a[1] + b[1]` etc. It is also possible
to perform operations on a whole array with a single
value:

    a[] *= 2; // multiple all elements by 2
    a[] %= 26; // calculate the modulo by 26 for all a's

Those operations might be optimized
by the compiler to use special processors instructions that
do the operations in one go.

정적배열과 동적배열 모두 `.length`라는 속성을 가지고 있으며, 이 값은 정적배열은 읽기만 가능하지만 동적배열은 값을 수정하여 길이를 늘리거나 줄일 수 있습니다. `.dup`속성은 배열의 복사본을 돌려줍니다.

`arr[idx]`와 같은 구문으로 배열을 인덱싱 할 때 `$` 문자는 그 배열의 길이를 뜻하는 특별한 문자입니다. `arr[$ - 1]`
When indexing an array through the `arr[idx]` syntax the special
`$` syntax denotes an array's length. `arr[$ - 1]` thus
references the last element and is a short form for `arr[arr.length - 1]`.

### 실습
### Exercise

`encrypt`함수는 완벽하게 비밀 메세지를 해독해줍니다.
The text should be encrypted using *Caesar encryption*
that shifts the characters in the alphabet using a certain index.
The to-be-encrypted text just contains characters in the range `a-z`
which should make things easier.

Complete the function `encrypt` to decrypt the secret message.
The text should be encrypted using *Caesar encryption*
that shifts the characters in the alphabet using a certain index.
The to-be-encrypted text just contains characters in the range `a-z`
which should make things easier.

### 더 자세하게
### In-depth

- [_Programming in D_의 배열(Arrays in _Programming in D_)](http://ddili.org/ders/d.en/arrays.html)
- [배열 명세서(Array specification)](https://dlang.org/spec/arrays.html)

## {SourceCode:incomplete}

```d
import std.stdio;

/**
Shifts every character in the
array `input` for `shift` characters.
The character range is limited to `a-z`
and the next character after z is a.

Params:
    input = array to shift
    shift = shift length for each char
Returns:
    Shifted char array
*/
char[] encrypt(char[] input, char shift)
{
    auto result = input.dup;
    // ...
    return result;
}

void main()
{
    // We will now encrypt the message with
    // Caesar encryption and a
    // shift factor of 16!
    char[] toBeEncrypted = [ 'w','e','l','c',
      'o','m','e','t','o','d',
      // The last , is okay and will just
      // be ignored!
    ];
    writeln("Before: ", toBeEncrypted);
    auto encrypted = encrypt(toBeEncrypted, 16);
    writeln("After: ", encrypted);

    // Make sure we the algorithm works
    // as expected
    assert(encrypted == [ 'm','u','b','s','e',
            'c','u','j','e','t' ]);
}
```
