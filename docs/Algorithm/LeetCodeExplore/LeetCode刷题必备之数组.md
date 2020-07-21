[TOC]

## 1. 前言

作为重要的线性数据结构， 我们i经常会跟数组打交道，而对数组的增删改查则是日常用到的操作。为了弄清楚这些常用操作，此博客则对这些操作进行一一梳理；

在此之前，先介绍一下数组容量和长度；

-   容量：指当前数组最多能容纳的元素个数；
-   长度：指当前数组中的元素个数；

```java
int[] arr = new int[10];
length = 0;
for(int i = 0; i < 5; i++){
    arr[length++] = i + 1;
}

System.out.println(“数组容量： ” + arr.length)；
System.out.println(“数组长度： ” + length；

```



## 2. 插入

### 2.1 插入元素到数组开头

要将元素插入数组开头位置，相当与同时将原来数组的元素整体向后移动一位；

```java
/**
* 插入元素到数组开头
* @param arr 待插入元素的数组
* @param val 待插入的元素
* @return 插入元素后的数组
*/
public int[] insertStart(int[] arr, int val){
    // 用于存放插入元素后的数据
    int[] destArr = new int[arr.length + 1];
    
    // 将元素插入新数组开头，同时将原数组整体赋值给新数组
    destArr[0] = val;
    for(int i = 0; i < arr.length; i++){
        destArr[i + 1] = arr[i];
    }
    
    return destArr;
}
```



### 2.2 插入元素到数组结尾

要将元素插入到数组结尾，直接赋值给数组尾部即可；

```java
/**
* 插入元素到数组开头
* @param arr 待插入元素的数组
* @param val 待插入的元素
* @return 插入元素后的数组
*/
public int[] insertEnd(int[] arr, int val){
    // 用于存放插入元素后的数据
    int[] destArr = new int[arr.length + 1];
    
    // 将元素插入新数组结尾，同时将原数组整体赋值给新数组
    destArr[arr.length] = val;
    for(int i = 0; i < arr.length; i++){
        destArr[i] = arr[i];
    }
    
    return destArr;
}
```



### 2.3 插入元素到数组任意位置

插入元素到任意位置，相当于只要把数组中插入位置后边的元素整体向后移动一位即可；

````java
/**
* 插入元素到数组任意位置
* @param arr 待插入元素的数组
* @param val 待插入的元素
* @param index 待插入元素的索引位置
* @return 插入元素后的数组
*/
public int[] insertAnyWhere(int[] arr, int index, int val){
    // 用于存放插入元素后的数据
    int[] destArr = new int[arr.length + 1];
    
    // 将元素插入新数组指定位置
    destArr[index] = val;
    // 将原数组插入元素位置前半段赋值给新数组
    for(int i = 0; i < index; i++){
        destArr[i] = arr[i];
    }
    // 将原数组插入元素位置后半段赋值给新数组
    for(int j = index; j < arr.length; j++){
        destArr[j + 1] = arr[j];
    }
    
    return destArr;
}
````



## 3. 删除

### 3.1 删除数组开头元素

删除开头元素，相当与将后边的元素整体向前移动一位；

```java
/**
* 删除数组开头元素
* @param arr 待删除元素的数组
* @return 删除元素后的数组
*/

public int[] deleteStart(int[] arr){
    // 删除元素后，数组容量减小
    int destArr = new int[arr.length - 1];
    
    // 删除开头元素，相当与将后边的元素整体向前移动一位
    for(int i = 1; i < arr.length; i++){
        destArr[i - 1] = arr[i];
    }
    
    return destArr;
}
```



### 3.2 删除数组末尾元素

直接将数组末尾元素删除即可；

```java
/**
* 删除数组末尾元素
* @param arr 待删除元素的数组
* @return 删除元素后的数组
*/

public int[] deleteEnd(int[] arr){
    // 删除元素后，数组容量减小
    int destArr = new int[arr.length - 1];
    
    // 删除最后一个元素，相当于不去管最后一个元素
    for(int i = 0; i < arr.length - 1; i++){
        destArr[i] = arr[i];
    }
    
    return destArr;
}
```



### 3.3 删除数组任意位置元素

删除任意位置元素，相当于不动删除位置前的元素，然后将删除元素后的u元素整体向前移动一位；

```java
/**
* 删除数组任意位置元素
* @param arr 待删除元素的数组
* @param index 带删除元素索引位置
* @return 删除元素后的数组
*/

public int[] deleteEnd(int[] arr, int index){
    // 删除元素后，数组容量减小
    int destArr = new int[arr.length - 1];
    
    // 删除任意位置元素，前半段保持
    for(int i = 0; i < index; i++){
        destArr[i] = arr[i];
    }
    
    // 后半段整体向前移动一位
    for(int j = index; j < arr.length - 1; j++){
        destArr[j] = arr[j + 1];
    }
    
    return destArr;
}
```

## 4. 查找

### 4.1 线性查找

线性查找即遍历数组，然后判断各元素是否是目标值，是则输出对应索引位置，否则返回 -1，时间复杂度为 $O(n)$；

```java
/**
* 线性查找
* @param array 
* @param target 要查找的目标值
* @return -1 说明未找到目标值，否则返回目标值索引位置
*/

public int linearSearch(int[] array, int target) {
    
    // 查找到目标值时，返回目标之索引位置
    for(int i = 0; i < array.length; i++){
    	if(target == array[i]){
            reurn i;
        }    
    }
        
    return -1;
}
```

### 4.2 二分查找

当数组长度很小时，使用线性查找方法很快就能找到目标值是否存在并返回对应索引位置，但当数组很大时，线性查找的方法效率就太低了。这时候二分查找是更理想的查找手段，二分查找实质是使用双指针，每次对半查找，大大提高效率，时间复杂度为 $O(logn)$；

```java
/**
* 二分查找
* @param array 
* @param target 要查找的目标值
* @return -1 说明未找到目标值，否则返回目标值索引位置
*/

public int binarySearch(int[] array, int target) {
    // 左右指针
    int left = 0; 
    int right = array.length - 1; 

    while(left <= right) {
        int mid = left + (right - left) / 2;
        // 当前值等于目标值时，返回当前索引位置
        if(array[mid] == target){
            return mid; 
        } else if (array[mid] < target){ // 当前值小于目标值，左指针向右移动一位
            left = mid + 1; 
        } else { // 当前值大于目标值，右指针向左移动一位
            right = mid - 1;
        }            
    }
    return -1;
}
```

![](https://gitee.com/cunyu1943/images/raw/master/ImgsUbuntu/20200510234310.png)

---
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/social-share.js/1.0.16/css/share.min.css">
<center><div class="social-share"></div></center>
<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/social-share.js/1.0.16/js/social-share.min.js"></script>