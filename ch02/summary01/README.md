# 2 쉘 스크립트 기초 문법

## 2.1 쉘 스크립트 생성, 실행

쉘 스크립트는 주로 `.sh` 확장자 파일로 작성한다.

- 시작 시 `#!/bin/bash`를 입력하여 해당 파일이 쉘 스크립트인 것을 알린다.

vi 에디터로 `hello.sh` 파일을 만들고 실행해 보자.

```bash
$ vi hello.sh
#!/bin/bash

echo "hello world"

:wq

# 실행
$ sh hello.sh
```

쉘 스크립트를 실행 시 터미널에 hello world 문자열이 출력된다.

---

### 2.1.1 chmod, echo 명령어

`chmod` 명령어를 이용하여 실행 권한을 부여한다면, 쉘 스크립트를 직접 실행할 수 있다.

```bash
$ chmod +x myshell.sh
$ ./myshell.sh            # "hello world" 문자열 출력됨
```

혹은 `echo` 명령어를 이용하면, 파일을 작성할 필요 없이 바로 쉘 스크립트를 실행할 수 있다.

```bash
$ echo "hello world"      # "hello world" 문자열 출력됨
```

---

## 2.2 변수 사용하기

쉘 스크립트에서 변수를 정의할 때는 일반적으로 변수 타입을 지정하지 않는다.

- 앞서 정의한 변수를 사용할 경우 \$ 기호를 앞에 붙이면 된다.

```bash
$ vi myshell.sh
#!/bin/bash

language="Korean"

echo "I can speak $language"
```

### <span style='background-color: #393E46; color: #F7F7F7'>&nbsp;&nbsp;&nbsp;📝 예제 1: 디렉터리 생성하기 &nbsp;&nbsp;&nbsp;</span>

쉘 스크립트에서 "Korean English Japan"이라는 문자열 변수를 정의한 뒤, 각 언어 이름으로 된 디렉터리를 생성하는 스크립트를 작성하라.

### <span style='background-color: #C2B2B2; color: #F7F7F7'>&nbsp;&nbsp;&nbsp;🔍 풀이&nbsp;&nbsp;&nbsp;</span>

```bash
vi make_dir.sh
#!/bin/bash

language="Korean English Japan"

mkdir $language
```

---

### 2.2.1 변수의 종류

변수는 크게 환경 변수와 사용자 변수로 나눌 수 있다.

- 환경 변수
  
  시스템에서 사용하는 변수

- 사용자 변수

  사용자가 임의로 정의하는 변수

또한 변수를 함수와 함께 사용하는 경우, 함수 내 지역 변수와 함수 밖 전역 변수 등으로도 나눌 수 있다.

- 함수는 `function` 키워드를 사용하여 정의한다.

```bash
$ vi myshell.sh
#!/bin/bash

function print() {
    echo $1
}

print "I can speak Korean"
```

위 쉘 스크립트를 실행 시, print 함수의 인자로 전달된 "I can speak Korean" 문자열이 출력된다.

```bash
$ sh myshell.sh    # "I can speak Korean" 문자열 출력됨
```

---

### 2.2.2 전역 변수

함수 선언 전 함수 밖에서 정의된 변수는 전역 변수로, 함수 안팎 어디서든 유효하다.

```bash
$ vi myshell.sh
#!/bin/bash

language="Korean"

function print() {
    echo "I can speak $language"
}

print
```

실행 시 "I can speak Korean" 문자열이 출력된다.

---

### 2.2.3 지역 변수

함수 안에서 정의된 지역 변수는 함수 밖에서 사용할 수 없다.

- `local` 키워드를 사용하여 지역 변수를 정의한다.

```bash
$ vi myshell.sh
#!/bin/bash

language="Korean"

function learn() {
    local learn_language="English"
    echo "I am learning $learn_language"
}

function print() {
    echo "I can speak $1"
}

learn
print $language
print $learn_language
```

실행 시 결과는 다음과 같다. print 함수에 지역 변수인 \$learn_language를 전달 시 해당 값을 출력하지 못하는 것을 알 수 있다.

```bash
$ sh myshell.sh
# I am learning English 문자열 출력
# I can speak Korean 문자열 출력
# I can speak 문자열 출력
```

---

### 2.2.4 예약변수 및 환경변수

시스템에서 미리 정의한 변수인 예약변수(환경변수) 목록을 살펴보자.

| 예약변수 | 설명 |
| --- | --- |
| HOME | 사용자의 홈 디렉터리 |
| PATH | 실행 파일을 찾을 디렉터리 경로 |
| FUNCNAME | 현재 함수의 이름 |
| LANG | 프로그램 사용 시 기본 지원 언어 |
| PWD | 현재 작업 중인 디렉터리 경로 |
| TERM | 터미널 종류 |
| SHELL | 현재 사용 중인 쉘 |
| USER | 사용자 이름 |
| USERNAME | 사용자 이름 |
| GROUP | 사용자 그룹(/etc/passwd 파일에 정의된 사용자 그룹) 출력 |
| DISPLAY | X 윈도 시스템에서 사용하는 디스플레이 이름 |
| COLUMNS | 현재 터미널의 컬럼 수 |
| LINES | 현재 터미널의 라인 수 |
| PS1 | 기본 프롬프트 변수 |
| PS2 | 보조 프롬프트 변수(기본값:>)<br/>명령을 "\\"를 사용하여 행을 연장 시 사용된다. |
| PS3 | select 명령어에서 사용되는 프롬프트 변수 |
| PS4 | 디버깅 모드에서 사용되는 프롬프트 변수(기본값:+) |
| BASH | 현재 Bash 실행 파일 경로 |
| BASH\_VERSION | 현재 Bash 버전 |
| BASH\_ENV | Bash 시작 시 실행되는 스크립트 파일 경로 |
| HISTFILE | 히스토리 파일 경로 |
| HISTFILESIZE | 히스토리 파일에 저장되는 히스토리 명령어 수 |
| HISTSIZE | 히스토리 명령어 수 |
| HOSTNAME | 호스트 이름 |
| HOSTTYPE | 시스템 하드웨어 종류 |
| MACHTYPE | 시스템 하드웨어 종류(HOSTTYPE보다 상세 정보) |
| MAIL | 메일 저장 경로 |
| LOGNAME | 로그인 이름 |
| TMOUT | 터미널 타임아웃 시간(초)(0일 경우 제한 없음) |
| SECONDS | Bash가 실행된 시간(초) |
| UID | 사용자 UID |
| OSTYPE | 운영체제 종류 |

이러한 예약변수는 `echo` 명령어를 사용해 터미널에서 바로 확인할 수 있다.

```bash
$ echo $HOME    # /home/username 출력됨
```

---

### 2.2.5 위치 매개변수

스크립트 수행 시 함께 전달하는 패러미터를 의미한다.

> 10개부터는 \${10}처럼 "{}"로 감싸야 한다.

| 위치 매개변수 | 설명 |
| --- | --- |
| \$0 | 실행한 스크립트 이름 |
| \$1 | 첫 번째 위치 매개변수 |
| \$\* | 전체 매개변수 값 |
| \$\@ | 전체 매개변수 값(쌍따옴표 ""에 따라 \$\*와 다른 결과) |
| \$# | 전체 매개변수 개수 |

> for문을 사용할 경우 `$@`는 매개변수를 별개로 취급하고, `$*`는 매개변수 전체를 하나의 문자열로 취급한다.

> 예를 들어 `$@`는 "Korean" / "English" / "Japanese Chinese"를 구분하지만, `$*`는 "Korean English Japanese Chinese"를 하나의 문자열로 인식한다.

```bash
$ vi mylanguage.sh
#!/bin/bash

echo "This shell script name is $0"
echo "I can speak $1 and $2"
echo "This shell script parameters are $*"
echo "This shell script parameters are $@"
echo "This shell script parameters count is $#"
```

위 쉘 스크립트를 실행 시, 다음과 같이 결과가 출력된다.

- 위치 매개변수를 전달할 때, 큰따옴표("")로 묶어서 전달할 경우 하나의 문자열로 인식한다.

```bash
$ sh mylanguage.sh Korean English "Japanese Chinese"
# This shell script name is mylanguage.sh
# I can speak Korean and English
# This shell script parameters are Korean English Japanese Chinese
# This shell script parameters are Korean English Japanese Chinese
# This shell script parameters count is 3
```

---

### 2.2.6 중괄호 사용

예를 들어 문자열과 문자열 사이에 외부에서 받은 패러미터를 삽입하고 싶을 수 있다. 하지만 \$만 사용하면 시스템은 어디서부터 어디까지가 변수명인지 알 수 없다.

- `${변수}`와 같이 중괄호를 사용하면 변수명을 명확하게 구분할 수 있다.

```bash
# 변수 AUTH_URL에 "www.example.com/"이 저장되어 있다고 하자.

# case 1
$ echo "http://$AUTH_URLlogin.html"    # $AUTH_URL을 인식하지 못한다.
                                       # http://www..html 출력됨
$ echo "http://${AUTH_URL}login.html"  # http://www.example.com/login.html 출력됨
```

---

### 2.2.7 확장 변경자

확장 변경자를 사용하면 예를 들어 변수가 선언되지 않았거나 NULL 값일 경우, 다른 문자열을 기본값으로 사용할 수 있게 작성할 수 있다.

- `${변수:-문자열}`: 변수가 설정되지 않았거나 NULL일 경우, 입력한 문자열을 사용한다. 

```bash
# 기본값 "redhat" 저장
$ OS_TYPE="redhat"
 
$ echo ${OS_TYPE:-ubuntu}    # 저장된 값이 있으므로 redhat 출력됨
```

반대로 기존에 변수가 값을 갖고 있지만, 다른 값으로 변수의 값을 설정하고 싶을 수 있다.

- `${변수:+문자열}`: 변수가 설정되어 있을 경우, 입력한 문자열로 치환한다.

```bash
$ OS_TYPE="ubuntu"

$ echo ${OS_TYPE:+redhat}  # 변수 OS_TYPE이 설정되어 있으므로 "redhat"으로 치환됨.
                           # redhat 출력됨 
```

(생략)

---