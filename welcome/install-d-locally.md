# D언어 컴파일러 설치하기

D언어의 공식 웹사이트인 [dlang.org](https://dlang.org/)에서 최신 버전의 레퍼런스(공식) 컴파일러인 **DMD**(디지털 마르스 D; Digital Mars D)를 [다운로드](http://dlang.org/download.html/) 받고 설치하실 수 있습니다:

## 윈도우즈
* [설치파일](http://downloads.dlang.org/releases/2.x/2.071.1/dmd-2.071.1.exe/)
* [압축파일](http://downloads.dlang.org/releases/2.x/2.071.1/dmd.2.071.1.windows.7z/)
* [chocolatey](https://chocolatey.org/packages/dmd/) 사용하기: choco install dmd

## 매킨토시
* .dmg [패키지](http://downloads.dlang.org/releases/2.x/2.071.1/dmd.2.071.1.dmg/)
* [압축파일](http://downloads.dlang.org/releases/2.x/2.071.1/dmd.2.071.1.osx.tar.xz/)
* [Homebrew](http://brew.sh/) 사용하기: brew install dmd


## 리눅스 / FreeBSD / 매킨토시
디렉토리에 빠르게 dmd를 설치하는 방법은 아래 쉘을 실행하시거나:
> curl -fsS https://dlang.org/install.sh | bash -s dmd

다양한 리눅스 배포판들을 위한 패키지가 제공되고 있으니 참고하세요.

* 아치 리눅스
* 데비안/우분투
* 페도라/CentOS
* 젠투
* 오픈수세

## 다른 컴파일러들
DMD 레퍼런스 컴파일러 외에도 다른 백엔드 컴파일러가 존재합니다. [dlang.org](https://dlang.org) 공식 홈페이지에서는 아래의 두 컴파일러를 다운로드 페이지에서 받으실 수 있습니다:

* GCC 백엔드를 사용하는 **[GDC](http://gdcproject.org/downloads/)** 컴파일러
* LLVM 백엔드 기반의 **[LDC](https://github.com/ldc-developers/ldc#installation/)** 컴파일러
GDC와 LDC 컴파일러는 항상 최신 버전의 DMD 프론트엔드 버전을 따라서 제공되지는 않지만, 대신에 더 나은 최적화 레벨과 ARM과 같은 다양한 플랫폼에서 사용이 가능합니다.

[자세한 사항](https://wiki.dlang.org/Compilers/)은 위키에서 찾아보실 수 있습니다.
