더보기

조건은 두가지 이다.

인덱스가 가장 커야하고

인용갯수가 인덱스 보다 크거나 같아야 한다.

일단 내림차순 정렬을 하면 인덱스를 비교하기 쉽다.

※ 인덱스의 수보다 크거나 같은 인용 갯수만 찾고 그중 가장 큰 것만 뽑으면 된다.

{ 25, 8, 5, 3, 3 }인경우

실재로 데이터는 [0]번째는 25

[1] 8

[2] 5

[3] 3

[4] 3

이렇게 들어가지만

i인덱스를 리턴할때는 0번째가아닌 1번째 부터 논문이 있기 때문에

i+1을 해준다.

 풀이를 위해 인덱스를 바꾸었다.

[1]번째는 25

[2] 8

[3] 5

[4] 3

[5] 3

인용된 수보다 인덱스가 작으면 탈락이다.

if(i+1 <= citations[i])를 하면

[4] [5]인덱스는 사용할 수 없다.

점점 인덱스는 커지는데 인용된 수는 작아지기 때문이다.

if(i+1 <= citations[i])

{

answer= i+1;

}

을 하면 가장큰 인덱스인 [3]을 대입할 수 있게 된다.

```
#include <string>
#include <vector>
#include <algorithm>
using namespace std;

int solution(vector<int> citations) {
    int answer = 0;
    sort(citations.begin(), citations.end(),greater<>());
    for (int i = 0; i < citations.size(); i++)
    {
        if (citations[0] == 0)
            return 0;
        if (i < citations[i])
        {
            //i는 인덱스이기 때문에 +1을 해줘야 인용된 책이 몇번째인지 알수 있다.
            answer = i + 1;
        }
    }
    return answer;
}
```