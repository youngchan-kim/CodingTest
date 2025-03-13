문제

<https://school.programmers.co.kr/learn/courses/30/lessons/340199>

[프로그래머스

SW개발자를 위한 평가, 교육, 채용까지 Total Solution을 제공하는 개발자 성장을 위한 베이스캠프

programmers.co.kr](https://school.programmers.co.kr/learn/courses/30/lessons/340199)

문제 풀이 핵심

지갑과 지폐의 가로세로의 비교수식

반복문 비교수식은 가로<가로 OR 세로<세로 (입장)

반복문 내에서의 비교 가로 >=세로 세로 >=가로 (탈출)

내 코드

```
#include <string>
#include <vector>

using namespace std;

//비교해야하는 대상은 가로가로 가로세로 세로가로 세로세로 4가지
//어차피 지폐가 지갑보다 크다면 접어야함
//가장긴 방향끼리 정렬하여 비교
//비교의 대상은 가로가로 세로세로 2가지가 됨
//소수점은 버린다고 했기 때문에 float형에서 int형으로 강제형변환

int solution(vector<int> wallet, vector<int> bill) {
    int billsize = 2;
    
    int answer = 0;

    //계속 접어야하기 때문에 무한 반복 while
    while (wallet[0] < bill[0] || wallet[1] < bill[1])
    {
        //접기전 90도 돌렸을때 지갑에 들어가는지 확인할것
        if (wallet[0] >= bill[1] && wallet[1] >= bill[0])
            break;
        //지갑의 크기와 돈의 크기의 수는 2개 이기 때문에 반복
        for (int i = 0; i < billsize; i++)
        {
            //지폐의 길이가 긴쪽을 접어야함
            if (wallet[i] < bill[i])
            {
                //접기
                answer++;
                if(bill[0] > bill[1])
                    bill[0] *= 0.5;
                else
                    bill[1] *= 0.5;
                break;
            }     
        }
        
    }

    return answer;
}
```

무엇을 배웠나.

엣지케이스를 생각하지 않고 풀었는데 실패가 너무 많이 나왔음

엣지케이스를 메모하는 습관이 필요하다.

오히려 다른 사람의 코드에서 많이 배운 문제

다른 사람 코드

```
#include <string>
#include <vector>

using namespace std;

int solution(vector<int> wallet, vector<int> bill) {
    int answer = 0;
    while (true)
    {
        int& maxi = bill[0] > bill[1] ? bill[0] : bill[1];
        int& mini = bill[0] <= bill[1] ? bill[0] : bill[1];
        if (maxi <= wallet[0] && mini <= wallet[1] ||
            maxi <= wallet[1] && mini <= wallet[0])
            break;
        maxi /= 2;
        answer++;
    }
    return answer;
}
```

참조자 선언(int&)과 삼항연산자(?)를 사용

삼항연산자로 bill[0] > bill[1] 이 참이면 bill[0] 거짓이면 bill[1] 을 리턴한다.

보통 메모리 주소에 있는 값을 변수로 받아서 사용하거나  선언된 변수  bill[0] ,bill[1]을 사용한다. 직접 참조함

이사람은 리턴된 값은 bill[0]이나 bill[1]의 값이 아닌 메모리 주소를 참조자 선언(int&)로 받아서 주소 자체를 사용한다.

변수를 선언하는 경우 다시  bill[0] ,bill[1]에 입력해야하는 코드가 필요하지만

참조자 선언(int& maxi)의 경우 bill[0] ,bill[1]의 주소를 가지고 있기 때문에 간접 참조하게된다.

 maxi에 새로운 값이 리턴되면  maxi에 입력된 주소 bill[0] ,bill[1]의 값이 변하게 된다.

참조자는 할당된 하나의 메모리 공간에 다른 이름을 붙이는 것을 말한다.  

참조자를 사용하게 되면 메모리 공간의 낭비를 줄 일 수 있게 된다.

변수를 선언하게되면 그때마다 새로운 메모리주소를 할당받고 메모리 공간를 가지는데 

참조자는 메모리 주소를 할당은 받지만 메모리 공간은 피참조자의 메모리 공간을 사용하게된다.

공간이라고 표현했지만 메모리 공간보다는 값이라는 말이 합당하다.

참조자는 크기가 큰 구조체와 같은 데이터를 함수의 인수로 전달해야하는 경우에 사용할 수 있다.

주의사항으로는 참조자의 타입은 대상이 되는 변수의 타입과 일치해야한다. int형이면 int& float형이면 float&이다.

참조자는 선언과 동시에 초기화되어야한다. 선언과 동시에 대상의 메모리 주소가 필요하기 때문이다.

참조자는  한번 초기화되면, 참조하는 대상을 변경할 수 없다.