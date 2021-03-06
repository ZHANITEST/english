## D언어에 오신 것을 환영합니다.
이 문서는 D언어에 대한 소개를 다룬 글(tour) 입니다.

## D언어란?
D는 독특하고 수 많은 기능들을 가진, 가지각색의 언어들을 위한 컴파일러 언어 입니다:

* 강력한 모델링 능력을 위한 높은 레벨의 구성
* 뛰어난 성능의 컴파일러 언어
* 정적 타이핑
* 운영체제 API와 하드웨어 제어를 위한 직접적인 인터페이스
* 무진장 빠른 컴파일-타임
* 메모리에 안전한 프로그래밍 지원(SafeD)
* 읽기 쉬운 코드로 유지보수가 용이
* 배우기 쉬움(C언어 스러운 문법과 자바와 같은 언어들과 비슷함)
* C 애플리케이션 바이너리 인터페이스 지원
* C++ 애플리케이션 바이너리 인터페이스 일부 지원
* 멀티 패러다임(엄격한 타입, 구조적, 객체지향, 제너릭, 함수형 프로그래밍, x86 어셈블리 지원)
* 에러 방지를 위해 내장되어 있는 방법들(계약 프로그래밍, 유닛테스트)

... 이 외에도 [다양한 기능들](http://dlang.org/overview.html)을 가지고 있습니다.

## 이 문서에 대해서
각 섹션마다 예제로 작성된 소스코드는 run단추를 누르면(또는 컨트롤-엔터를 눌러도 됩니다) 컴파일 후 실행결과를 볼 수 있습니다

## 기여
이 문서는 오픈소스이며, 이 문서가 더 나아질 수 있도록 주시는 도움들에 대해서 감사하게 생각합니다.
## {SourceCode}

```d
import std.stdio;

// Let's get going!
void main()
{
    writeln("Hello World!");
}
```
