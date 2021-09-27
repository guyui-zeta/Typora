# 一、栈与队列（简单）

# 剑指 Offer 09. 用两个栈实现队列

![image-20210912181339331](D:/Typora/Typora_Note/Java/DataStructure/%E5%89%91%E6%8C%87Offer%20.assets/image-20210912181339331.png)
## 思路&方法
题目已经标明了方法：双栈。
## 【方法1：自己的思路】
-   入队操作：正常入栈
-   出队操作：颠倒两个栈，出栈，再颠倒两个栈
时间复杂度：O(n)
空间复杂度：O(n)
```java
class CQueue {
    Stack<Integer> stack1;
    Stack<Integer> stack2;
    public CQueue() {
        stack1 = new Stack<>();
        stack2 = new Stack<>();
    }
    //插入操作就直接入栈
    public void appendTail(int value) {
        stack1.push(value);
    }
    //删除操作需要先调换栈，删除后，再调换回来
    public int deleteHead() {
        while(!stack1.empty()){
            stack2.push(stack1.pop());
        }
        int peek = -1;
        if(!stack2.empty()){
            peek = stack2.pop();
        }
        
        while(!stack2.empty()){
            stack1.push(stack2.pop());
        }
        return peek;
    }
}
```
## 【方法2：优化版本】
-   入队操作：正常入栈stack1
-   出队操作【不颠倒了】：  
    如果 stack2 为空，则将 stack1 里的所有元素弹出插入到 stack2 里   
    如果 stack2 仍为空，则返回 -1，否则从 stack2 弹出一个元素并返回
```java
class CQueue {
    Stack<Integer> stack1;
    Stack<Integer> stack2;
    public CQueue() {
        stack1 = new Stack<>();
        stack2 = new Stack<>();
    }
    //插入操作就直接入栈
    public void appendTail(int value) {
        stack1.push(value);
    }
    //删除操作：先判断stack2是否为空，为空就把stack1中所有元素push进来，如果还为空就return -1，否则return pop()
    public int deleteHead() {
        if(stack2.isEmpty()){
            while(!stack1.isEmpty()){
                stack2.push(stack1.pop());
            }
        }
        if(stack2.isEmpty()){
            return -1;
        }
        return stack2.pop();
    }
}
```

## 总结&注意
**总结：**
通过两个栈之间的交替，实现从栈的“先进后出”转换为队列的“先进先出”


# 剑指 Offer 30. 包含min函数的栈
![image-20210912181356681](D:/Typora/Typora_Note/Java/DataStructure/%E5%89%91%E6%8C%87Offer%20.assets/image-20210912181356681.png)
## 思路&方法
**【错误思路】**
定义一个全局变量/实例变量min，在push的时候更新min的值【错误】
因为在pop出最小的值后，min并没有更新  
注意题目要求时间复杂度为O(1)，所有采用最小栈/辅助栈的方式实现，新增一个辅助栈，通过降序的形式记录栈中的最小值

## 【方法1：辅助栈/单调栈/最小栈】
```java
class MinStack {
    //思路：
    //新增一个单调（递减）栈来作为辅助栈，辅助栈栈顶为主栈中的最小值
    //psuh操作：主栈直接进，辅助栈比较后，降序就添加
    //pop操作：主栈直接出，辅助栈比较后，如果相等就出
    //top操作：返回主栈的栈顶元素
    //min操作：返回辅助栈的栈顶操作
    Stack<Integer> mainStack;
    Stack<Integer> minStack;
    /** initialize your data structure here. */
    public MinStack() {
        mainStack = new Stack<>();
        minStack = new Stack<>();
    }
    public void push(int x) {
        mainStack.push(x);
        if(minStack.isEmpty() || x <= minStack.peek()){
            minStack.push(x);
        }
    }
    public void pop() {
        int val = mainStack.pop();
        if(minStack.peek() == val){
            minStack.pop();
        }
    }  
    public int top() {
        return mainStack.peek();
    }  
    public int min() {
        return minStack.peek();
    }
}
```
## 总结&注意   
**总结：**
一般提到时间复杂度O(1)的时候，就是不能通过遍历去操作了，需要单次操作就能得到结果，这是思路就引向单调栈、辅助栈这样的操作了



# -------------------------------------------

# 二、链表（简单）

# 剑指 Offer 06. 从尾到头打印链表

![image-20210912181409779](D:/Typora/Typora_Note/Java/DataStructure/%E5%89%91%E6%8C%87Offer%20.assets/image-20210912181409779.png)

## 思路&方法

常规逆序题，第一反应是递归和栈实现



## 【方法1：递归实现逆序】

- 通过递归实现存栈，从而实现逆序输出

```java
class Solution {
    List<Integer> arr = new ArrayList<>();
    public int[] reversePrint(ListNode head) {
        //思路：遇到逆序输出，先想到用递归/栈的方法实现
        reverse(head);
        int[] res = new int[arr.size()];
        for(int i = 0; i < arr.size(); i++){
            res[i] = arr.get(i);
        }
        return res;
        
    }
    public void reverse(ListNode temp){
        if(temp == null){
            return;
        }
        reverse(temp.next);
        arr.add(temp.val);
    }
    
}
```

## 【方法2：栈实现】

- 通过存栈来实现递归一样的效【问题：stack.size()复合使用会出错，需要单独赋值？？？？】

```java
class Solution {
    public int[] reversePrint(ListNode head) {
        Stack<Integer> stack = new Stack<>();
        ListNode temp = head;
        while(temp != null){
            stack.push(temp.val);
            temp = temp.next;
        }
        int size = stack.size();
        int[] res = new int[size];
        for(int i = 0; i < size; i++){
            res[i] = stack.pop();
        }
        return res;   
    }
}
```



## 总结&注意







# 剑指 Offer 24. 反转链表

![image-20210912181512073](D:/Typora/Typora_Note/Java/DataStructure/%E5%89%91%E6%8C%87Offer%20.assets/image-20210912181512073.png)

## 思路&方法

看到逆序，第一个想到的还是栈or递归

### 【方法1：辅助栈实现逆序】

- 用一个栈来实现逆序，但是要注意像空链表这样的特殊情况
- 时间复杂度：O（n）
- 空间复杂度：O（n）

```java
class Solution {
    public ListNode reverseList(ListNode head) {
        //1.辅助栈实现
        if(head == null || head.next == null){
            return head;
        }
        Stack<ListNode> stack = new Stack<>();
        ListNode temp = head;
        while(temp != null){
            stack.push(temp);
            temp = temp.next;
        }
        int firstVal = stack.pop().val;
        ListNode newHead = new ListNode(firstVal);
        ListNode newTemp = newHead;
        while(!stack.isEmpty()){
            newTemp.next = stack.pop();
            newTemp = newTemp.next;
        }
        newTemp.next = null;
        return newHead;
    }
}
```

### 【方法2：递归实现逆序】

- 递归方法比较难理解
- 1.先遍历到最后一个节点，作为newHead
- 2.然后交换head和head.next的位置
- 时间复杂度：O（n）
- 空间复杂度：O（n）

```java
head.next.next = head;
head.next=null;//为了保证避免出现环链表，第一个节点最后要指向null，因此每次都以这个形式递归
```

- 3.每次递归返回newHead，作为下一次递归的新头节点来使用

![img](D:/Typora/Typora_Note/Java/DataStructure/%E5%89%91%E6%8C%87Offer%20.assets/9DEDAA7A8600D82021C70150ABFE42B9.jpg)

```java
class Solution {
    public ListNode reverseList(ListNode head) {
        //递归实现逆序
        if(head == null || head.next == null){
            return head;
        }
        ListNode newHead = reverseList(head.next);
        head.next.next = head;
        head.next = null;
        return newHead;
    }
}
```

### 【方法3：迭代双指针法】

- 通过双指针来保存该节点和前节点：curr和prev，可以实现前、中、后三个节点的掌握
- 然后通过迭代的方法，一点点往后推移实现逆序
- 时间复杂度：O（n）
- 空间复杂度：O（1）

```java
class Solution {
    public ListNode reverseList(ListNode head) {
        //迭代双指针法
        ListNode prev = null;
        ListNode curr = head;
        while(curr != null){
            ListNode next = curr.next;
            curr.next = prev;
            prev = curr;
            curr = next;
        }
        return prev;
    }
}
```

## 总结&注意

1.遇到涉及到链表的【前】、【中】、【后】节点的题目，可以考虑一下双指针迭代法



# 剑指 Offer 35. 复杂链表的复制

![image-20210912190355393](D:/Typora/Typora_Note/Java/DataStructure/%E5%89%91%E6%8C%87Offer%20.assets/image-20210912190355393.png)

## 思路&方法

**注意点：**由于复杂链表存在一个random节点，在创建该节点的时候，其指向的random节点可能还没创建

1.【哈希表+回溯】可以利用递归（回溯）的方法，先创建该节点，然后递归地创建random节点和next节点。但是会出现重复创建，浪费内存空间。所以加入hashset来判断是否重复创建

2.【拆分+迭代】如果复制操作在两个链表上进行，需要再加HashMap将新老节点对应起来，否则会出现：新节点的random指向老节点的同时，新节点的next指向新节点。所以选择在同一个链表上操作，新老节点会有一个前后的对应关系，从而省去了N的内存空间来创建HashMap

### 【方法1：哈希表+回溯】

- ①先复制该节点，并将新老节点存入map
- ②递归回溯来创建random节点
- ③递归回溯来创建next节点
- 时间复杂度：O（n）
- 空间复杂度：O（n）

```java
class Solution {
    Map<Node,Node> map = new HashMap<>();
    public Node copyRandomList(Node head) {
        //方法1：回溯+哈希表
        //思路：1.先复制值
        //2.再通过递归去创建【next和random】，需要判断哈希表中是否存在
        if(head == null){
            return null;
        }
        if(!map.containsKey(head)){
            Node newNode = new Node(head.val);
            map.put(head,newNode);
            newNode.next = copyRandomList(head.next);
            newNode.random = copyRandomList(head.random);
        }
        return map.get(head);
    }
}

```

### 【方法2：拆分+迭代】

- ①在原先节点的后面，赋值一个节点作为待拆分的节点
- ②给每一个新的节点，赋值random节点
- ③拆分并赋值next，连接起来
- 时间复杂度：O（n）
- 空间复杂度：O（1）

```java
class Solution {
    public Node copyRandomList(Node head) {
        //方法2：迭代+拆分
        //思路：
        //1.在原先结点的后面，复制一个节点作为待拆分节点
        //2.给每一个后面的结点，赋值random结点
        //3.拆分并赋值next连接
        //迭代
        if(head == null){
            return null;
        }
        for(Node temp = head; temp != null; temp = temp.next.next){
            Node newNode = new Node(temp.val);
            newNode.next = temp.next;
            temp.next = newNode;
        }
        //赋值random
        for(Node temp = head;temp != null; temp = temp.next.next){
            Node newNode = temp.next;
            newNode.random = (temp.random == null) ? null : temp.random.next;
        }
        Node newHead = head.next;
        //拆分
        for(Node temp = head; temp != null; temp = temp.next){
            Node newNode = temp.next;
            temp.next = temp.next.next;
            newNode.next = (newNode.next == null) ? null:newNode.next.next;
        }
        return newHead;

    }
}
```

## 总结&注意

1.遇到这种关联的属性/对象可能出现"未创建/不存在"的现象，要多多考虑回溯法，先创建好本体，然后回过头来创建附属属性

2.遇到需要额外空间来复制数据的时候，可以选择在原有的数据内存中复制，然后拆分，节省O(n)的内存空间

# -------------------------------------------

# 三、字符串（简单）

# 剑指 Offer 05. 替换空格

![image-20210914140747086](D:/Typora/Typora_Note/Java/DataStructure/%E5%89%91%E6%8C%87Offer%20.assets/image-20210914140747086.png)

## 思路&方法

常规题，利用StringBuffer来进行替换和追加即可

### 【方法1：常规思路】

- 时间复杂度：O（n）
- 空间复杂度：O（n）

```java
class Solution {
    public String replaceSpace(String s) {
        //思路：先计算整个字符串中的空格数量，然后扩容
        //然后填入新String中，遇到空就替换成%20
        //或者直接使用StringBuffer来append
        StringBuffer sb = new StringBuffer();
        for(int i = 0; i < s.length(); i++){
            char data = s.charAt(i);
            if(data == ' '){
                sb.append("%20");
            }else{
                sb.append(data);
            }
        }
        return sb.toString();
    }
}

```



## 总结&注意

没什么要注意的

# 剑指 Offer 58. ||.左旋字符串

![image-20210914142035929](https://i.loli.net/2021/09/14/eh5y6jt1QDTO89H.png)

## 思路&方法

提供三种方法：切片函数api、StringBuffer、String基本操作

1.利用API中的切片函数 + 拼接的方式

2.利用StringBuffer实现append

3.利用String实现赋值

### 【方法1：切片函数subString + 拼接】

- 时间复杂度：O（n）切片是线性的
- 空间复杂度：O（n）切片完的总长度是n

```java
class Solution {
    public String reverseLeftWords(String s, int n) {
        //1.字符串切片+拼接
        //注意：substring是左闭右开的[)
        return s.substring(n,s.length()) + s.substring(0,n);
    }
}

```

### 【方法2：StringBuffer实现append】

- 时间复杂度：O（n）
- 空间复杂度：O（n）

```java
class Solution {
    public String reverseLeftWords(String s, int n) {
        //2.StringBuffer遍历
        StringBuffer sb = new StringBuffer();
        for(int i = n; i < s.length(); i++){
            sb.append(s.charAt(i));
        }
        for(int i = 0; i < n; i++){
            sb.append(s.charAt(i));
        }
        return sb.toString();
    }
}

```

### 【方法3：String遍历+取模骚操作】

- 时间复杂度：O（n）
- 空间复杂度：O（n）

```java
public String reverseLeftWords(String s, int n) {
        //2.String遍历，用取余巧妙运算
        String newS = "";
        for(int i = n; i < s.length() + n; i++){
            newS += s.charAt(i%s.length());
        }
        return newS;····
    }
```



## 总结&注意

1.字符串题的三种**最常规**的做法，有很好的的参考价值

2.遇到这种环形结构的题目，**取模**会用到很多，而且有时候效果出奇

# -------------------------------------------

# 四、查找算法（简单）

# 剑指 Offer 03. 数组中重复的数字

![image-20210914142938586](D:/Typora/Typora_Note/Java/DataStructure/%E5%89%91%E6%8C%87Offer%20.assets/image-20210914142938586.png)

## 思路&方法

1.找重复数字，第一个想到就是用hashset的方法，快速判断是否含有重复数字

2.排序后，通过一次遍历找到连续出现的，或者与下标不同的元素

3.充分利用题干信息，通过下标与元素的相等关系来交换位置

### 【方法1：HashSet方法】

- 以O(n)的空间来换取O(1)的时间

```java
class Solution {
    public int findRepeatNumber(int[] nums) {
        //1.hashset方法
        Set<Integer> set = new HashSet<>();
        for(int data : nums){
            if(set.contains(data)){
                return data;
            }
            set.add(data);
        }
        return -1;
    }
}

```

### 【方法2：排序+一次遍历】

省略

### 【方法3：利用题干信息】

```java
class Solution {
    public int findRepeatNumber(int[] nums) {
        //2.充分利用题干的0~n-1
        //思路：一次遍历，每次遍历将元素置换到和下标相同的位置，第二次遍历到该元素时，应该下标相同，否则重复
        //如何判断第二次遍历时相同？nums[nums[i]] == nums[i]
        //该在的位置已经有人了
        //注意：1.在遍历的过程中，必须保证遍历过的该位置上的数字正确
        //2.在交换的时候涉及到已更改的下标，需要及时更新
        int i = 0; 
        while(i < nums.length){
            if(nums[i] == i){
                //确保归位在i++
                i++;
                continue;
            }
            if(nums[nums[i]] == nums[i]){
                return nums[i];
            }else{
                int temp = nums[i];
                nums[i] = nums[nums[i]];
                nums[temp] = temp;
            }
        }
        return -1;
    }
}

```



## 总结&注意

1.找重复的数字，常规思路就是通过hashset的O（n）空间来换取O（1）快速查找的时间

2.题干信息会有新的突破口

# 剑指 Offer 53. |.在排序数组中查找数字|

![image-20210914144321792](D:/Typora/Typora_Note/Java/DataStructure/%E5%89%91%E6%8C%87Offer%20.assets/image-20210914144321792.png)

## 思路&方法

1.遇到查找题，并且线性可以实现，那肯定**二分法**



### 【方法1：二分法查找】

- 时间复杂度：O（logn）
- 空间复杂度：O（1）

```java
class Solution {
    public int search(int[] nums, int target) {
        //关键词：有序 线性查找 →想到二分法可以将n→logn
        int left = 0;
        int right = nums.length - 1;
        int res = -1;
        while(left <= right){
            int mid = left - (left - right)/2;
            if(nums[mid] == target){
                res = mid;
                break;
            }else if(nums[mid] < target){
                left = mid + 1;
            }else if(nums[mid] > target){
                right = mid - 1;
            }
        }
        //没找到返回0
        if(res == -1){
            return 0;
        }
        int res_left = res - 1;
        int res_right = res + 1;
        int count = 1;
        //向res的左右遍历，计算次数
        while(res_left >= 0 && nums[res_left] == nums[res]){
            res_left--;
            count++;
        }
        while(res_right < nums.length && nums[res_right] == nums[res]){
            res_right++;
            count++;
        }
        return count;
    }
}
```



## 总结&注意

查找就用二分法



# 剑指 Offer 53. ||.0~n-1中缺失的数字

![image-20210915092147781](D:/Typora/Typora_Note/Java/DataStructure/%E5%89%91%E6%8C%87Offer%20.assets/image-20210915092147781.png)

## 思路&方法

通过线性查找来遍历数组，就可以用二分法来实现

### 【方法1：二分法】

- 如果mid的下标与值相等，说明target在右边
- 如果mid 的下标与值不同，如果mid - 1与值相等，则返回mid，否则target在左边

```java
class Solution {
    public int missingNumber(int[] nums) {
        //思路：用二分法来实现查找
        //如果mid的下标与值相等，说明在右边
        //如果mid的下标与值不同，如果mid-1与值相同，则返回mid。否则在左边
        int left = 0;
        int right = nums.length - 1;
        if(nums[0] != 0){
            return 0;
        }
        if(nums[nums.length - 1] == nums.length - 1){
            return nums.length;
        }
        while(left <= right){
            int mid = left - (left - right)/2;
            if(nums[mid] == mid){
                left = mid + 1;
            }else{
                if(nums[mid - 1] == mid - 1){
                    return mid;
                }else{
                    right = mid - 1;
                }
            }
        }
        return -1;
    }
}
```



## 总结&注意

查找就用二分法

# -------------------------------------------

# 五、查找算法（中等）

# 剑指 Offer 04. 二维数组的查找

![image-20210916090227500](D:/Typora/Typora_Note/Java/DataStructure/%E5%89%91%E6%8C%87Offer%20.assets/image-20210916090227500.png)

## 思路&方法

通过大小区域的判定，选择对角线上的点开始遍历

如果该点比target大，则列--

如果该点比target小，则行++

注意：有两个限制条件，都不能越界

### 【方法1：对角线逼近】

```java
class Solution {
    public boolean findNumberIn2DArray(int[][] matrix, int target) {
        //思路：通过大小区域的判定，选择对角线上的点开始遍历；
        //如果该点比target大，则列--
        //如果该点比target小，则行++
        //注意：有两个限制条件，都不能越界
        if(matrix.length == 0){
            return false;
        }
        int col = matrix[0].length - 1;
        int row = 0;
        while(col >= 0 && row < matrix.length){
            if(matrix[row][col] == target){
                return true;
            }else if(matrix[row][col] > target){
                col--;
            }else if(matrix[row][col] < target){
                row++;
            }
        }
        return false;
    }
}
```

## 总结&注意

特殊题目，特殊方法

注意有两个边界条件



# 剑指 Offer 11. 旋转数组的最小数字

![image-20210916090655005](D:/Typora/Typora_Note/Java/DataStructure/%E5%89%91%E6%8C%87Offer%20.assets/image-20210916090655005.png)

## 思路&方法

可以通过线性查找实现的，一定可以用二分法

### 【方法1：二分法】

- 根据图像，可以分为两段，最小值一定在右边部分，所以所有的逼近都往右边操作
- 取mid，有三种情况
- 如果numbers[mid] > numbers[right] ：min在右边，left = mid + 1
- 如果numbers[mid] < numbers[right] ：min在左边，right = mid【不能放过任何一个可能】
- 如果numbers[mid] = numbers[right] ：保留一个mid，right--【也是为了往右中逼近】

```java
class Solution {
    public int minArray(int[] numbers) {
       //思路：
       //所有的办法都是线性的，最坏情况都是n，所以要考虑二分法
       //取mid，如果numbers[mid]比numbers[right]大：left = mid + 1
       //如果numbers[mid]比numbers[right]小：right = mid,不能放过一个可能
       //如果numbers[mid]等于numbers[right]:right--;
       //直到left >= right时，return right
       //注意：
       //二分法的比较要单一，不能左比一下右比一下
       //未旋转情况也要考虑
       if(numbers[numbers.length - 1] > numbers[0]){
           return numbers[0];
       }
       int left = 0;
       int right = numbers.length - 1;
       while(left < right){
           int mid = left - (left - right)/2;
           if(numbers[mid] > numbers[right]){
               left = mid + 1;
           }else if(numbers[mid] < numbers[right]){
               right = mid;
           }else{
               //mid == right的时候
                right--;
           }
       }
       return numbers[right];
    }
}
```



## 总结&注意

二分法的变形使用，需要注意二分法的比较对象要单一，不能左比较一下，右比较一下，特殊情况



# 剑指 Offer 50. 第一个只出现一次的字符

![image-20210916092628987](D:/Typora/Typora_Note/Java/DataStructure/%E5%89%91%E6%8C%87Offer%20.assets/image-20210916092628987.png)

## 思路&方法

通过字符-次数的对应关系，想到用hashmap来映射次数



### 【方法1：哈希表】

- 时间复杂度：O(n)
- 空间复杂度：O(n)

```java
class Solution {
    public char firstUniqChar(String s) {
        //思路：
        //用map记录字符和出现的次数
        //然后通过遍历找出第一个只出现一次的字符
        Map<Character,Integer> map = new HashMap<>();
        for(int i = 0; i < s.length(); i++){
            if(!map.containsKey(s.charAt(i))){
                map.put(s.charAt(i), 1);
            }else{
                int count = map.get(s.charAt(i));
                map.put(s.charAt(i), count + 1);
            }
        }
        for(int i = 0; i < s.length(); i++){
            if(map.get(s.charAt(i)) == 1){
                return s.charAt(i);
            }
        }
        return ' ';
    }
}
```



## 总结&注意

没有更好的办法

# -------------------------------------------

# 六、搜索与回溯算法（简单）

# 剑指 Offer 32-I. 从上到下打印二叉树

![image-20210922082839309](D:/Typora/Typora_Note/Java/DataStructure/%E5%89%91%E6%8C%87Offer%20.assets/image-20210922082839309.png)

## 思路&方法

根据题意，很明显就是需要BFS（深度优先遍历）来实现，通过队列的方式实现BFS

### 【方法1：深度优先遍历BFS】

- 1.先将头节点入队列
- 2.每次出队列一个元素，并把他的左右子节点入队
- 3.相同操作
- 时间复杂度：O（n）
- 空间复杂度：O（n）

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public int[] levelOrder(TreeNode root) {
        //BFS广度优先搜索，利用队列实现
        //思路：
        //1.先讲root结点入队列
        //2.每次出队列一个节点，然后将其左右节点分别入队
        //3.相同操作
        //注意：Queue是接口，Deque是他的实现类
        if(root == null){
            return new int[0];
        }
        Queue<TreeNode> q = new LinkedList<>();
        List<Integer> res = new ArrayList<>();
        TreeNode temp = root;
        q.add(temp);
        while(!q.isEmpty()){
            TreeNode t = q.poll();
            res.add(t.val);
            if(t.left != null){
                q.add(t.left);
            }
            if(t.right != null){
                q.add(t.right);
            }      
        }
        int[] RES = new int[res.size()];
        for(int i = 0; i < RES.length; i++){
            RES[i] = res.get(i);
        }
        return RES;

    }
}

```



## 总结&注意

 BFS和DFS的应用，一个用队列，一个用栈实现



# 剑指 Offer 32-II. 从上到下打印二叉树II

![image-20210922083256239](D:/Typora/Typora_Note/Java/DataStructure/%E5%89%91%E6%8C%87Offer%20.assets/image-20210922083256239.png)

## 思路&方法

和上一题思路一样，也是用队列实现的BFS，不同之处在于打印的方式

打印方式：每次在出队列前，记录下队列的size，作为改成该层的节点个数来记录进list

### 【方法1：BFS+特殊打印】

- 特殊打印：
- 1.每次出队列前记录队列的size，作为该层的节点个数
- 2.按照size，将节点保存进list中，并添加左右子节点
- 3.将小list添加进大list中

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        //还是用BFS，只不过输出方式变了
    
        Deque<TreeNode> q = new LinkedList<>();
        List<List<Integer>> res = new ArrayList<>();
        if(root != null){
            //应对null的方法
            q.add(root);
        }
        while(!q.isEmpty()){
            //注意：里面的list需要每次都新建
            List<Integer> temp = new ArrayList<>();
            //此时size只记录了一次，即换行时定的size
            int size = q.size();
            for(int i = size; i > 0; i--){
                TreeNode t = q.poll();
                temp.add(t.val);
                if(t.left != null){
                    q.add(t.left);
                }
                if(t.right !=null){
                    q.add(t.right);
                }
            }
            res.add(temp);
        }
        return res;
    }
}
```

## 总结&注意

注意：小list需要每次重新创建，否则用的是同一个list

# 剑指 Offer 32-III. 从上到下打印二叉树III

![image-20210923082339192](D:/Typora/Typora_Note/Java/DataStructure/%E5%89%91%E6%8C%87Offer%20.assets/image-20210923082339192.png)

## 思路&方法

1.按照常规的BFS进行广度优先遍历，然后根据层数的奇偶性来决定逆序还是正序保存

2.双端队列：通过在队列的头尾加元素来实现正序和逆序记录，利用LinkedList实现

![image-20210923083204658](D:/Typora/Typora_Note/Java/DataStructure/%E5%89%91%E6%8C%87Offer%20.assets/image-20210923083204658.png)

### 【方法1：BFS+特殊打印】

```java
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        //思路：其实还是队列实现BFS，只不过在打印方式上做了改变
        //通过记录层数来决定正序输出还是逆序输出的依据
        //1.将root结点入队
        //2.记录队列的size，作为改层的结点数，并作为换行的依据
        //3.记录每层的层数变化，作为正序还是逆序的依据
        Queue<TreeNode> q = new LinkedList<>();
        List<List<Integer>> res = new ArrayList<>();
        if(root != null){
            q.add(root);
        }
        int count = 0;
        while(!q.isEmpty()){
            count++;//记录层数
            int size = q.size();//记录该层的元素个数
            List<Integer> list = new ArrayList<>();
            for(int i = 0; i < size; i++){
                TreeNode t = q.poll();
                list.add(t.val);
                if(t.left != null){
                    q.add(t.left);
                }
                if(t.right != null){
                    q.add(t.right);
                }
            }
            if(count % 2 == 0){
                List<Integer> newList = new ArrayList<>();
                for(int i = list.size() - 1; i >= 0; i--){
                    newList.add(list.get(i));
                }
                list = newList;
            }
            res.add(list);
        }
        return res;
    }
}
```

### 【方法2：BFS+双端队列】

- 利用双端队列来实现正序和逆序的记录
- 正序：addLast（node.val）在队列（链表）的末位添加元素
- 逆序：addFirst（node.val）在队列（链表）的头部添加元素

```java
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        Queue<TreeNode> queue = new LinkedList<>();
        List<List<Integer>> res = new ArrayList<>();
        if(root != null) queue.add(root);
        while(!queue.isEmpty()) {
            LinkedList<Integer> tmp = new LinkedList<>();
            for(int i = queue.size(); i > 0; i--) {
                TreeNode node = queue.poll();
                if(res.size() % 2 == 0) tmp.addLast(node.val); // 偶数层 -> 队列头部
                else tmp.addFirst(node.val); // 奇数层 -> 队列尾部
                if(node.left != null) queue.add(node.left);
                if(node.right != null) queue.add(node.right);
            }
            res.add(tmp);
        }
        return res;
    }
}
```



# 总结&注意

常规的BFS题，在打印的机制里添加了双端队列这个概念

以后的正序逆序打印都可以用双端队列来实现

# -------------------------------------------

# 七、搜索与回溯算法（简单）

# 剑指 Offer 26. 树的子结构

![image-20210923110001958](D:/Typora/Typora_Note/Java/DataStructure/%E5%89%91%E6%8C%87Offer%20.assets/image-20210923110001958.png)

## 思想&方法



**递归和回溯的区别：**

- **递归：**

  - 为了描述问题的某一状态，必须用到该状态的上一状态，而描述上一状态，又必须用到上一状态的上一状态……这种用自已来定义自己的方法，称为递归定义。
  - 递归是一种算法结构，递归会出现在子程序中自己调用自己或间接地自己调用自己。

- **回溯：**

  - 从问题的某一种可能出发, 搜索从这种情况出发所能达到的所有可能, **当这一条路走到” 尽头 “的时候, 再倒回出发点**, 从另一个可能出发, 继续搜索. 这种不断” 回溯 “寻找解的方法, 称作” 回溯法 “。
  - 回溯是一种算法思想，可以用递归实现。通俗点讲回溯就是一种试探，类似于穷举

  **什么场景常用到回溯：**图或树的遍历找最优解/可行解的场景，往往会涉及到回溯

### 【方法1：遍历+回溯】

- 由于需要通过遍历AB才能确定是否是子结构，所以想到回溯
- 思路：【遍历A+回溯AB】
  - 1.遍历A树
  - 2.通过回溯的思想，比较A是否包含B【穷举，左右到底，false就回溯上去】
  - 3.通过递归实现回溯，定义子问题：A是否包含B【A节点是否等于B节点】
- 递归问题思路：【子问题】
  - 1.如果B==null，说明B已经遍历完，返回True
  - 2.如果A==null，说明A完了，B还没完，返回False
  - 3.如果A.val ！= B.val，说明该节点不相同，返回False
  - 4.递归操作： return isContains(A.left,B.left) && isContains(A.right,B.right);

```java
class Solution {
    public boolean isSubStructure(TreeNode A, TreeNode B) {
        //由于需要通过遍历AB才能确定是否是子结构，所以想到回溯
        //思路：【遍历A+回溯AB】
        //1.遍历A树
        //2.通过回溯的思想，比较A是否包含B【穷举，左右到底，false就回溯上去】
        //3.通过递归实现回溯，定义子问题：A是否包含B【A节点是否等于B节点】
        //递归问题思路：【子问题】
        //1.如果B==null，说明B已经遍历完，返回True
        //2.如果A==null，说明A完了，B还没完，返回False
        //3.如果A.val ！= B.val，说明该节点不相同，返回False
        //4.递归操作： return isContains(A.left,B.left) && isContains(A.right,B.right);
        if(A == null || B == null){
            return false;
        }
        return isContains(A,B) || isSubStructure(A.left, B) || isSubStructure(A.right, B);
    }
    public boolean isContains(TreeNode A, TreeNode B){
        if(B == null){
            return true;
        }
        if(A == null || A.val != B.val){
            return false;
        }
        return isContains(A.left,B.left) && isContains(A.right,B.right);
    }
}
```



## 总结&注意

一般涉及到图或树的复杂遍历，都会用到回溯



# 剑指 Offer 27. 二叉树的镜像

![image-20210923132803258](D:/Typora/Typora_Note/Java/DataStructure/%E5%89%91%E6%8C%87Offer%20.assets/image-20210923132803258.png)

## 思路&方法

一看就是需要遍历，然后从后往前开始交换子节点————回溯大法

### 【方法1：回溯-从下往上交换】

- 1.先进行堆栈，直到叶子节点
- 2.开始交换左右子节点，并往上回溯

```java
class Solution {
    public TreeNode mirrorTree(TreeNode root) {
        //涉及到树的遍历，肯定会需要涉及回溯
        //思路：【回溯+交换左右子节点】
        change(root);
        return root;
    }
    public void change(TreeNode temp){
        if(temp == null){
            return ;
        }
        //先进行堆栈，到叶子节点【先递归，压栈】
        change(temp.left);
        change(temp.right);
        //开始交换【后功能】
        TreeNode t = temp.left;
        temp.left = temp.right;
        temp.right = t;
        //往上回溯【再return】
        return;
    }
}
```

### 【方法2：栈实现-从上往下交换】

- 树的遍历可以用递归，还可以用DFS或BFS的队列和栈的形式实现

```java
class Solution {
    public TreeNode mirrorTree(TreeNode root) {
        //栈实现或队列实现【只是为了遍历】
        //思路：类似于DFS，从上往下交换
        //栈的作用：用于遍历
        Stack<TreeNode> Stack = new Stack<>();
        TreeNode temp = root;
        if(temp != null){
            Stack.add(temp);
        }
        while(!Stack.isEmpty()){
            TreeNode t1 =  Stack.pop();
            if(t1.left != null){
                Stack.add(t1.left);
            } 
            if(t1.right != null){
                Stack.add(t1.right);
            } 
            TreeNode t = t1.left;
            t1.left = t1.right;
            t1.right = t;
        }
        return root;
    }
}
```

## 总结&注意

**回溯的精髓：**==<u>先递归，后功能，再return</u>==

**树的遍历方法：**==<u>递归、DFS【栈】、BFS【队列】</u>==

# 剑指 Offer 28. 对称的二叉树

![image-20210923152146226](D:/Typora/Typora_Note/Java/DataStructure/%E5%89%91%E6%8C%87Offer%20.assets/image-20210923152146226.png)

## 思路&方法

涉及到树的遍历，需要用到递归

递归的子问题：判断传入的两个节点是否对称

递归子问题的判断条件：

- 1.【终止条件】如果两个节点同时==null，则返回true
- 2.【终止条件】如果两个节点不同时==null，或者两个节点不相等，则返回false
- 3.【压栈条件】如果两个节点相等，则继续压栈遍历

### 【方法1：递归】

```java
class Solution {
    public boolean isSymmetric(TreeNode root) {
        //涉及到树的遍历，需要用递归
        //思路：
        //1.定义递归子问题：判断传入的两个节点是否对称
        //递归子问题的判断条件：
        //1.如果两个节点相等，则继续压栈【剩余的压栈条件】
        //2.如果两个节点不相等，返回false【终止条件】
        //3.如果两个节点同时等于null，则返回true【终止条件】
        //4.如果两个节点不同时等于null，则返回false【终止条件】
        return root == null ? true : recur(root.left,root.right);
    }
    public boolean recur(TreeNode L,TreeNode R){
        //如果两个节点同时为null，则返回true
        if(L == null && R == null){
            return true;
        }
        //如果两个节点不同时为null，或者不相等，则返回false
        if(L == null || R == null || L.val != R.val){
            return false;
        }
        //剩余条件：如果两个节点相等
        return recur(L.left,R.right) && recur(L.right,R.left);
    }
}
```

## 总结&注意

**==递归的模板：==**

- 1.设计子问题的功能：
- 2.设计递归的【终止条件】
- 3.设计递归的【压栈条件】，往往是终止条件挑剩下的
- 4.如果返回值是boolean类型，需要分清楚&&和||的区别
  - &&：表示都要成立
  - ||：表示只需要成立一个即可

# -------------------------------------------

# 八、动态规划（简单）

# 剑指 Offer 10-I. 斐波那契数列

![image-20210925141020989](D:/Typora/Typora_Note/Java/DataStructure/%E5%89%91%E6%8C%87Offer%20.assets/image-20210925141020989.png)

## 思路&方法

典型的动态规划题目（DP）

https://www.zhihu.com/question/23995189

注意：不能采用数组记录数据，否则就违背了”无后效性“原则

### 【方法1：动态规划DP】

```java
class Solution {
    public int fib(int n) {
        //典型的动态规划题目
        //思路：
        //注意：不能用数组记录，否则就违背了“无后效性”原则
        //1.确定关系式
        //2.不用数组，用迭代完成DP
        if(n == 0){
            return 0;
        }
        if(n == 1){
            return 1;
        }
        int a = 0;
        int b = 1;
        int sum = 0;
        for(int i = 2; i < n+1; i++){
            sum = (a + b) % 1000000007;
            a = b;
            b = sum;
        }
        return sum;
    }
}
```

##  总结&注意

- 动态规划的特征：
  - 无后效性：
    - “未来与过去无关”，只需要值，不关心如何计算。如果给定某一阶段的状态，则在这一阶段以后过程的发展不受这阶段以前各段状态的影响。
  - 最优子结构：
    - 大问题的最优解可以由小问题的最优解推出，需要将大问题拆成几个小问题
- 注意：
  - 动态规划忌讳用数组记录数据，因为违背“无后效性”，需要用迭代的方式计算



# 剑指 Offer 10-II. 青蛙跳台阶问题

![image-20210925152157563](D:/Typora/Typora_Note/Java/DataStructure/%E5%89%91%E6%8C%87Offer%20.assets/image-20210925152157563.png)

## 思路&方法

斐波那契数列的包装题

### 【方法1：动态规划DP】

![image-20210925152228016](D:/Typora/Typora_Note/Java/DataStructure/%E5%89%91%E6%8C%87Offer%20.assets/image-20210925152228016.png)

```java
class Solution {
    public int numWays(int n) {
        //斐波那契数列的包装题
        //思路：
        //跳到n前有两种可能，跳一下or跳两下
        //得出关系式：f(n) = f(n - 1) + f(n - 2)
        if(n == 0){
            return 1;
        }
        if(n == 1){
            return 1;
        }
        int a = 1;
        int b = 1;
        int sum = 0;
        for(int i = 2; i < n + 1; i++){
            sum = (a + b) % 1000000007;
            b = a;
            a = sum;
        }
        return sum;
    }
}
```

## 总结&方法

经典DP



# 剑指 Offer 63. 股票的最大利润

![image-20210927105941970](D:/Typora/Typora_Note/Java/DataStructure/%E5%89%91%E6%8C%87Offer%20.assets/image-20210927105941970.png)

## 思路&方法

利用动态规划实现，不断迭代最小买入价（min）和最大利润值（max_profile）来求得最大利润

### 【方法1：动态规划】

- 1.设置min，记录最小买入价格
- 2.设置max_profile，记录最大收益值

```java
class Solution {
    public int maxProfit(int[] prices) {
        //动态规划（DP）题：
        //思路：
        //1.设置min，记录最小买入值
        //2.设置max_profile，记录最大收益值
        if(prices == null || prices.length == 0){
            return 0;
        }
        int min = prices[0];
        int max_Profit = 0;
        for(int i = 0; i < prices.length; i++){
            if(prices[i] < min){
                min = prices[i];
            }
            if(prices[i] - min > max_Profit){
                max_Profit = prices[i] - min;
            }
        }
        return max_Profit;
    }
}
```

## 总结&注意

动态规划思想逐渐成熟

# 九、动态规划（中等）

# 剑指 Offer 42. 连续子数组的最大和



