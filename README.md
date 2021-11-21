# Homework

## getopt, getopts 명령어

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






144124124






