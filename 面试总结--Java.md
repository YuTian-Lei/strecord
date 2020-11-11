## mybatis中$和#的区别

- \#{ }是预编译处理，MyBatis在处理#{ }时，它会将sql中的#{ }替换为？
- ${ }是字符串替换， MyBatis在处理${ }时,它会将sql中的${ }替换为变量的值，传入的数据不会加两边加上单引号

## 行锁和表锁区别

### 行锁

- 支持的存储引擎：Innodb
- InnoDB行锁是通过给索引上的索引项加锁来实现的，意味着：只有通过索引条件检索数据，InnoDB才使用行级锁，否则，InnoDB将使用表锁
- 适用场景：有大量按索引条件并发更新少量不同数据，同时又有并发查询的应用
- 特点：开销大，加锁慢；会出现死锁；锁定粒度小，发生锁冲突的概率低，并发度高

### 表锁

- 支持的存储引擎：Innodb、MYIsam
- 适用场景：以查询为主，只有少量按索引条件更新数据的应用
- 特点：开销小，加锁快；不会出现死锁；锁定力度大，发生锁冲突概率高，并发度最低

## 线上服务器mysql 100%占用

- top命令，确认是否mysql问题
- 登录mysql服务器，执行show full processlist; 查询那几条sql语句问题
- explain查看执行计划
- 优化

## 删除重复元素,保留最小id

```sql
DELETE 
FROM
USER 
WHERE
id NOT IN (
   SELECT
	 * 
   FROM (SELECT MIN( id ) FROM USER GROUP BY username) a
)
```

## 为什么选b+树作为索引结构

- 减少磁盘读写IO(Mysql如何衡量查询效率呢？磁盘IO次数)
- 范围查询

## 关联查询和子查询区别

- 子查询虽然很灵活，但是执行效率并不高
- 执行子查询时，MYSQL需要创建临时表，查询完毕后再删除这些临时表
- 可以使用连接查询（JOIN）代替子查询，连接查询不需要建立临时表，因此其速度比子查询快

## http1.0 1.1 2.0区别

### HTTP1.0 HTTP 1.1主要区别

- 长连接
- 节约带宽
- HOST域

### HTTP1.1 HTTP 2.0主要区别

- 多路复用
- 二进制分帧
- 数据压缩
- 服务器推送

## 算法

### 1.只出现一次的数字leetcode 136

```Java
//异或
class Solution {
    public int singleNumber(int[] nums) {
        int num = 0;
        for(int i = 0; i < nums.length; i++){
            num = num ^ nums[i];
        }
        return num;
    }
}

```

###  2.反转二叉树

```Java
public class Solution {
    public void Mirror(TreeNode root) {
      if(root==null){
          return;
      }
      Mirror(root.left);
      Mirror(root.right);
      TreeNode temp=root.left;
      root.left=root.right;
      root.right=temp;
    }
}
```

### 3.判断二叉树是否对称

```Java
public class Solution {
    boolean isSymmetrical(TreeNode pRoot)
    {
        if(pRoot == null)
            return true;
        
        return isSymmetrical(pRoot.left,pRoot.right);
        
    }
    boolean isSymmetrical(TreeNode left,TreeNode right){
        if(left == null || right == null)
            return left == right;
        
        if(left.val != right.val)
            return false;
        
        return isSymmetrical(left.left,right.right) && isSymmetrical(left.right,right.left);
    }
}
```

### 4.leetcode 121. 买卖股票的最佳时机

```Java
    public int maxProfit(int[] prices) {
       int max = 0;
       for(int i = 0; i < prices.length; i++){
           for(int j = i + 1; j < prices.length; j++)
            max = Math.max(prices[j]-prices[i],max);
       }
       return max;
    }
     
       public int maxProfit(int[] prices) {
       int max = 0;
       int in = 0;
       for(int index = 1; index < prices.length; index++){
           if(prices[index] < prices[in])
               in = index;
            else
               max = Math.max(max,prices[index] - prices[in]);
       }
       return max;
    }
```

### 5.122. 买卖股票的最佳时机 II

```Java
    public int maxProfit(int[] prices) {
        int maxPro = 0, tmp = 0;
        for (int i = 1; i < prices.length; ++i) {
            tmp = prices[i] - prices[i-1];
            if (tmp > 0)
                maxPro += tmp;
        }
        return maxPro;

    }
```

### 6.StringBuffer原理

- 维持一个char数组
- append新字符串，先扩容数组，然后将新字符串放到新数组中

### 7. 2亿数据如何去重

- 对大文件进行hash成很多小文件
- 针对小文件进行数据处理