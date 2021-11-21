# Homework

## 쉘  getopt, getopts 명령어

### getopt

getopt는 유닉스 시스템 연구소(USL)에 의해 구현된 프로그램 getopt 이다.

그러나 이 버전은 인용이 되지 않았기 때문에, **Metacharacter**를 다룰 수 없었고, **FreeBSD**로 상속되었다.

>**FreeBSD** : BSD계열의 오픈소스 운영체제 중 하나.
>>![111454](https://user-images.githubusercontent.com/94778099/142760468-c954f2e9-78a8-4faa-8626-1c5475e6ae07.PNG)

>**metacharacter** : 컴퓨터 프로그램에 특별한 의미를 갖는 문자
>>![11123](https://user-images.githubusercontent.com/94778099/142760484-46c77337-3efd-4e22-9ee6-f3851f19419b.png)


1986년 USL은 Metacharacter와 **whitespace**로 인해 안전하지 않은 것을 보고, 더 이상 허용하지 않기로 결정 하였다.

>**whitespace** : 공백문자

대신 유닉스 SVR3 본 쉘을 위한 내장 getopts 명령어를 만들었다.

출처 : [Wikipedia_getopt](https://en.wikipedia.org/wiki/Getopt#In_Shell)

### getopts

getopts라는 명령어는 인자를 받는 방법중 하나이다.

getopt명령어의 장점으로는

1. 다양한 입력 값이 존재할 경우 사용자와 개발자의 편의를 보장하기 위함 이고

2. 스크립트를 보다 체계적으로 관리할 수 있기 때문이다.

**예시**

```shell script

 #!/bin/bash

## 도움말 출력하는 함수
help() {
        echo "splt [OPTIONS] FILE"
                echo "    -h         도움말 출력."
                echo "    -a ARG     인자를 받는 opt."
                echo "    -b ARG     인자를 받는 opt2."
                exit 0
}
while getopts "a:b:h" opt
do
case $opt in
a) arg_a=$OPTARG
echo "Arg A: $arg_a"
;;
b) arg_b=$OPTARG
echo "Arg B: $arg_b"
echo "$arg_b"
;;
h) help ;;
?) help ;;
esac
done

```

getopts 부분을 살펴보자.

```

while getopts "a:b:h" opt

```

getopts 뒤에 "a: b: h"에서 a와 b는 인자를 받기 때문에 각각 뒤에 :가 붙었고, h는 필요하지 않기 때문에 :가 붙지 않았다.

```

a) arg_a=$OPTARG
echo "Arg A: $arg_a"
;;
b) arg_b=$OPTARG
echo "Arg B: $arg_b"
echo "$arg_b"
;;
h) help ;;
?) help ;;****

```

>$OPTARG는 사용자가 입력한 인자 값이다.

a와 b가 받은 인자는 "arg_a = $OPTARG", echo"Arg A: $arg_a " 를 통해서 사용자가 입력한 값 그대로인 Arg A : *사용자입력값* 또는 Arg B : *사용자입력값* 이 된다.

**스크립트 실행결과**

```

root@ubuntu:/shell# ./getopt.sh -a hello
Arg A: hello

root@ubuntu:/shell# ./getopt.sh -b hi
Arg B: hi
hi

root@ubuntu:/shell# ./getopt.sh -h
splt [OPTIONS] FILE
    -h         도움말 출력.
    -a ARG     인자를 받는 opt.
    -b ARG     인자를 받는 opt2. 

```

a에 인자 hello를 넣으니, Arg A: hello가 튀어나왔다.

마찬가지로 b에 인자 hi를 넣으니, Arg B: hi가 튀어나왔다.

-h를 하니, 도움말을 출력하는 함수인 help()가 튀어나왔다.


출처: https://systemdesigner.tistory.com/17 [System Designer]

-----
## 리눅스 sed, awk 명령어

### sed 명령어

**sed** 명령어에서 sed 는 streamlined editor를 의미한다. **능률적인 에디터** 란 뜻이다.

그렇다면, sed는 무슨 명령어인가?

1. sed 명령어는 편집에 특화된 명령어이다. 수정 치환 삭제 글추가 등 편집기 기능은 왠만하게 다 가능하다.
2. sed 명령어는 명령행에서 파일을 인자로 받아, 명령어를 통해 작업한 후 결과를 화면으로 확인하는 방식이다.
3. sed 명령어는 sed명령어를 이용하면 파일을 변경했을 경우, 원본을 손상시키지 않는다.(편집 결과를 저장하기 전 까지는) 출력한 결과가 원본과 달라도, 손상시키지 않는다.

### sed 명령어 종류

예시로 쓸 파일.
![123333](https://user-images.githubusercontent.com/94778099/142762406-af753730-d491-4f03-ae03-8524db3aec35.PNG)

>왼쪽 노란색 숫자는 행을 뜻함.

1.특정 범위만큼 파일내용 출력하기

```

a. sed -n '1p' **employees:**
b. sed -n '1,3p' employees:
c. sed -n '8,$p' employees:

```
>>employees는 파일 이름 임.
>>여기서 -n 는 작업한 부분만 억제해서 출력시키는 옵션이다. p와 같이 써줘야 한다.

![12412444](https://user-images.githubusercontent.com/94778099/142762516-b3d2b1d9-ce5b-4067-833c-66abcd3f4abc.PNG)
>>이것은 출력 결과 입니다.


> a는 첫번째 라인만 출력 합니다.(빨간색)
> b는 1번행부터 3번행까지 출력 합니다.(노란색)
> c는 8번행부터 끝행까지 출력 합니다.(파란색)

2.특정 단어로 시작하는 행들만 추출하기

```

a. sed -n '/^107/p' employees
b. sed -n '/103/p' employees

```

> a는 107로 시작하는 행만 출력해서 보여줍니다.(^로인해 107로 시작하는 행만 출력하게 합니다.)
>> ![캡처5666](https://user-images.githubusercontent.com/94778099/142762910-0cace197-ba43-4842-98d6-fe67d5063312.PNG)

> b는 103이 있는 행을 출력해서 보여줍니다.
>>![캡처441221](https://user-images.githubusercontent.com/94778099/142762914-3bcf9d61-c8f1-494a-8030-2a68affc554b.PNG)

3. 파일에서 공백으로 이루어지거나 빈줄 제거하기

```

a. sed '/^$/d' employees
b. sed '/^$/d' employees > new_employees
c. sed '/^*$/d' employees > new_employees

```

> a는 employees 파일에서 빈 라인들을 지운 후 내용을 출력해줍니다.
>>![3-a](https://user-images.githubusercontent.com/94778099/142763210-565e1ce4-bcac-4901-8832-d0f9d0e12923.PNG)

> b는 employees 파일에서 빈 라인을 삭제한 후, 결과를 new_employees라는 파일명으로 저장합니다.
>>![3-1b](https://user-images.githubusercontent.com/94778099/142763214-47044e1e-b107-4b19-aa01-f606b9622110.PNG)

> c는 employees파일에서 빈 라인들이나 공백으로 채워진 행들을 삭제한 후 new_employees라는 파일명으로 저장합니다.
>>![3-2c](https://user-images.githubusercontent.com/94778099/142763217-121ba20e-eeac-4a8a-bcd3-e0aa77bc9f2e.PNG)

4.단어 치환

```

a. sed 's/IT_PROG/DEVELOPER/g' employees
b. sed 's/IT_PROG/DEVELOPER/g' employees > new_employees
c. sed 's/it_prog/DEVELOPER/gi' employees > new_employees

```
>여기서 s가 치환의 의미를 가집니다.
> a는 파일 전체에서(/g가 그 역할을 합니다) IT_PROG 를 DEVELOPER로 바꿉니다.
> b는 파일 전체에서 IT_PROG를 DEVELOPER로 바꾸는데, 그걸 new_employees라는 파일에 저장합니다.
> c는 파일 전체에서 it_prog를 DEVELOPER로 바꾸는데, 단 변경단어를 찾을 때 대소문자를 무시합니다.(/gi 중 i가 그 역할을 합니다.)
>>![4](https://user-images.githubusercontent.com/94778099/142763377-8717ef1b-d7f0-44ce-9f81-303e924eb713.PNG)

etc. SED subcommand 명령어 종류와 의미

| subcommand | 의미 |
|:----:|:---------|
| a\ | 전체 행에 하나 이상의 새로운 행을 추가한다. |
| c\ | 현재 행의 내용을 새로운 내용으로 교체한다. |
| d | 행을 삭제한다. |
| i\ | 현재 행의 위에 텍스트를 삽입한다. |
| h | 패턴 스페이스의 내용을 홀드 스페이스에 복사한다. |
| H | 패턴 스페이스의 내용을 홀드 스페이스에 추가한다. |
| g | 홀드 스페이스의 내용을 패턴 스페이스에 복사한다. (패턴 스페이스가 비어있지 않은 경우에는 덮어쓰기) |
| G | 홀드 스페이스의 내용을 패턴 스페이스에 복사한다. (패턴 스페이스가 비어있지 않은 경우에는 그 뒤에 추가) |
| l | 출력되지 않는 특수문자를 명확하게 출력한다. |
| p | 행을 출력한다. |
| n | 다음 입력 행을 첫 번째 명령어가 아닌 다음 명령어에서 처리하게 한다. |
| q | sed를 종료한다. |
| r | 파일로부터 행을 읽어온다. |
| ! | 선택된 행을 제외한 나머지 전체 행에 명령어를 적용한다. |
| s | 문자열을 치환한다. |

| s와 쓰이는 플래그 | 의미 |
|:----:|:---------|
| g | 치환이 행 전체에 대해 이뤄진다. |
| p | 행을 출력한다. |
| w | 파일에 쓴다. |
| x | 홀드 버퍼와 패턴 스페이스의 내용을 서로 맞바꾼다. |
| y | 한 문자를 다른 문자로 변환한다. (y에 정규표현식 메타문자를 사용할 수 없다.)|

출처 및 참고자료 : https://jhnyang.tistory.com/287#google_vignette

### awk 명령어

awk 명령어는 하나 이상의 공백으로 구분 된 여러 열의 텍스트 데이터를 처리 할 때 유용한 도구이다.

**테스트 파일 입니다.**
>![test1](https://user-images.githubusercontent.com/94778099/142764053-a42727ed-2a5f-4b45-85c5-cb1c3b46968f.PNG)

1.열만 출력하기

```
awk '{ print $1 }' ./b.txt

```
>여기서 b.txt는 파일이름 입니다.
>**결과**
>>![test2](https://user-images.githubusercontent.com/94778099/142764093-93b042bf-c4cf-4110-b1ae-57e975c3fa00.PNG)

```

awk '{ print $1,$2 }' ./b.txt

```
>이렇게 하면 여러개의 열을 출력 할 수 있습니다.
>**결과**
>>![test3](https://user-images.githubusercontent.com/94778099/142764117-864156ca-ea55-40fe-ae77-1a49b5a0b347.PNG)

2.특정 패턴이 있는 레코드를 출력

```

 awk '/rea/' ./b.txt

```
>rea라는 문자열이 포함된 레코드를 출력합니다.
>>**결과**
>>![test4](https://user-images.githubusercontent.com/94778099/142764184-014884de-ec5e-4934-ba3c-a17f91cf88e4.PNG)

3.출력의 내용 첨가

```

awk '{ print ("name : " $1, ", "  "phone : " $2) }' ./b.txt

```

>앞에 name : reakwon, phone : 010-1234-5678 식으로 name과 phone를 추가합니다.
>>**결과**
>>![test5](https://user-images.githubusercontent.com/94778099/142764251-41eadec2-c949-43ca-92b9-6b02e9c1fcc9.PNG)

4.if문 사용

```

awk '{ if ( $5 >= 80 ) print ($0) }' ./b.txt

```

>5번째 열인 점수가 80점 이상인 사람을 출력하는 조건입니다.
>>**결과**
>>![test6](https://user-images.githubusercontent.com/94778099/142764299-d33550bc-0eb4-424b-ab2b-51e31616b185.PNG)

and인 &&와, or인 ||를 이용해서 다중조건을 쓸 수 있습니다.

>예시

```

awk '{ if ( $4 == "M" && $5 >= 80) print ($0) }' ./b.txt

```
>>**결과**
>>![test7](https://user-images.githubusercontent.com/94778099/142764343-4f6c4427-df8a-4369-850e-a70d3f05960a.PNG)

5.내장함수 이용

```

 awk '{ print ("name leng : " length($1), "substr(0,3) : " substr($1,0,3)) }' ./b.txt

```

>이름 길이를 알아낸 후, 앞에서 3글자만 출력하는 명령 입니다.
>>**결과**
>>![test8](https://user-images.githubusercontent.com/94778099/142764405-b12dcf05-f67e-4eef-ad4b-ebb261393d98.PNG)

6.반복문

```

awk '{
for(i=0;i<2;i++)
 print( "for loop :" i "\t" $1, $2, $3)
}' ./b.txt

```

>for문을 이용하여 출력을 2번 진행한 것 입니다.
>>**결과**
>>![test9](https://user-images.githubusercontent.com/94778099/142764447-b49c4638-a715-4779-aded-abf06a143c04.PNG)

7. begin, end pattern과 변수사용

```

awk '
> BEGIN {
>  sum = 0
>  cnt = -1
> }
>
> {
>  sum += $5
>  cnt++
> }
>
> END {
>  avg = sum/cnt
>  print ("sum :" sum ", average :" avg)
> }' ./b.txt

```

>begin에서 초기화가 이루어 지고, end에서 결과를 출력합니다.

출처 및 참고 : https://reakwon.tistory.com/163
