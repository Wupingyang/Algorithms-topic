# BFS-岛屿数量

给定一个由 '1'（陆地）和 '0'（水）组成的的二维网格，计算岛屿的数量。

一个岛被水包围，并且它是通过水平方向或垂直方向上相邻的陆地连接而成的。

你可以假设网格的四个边均被水包围。

示例：

输入：

11000

11000

00100

00011

输出：

3

**题目理解**

岛屿数量就是网格中'1'组成的数字块的数量

**基本思想**

* 将左上角第一个'1'的位置入栈，其中位置使用pair表示（pair<int, int> pos = {row, col}格式）
* 入栈以后，将该位置的值改为'0'（通过后面对'1'的判断，解决了重复使用问题）
* 以该点为中心，将周围值为'1'的坐标都入栈（位置向量{1, -1}，横坐标和纵坐标分别+1或者-1, 产生四个周围点的坐标）
* 将该点出栈，继续以上步骤
* 当栈为空的时候，说明该数字块结束，开始寻找下一个数字块。

**参考代码**

1.代码模板
```c++
/**
 * Return the length of the shortest path between root and target node.
 */
int BFS(Node root, Node target) {
    Queue<Node> queue;  // store all nodes which are waiting to be processed
    Set<Node> used;     // store all the used nodes
    int step = 0;       // number of steps neeeded from root to current node
    // initialize
    add root to queue;
    add root to used;
    // BFS
    while (queue is not empty) {
        step = step + 1;
        // iterate the nodes which are already in the queue
        int size = queue.size();
        for (int i = 0; i < size; ++i) {
            Node cur = the first node in queue;
            return step if cur is target;
            for (Node next : the neighbors of cur) {
                if (next is not in used) {
                    add next to queue;
                    add next to used;
                }
            }
            remove the first node from queue;
        }
    }
    return -1; // there is no path from root to target
}
```

2.测试代码
```c++
class Solution {
public:
    int numIslands(vector<vector<char>>& grid) {
      if(grid.size()<1) return 0;
      int row = grid.size();
      int col = grid[0].size();
	
      int num = 0;
      vector<int> pos = {1, -1};
	
      for(int r=0; r<row; ++r){
        for(int c=0; c<col; ++c){
	  if(grid[r][c] == '1'){
	    ++num;
	    grid[r][c] = '0';
	    queue<pair<int,int>> q;
	    q.push({r, c});
	    while(!q.empty()){
	      for(int i=0;i<q.size();++i){
	        auto cur = q.front();
	        q.pop();
		int x = cur.first;
		int y = cur.second;
		for(int m=0; m<2; ++m){
		  for(int n=0; n<2; ++n){
		    int tmpX = x, tmpY = y;
		      if(m == 0){
		        tmpX += pos[n];
		      }
		      if(m == 1){
		        tmpY += pos[n];
		      }
		      if(tmpX>=0 && tmpY>=0 && tmpX<row && tmpY<col && grid[tmpX][tmpY]=='1'){
		        q.push({tmpX, tmpY});
			grid[tmpX][tmpY] = '0';
	              }
		  }
                }
	      }
            }
          }
        }
      }
      return num;
    }
};
```
