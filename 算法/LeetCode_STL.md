---
categories:
  - 算法
title: LeetCode_STL
abbrlink: b1cc0d0c
---
## 数组

```c++
int[] bucket = new int[14];
sort(arr.begin(), arr.end());
```



## String

```C++
s = to_string(int val);
num = stoi(s);
len = s.length();
for (size_t i = 0; i < s.size(); ++i)
s.push_back(c);
for(auto c:string){}
reverse(s.begin(),s.end());
char c = s.charAt(int i);
```

## 队列

```C++
queue<int> q;
q.push(value);
q.empty();
top = q.front();
q.pop();
q.emplace(root);
q.emplace_back(root);
```



## 栈

```c++
stack<int> stack1;
vector<vector <int> > ivec(m ,vector<int>(n,0)); //m*n的二维vector，所有元素初始化为0
void assign(const_iterator first, const_iterator last)
stack.empty();
stack.pop();
stack.push(value);
value = stack.top();
```

## set

```c++
set<int> s;

```



## 向量

```c++ 
vector<int>res;
vector.size();
vector.push_back();
vector.pop_back(); //最后一个元素
for(it=tmp.begin();it!=tmp.end();it++){}
```



## 链表

```c++
struct ListNode {
      int val;
      ListNode *next;
      ListNode(int x) : val(x), next(NULL) {}
 };

head = head->next;
value = head -> val;
ListNode* tail = nullptr;
```

## 哈希表

```c++
unordered_map<Node*, Node*> map;
map[key]=a;
!map.find(c) == map.end() #是否在表中
```

## 哈希集

```
unordered_set<ListNode*>visited;
visited.insert(head);
if(visited.count(head));
```

