## 基础算法
目录

| 算法    | 代码            | 题目                                                                                                              |
|-------|---------------|:----------------------------------------------------------------------------------------------------------------|
| 快排    | [快排](#快排)     | [acwing785](https://www.acwing.com/problem/content/787/)                                                        |
| 归并排序  | [归并排序](#归并排序) | [acwing787](https://www.acwing.com/problem/content/description/789/)                                            | 
| 整数二分  | [二分](#二分)     | [acwing 791](https://www.acwing.com/problem/content/791/), [leetcode求平方根](https://leetcode.cn/problems/jJ0w9p/) |
| 浮点二分  | [浮点二分](#浮点二分) | [acwing 791](https://www.acwing.com/problem/content/791/)                                                       |
| 高精度加法 |[高精度加法](#高精度加法)| [leetcode 451](https://leetcode.cn/problems/add-strings/)                                                       |
| 高精度减法 |[高精度减法](#高精度减法)|[acwing 792](https://www.acwing.com/problem/content/description/794/)|
#### 快排
```c++
//  快排
void quick_sort(int q[], int l ,int r){
    if(l >= r ) return;
    // Q: 为什么是j = r+1,i = l-1
    // A： 因为 与下面的做法 do i++ 相匹配
    // Q： x的取值问题
    // A： x可以取，左边界，右边界，随机数，中间值
    int i = l-1, j = r+1, x = q[l + r>>1];
    while( i < j){
        do i++; while( x > q[i]);
        do j--; while( x < q[j]);
        if( i < j) swap(q[i], q[j]);
    }

    quick_sort(q,l,j);
    quick_sort(q,j+1,r);

}
```
#### 归并排序
```c++
void merge_sort(int q[], int l, int r){
    if( l >= r) return ;
    int mid = l+r>>1;
    merge_sort(q, l,mid);
    merge_sort(q, mid+1, r);
    int k = 0,i = l, j = mid+1;
    while( i <= mid && j <= r){
        if(q[i] < q[j] ) tmp[k++] = q[i++];
        else tmp[k++] = q[j++];
    }
    while(i <= mid) tmp[k++] = q[i++];
    while(j <= r) tmp[k++] = q[j++];
    k = 0,i = l;
    for(; i <= r ;i++, k++){
        q[i] = tmp[k];
    }
}
```

#### 二分
```c++
// 区间[l, r]被划分成[l, mid]和[mid + 1, r]时使用：
int bsearch_1(int l, int r)
{
    while (l < r)
    {
        int mid = l+(r-l)/2;
        if (check(mid)) r = mid;    // check()判断mid是否满足性质
        else l = mid + 1;
    }
    return l;
}
// 区间[l, r]被划分成[l, mid - 1]和[mid, r]时使用：
int bsearch_2(int l, int r)
{
while (l < r)
{
int mid = l + r + 1 >> 1;
if (check(mid)) l = mid;
else r = mid - 1;
}
return l;
}
```

#### 浮点二分
```c++
bool check(double x) {/* ... */} // 检查x是否满足某种性质

double bsearch_3(double l, double r)
{
    const double eps = 1e-6;   // eps 表示精度，取决于题目对精度的要求
    while (r - l > eps)
    {
        double mid = (l + r) / 2;
        if (check(mid)) r = mid;
        else l = mid;
    }
    return l;
}
```

#### 高精度加法
```c++
vector<int>add(vector<int>&a, vector<int>&b){
    if( a.size() < b.size() ) return add(b,a);
    int carry = 0;
    vector<int>ans;
    for(int i = 0; i < a.size(); i++){
        carry += a[i];
        if( i < b.size() ) carry += b[i];
        ans.push_back( carry % 10);
        carry /= 10;
    }
    if(carry)ans.push_back(1);
    return ans;
}
```
#### 高精度减法
```c++
// 判断是否有 A>=B
bool cmp(vector<int> &A, vector<int> &B)
{
    // 位数不同
    if (A.size() != B.size()) return A.size() > B.size();
    
    // 位数相同
    for (int i = A.size() - 1; i >= 0; --i)
        if (A[i] != B[i])
            return A[i] > B[i];
    
    return true;
}


vector<int> sub( vector<int>&a, vector<int>&b){
    int t = 0;
    vector<int>ans;
    for(int i = 0; i < a.size(); i++){
        t = a[i]-t;
        
        if( i < b.size() ) t -= b[i];
        
        ans.push_back( (t+10)%10 );
        if( t < 0) t = 1;
        else t = 0;
    }
    while(ans.size() > 1 && ans.back() == 0) ans.pop_back();
    return ans;
}
```