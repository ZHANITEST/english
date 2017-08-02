# 연관배열

D언어에는 우리가 해쉬 맵(hash map)이라고 알고 있는 *연관적인 배열*이라는 기능을 내장하고 있습니다.
문자열(`string`)인 키(key)와 정수형(`int`)인 값(value)으로 이루어진 연관배열은 아래와 같이 선언합니다:
An associative array with a key type of `string` and a value type
of `int` is declared as follows:

    int[string] arr;

키(key)를 사용해 값(value)에 접근하거나 저장도 할 수 있습니다:

    arr["key1"] = 10;

연관배열에 해당 키(key)가 존재하는 지 확인하려면 `in`문을 사용합니다:

    if ("key1" in arr)
        writeln("Yes");

`in` 표현식은 `null`외의 다른 포인터를 찾았을 때 값에 대란 포인터를 돌려줍니다.
이로서 겹치는 것인지 확인하거나 
The `in` expression returns a pointer to the value if it
can be found or a `null` pointer otherwise. Thus existence check
and writes can be conveniently combined:

    if (auto val = "key1" in arr)
        *val = 20;

Access to a key which doesn't exist yields an `RangeError`
that immediately aborts the application. For a safe access
with a default value, `get(key, defaultValue)` can be used.

AA's have the `.length` property like arrays and provide
a `.remove(val)` member to remove entries by their key.
It is left as an exercise to the reader to explore
the special `.byKey` and `.byValue` ranges.

### 더 자세하게

- [Associative arrays in _Programming in D_](http://ddili.org/ders/d.en/aa.html)
- [Associative arrays specification](https://dlang.org/spec/hash-map.html)
- [std.array.byPair](http://dlang.org/phobos/std_array.html#.byPair)

## {SourceCode}

```d
import std.stdio;

/**
Splits the given text into words and returns
an associative array that maps words to their
respective word counts.

Params:
    text = text to be splitted
*/
int[string] wordCount(string text)
{
    // The function splitter lazily splits the
    // input into a range
    import std.algorithm.iteration: splitter;
    import std.string: toLower;

    // Indexed by words and returning the count
    int[string] words;

    foreach(word; splitter(text.toLower(), " "))
    {
        // Increment word count if word
        // has been found.
        // Integers are by default 0.
        words[word]++;
    }

    return words;
}

void main()
{
    string text = "D is a lot of fun";

    auto wc = wordCount(text);
    writeln("Word counts: ", wc);

    // possible iterations:
    // byKey, byValue, byKeyValue
    foreach (word; wc.byValue)
        writeln(word);
}
```
