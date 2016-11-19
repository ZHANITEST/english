# D 프로그램 실행하기

D에는 컴파일러인 dmd와 스크립트와 비슷한 실행 도구인 rdmd 그리고 패키지 매니저인 dub이 포함되어 있습니다.

## DMD 컴파일러
DMD 컴파일러는 D코드 파일을 컴파일하여 바이너리를 만듭니다. 커맨드라인에서 코드파일 이름을 입력하면 됩니다:

    dmd hello.d

이 외에도 다양한 컴파일 옵션(플래그)들을 가지고 있는데, `dmd --help`을 입력하시거나 [온라인 문서](https://dlang.org/dmd-windows.html#switches)를 참고하시기 바랍니다.

## 자유롭게 바로 편집하는 rdmd
DMD 컴파일러와 함께 배포되는 rdmd 도구는 컴파일 시 필요한 의존성들과 자동으로 실행하여 결과를 출력해줍니다.

    rdmd hello.d

리눅스 시스템의 경우 `#!/usr/bin/env rdmd`를 맨 첫줄에 추가해주면 D 파일을 스크립트처럼 실행할 수 있습니다.

[온라인 문서](https://dlang.org/rdmd.html)나 `rdmd --help`를 입력하시면 자세한 옵션(플래그)들을 확인할 수 있습니다.

## **dub** 패키지 매니저
[dub](http://code.dlang.org)은 D의 표준 패키지 매니저 입니다. dub이 설치되어 있다면, 새 프로젝트를 생성할 때 아래 커맨드라인과 같이 입력합니다:

    dub init hello


dub을 실행하면 이 폴더에, 필요한 모든 의존성들이 담겨지고 프로그램이 컴파일된 후 실행됩니다. `dub build`를 입력하면 프로젝트가 컴파일 됩니다.

[온라인 문서](https://code.dlang.org/docs/commandline)나 `dub hlep`를 실행하여 자세한 명령어와 기능들을 확인할 수 있습니다.

dub 패키지들(라이브러리)은 [dub의 웹 사이트](https://code.dlang.org)에서도 찾아볼 수 있습니다.
