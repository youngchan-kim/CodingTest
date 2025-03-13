문제 풀이

```
#include <string>
#include <vector>
#include <algorithm>
using namespace std;

vector<int> solution(vector<int> array, vector<vector<int>> commands) {
    vector<int> answer;
    vector<int> save;
    for (int i = 0; i < commands.size(); i++)
    {
        int start=0;
        int end=0;
        int pick=0;
        save.clear();
        for (int j = 0; j < commands[i].size(); j++)
        {
            if(j == 0)
                start=commands[i][j] - 1;
            else if(j == 1)
                end=commands[i][j] - 1;
            else
                pick=commands[i][j] - 1;
        }

        for (int j = 0; j < array.size(); j++)
        {
            if (start <= j)
            {
                save.push_back(array[j]);
                if (end == j)
                    break;
            }
            
        }

        sort(save.begin(), save.end());
        for (int j = 0; j < save.size(); j++)
        {
            if (pick == j)
            {
                answer.push_back(save[j]);
            }
        }

    }


    return answer;
}
```

다시 푸는 코드

```
#include <string>
#include <vector>
#include <algorithm>
using namespace std;

vector<int> solution(vector<int> array, vector<vector<int>> commands) {
    vector<int> answer;
    vector<int> array_ans;
    int repeat = commands.size();
    for (int i = 0; i < repeat; i++)
    {
        int resized = commands[i][1] - commands[i][0] + 1;
        array_ans.clear();
        array_ans.assign(array.begin() + commands[i][0]-1, array.begin() + commands[i][1]);
        sort(array_ans.begin(), array_ans.end());
        answer.push_back(array_ans[commands[i][2] - 1]);
    }
    return answer; 
}
```

다른 사람 코드

더보기

vector는 깊은 복사라 가능

```
#include <string>
#include <vector>
#include <algorithm>

using namespace std;

vector<int> solution(vector<int> array, vector<vector<int>> commands) {
    vector<int> answer;
    vector<int> temp;

    for(int i = 0; i < commands.size(); i++) {
        temp = array;
        sort(temp.begin() + commands[i][0] - 1, temp.begin() + commands[i][1]);
        answer.push_back(temp[commands[i][0] + commands[i][2]-2]);
    }

    return answer;
}
```

다른 사람 풀이

더보기

어떤 식으로 돌아가는지 파악해 볼 것

```
#include <functional>
#include <string>
#include <vector>
#include <algorithm>
using namespace std;

typedef struct segment_node {
    int begin, end;
    segment_node* left;
    segment_node* right;
    vector<int> segmentation_array;
};

function<void(vector<int> &, vector<vector<int>> &, vector<int> &)> segment_tree = [](vector<int> &array, vector<vector<int>> &command, vector<int> &answer)
{
    function<bool(const int &, const int &)> cmp = [&](const int &a, const int &b) {
        return array[a] <= array[b];
    };
    vector<int> numbering(array.size(), 0);
    for (int i = 0; i < array.size(); i++) numbering[i] = i;

    int range_begin, range_end, obt, cnt, temp;
    function<void(segment_node*)> segment_search = [&](segment_node* node)
    {
        temp = (node->end - node->begin + 1) / 2;

        if (node->begin == node->end) {
            answer.push_back(array[node->segmentation_array[0]]);
            return;
        }
        else if (node->begin <= range_begin && range_end <= node->begin + temp - 1) {
            if (node->left == NULL) {
                segment_node *left_node;

                left_node = new segment_node;
                left_node->begin = node->begin;
                left_node->end = node->begin + temp - 1;
                left_node->left = NULL;
                left_node->right = NULL;
                left_node->segmentation_array.assign(numbering.begin() + left_node->begin, numbering.begin() + left_node->end + 1);
                sort(left_node->segmentation_array.begin(), left_node->segmentation_array.end(), cmp);
                node->left = left_node;
            }
            segment_search(node->left);
        }
        else if (node->begin + temp <= range_begin && range_end <= node->end) {
            if (node->right == NULL) {
                segment_node *right_node;

                right_node = new segment_node;
                right_node->begin = node->begin + temp;
                right_node->end = node->end;
                right_node->left = NULL;
                right_node->right = NULL;
                right_node->segmentation_array.assign(numbering.begin() + right_node->begin, numbering.begin() + right_node->end + 1);
                sort(right_node->segmentation_array.begin(), right_node->segmentation_array.end(), cmp);
                node->right = right_node;
            }

            segment_search(node->right);
        }
        else
        {
            cnt = 0;
            for (auto &x : node->segmentation_array) {
                if (range_begin <= x && x <= range_end) {
                    cnt++;

                    if (cnt == obt) {
                        answer.push_back(array[x]);
                        return;
                    }
                }
            }
        }
    };

    segment_node* root;
    root = new segment_node;
    root->begin = 0;
    root->end = numbering.size() - 1;
    root->left = NULL;
    root->right = NULL;
    root->segmentation_array = numbering;
    sort(root->segmentation_array.begin(), root->segmentation_array.end(), cmp);

    for (auto &x : command) {
        range_begin = x[0] - 1;
        range_end = x[1] - 1;
        obt = x[2];
        segment_search(root);
    }
};

vector<int> solution(vector<int> array, vector<vector<int>> commands) 
{
    vector<int> answer;//(commands.size(), 0);
    vector<pair<int,int>> arr(array.size(), pair<int,int>());

    for(int i = 0 ; i < array.size() ; i++){
        arr[i].first = array[i];
        arr[i].second = i + 1;
    }

    segment_tree(array, commands, answer);

    return answer;
}
```