## 운영체제 구조

- 운영체제는 프로그램이 실행되는 환경을 제공
- 운영체제를 살펴보기위한 관점
    1. 운영체제가 제공하는 서비스에 초점
    2. 운영체제가 사용자와 프로그래머에게 제공하는 인터페이스에 초점
    3. 시스템의 구성요소와 그들의 상호 연결에 초점

### 목표

- 운영체제에서 제공하는 서비스를 식별
- 운영체제 서비스를 제공하기 위해 시스템 콜을 사용하는 방법
- 운영체제 설계를 위한 모놀리식, 계층화, 마이크로 커널, 모듈 및 하이브리드 전략 비교
- 운영체제 부팅 프로세스
- 운영체제 성능 모니터링 도구
- Linux 커널과 상호 작용하기 위한 커널 모듈 설계 및 구현

### 운영체제 서비스

- 운영체제는 프로그램 실행 환경 제공
- 프로그램과 프로그램 사용자에게 특정 서비스를 제공
- 사용자 인터페이스 - User Interface
    - 거의 모든 운영체제는 **사용자 인터페이스(UI)**를 제공한다
    - 가장 일반적으로 **그래픽 사용자 인터페이스(GUI)**가 있다
    - 휴대전화 및 태블릿 환경에서는 **터치스크린 인터페이스**가 제공 된다
    - 이외 **명령어 라인 인터페이스(CLI)** 가 있다
- 프로그램 수행 - Program Execution
    - 시스템은 프로그램을 메모리에 적재해 실행 할 수 있어야 한다
    - 프로그램은 정상 또는 비정상적 이든 실행을 끝낼 수 있어야 한다
- 입출력 연산 - I/O Operation
    - 수행중인 프로그램은 입출력을 요구 할 수 있다
    - 파일 또는 입출력 장치가 연관될 수 있다
    - 특정 장치는 특수한 기능을 요구할 수 있다
        - 네트워크 인터페이스 또는 파일 시스템 등
    - 효율과 보호를 위해 사용자는 입출력 장치를 직접 제어할 수 없으며 운영체제가 입출력 수행의 수단을 제공해야 함
- 파일 시스템 조작 - File System Manipulation
    - 프로그램은 파일을 읽고 쓸 필요가 있다
    - 프로그램은 이름에 의해 파일을 검색, 생성 및 삭제 할 수 있어야 하며 파일의 정보를 열거 할 수 있어야 한다
    - 몇몇 프로그램은 파일 소유권에 기반하여 파일 및 디럭토리 접근을 허가 또는 거부 할 수 있게 한다
- 통신 - Communication
    - 프로세스간 정보를 교환해야 할 필요가 있는 상황
        1. 동일한 컴퓨터에서 수행되는 프로세스
            1. 공유 메모리를 통해 구현
        2. 네트워크에 의해 함께 묶여 있는 서로다른 컴퓨터에서 수행되는 프로세스
            1. 메시지 전달(Message passing) 기법을 사용하여 구현
            2. [패킷](https://bit.ly/3H8KElr)들이 운영체제에 의해 프로세스들 사이를 이동
- 오류 탐지 - Error Detection
    - 운영체제는 모든 가능한 오류를 항상 의식하고 있어야 함
    - CPU, 메모리 하드웨어, 입출력장치, 네트워크의 접속 실패, 사용자 프로그램 에서 일어날 수 있음
    - 운영체제는 올바르고 일관성 있는 계산을 보장해야 함
- 자원 할당 - Resource Allocation
    - 다수의 프로세스나 다수의 작업이 동시에 실행될 때 각각 자원을 할당 해야 함
    - CPU 사이클, 메인메모리, 파일 저장 들은 특수한 할당 코드를 가질 수 있음
    - 입출력 장치들은 훨씬 일방적인 요청과 방출 코드를 가질 수 있음
- 기록 작성 - Logging
    - 회계 또는 단순히 사용 통계를 내기위해 사용됨
- 보호와 보안 - Protection and Security
    - 서로다른 여러 프로세스가 병행하게 수행될때 한 프로세스가 다른 프로세스나 운영체제 자체를 방해해서는 안된다
    - 보호
        - 시스템 자원에 대한 모든 접근이 통제 되도록 보장하는 것
    - 보안
        - 각 사용자가 자원에 대한 접근을 원할 때 통상 패스워드를 사용해서 시스템에게 자기 자신을 인증하는 것
        - 네트워크 어댑터 등과 같은 외부 입출력 장치들을 부적합한 접근 시도로부터 지킨다
        - 침입의 탐지를 위해 모든 접속을 기록하는 것으로 범위를 넓힘

### 사용자와 운영체제 인터페이스

- 명령 인터프리터 - Command Interpreter
    - 대부분 운영체제는 명령 인터프리터를 프로세스가 시작 되거나 사용자가 처음 로그온 할 때 수행되는 특수한 프로그램으로 취급
    - 명령 인터프리터를 제공하는 시스템에서 해석기는 **쉘(Shell)** 이라 함
    - 명령 인터프리터의 중요한 기능은 사용자가 지정한 명령을 가져와서 수행하는 것
    - 많은 명령은 파일을 조작한다
        - 생성, 삭제, 리스트, 프린ㅌ, 복사, 수행 등
        - 구현 방식
            1. 명령 인터프리터 자체가 명령을 실행할 코드를 가지는 경우
            2. 시스템 프로그램에 의해 대부분의 명령을 구현하는 것
- 그래픽 기반 사용자 인터페이스 - Graphical User Interface
    - 마우스를 기반으로하는 윈도 메뉴 시스템을 사용
    - KDE 및 GNU 프로젝트에 의한 GNOME 데스크톱을 통해 GUI 인터페이스를 사용 할 수 있음
- 터치스크린 인터페이스 - Touch Screen Interface
    - 대부분의 모바일 시스템에는 명령 라인 인터페이스나 마우스 및 키보드 시스템이 실용적이지 않음
    - 스마트폰 및 휴대용 태블릿 컴퓨터는 일반적으로 터치스크린 인터페이스를 사용

### 시스템 콜 - System Call

- 운영체제에 의해 사용가능한 서비스를 인터페이스로 제공하는 것
- 응용 프로그래밍 인터페이스 - Application Programming Interface
    - API 는 각 함수에 전달되어야 할 매개변수
    - 프로그래머가 기대할 수 잇는 반환 값
    - 위 두가지를 포함하여 응용 프로그래머가 사용 가능한 함수의 집합을 명시
    - 실행시간 환경 - RTE
        - 컴파일러 또는 인터프리터를 포함하여 응용 프로그램을 실행하는데 필요한 전체 소프트웨어 제품군 및 다른 소프트웨어를 포함하는 것
        - 운영체제가 제공하는 시스템 콜에 대한 연결고리 역할을 하는 시스템 콜 인터페이스 제공
    - 매개변수 운영체제 전달 방법
        - 레지스터 활용
            - 레지스터 < 매개변수 개수
                - 메모리 내의 블록이나 테이블에 저장, 주소가 레지스터 내에 매개변수로 전달
            - 레지스터 > 매개변수 개수
                - 레지스터로 전달
            - 스택과 블록을 활용 할 경우 매개변수의 개수나 길이 제한이 없다
            - Linux 에서는 매개변수 5개를 기점으로 방식을 조합해서 사용한다
- 시스템 콜의 유형 - Types of System Calls
    - 프로세스 제어
        - end, abort, load, execute, attributes
    - 파일 조작
        - create, delete, open, close
    - 장치 조작
        - release, detach, attach
    - 정보 유지보수
        - 시간 날짜 설정
        - 데이터 설정
        - 프로세스, 파일, 장치 속성
        - 메모리 덤프
    - 통신
        - 통신 연결
        - 메시지 송수신
    - 보호
        - 파일 권한
    

### 시스템 서비스 - System Service

- 파일관리
    - 파일과 디렉토리에 대해 생성, 삭제, 복사, 개명, 인쇄, 열거, 조작
- 상태정보
    - 단순한 프로그램들은 시스템에게 상태정보를 묻는다
    - 복잡한 프로그램은 상세한 성능, 로깅 및 디버깅 정보를 제공한다
- 파일변경
    - 디스크나 다른 저장 장치에 저장된 파일의 내용을 생성 및 변경 하기 위해 에디터를 사용 할 수 있다
    - 파일의 내용을 검색하거나 변환 하기 위해 특수한 명령어가 제공되기도 한다
- 프로그래밍언어지원
    - 일반적인 프로그래밍 언어들에 대한 컴파일러, 어셈블러, 디버거 및 해석기가 운영체제와 함께 제공되거나 별도로 다운로드 할 수 있다
- 프로그램 적재와 수행
    - 프로그램이 어셈블 되거나 컴파일 된 후 수행 되려면 반드시 메모리에 적재 되어야 한다
    - 시스템은 다음을 제공 할 수 있다
        - Absolute loader
        - Relocatable loader
        - Linkage editor
        - Overlay loader
    - Loader
        - 프로그램을 주기억장치에 적재하는 시스템 소프트웨어
        - 메모리 할당(Allocation), 연결(Linking), 재배치(Relocation), 적재(Loading) 기능을 수행
        - 컴파일러나 링커 등의 소프트웨어가 로더의 기능을 대신하여 수행 되기도 함
    - Linker
        - 링커는 컴파일러에 의해 번역한 목적 프로그램과 라이브러리 등을 연결하여 실행 가능한 모듈을 만드는 시스템 소프트웨어
- 통신
    - 프로세스, 사용자 및 다른 컴퓨터 시스템들 사이에 가상 접속을 이루기 위한 기법을 제공
- 백그라운드 서비스
    - 모든 범용 시스템은 부트할 때 특정 시스템 프로그램을 시작 시킬 수 있는 방법을 가짐
    1. 주어진 프로세스를 완수하고 종료
    2. 시스템이 정지 될 때 까지 계속해서 실행되는 프로세스 - Daemon

### 링커와 로더 - Linkers and Loaders

- 프로그램을 메모리로 가져와 프로세스 형태로 배치 되어야 CPU 에서 실행 가능하다
- 재배치 가능 오브젝트 파일
    - 소스파일 -컴파일→ 오브젝트 파일
- 링커
    - 재배치 가능 오브젝트 파일 → Binary file 로 결합
    - 다른 오브젝트 파일 또는 라이브러리도 포함 될 수 있다
- 로더
    - 이진 실행 파일을 메모리에 적재 하는데 사용됨
        - CPU 코어에서 실행 할 수 있는 상태
- 재배치
    - 링크 및 로드와 관련된 활동을 말함
    - 프로그램 부분에 최종 주소를 할당, 프로그램 코드와 데이터를 해당 주소와 일치 하도록 조정
    - 프로그램이 실행될 때 코드가 라이브러리 함수를 호출하고 변수에 접글 할 수 있게 함
- 실행 절차 설명
    - 명렁어 라인에 프로그램 이름을 입력하면 Shell 은
    1. `fork()` 시스템 콜을 사용하여 프로그램을 실행하기 위한 새 프로세스를 생성
    2. `exec()` 시스템 콜로 로더를 호출
    3. `exec()` 에 실행 파일 이름을 전달
    4. 로더는 새로 생성된 프로세스의 주소공간을 사용, 지정된 프로그램을 메모리에 적재
    - 대부분 시스템은 프로그램이 적재 될때 라이브러리를 동적으로 링크 할 수 있게 한다
        - 윈도우, DDL - Dynamic Linked Library
        - 장점
            - 실행파일에서 사용되지 않을 수 있는 라이브러리를 링크하고 로드하지 않아도 된다
            - 여러 프로세스가 동적으로 링크된 라이브러리를 공유 할 수 있다
                - 메모리 사용이 크게 절약 될 수 있다
        - 단점
            - 라이브러리는 조건부로 링크되며 실행에 필요한 경우 적재된다
    - 오브젝트 파일 및 실행파일은 표준화된 형식을 가진다
        - 표준화 형식
            - 컴파일된 기계코드 및 프로그램에서 기호 테이블(참조되는 함수 및 변수에 대한 메타데이터)을 포함
            - UNIX 및 Linux
                - ELF - Executavle and Linkable Format
            - Window
                - PE - Portable Executable
            - macOS
                - Mach-O

### 응용 프로그램이 운영체제마다 다른 이유

- 하나의 API 집합을 호출하도록 설계된 응용 프로그램은 해당 API를 제공하지 않는 운영체제에서는 작동하지 않는다.
- 예를들어
    - Window 용으로 컴파일된 프로그램은 macOS 에서 실행이 안된다
    - Android apk 파일은 iOS 에서 설치가 안된다

### 운영체제 설계 및 구현

- 요구조건(시스템 목표와 명세)들은 기본적으로 `사용자 목적` 과 `시스템 목적` 의 두 그룹으로 나뉜다
- 기법과 정책을 분리하는 것이 가장 중요한 원칙이다
    - 기법
        - 어떤일을 `어떻게` 할 것 인가 결정하는 것
    - 정책
        - `무엇`을 할것인가 결정 하는 것
    - 질문이 무엇이 아니라 `어떻게` 일때마다, 반드시 결정 되어야 하는것은 `기법`이다.
    

### 운영체제 구조

- 모놀리식 구조 == 밀접하게 결합된 시스템
    - 커널의 모든 기능을 단일 주소 공간에서 실행되는 단일 정적 이진 파일에 넣는것
    - 쉽게말해 모든 기능을 하나의 실행파일에 담는다
    - 장점
        - 시스템 콜 인터페이스에는 오버헤드가 거의 없다
        - 커널 안에서 통신속도가 빠르다
- 계층적 접근 == 느슨하게 결합된 시스템
    - 기능이 특정 기능 및 한정된 기능을 가진 개별적이며 작은 구성요소로 나뉨
    - 나뉘어진 기능들을 합쳐서 커널을 구성
    - 장점
        - 한 구성요소의 변경이 해당 구성요소에만 영향을 미치고 다른 구성요소에는 영향을 미치지 않음
        - 시스템 구현자가 시스템의 내부 작동을 더 자유롭게 생성하고 변경 할 수 있음
        - 디버깅의 간담함
    - 단점
        - 운영체제 서비스를 얻기 위해 사용자 프로그램이 여러 계층을 통과해야 한다
        - 오버헤드가 크다
- 마이크로 커널 == 느슨하게 결합된 시스템
    - 경량화 된 커널
    - 통신 외에 최소한의 프로세스와 메모리 관리를 제공
    - 클라이언트 프로그램과 시스템 서비스는 직접 상호작용하지 않고 마이크로커널을 통해 상호작용 한다
- 모듈 == 느슨하게 결합된 시스템
    - Loadable Kernel Modules = `LKM`
    - 커널은 핵심적인 구성요소의 집합을 가짐
    - 부팅 때 또는 실행중에 부가적인 서비스들을 모듈을 통하여 링크 함
    - 요약) 커널은 핵심 서비스를 제공하고 다른 서비스들은 커널이 실행되는 동안 동적으로 구현 됨
    - 계층구조와 유사점
        - 커널의 각 부분이 정의되고 보호된 인터페이스를 가지는 점
    - 마이크로 커널과 유사점
        - 중심 모듈은 핵심 기능만 가지고 있고 다른 모듈의 적재 방법과 타 모듈과 어떻게 통신하는지 안다는 점
