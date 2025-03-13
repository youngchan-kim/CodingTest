<https://school.programmers.co.kr/learn/courses/30/lessons/12937>

[프로그래머스

코드 중심의 개발자 채용. 스택 기반의 포지션 매칭. 프로그래머스의 개발자 맞춤형 프로필을 등록하고, 나와 기술 궁합이 잘 맞는 기업들을 매칭 받으세요.

programmers.co.kr](https://school.programmers.co.kr/learn/courses/30/lessons/12937)

나의 풀이

```
#include <string>
#include <vector>

using namespace std;
string result_string(int num)
{
    if(num%2 ==0)
      return "Even";
    else
        return "Odd";
}
string solution(int num) {
    string answer = "";
    answer = result_string(num);
    return answer;
}
```

다시 푸는 나의 풀이

```
/*
정수 num이 짝수일 경우 "Even"을 반환하고 
홀수인 경우 "Odd"를 반환하는 함수
*/
#include <string>
#include <vector>

using namespace std;

string solution(int num) {
    //이진 수 화한 num의 값이 1자리가 1에 and 연산된 값이 1이면 홀수 0이면 짝수가 된다.
    return num & 1 ? "Odd" : "Even";
}
```