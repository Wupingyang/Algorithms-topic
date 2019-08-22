# 完全平方数

给定正整数$n$，找到若干个完全平方数（比如 1, 4, 9, 16, ...）使得它们的和等于$n$。你需要让组成和的完全平方数的个数最少。

示例 1:

输入: n = 12
输出: 3 
解释: 12 = 4 + 4 + 4.

**基本思想**

* 特殊情况：$n$是完全平方数，直接返回1即可
* 辅助函数：
  * 判断完全平方数：根据公式$1+3+5+\cdots+(2*n-1)=n^2$
  * 小于$n$的最大平方数
* 采用BFS思想：
  * 每次找到小于$n$的完全平方数，例如n=12，则找到9, 4, 1
  * 将n减去完全平方数的剩余值入队列
  * 如果剩余值为完全平方数，则全部结束
  
**参考代码**

```c++
class Solution {
public:
  bool isSqrt(int n){
    for(int i=1;n>0;i+=2) n-=i;
      return n == 0;
    }
  int maxSqrt(int n){
    if(n<=1) return 1;
    if(isSqrt(n)){
      return pow(int(sqrt(n-1)),2);
    }else{
      return pow(int(sqrt(n)),2);
    }
}
  int numSquares(int n) {
    if(n<4) return n;
    if(isSqrt(n)) return 1; // 特殊情况	
    queue<int> q_rem;
    q_rem.push(n);
    int num = 0;
    while(!q_rem.empty()){
      int flag_while = 0;
      int size = q_rem.size();
      ++num;
      for(int i=0;i<size;++i){
      int cur_rem = q_rem.front();
      q_rem.pop();
      int tmp_cur_rem = cur_rem;
      int flag_for = 0;
      while(1){
        int tmp = maxSqrt(tmp_cur_rem);
        q_rem.push(cur_rem-tmp);
        if(isSqrt(cur_rem-tmp)){
          flag_for = 1;
          break;
        }
        if(tmp == 1) break;
        tmp_cur_rem = tmp;
      }
      if(flag_for){
        flag_while = 1;
        break;
      }
    }
    if(flag_while){
      break;
    }
  }
  return num+1;
  }
};
```
