#  回溯算法的框架

```java
result = []
void backtrack(路径, 选择列表):
    if 满足结束条件:
        result.add(路径)
        return

    for 选择 in 选择列表:
    	# 做选择
    	将该选择从选择列表移除
    	路径.add(选择)
    	backtrack(路径, 选择列表)
    	# 撤销选择
    	路径.remove(选择)
    	将该选择再加入选择列表
```
* 其核心就是 for 循环里面的递归，在递归调用之前「做选择」，在递归调用之后「撤销选择」

#  46. 全排列
* [原题地址](https://leetcode-cn.com/problems/permutations/)
```java
class Solution {
    List<List<Integer>> list = new ArrayList<>();
    public List<List<Integer>> permute(int[] nums) {
        LinkedList<Integer> track = new LinkedList<>();
        backtrack(nums,track);
        return list;
    }
    
    private void backtrack(int[] nums,LinkedList<Integer> track){
        if(track.size()==nums.length){
            list.add(new LinkedList(track));
            return;
        }

        for(int num:nums){
            if(track.contains(num)){
                continue;
            }
            track.add(num);
            backtrack(nums,track);
            track.removeLast();
        }
    }
}
```
#  78. 子集
* [原题地址](https://leetcode-cn.com/problems/subsets/)

```java
class Solution {
    List<List<Integer>> list = new ArrayList<>();
    public List<List<Integer>> subsets(int[] nums) {
        LinkedList<Integer> track = new LinkedList<>();
        backtrack(nums,0,track);
        return list;
    }

    private void backtrack(int[] nums,int start,LinkedList<Integer> track){
        list.add(new LinkedList(track));
        for(int i = start;i < nums.length;i++){
            track.add(nums[i]);
            backtrack(nums,i+1,track);
            track.removeLast();
        }
    }
}
```
#  77. 组合
* [原题地址](https://leetcode-cn.com/problems/combinations/)

```java
class Solution {
    List<List<Integer>> list =new ArrayList<>();
    public List<List<Integer>> combine(int n, int k) {
        LinkedList<Integer> track = new LinkedList<>();
        backtrack(n,k,1,track);
        return list;
    }

    private void backtrack(int n,int k,int start,LinkedList<Integer> track){
        if(track.size()==k){
            list.add(new LinkedList(track));
            return;
        }

        for(int i = start;i<=n;i++){
            track.add(i);
            backtrack(n,k,i+1,track);
            track.removeLast();
        }
    }
}
```
#  51. N皇后
* [原题地址](https://leetcode-cn.com/problems/n-queens/)

```java
 class Solution {
    List<List<String>> list = new ArrayList<>();
    public List<List<String>> solveNQueens(int n) {
        if(n<=0){
            return list;
        }
        char[][] board = new char[n][n];
        for(char[] chars:board) Arrays.fill(chars,'.');
        backtrack(board,0);
        return list;
    }

    private void backtrack(char[][] board,int row){
        if(board.length==row){
            list.add(charToString(board));
            return;
        }
        int n = board[row].length;
        for(int col = 0;col<n;col++){
            if(!isValid(board,row,col)){
                continue;
            }
            board[row][col] = 'Q';
            backtrack(board,row+1);
            board[row][col] = '.';
        }

    }

    private List<String> charToString(char[][] arr){
        List<String> res = new ArrayList<>();
        for(char[] chars:arr){
            //String.valueOf(char[] data) : 将 char 数组 data 转换成字符串
            res.add(String.valueOf(chars));
        }
        return res;
    }

    private boolean isValid(char[][] board,int row,int col){
        int rows = board.length;
        for(char[] chars:board) if(chars[col]=='Q') return false;
        //右上角
        for(int i = row-1,j=col+1;i>=0 && j < rows;i--,j++){
            if(board[i][j]=='Q') return false;
        }
        //左上角
        for(int i = row-1,j=col-1;i>=0 && j>=0;i--,j--){
            if(board[i][j]=='Q') return false;
        }
        return true;
    }
}
```
#  37. 解数独
* [原题地址](https://leetcode-cn.com/problems/sudoku-solver/)

```java
class Solution {
    public void solveSudoku(char[][] board) {
        backtrack(board,0,0);
    }


    boolean backtrack(char[][] board, int i, int j) {
        int m = 9, n = 9;

        if (i == m) {
            // 找到一个可行解，触发 base case
            return true;
        }

        if (j == n) {
            // 穷举到最后一列的话就换到下一行重新开始。
            return backtrack(board, i + 1, 0);
        }
        
        if (board[i][j] != '.') {
            // 如果有预设数字，不用我们穷举
            return backtrack(board, i, j + 1);
        } 

        for (char ch = '1'; ch <= '9'; ch++) {
            // 如果遇到不合法的数字，就跳过
            if (!isValid(board, i, j, ch))
                continue;

            board[i][j] = ch;
            // 如果找到一个可行解，立即结束
            if (backtrack(board, i, j + 1)) {
                return true;
            }
            board[i][j] = '.';
        }
        // 穷举完 1~9，依然没有找到可行解，此路不通
        return false;
    }

    // 判断 board[i][j] 是否可以填入 n
    boolean isValid(char[][] board, int r, int c, char n) {
        for (int i = 0; i < 9; i++) {
            // 判断行是否存在重复
            if (board[r][i] == n) return false;
            // 判断列是否存在重复
            if (board[i][c] == n) return false;
            // 判断 3 x 3 方框是否存在重复
            if (board[(r/3)*3 + i/3][(c/3)*3 + i%3] == n)
                return false;
        }
        return true;
    }

}
```

```java
class Solution {
    public void solveSudoku(char[][] board) {
        backtrack(board,0,0);
    }

    private boolean backtrack(char[][] board,int i,int j){
        int m = 9,n = 9;
        if(i==m){
            return true;
        }

        if(j==n){
           return backtrack(board,i+1,0);
        }

        if(board[i][j]!='.'){
            return backtrack(board,i,j+1);
        }

        for(char ch = '1';ch <= '9';ch++){
            if(!isValid(board,i,j,ch)) continue;

            board[i][j] = ch;
            if(backtrack(board,i,j+1)){
                return true;
            }
            board[i][j] = '.';
        }

        return false;
    }

    private boolean isValid(char[][] board,int i,int j,char ch){

        for(int col=0;col<9;col++){
            if(board[i][col]==ch) return false;
        }

        for(int row=0;row<9;row++){
            if(board[row][j]==ch) return false;
        }

		// 判断 3 x 3 方框是否存在重复
        for(int k = 0;k<9;k++){
            if(board[(i/3)*3+k/3][(j/3)*3+k%3]==ch) return false;
        }
        return true;
    }
}
```
#  22. 括号生成
* [原题地址](https://leetcode-cn.com/problems/generate-parentheses/)

```java
class Solution {
    List<String> list = new ArrayList<>();
    public List<String> generateParenthesis(int n) {
        dfs(n,n,"");
        return list;
    }
    void dfs(int left,int right,String str){
        if(left==0&&right==0){
            list.add(str);
            return;
        }

        if(left>right) return;

        if(left>0){
            dfs(left-1,right,str+"(");
        }
        if(right>left){
            dfs(left,right-1,str+")");
        }
    }
}
```
**你知道的越多，你不知道的越多。
有道无术，术尚可求，有术无道，止于术。
如有其它问题，欢迎大家留言，我们一起讨论，一起学习，一起进步**
