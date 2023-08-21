# 2 쉘 스크립트 기초 문법

## 2.5 연산자

(생략)

---

## 2.6 정규 표현식

정규 표현식(regular expression)은 특정한 규칙을 가진 문자열의 집합을 표현하는 데 사용하는 형식 언어다.

---

### 2.6.1 POSIX 기본 및 확장 문법

| 표현식 | 설명 |
| --- | --- |
| \^x | x문자로 시작하는 문자열 |
| x\$ | x문자로 끝나는 문자열 |
| .x | x문자로 끝나는 한 자리 수 문자열 |
| x+ | x문자가 반드시 한 번 이상 반복되는 문자열 |
| x? | x문자가 존재할 수도, 존재하지 않을 수도 |
| x\* | x문자가 0번 이상 반복되는 문자열 |
| x|y | or. x 혹은 y 문자가 존재 | 
| (x) | 그룹 |
| (x)(y) | 앞에서부터 그룹 인덱스를 부여 |
| (x)(?:y) | 예외 그룹. 그룹 인덱스를 부여하지 않음 |
| x{n} | 반복을 표현. x문자가 n번 반복 |
| x{n,} | 반복을 표현. x문자가 n번 이상 반복 |
| x{n,m} | 반복을 표현. x문자가 n번 이상, m번 이하 반복 |
| - | A-Z처럼 알파벳이나 숫자 범위를 나타낼 때 사용 |
| [...] | 문자들을 집합화 |
| \\ | 이스케이프. 원래 문자 의미대로 해석 |
| \b | 단어 끝 공백 문자열 |
| \B | 라인 끝 공백 문자열 |
| \< | 단어 시작에서 공백 문자열 |
| \> | 단어 끝에서 공백 문자열 |

---

## 2.6.2 POSIX 문자 클래스

**POSIX**(Portable Operating System Interface) 문자클래스 역시 정규 표현식에서 많이 사용된다.

| 표현식 | 설명 |
| --- | --- |
| [:alnum] | 알파벳이나 숫자로 이루어진 문자열 |
| [:alpla:] | 알파벳 문자를 의미. [A-Za-z]와 동일 |
| [:blank:] | 스페이스나 탭을 의미 |
| [:cntrl:] | 제어 문자를 의미 |
| [:digit:] | 숫자를 의미. [0-9]와 동일 |
| [:graph:] | 출력 가능한 문자를 의미 |
| [:lower:] | 소문자를 의미. [a-z]와 동일 |
| [:print:] | 출력 가능한 문자를 의미. ASCII 32~126 사이 문자들과 일치<br/>[:graph:]와 유사하지만 스페이스를 포함 |
| [:punct:] | 구두점 문자를 의미 |
| [:space:] | 공백 문자를 의미 |
| [:upper:] | 대문자를 의미. [A-Z]와 동일 |
| [:xdigit:] | 16진수 숫자를 의미 |

---

### 2.6.3 다양한 정규 표현식 예제

> [정규 표현식 예제](https://github.com/naleeJang/Easy-Shell-Script/tree/master/03.RegularExpression): 03.RegularExpression/expression.txt 예제 이용

- 메타 문자 `.` 이용

  뉴라인을 제외한 한 개의 문자를 찾는 `.`를 이용해서, (1) C로 시작하여 U로 끝나는 세 글자 문자, (2) C로 시작해 e로 끝나는 네 글자 문자를 탐색한다.

```bash
grep 'C.U' expression.txt

# 출력: CPU model is Intel(R) Core(TM) i7-8665U CPU @ 1.90GHz 
```

```bash
grep 'C..e' expression.txt

# 출력: CPU model is Intel(R) Core(TM) i7-8665U CPU @ 1.90GHz
```

- 메타 문자 `*`, `\`, `?`와 문자클래스 `[:lower:]`를 사용

  q로 시작하며 ?로 끝나는 단어를 찾는다. q와 ? 사이는 영문 소문자로 이루어져야 한다.

  > `[:lower:]`를 `grep`과 함께 사용할 경우 중첩 대괄호(`[[]]`)를 사용해야 한다.

```bash
grep -E 'q[[:lower:]]*\?' expression.txt 
# 출력: Do you have any questions? or Do you need any help?
```

- 메타 문자 `+`와 `^`를 이용할 경우

  (1) -2로 시작해 -로 끝나며 2가 계속 반복되는 단어, (2) 라인 문자가 \#로 시작하는 라인

```bash
grep -E '\-2+\-' expression.txt

# 출력: phone: 010-2222-5668
```

```bash
grep '^#' expression.txt

# 출력:
##===========================================#
## Date: 2020-05-05
## Author: NaleeJang
## Description: regular expression test file
##===========================================#
## System Information
## Help
## Contacts
```

- 메타 문자 `^`, `{N}`, `{N,}`, `[:alpha:]`를 이용할 경우

   알파벳 5글자로 시작하며, 알파벡 뒤에 :로 끝나는 단어가 있는 라인

```bash
grep -E '^[[:alpha:]]{5}:' expression.txt 

# 출력: phone: 010-2222-5668
```

- 메타 문자 `{N,M}`, `$`, `[:alpha:]`, `[:digit:]`을 이용할 경우

   라인 종료 시 알파벳 4~6글자를 갖는 단어로 끝나는 라인

```bash
grep -E '[[:alpha]]{4,6}$' expression.txt

# 출력:
# Regular Expression
## Author: NaleeJang
## Description: regular expression test file
## System Information
## Help
## Contacts
```

- 메타문자 `^`, `^$`를 이용할 경우

   라인 시작 시 \#로 시작하고, 공백인 라인 제거

   > 라인 시작을 알리는 `^`와 종료를 알리는 `$`가 합쳐지면 공백 라인을 의미하게 된다.(`^$`)

   - `-v` 옵션: 패턴과 일치하지 않는 모든 라인 출력

```bash
cat expression.txt | grep -v '^#' | grep -v '^$'

# 출력:
# ====================
#  Regular Expression
# ====================
  
# Today is 05-May-2020.
# Current time is 6:04PM.
# This is an example file for testing regular expressions.        This example file includes control characters.
# CPU model is Intel(R) Core(TM) i7-8665U CPU @ 1.90GHz
# Memory size is 32GiB
# Disk is 512 GB
# IP Address is 192.168.35.7
# Do you have any questions? or Do you need any help?
# If you have any questions, Please send a mail to the email below.
# e-mail: nalee999@gmail.com
# phone: 010-2222-5668
```

- 메타 문자 `\<`, `\>`를 이용할 경우

  (1) C로 시작하는 단어가 있는 라인, (2) n으로 끝나는 단어가 있는 라인

```bash
grep '\<C' expression.txt

# Current time is 6:04PM.
# CPU model is Intel(R) Core(TM) i7-8665U CPU @ 1.90GHz
# # Contacts
```

```bash
grep 'n\>' expression.txt

# 출력:
#  Regular Expression
# # Description: regular expression test file
# This is an example file for testing regular expressions.        This example file includes control characters.
# # System Information
```

- 메타 문자 `{N,}`과 문자클래스 `[:alpha:]`, `[:punct:]`를 이용할 경우

   알파벳 6글자 이상이며, 문장 부호로 끝나는 단어가 있는 라인

```bash
grep -E '[[:alpha:]]{6,}[[:punct:]]' expression.txt

# 출력:
# # Author: NaleeJang
# # Description: regular expression test file
# This is an example file for testing regular expressions.        This example file includes control characters.
# Do you have any questions? or Do you need any help?
# If you have any questions, Please send a mail to the email below.
```

---