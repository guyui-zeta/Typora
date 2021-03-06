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
  - 动态规划忌讳用数组记录数据，因为违背“无后效性”，需要用迭代的方式计算——【**==滚动数组来优化空间==**】



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

# -------------------------------------------

# 九、动态规划（中等）

# 剑指 Offer 42. 连续子数组的最大和

![image-20211003225916421](D:/Typora/Typora_Note/Java/DataStructure/%E5%89%91%E6%8C%87Offer%20.assets/image-20211003225916421.png)

## 思路&方法

利用动态规划实现，首先找到动态规划转移方程，然后根据转移方程，一步步迭代子数组的最大值max_temp

- 动态规划转移方程：
  - f(i) = max{f(i - 1) + nums[i], nums[i]}
  - f(i)代表以nums[i]结尾的最大子数组之和

### 【方法1：动态规划】

```java
class Solution {
    public int maxSubArray(int[] nums) {
        //思路：利用动态规划实现
        //动态规划转移方程：f(i)代表以nums[i]结尾的最大子数组之和
        //f(i) = max{f(i - 1) + nums[i], nums[i]}//前一个最大和f(i - 1)加nums[i]，还是nums[i]单独一个大
        //通过迭代max的方法不利用额外的空间来存储
        int res = nums[0];//存储最终最大的值，需要不断迭代
        int max_temp = nums[0];
        for(int i = 1; i < nums.length; i++){
            if(max_temp + nums[i] > nums[i]){
                max_temp = max_temp + nums[i];
            }else{
                max_temp = nums[i];
            }
            if(res < max_temp){
                res = max_temp;
            }
        }
        return res;
    }
}
```



### 【方法2：分治没做】

## 总结&注意

1.动态规划题目，最重要的部分就是找到动态规划转移方程，首先要有一次遍历的思想，然后每次遍历到的f(i)能与题目要求贴合，即每次遍历时的f(i)都能代表该i下的解——**==动态规划的精髓==**【例如本题f(i)即代表：以nums[i]结尾的子数组中的最大值】



# 剑指 Offer 47. 礼物的最大价值

![image-20211003232420855](D:/Typora/Typora_Note/Java/DataStructure/%E5%89%91%E6%8C%87Offer%20.assets/image-20211003232420855.png)

## 思路&方法

和42题思路一致，一维的最大值变成了二维的最大值

### 【方法1：动态规划（自己的思路）】

- 动态规划转移方程：
  - f(i,j) = max{f(i-1,j), f(i,j-1)} + f(i,j)
- 由于需要记录左和上两个值，所以用了一个二维数组来记录【可以优化】

```java
class Solution {
    public int maxValue(int[][] grid) {
        //思路：同上一题
        int res = 0;
        int max_temp = 0;
        int[][] temp = new int[grid.length][grid[0].length];
        for(int i = 0; i < grid.length; i++){
            for(int j = 0; j < grid[0].length; j++){
                if(i - 1 >= 0 && j - 1 >=0){
                    if(temp[i - 1][j] + grid[i][j] > temp[i][j - 1] + grid[i][j]){
                        max_temp = temp[i - 1][j] + grid[i][j];
                    }else{
                        max_temp = temp[i][j - 1] + grid[i][j];
                    }
                }else if(i - 1 < 0 && j - 1 >= 0){
                    max_temp = temp[i][j - 1] + grid[i][j];
                }else if(j - 1 < 0 && i - 1 >= 0){
                    max_temp = temp[i - 1][j] + grid[i][j];
                }else{
                    max_temp = grid[i][j];
                }
                temp[i][j] = max_temp;
                res = max_temp;
            }
        }
        return res;
    }
}
```

### 【方法2：动态规划（空间上的优化）】

- 可以用原二维数组代替额外使用的空间

```java
class Solution {
    public int maxValue(int[][] grid) {
        //思路：同上一题
        int res = 0;
        int max_temp = 0;
        for(int i = 0; i < grid.length; i++){
            for(int j = 0; j < grid[0].length; j++){
                if(i - 1 >= 0 && j - 1 >=0){
                    if(grid[i - 1][j] + grid[i][j] > grid[i][j - 1] + grid[i][j]){
                        max_temp = grid[i - 1][j] + grid[i][j];
                    }else{
                        max_temp = grid[i][j - 1] + grid[i][j];
                    }
                }else if(i - 1 < 0 && j - 1 >= 0){
                    max_temp = grid[i][j - 1] + grid[i][j];
                }else if(j - 1 < 0 && i - 1 >= 0){
                    max_temp = grid[i - 1][j] + grid[i][j];
                }else{
                    max_temp = grid[i][j];
                }
                grid[i][j] = max_temp;
                res = max_temp;
            }
        }
        return res;
    }
}
```

## 总结&注意

经典的动态规划题目的二维版本

- **==动态规划的精髓：==**
  - 状态：f(i)，f(i)能诠释动态规划的最终目的
  - 转移方程：表述递推关系



# -------------------------------------------

# 十、动态规划（中等）

# 剑指 Offer 46. 把数字翻译成字符串

## 思路&方法

动态规划

### 【方法1：动态规划（自己的纰漏版本）】

- 没有考虑到两个以上且间隔的情况，需要排列组合

```java
class Solution {
    public int translateNum(int num) {
        //动态规划
        //状态转移方程：
        //f(i) = f(i - 1) + {if(num[i] > 5 && num[i - 1] == 1) || if(num[i] < 5 && 1 <= num[i - 1] <=2)}=1
        //但由于int只能从个位数往左取值，所以根据实际情况需要更改
        int res = 1;
        int right = num % 10;
        int left = num % 10;
        num /= 10;
        while(num > 0){
            left = num % 10;//取出最右位数字，即个位
            if(right > 5 && left == 1){
                res++;
            }else if(right <= 5 && (left >= 1 && left <=2)){
                res++;
            }
            right = left;
            num /= 10;
        }
        return res;

    }
}
```



### 【方法2：动态规划+滚动数组】

- 类似于青蛙跳台阶题
  - dp[i] = dp[i - 1] + dp[i - 2]【如果i-1和i能满足10-25的话】
  - dp[i - 1]：第i位单独翻译，所以dp[i - 1]×1
  - dp[i - 2]：第i位和第i-1位一起翻译，所以dp[i - 2]×1
- 只涉及到前两项有关，所以运用滚动数组实现空间优化

![image-20211005084426574](剑指Offer .assets/image-20211005084426574.png)

```java
class Solution {
    public int translateNum(int num) {
        //动态规划+滚动数组
        //状态转移方程：
        //dp[i] = dp[i - 1]×1 + dp[i - 2]×1【第i、i-1两位满足翻译条件】
        int dp_i = 1;
        int dp_i1 = 0;
        int dp_i2 = 0;
        String nums = String.valueOf(num);
        for(int i = 0; i < nums.length(); i++){ 
            //更新dp[i - 1]和dp[i - 2]
            dp_i2 = dp_i1;
            dp_i1 = dp_i;
            //更新dp[i]
            dp_i = dp_i1;
            //避免i=0溢出
            if(i == 0){
                continue;
            }
            //substring左闭右开！！！！
            String sub = nums.substring(i - 1, i + 1);
            if(sub.compareTo("10") >= 0 && sub.compareTo("25") <= 0){
                dp_i += dp_i2;
            }
        }
        return dp_i;
    }
}
```

## 总结&注意

- 1.substring[i-1,i+1]：是左闭右开的！！
- 2.动态规划涉及到参数的迭代，一般用滚动数组来实现空间上的优化



# 剑指 Offer 48. 最长不含重复字符的子字符串

![image-20211005095658795](剑指Offer .assets/image-20211005095658795.png)

## 思路&方法

- 1.动态规划
  - 一般涉及到最大值的迭代的，都能用动态规划实现
- 2.滑动窗口
  - 补充思路

### 【方法1：动态规划 + 哈希表】

- 转移方程思路：
  - 如果与s[j]相同的元素s[i]不存在，或者在dp[j - 1]的范围之外，则dp[j] = dp[j - 1] + 1
  - 如果与s[j] 相同的元素s[i]存在，且i在dp[j - 1]的范围之内即（dp[j - 1] >= j - i ），则dp[j] = j - i;
- 剩余的问题：如何得到i
  - HashMap：利用哈希表来记录每一个元素最后出现的下标，即i
- 复习HashMap一个重要的API：
  - map.getOrDefalut(element,value);
  - 如果找到了为element的key，就返回对应的value值，否则范围默认的value
- 时间复杂度：O(n)用于动态规划的一次遍历
- 空间复杂度：O(128) = O(1)，因为字符的ASCII范围是0-127，所以Map最多用128的空间，所以为O(1)

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        //动态规划
        //转移方程：
        //dp[j] = ①dp[j - 1] + 1, dp[j - 1] < j - i; ②j - i, dp[j - 1] >= j - i
        //其中i为与s[j]相同的最近的下标
        //Q：如何获得i？A：用哈希表来记录每个元素的最后一次出现的下标
        Map<Character, Integer> map = new HashMap<>();
        int temp = 0;
        int res = 0;
        for(int j = 0; j < s.length(); j++){
            //取出最后一个值i
            int i = map.getOrDefault(s.charAt(j), -1);//如果找到了s[j]，就返回s[j]最后一次常出现的下标，否则返回-1
            //更新map的值
            map.put(s.charAt(j),j);
            temp = (temp < j - i) ? (temp + 1) : (j - i);
            if(temp > res){
                res = temp;
            }
        }
        return res;
    }
}
```

### 【方法2：双指针（滑动窗口） + 哈希表】

- 利用双指针left和right来维护滑动窗口
  - 如果map中存在，则更新left
- 利用哈希表来记录最后一个下标

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        //滑动窗口+哈希表
        //通过left和right维护滑动窗口，并用哈希表记录最后一个下标
        //right维护的窗口依然是以right结尾的最长不重复字符串
        
        Map<Character,Integer> map = new HashMap<>();
        int left = 0;
        int max = 0;
        for(int right = 0; right < s.length(); right++){
            if(map.containsKey(s.charAt(right))){
                //如果map中存在s[right]，则更新left
                left = Math.max(left, map.get(s.charAt(right)) + 1);
                //为了应对完全没有重复的例子，所以在这里和结果那都+1
            }
            map.put(s.charAt(right), right);
            max = Math.max(max, right - left + 1);
        }
        return max;
    }
}
```









# -------------------------------------------

# 十一、双指针（简单）

# 剑指 Offer 18. 删除链表的节点

![image-20211007220901595](剑指Offer .assets/image-20211007220901595.png)

## 思路&方法

遇到删除链表节点或者找某个节点的题目，首先想到双节点

### 【方法1：双指针】

- 快慢双指针，一前一后来遍历链表

```java
class Solution {
    public ListNode deleteNode(ListNode head, int val) {
        //双指针
        //思路：
        //快慢指针fast、slow分别遍历前后
        ListNode temp = head;
        ListNode slow = temp;
        ListNode fast = temp.next;
        //删除节点是头节点情况
        if(temp.val == val){
            return temp.next;
        }
        while(fast != null){
            if(fast.val == val){
                slow.next = fast.next;
                break;
            }
            slow = slow.next;
            fast = fast.next;
        }
        return temp;
    }
}
```



# 总结&注意

常规的快慢双指针题目



# 剑指 Offer 22. 链表中倒数第k个节点

![image-20211007221752036](剑指Offer .assets/image-20211007221752036.png)

## 思路&方法

1.快慢双指针法，间隔k

2.常规递归方法

### 【方法1：快慢双指针】

- 定义快慢双指针，fast为head的后k个节点
- 遍历直到fast == null，此时slow就是倒数第k个节点

```java
class Solution {
    public ListNode getKthFromEnd(ListNode head, int k) {
        //1.双指针法、2.递归法
        //思路：
        //定义快慢双指针，fast为head的后k个节点
        //遍历直到fast==null，此时slow就是倒数第k个节点
        ListNode slow = head;
        ListNode fast = head;
        for(int i = 0; i < k; i++){
            fast = fast.next;
        }
        while(fast != null){
            slow = slow.next;
            fast = fast.next;
        }
        return slow;
    }
}
```

### 【方法2：递归方法】

- 先堆栈，再计数，后返回倒数第k个数

```java
class Solution {
    int count = 0;
    public ListNode getKthFromEnd(ListNode head, int k) {
        //1.双指针法、2.递归法
        //思路：递归法
        //递归：往后遍历节点
        //终止条件：节点为null ，计数=k时 
        //特殊情况
        //终止条件
        if(head == null){
            return head;
        }
        ListNode res = getKthFromEnd(head.next,k);
        count++;
        if(count == k){
            return head; 
        }
        return res;
    }
}
```

# 总结&注意

还是双指针比较好用



# -------------------------------------------

# 十二、双指针（简单）

# 剑指 Offer 25. 合并两个排序的链表

![image-20211008232808291](剑指Offer .assets/image-20211008232808291.png)

## 思路&方法

双指针

### 【方法1：双指针（迭代）】

- 用一个新的链表节点存储结果，然后用双指针分别遍历两个链表，依次进入新链表
- 遍历完后，把剩余的链表节点加入进去
- 问题：没用哑结点，会导致新链表的第一个节点无法确定

```java
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        //双指针
        //思路：
        //用一个新的链表节点存储结果
        //双指针分别遍历两个链表，依次入链表
        if(l1 == null){
            return l2;
        }
        if(l2 == null){
            return l1;
        }
        //没有用哑结点就会这样
        ListNode res = null;
        if(l1.val < l2.val){
            res = new ListNode(l1.val);
            l1 = l1.next;
        }else{
            res = new ListNode(l2.val);
            l2 = l2.next;
        }
        
        ListNode temp = res;
        while(l1 != null && l2 != null){
            if(l1.val < l2.val){
                temp.next = new ListNode(l1.val);
                l1 = l1.next;
            }else{
                temp.next = new ListNode(l2.val);
                l2 = l2.next;
            }
            temp = temp.next;
        }
        if(l1 != null){
            temp.next = l1;
        }
        if(l2 != null){
            temp.next = l2;
        }
        return res;


    }
}
```



### 【方法1：双指针（迭代）引入哑结点】

- 哑结点：
  - 放在第一个存放数据节点之前、头节点之后的节点。
  - 作用：可以用在一些需要一个前驱节点的场合【就是再加一个头指针的意思】

```java
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        //双指针（+哑结点）
        //思路：
        //用一个新的链表节点存储结果
        //双指针分别遍历两个链表，依次入链表
        if(l1 == null){
            return l2;
        }
        if(l2 == null){
            return l1;
        }
        //用了哑结点，方便很多
        ListNode res = new ListNode(-1);

        ListNode temp = res;
        while(l1 != null && l2 != null){
            if(l1.val < l2.val){
                temp.next = new ListNode(l1.val);
                l1 = l1.next;
            }else{
                temp.next = new ListNode(l2.val);
                l2 = l2.next;
            }
            temp = temp.next;
        }
        if(l1 != null){
            temp.next = l1;
        }
        if(l2 != null){
            temp.next = l2;
        }
        return res.next;
    }
}
```

## 总结&思路

字节二面秃头出的题目



# 剑指 Offer 52. 两个链表的第一个公共节点

![image-20211008232841475](剑指Offer .assets/image-20211008232841475.png)

## 思路&方法

- 找重复的节点引用：hashset
- 双指针？【浪漫相遇】

### 【方法1：hashSet】

- 通过set存储节点引用（注意不是val）
- 通过一次遍历找到第一个公共节点
- 时间复杂度：o（n）
- 空间复杂度：o（n）

```java
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        //haset找出重复的第一个公共节点（存的是节点引用，而不是val）
        ListNode tempA = headA;
        ListNode tempB = headB;
        Set<ListNode> set = new HashSet<>();
        while(tempA != null){
            set.add(tempA);
            tempA = tempA.next;
        }
        while(tempB != null){
            if(set.contains(tempB)){
                return tempB;
            }
            tempB = tempB.next;
        }
        return null;
    }
}
```

### 【方法2：双指针：浪漫相遇】

- 你变成我，走过我走过的路。
  我变成你，走过你走过的路。
  然后我们便相遇了.
- 人话：
  - 两个节点数总和一样，一快一慢（长短不同）总会套圈，相同速度，第一圈就会相遇

![image-20211009091947965](剑指Offer .assets/image-20211009091947965.png)

```java
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        //双指针：浪漫相遇
        if(headA == null && headB == null){
            return null;
        }
        ListNode pA = headA;
        ListNode pB = headB;
        while(pA != pB){
            pA = (pA == null) ? headB : pA.next;
            pB = (pB == null) ? headA : pB.next;
        }
        return pA;
    }
}
```

## 总结&思路

- 常规思路：利用hashset来存储节点的引用，从而找出第一公共节点
- 骚操作：双指针浪漫相遇

# -------------------------------------------

# 十三、双指针（简单）

# 剑指 Offer 21.  调整数组顺序使奇数位于偶数前面

![image-20211009092707715](剑指Offer .assets/image-20211009092707715.png)

## 思路&方法

- 双指针：类快排方法

### 【方法1：双指针】

- 1.头尾双指针，分别往中间遍历
- 2.找出左边第一个偶数位，找出右边第一个奇数位
- 3.交换两位 

```java
class Solution {
    public int[] exchange(int[] nums) {
        //双指针法
        //思路：
        //1.用两个指针，分别从头尾开始往中间遍历
        //2.找到左边第一个偶数位，找到右边第一个奇数位，并进行互换
        //3.直到左右相等
        int left = 0;
        int right = nums.length - 1;
        while(left < right){
            while(nums[right] % 2 == 0 && left < right){
                //奇数过
                right--;
            }
            while(nums[left] % 2 == 1 && left < right){
                //偶数过
                left++;
            }
            int temp = nums[left];
            nums[left] = nums[right];
            nums[right] = temp;
        }
        return nums;
    }
}
```

## 总结&思路

- 类似于快排的做法

# 剑指 Offer 57. 和为s的两个数字

![image-20211018222951275](剑指Offer .assets/image-20211018222951275.png)

## 思路&方法

- 双指针，快排的思想还是很常用的！

### 【方法1：双指针】

- 1.头尾双指针，往中间遍历数组
- 2.如果nums[left] == target - nums[right]，则返回两数【用减法，为了防止相加溢出】
- 3.如果target - nums[right] > nums[left]，则left++
- 4.如果target - nums[left] < nums[right]，则right--

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        //双指针
        //思路：
        //1.先遍历一次数组，找到比target小的最后一个下标==多此一举
        //2.头尾开始往中间遍历：
        //3.如果left + right == target，则返回left
        //4.如果target - right > left，则left++
        //5.如果target - left < right，则right--
        int left = 0;
        int right = nums.length - 1;
        int[] res = new int[2];
       
        while(left < right){
            if(nums[left] + nums[right] == target){
                res[0] = nums[left];
                res[1] = nums[right];
                break;
            }
            if(target - nums[right] > nums[left]){
                left++;
            }
            if(target - nums[left] < nums[right]){
                right--;
            }
        }
        return res;   
    }
}
```

## 总结&思路

- 双指针的天花板应该就是快排的思想了

# 剑指 Offer 58-I. 翻转单词顺序 

![image-20211019214655070](剑指Offer .assets/image-20211019214655070.png)

## 思路&方法

- 双指针

### 【方法1：双指针】

- 思路：
  - 1.设置两个末位指针，作为扫描单次的滑动窗口左右下标，左指针去找到前面最近的一个空格
  - 2.找到下标后，通过substring的方式append到新的StringBuilder中去
  - 3.更新两个指针都为左指针的下标【去掉重复的空格】
  - 4.重复上述操作，直到左指针==0
- 时间复杂度：o(n)
- 空间复杂度：o(n)

```java
class Solution {
    public String reverseWords(String s) {
        //双指针
        //思路：
        //1.设置两个末位指针，一个作为尾，一个作为头，去找到前面最近的一个空格
        //2.找到这两个下标，然后通过subString的方式append到新的StringBuilder中去
        //3.更新两个指针都为头指针下标【要找到下一个非空位】，重复上述操作，直到头尾下标==0
        int end = s.length() - 1;
        
        StringBuilder sb = new StringBuilder();
        //去尾的空格
        while(end > 0 && s.charAt(end) == ' '){
            end--;
        }
        int sentinel = end;
        while(sentinel >= 0){
            while(sentinel >= 0 && s.charAt(sentinel) != ' '){
                sentinel--;
            }
            //找到了最近的一个空格
            sb.append(s.substring(sentinel + 1, end + 1));
            while(sentinel >= 0 && s.charAt(sentinel) == ' '){
                sentinel--;
            }
            //找到最近一个不是空格的字符
            end = sentinel;
            //去头的空格
            if(end != -1){
                sb.append(" ");
            }
        }

        return sb.toString();
    }
}
```

## 总结&注意

- 总结：
  - 双指针还可以作为滑动窗口，来实现字符串的一些扫描操作
- 注意：
  - 1.end > 0 && s.charAt(end) == ' '的作用：前面是为了短路后面的判断，防止index=-1而向下溢出
  - 2.字符串.substring：左闭右开

# -------------------------------------------

# 十四、搜索与回溯算法（中等）

# 剑指 Offer 12. 矩阵中的路径

![image-20211020230106827](剑指Offer .assets/image-20211020230106827.png)

# -------------------------------------------

# 十五、搜索与回溯算法（中等）

# 剑指 Offer 34. 二叉树中和为某一值的路径

![image-20211019215958594](剑指Offer .assets/image-20211019215958594.png)

# 思路&方法



## 【方法1：回溯法（先序遍历=DFS）】

- 利用先序遍历的方法，先实现遍历**==【所有的树的题目，要先搭好遍历的框架：先序 or 中序 or DFS or HFS】==**

- 先序遍历的框架 

```java
class Solution {
    LinkedList<List<Integer>> res = new LinkedList<>();
    LinkedList<Integer> path = new LinkedList<>(); 

    public List<List<Integer>> pathSum(TreeNode root, int target) {
        recur(root,target);
        return res;
    }
    void recur(TreeNode root, int target){
        if(root == null){
            return ;
        }
        //先序遍历的操作
        xianxuFun(root);
        recur(root.left, target);
        recur(root.right, target);
    }
}
```

- 在框架上实现路径的记录
  - 先序操作：
    - 每次在path路径上加入该节点
    - 更新target，减去该节点的值
  - 终止条件：
    - 当遍历到叶子节点时&&target==0，此时将path复制快照，并且加入到res中
  - 左右堆栈
  - 删除该节点：
    - 在该节点向上回溯时，要从path中删除，为了其他分支能用到这个path

```java
class Solution {
    LinkedList<List<Integer>> res = new LinkedList<>();
    LinkedList<Integer> path = new LinkedList<>(); 

    public List<List<Integer>> pathSum(TreeNode root, int target) {
        recur(root,target);
        return res;
    }
    void recur(TreeNode root, int target){
        if(root == null){
            return ;
        }
        //先序遍历，先加入到path中
        path.add(root.val);
        target -= root.val;
        if(target == 0 && root.left == null && root.right == null){
            //每有一个叶子节点符合条件，就拷贝这个path，防止后面add和pop破坏正确的path
            //复制一个快照来保存正确的path
            res.add(new LinkedList(path));
        }
        recur(root.left, target);
        recur(root.right, target);
        //为了重复利用每一条path，回溯前需要删除该节点
        //例如：7用完得删掉，才能加2
        path.removeLast();
    }
}
```



- 自己的思路，思路是正确的的，但还不够完善

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    List<Integer> element = null;

    public List<List<Integer>> pathSum(TreeNode root, int target) {
        //返回空的树
        if(root == null){
            return res;
        }
        if(root.left == null && root.right == null && root.val == target){
            element = new ArrayList<>();
            element.add(root.val);
            res.add(element);
            return res;
        }
        if(root.left == null && root.right == null && root.val != target){
            return res;
        }
        if(root.left != null){
            ispathSum(root.left,target - root.val);
        }
        if(root.right != null){
            ispathSum(root.right,target - root.val); 
        }
        
        for(List<Integer> list : res){
            list.add(root.val);
            Collections.reverse(list);
        }
        
        return res;


    }
    int isleft = 0;
    int isright = 0;
    public int ispathSum(TreeNode root, int target){
        if(root.left == null && root.right == null && root.val == target){
            //遍历到叶节点，并且叶节点等于相减后最后的target
            element = new ArrayList<>();
            element.add(root.val);
            res.add(element);
            return 1;
        }
        if(root.left == null && root.right == null && root.val != target){
            //不是target路径
            return 0;
        }
        if(root.left != null){
            isleft = ispathSum(root.left,target - root.val);
        }
        if(root.right != null){
            isright = ispathSum(root.right,target - root.val);    
        }
        
        if(isleft == 1 || isright == 1){
            element.add(root.val);
            return 1;
        }
        return 0;
    }
}
```

## 总结&注意

- 总结：
  - 1.涉及到二叉树的遍历时，一般都是DFS（先序遍历），因此可以先搭好遍历框架
  - 2.DFS（先序遍历）：
    - 由于设计到路径公用，所以，用完该节点并向上回溯时，需要把该节点从路径中删除
  - 3.本题中的路径由于在回溯的过程中会经历删除节点，所以在叶子节点添加到res的时候，需要复制一个快照进行添加，避免出现回溯到根节点时的path是[]空的情况
- 注意：
  - **==做二叉树遍历相关的题目，先不要着急搅进去。先搭好遍历的框架【一般为先序遍历（DFS） 】==**
