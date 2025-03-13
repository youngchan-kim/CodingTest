내 코드

```
#include <string>
#include <vector>
#include <algorithm>
using namespace std;

int GCD(int a, int b)
{
    int mod = a % b;
    while (mod >0)
    {
        a = b;
        b = mod;
        mod = a % b;
    }
    return b;
}

int solution(vector<int> arr) {
    int answer = 0;
    int lcm = arr[0];
    for (int i = 1; i < arr.size(); i++)
    {
        answer = GCD(lcm, arr[i]);
        lcm = lcm * arr[i] / answer;
    }

    return lcm;
}
```

최대공약수를 구하는 방법을 아는 게 중요한 문제였다.

여러가지의 최대공약수를 구하는 방법이 있었지만

그중 유클리드 호제법을 사용하였다.

**유클리드 호제법**

f(a, b) = gcd(a, b)라 하자.

[gcd(a, b)는 최대공약수를 구해주는 함수이다.]

[a % b는 b로 a를 나누었을 때 나오는 나머지 값이다.]

가정 1 ) a % b == 0 이라면, f(a, b) = b가 성립한다.  
가정 2 ) a % b != 0 인 경우, f(a, b) = f(b, a % b)가 된다.

f(a, b) = f(b, a % b)식이 반복되면

a%b가 0이되고  가정 1이 성립된다.

가정 1이 성립될때 b는 최대 공약수가 된다.

최소 공배수를 구하는 방법은

해당 식을 a\* b = gcd(a, b) \* lcm(a, b)은 참이다. 라고 증명되었다.

[lcm(a, b)는 최소공배수를 구해주는 함수이다.]

우리가 알고 있는 값은 a, b, gcd(a, b)의 값을 알고 있다.

즉 식을 바꾸면

lcm(a,b) = a\*b / gcd(a, b)가 된다.

여러 방법으로 풀 수 있는데 재귀함수로 풀이한 다른사람 코드입니다.

더보기

```
#include <string>
#include <vector>
#include <algorithm>

using namespace std;

int gcd(int x, int y) { return x % y == 0 ? y : gcd(y, x % y); }
int lcm(int x, int y) { return x * y / gcd(x, y); }
int solution(vector<int> arr) {
    int answer = arr[0];
    for (int i = 1; i < arr.size(); i++)
        answer = lcm(answer, arr[i]);
    return answer;
}
```

gcd를 재귀함수로 만들어 풀이한 코드

이론은 끄적여 놓은것과 같아요