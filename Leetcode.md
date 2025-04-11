

#   														数组与字符串

## 寻找数组中心下标

> 给你一个整数数组 `nums` ，请计算数组的 中心下标 。
>
> 数组 中心下标 是数组的一个下标，其左侧所有元素相加的和等于右侧所有元素相加的和。
>
> 如果中心下标位于数组最左端，那么左侧数之和视为 `0` ，因为在下标的左侧不存在元素。这一点对于中心下标位于数组最右端同样适用。
>
> 如果数组有多个中心下标，应该返回 最靠近左边 的那一个。如果数组不存在中心下标，返回 `-1` 。
>

```java
public int pivotIndex(int[] nums) {
        int sum_all = 0;
        for(int i = 0 ; i < nums.length; i++){
            sum_all = sum_all + nums[i];  
        }
        int sum = 0;
        for (int i = 0; i < nums.length; i++){
            if(sum_all - sum - nums[i] == sum){
                return i;
            }
            sum = sum + nums[i];
        }
        return -1;
    }
```



- 思路：

  先计算数组总和 `sum_all`。

  再遍历数组，`sum`变量存储左边总和，则右边总和为:`sum_all - sum - nums[i]`

## 搜索插入位置

> 给定一个排序数组和一个目标值，在数组中找到目标值，并返回其索引。如果目标值不存在于数组中，返回它将会被按顺序插入的位置。
>
> 请必须使用时间复杂度为 `O(log n)` 的算法。

```java
public int searchInsert(int[] nums, int target) {
        int start = 0;
        int end = nums.length - 1;
        int mid = 0;   
        while(start <= end){
            mid = (start + end) / 2;
            if(target < nums[mid]){
                end = mid - 1;
            }
            else if(target > nums[mid]){
                start = mid + 1;
            }
            else{
                return mid;
            }
        }
        return start;
    }
```

- 为什么取等号:**每次进入循环**中并不能保证 `nums[start] < target < nums[end]`,因为 `start` 与 `end` 是通过 `mid + 1` `mid - 1`得到的，只能保证与 上一个`nums[mid]`的大小关系。

## 排序算法

总结：

- ![image-20230721173710784](https://s2.loli.net/2023/07/21/mZla5D7B6qWypPf.png)

- 算法复杂度与特性

  - | 排序方法 | 时间复杂度(平均) | 时间复杂度(最坏) | 时间复杂度(最好) | 空间复杂度 | 稳定性 |
    | -------- | ---------------- | ---------------- | ---------------- | ---------- | ------ |
    | 冒泡排序 | O(n^2^)          | O(n^2^)          | O(n)             | O(1)       | 稳定   |
    | 选择排序 | O(n^2^)          | O(n^2^)          | O(n^2^)          | O(1)       | 不稳定 |
    | 插入排序 | O(n^2^)          | O(n^2^)          | O(n)             | O(1)       | 稳定   |
    | 希尔排序 | O(nlogn)         | O(n^2^)          | O(n)             | O(1)       | 不稳定 |
    | 归并排序 | O(nlogn)         | O(nlogn)         | O(nlogn)         | O(n)       | 稳定   |
    | 快速排序 | O(nlogn)         | O(n^2^)          | O(nlogn)         | O(logn)    | 不稳定 |
    | 堆排序   | O(nlogn)         | O(nlogn)         | O(nlogn)         | O(1)       | 不稳定 |
    | 计数排序 | O(n+k)           | O(n+k)           | O(n+k)           | O(n+k)     | 稳定   |
    | 桶排序   | O(n+k)           | O(n^2^)          | O(n+k)           | O(n+k)     | 稳定   |
    | 基数排序 | O(n*k)           | O(n*k)           | O(n*k)           | O(n+k)     | 稳定   |
    
  

### 冒泡排序

- 按从小到大排序

- 比较相邻两个数大小，后数小于前数则交换。
- 外层循环n-1次，每次确定一个最大数的位置。
- 内层循环n-1-i次，冒泡次数，确定第i大的数的位置。
- O(N^2^),O(1),**稳定**。

```java
public static int[] solution(int[] nums) {
        int n = nums.length;
        for( int i = 0; i < n-1;i++){
            for( int j = 0; j < n-1-i; j++ ){
                if(nums[j+1] < nums[j]){
                    int temp = nums[j+1];
                    nums[j+1] = nums[j];
                    nums[j] = temp;
                }
            }
        }
        return nums;                                
    }   
```

### 选择排序

- 从未选择的的元素种选择最小(大)的元素，按顺序加入到结果数组中:进行交换操作。

- 重复操作。

- 采用in-place算法，不申请额外数组空间。

- 从第一个数开始选择一个最小的数，使得前i个数有序。

- 每次记录第i大数的index，与当前位置数进行交换。

- O(N^2^),O(1)，**不稳定**

  ```java
  public static int[] solution(int[] nums) {
          int temp = 0;
          int minIndex = 0;
          for( int i = 0; i<nums.length-1;i++){
              minIndex = i;
              for( int j = i+1;j<nums.length;j++){   
                  if(nums[j] < nums[minIndex]){
                      minIndex = j;
                  }
              }
              temp = nums[i];
              nums[i] = nums[minIndex];
              nums[minIndex] = temp;
          }
          return nums;                                 
      }
  ```



### 插入排序

- 采用in-place方法，不申请额外的空间。
- 从第一位数开始，认为前面的数都按顺序排列好。
- 一次选择后面第i个数字，从i-1往前扫描，边扫描一边移动数组，直到找到对应数字位置插入。
- O(N^2^),O(1)，**稳定**

```java
 public static int[] solution(int[] nums) {
        int n = nums.length;
        int index = 0;
        int temp = 0;
        
        for( int i = 1; i < n;i++){
            temp = nums[i];
            index = i-1;
            while(index>=0 && temp < nums[index]){
                nums[index+1] = nums[index]; //往后移动数组
                index = index - 1;
            }
            // 遇到插入位置或者扫描完成。因为移动index后面一位是空位置。
            nums[index+1] = temp;
        }
        return nums;
    }
```





### **希尔排序**

- 先将序列分为组内元素间隔为gap的几组，再分别在每组中进行插入排序。
- 减小gap直到gap为1，gap即为组数。
- O(N*log(N)),O(1)，不稳定

```java
 public static int[] solution(int[] nums) {
        int n = nums.length;
        int gap = 0;
        for(gap = n/ 2; gap>=1; gap=gap/2){
            for(int i = gap;i<n;i++){ //每组第二个数开始。
                int current = nums[i]; //取出当前数
                int index = i - gap;
                while(index>=0 && current<nums[index]){
                    nums[index + gap] = nums[index]; //移动每组中的数找到插入位置
                    index = index - gap;
                }
                nums[index + gap] = current;

            }
        }
        return nums;
                                  
    }
```



### **归并排序**

- 将数组进行分组，从最小的组开始，即只有一个元素的组。

- 将每组进行合并，合并时保持有序。合并的两个组分别有序。

- 递归与非递归两种方式。

- 0(N*log(N)),O(N),稳定

  ```java
   public static int[] mergeSort(int[] nums,int left,int right) {
          if(left < right){
              int mid = (left + right) / 2;
              mergeSort(nums, left, mid);
              mergeSort(nums, mid+1, right);
              mergeTwoslice(nums, left, mid, right);
          }
          return nums;                 
      }
  
  public static int[] mergeSort_NoRecur(int[] nums) {
      for(int slice = 1; slice < nums.length; slice = slice * 2){
           for(int left = 0; left<nums.length;left = left+2*slice){
               int mid = Math.min(left + slice - 1,nums.length - 1); // mid == nums.length - 1 时，right = nums.length - 1,表明只有一个slice,即保持该slice原样。
               int right = Math.min(left + 2*slice -1 ,nums.length -1); // right == nums.length - 1 时,第二个slice不完整，为[mid+1,nums.length-1],将该slice进行合并。
                 mergeTwoslice(nums, left, mid, right);
              }
          }    
       	return nums;                                 
      }
  
      public static int[] mergeTwoslice(int[]nums, int left, int mid,int right) {
          int[] merged = new int[right - left + 1];
          int i = left;
          int j = mid + 1;
          int k = 0;
          while(i <= mid && j <= right){
              if(nums[i] < nums[j]){
                  merged[k] = nums[i];
                  k = k + 1 ;
                  i = i + 1;
              }
              else{
                  merged[k] = nums[j];
                  k = k + 1;
                  j = j + 1;
              }
          }
          while(i<=mid){
              merged[k] = nums[i];
              k = k + 1;
              i = i + 1;
          }
          while( j <= right){
              merged[k] = nums[j];
              k = k + 1;
              j = j + 1;
          }
  
          k = 0;
          while(left <= right){
              nums[left] = merged[k];
              left = left + 1;
              k = k + 1;
          }
          return nums;                                     
      }
  ```

  

### **快速排序**

- 每一次找到一个数称为pivot基准数在数组中的位置（进行排序)使得左边的数<=pivot，右边的数>=pivot。返回其位置index。
- 递归快排quicksort(left,index-1)
- 递归快排quicksort(index+1,right)
- O(N*log(N)),O(log(N))，不稳定

```java
public static int[] quickSort(int[] nums,int left,int right) {
        if(left < right){
            int pivot_index = partition(nums,left,right);
            quickSort(nums, left, pivot_index - 1);
            quickSort(nums, pivot_index + 1, right);
        }
        return nums;
                                        
    }

    public static int partition(int[] nums,int left,int right) {
        int pivot = nums[left];
        int i = left;
        int j = right;
        // 抽取了left，i位置为空，从right开始扫描交换。
        while( i < j){
            while( i < j && nums[j] >= pivot){
                j = j - 1;
            }
            if( i < j){
                nums[i] = nums[j];
                i = i + 1;
            }

            //交换后j位置为空，从left开始扫描交换。
            while( i < j && nums[i] <= pivot){
                i = i + 1;
            }
            if(i < j){
                nums[j] = nums[i];
                j = j - 1; 
            }
        }
        nums[i] = pivot;
        return i;

        //循环后 i一定等于j吗？
    }
```



### **堆排序**

- 完全二叉树

  > 除了最后一层其他每一层节数都达到最大值，最后一层节点从左到右连续。
  >
  > ![image-20230728171539255](https://s2.loli.net/2023/07/28/fumiYN65pUCIMJF.png)

- 堆

  > 如果完全二叉树中的每个父节点的值都大于等于或者小于等于 左右子节点，则为堆。
  >
  > ![image-20230728171952524](https://s2.loli.net/2023/07/28/lgpQGDC4t1ejEm8.png)

- 用数组存放堆

  - ![image-20230728172031699](https://s2.loli.net/2023/07/28/iSV8eTuFC3DEcmK.png)

  - 下标为k的跟，左孩子为2k，有孩子为2k+1。下表为k则父节点为 k/2。

- 插入堆元素:上浮操作：小根堆

  - 新元素插入到数组末尾，也就是堆尾。

  - 比较父节点，小于父节点则进行交换上浮，直到大于父节点。

  - ```java
    public static void swap(int[] nums,int i,int j) {
            int temp = nums[i];
            nums[i] = nums[j];
            nums[j] = temp;                                   
        }
                                         
        public static void siwm(int[]nums,int insert) {
            int parent = (insert-1) / 2;
            while(parent >= 0 && nums[parent] > nums[insert]){
                swap(nums, parent, insert);
                insert = parent;
                parent = (insert-1) / 2;
            }
                                            
        }
        
        public static void swim_recru(int[] nums,int insert) {
            int parent = (insert-1) / 2;
            if(nums[parent] > nums[insert]){
                swap(nums, parent, insert);
                swim_recru(nums, parent);
            }
        }
    
    ```

- 删除堆元素:下沉

  - 交换堆顶与堆底元素，下沉堆顶。

  - 比较左右子节点，找到最小的元素，如果最小元素为子节点，进行交换。知道最小元素为当前跟节点。

  - 用 `heapSize`记录当前堆的大小:如果下标为0开始的堆，则为堆大小-1.

  - ```java
    public static void sink(int[]nums,int parent,int heapSize) {
            int leftchild = parent * 2 + 1;
            int rightchild = parent * 2 + 2;
            int minIndex = parent;
            if(leftchild <= heapSize && nums[leftchild] < nums[minIndex]){
                minIndex = leftchiled;
            }
            else if(rightchild <= heapSize && nums[rightchild] < nums[minIndex]){
                minIndex = rightchild;
            }
    
            if(minIndex != parent){
                swap(nums, parent, minIndex);
                sink(nums, minIndex, heapSize);
            }
        }
    ```

- 堆排序

  - 从小到大排序使用大根堆，每次删除根节点即将根节点数与数组最后一个数交换然后下沉。
  - 创建堆:从倒数第一个非叶子节点开始下沉。
  - 交换，下沉调整堆。
  - O(log(N)),O(1),不稳定
  
  ```java
  public static int[] heap_sort(int[] nums) {
          // 构建大顶堆,从最后一个非叶子节点开始下沉
          for(int i = (nums.length - 2 ) / 2; i>=0;i--){
              sinkRecru(nums, i, nums.length - 1);
          }
  
          for(int i = nums.length - 1; i>=0; i--){
              // 交换堆顶和最后一个元素,即将最大值放到最后
              int temp = nums[0];
              nums[0] = nums[i];
              nums[i] = temp;
              sinkRecru(nums, 0, i-1);
          }
          return nums;                                     
      }
  
      // 从上往下调整
      public static int[] sink(int[] nums,int parent,int heapSize) {
          int temp = nums[parent];
          int maxChild = parent * 2 + 1;
          while(maxChild <= heapSize){
              if(maxChild + 1 <= heapSize && nums[maxChild + 1] >= nums[maxChild]){
                  maxChild = maxChild + 1;
              }
              if(nums[maxChild] <= temp){
                  break;
              }
              // 最大值在子节点
              nums[parent] = nums[maxChild];
              parent = maxChild;
              maxChild = parent * 2 + 1;
          }
          nums[parent] = temp;     
          return nums;                
      }
  
      public static int[] sinkRecru(int[] nums,int parent,int heapSize) {
          int leftChild = parent * 2 + 1;
          int rightChild = parent * 2 + 2;
          int maxIndex = parent;
  
          if(leftChild <= heapSize && nums[leftChild] > nums[maxIndex] ){
              maxIndex = leftChild;
          }
          if(rightChild <= heapSize && nums[rightChild] >nums[maxIndex]){
              maxIndex = rightChild;
          }
  
          // 递归下沉
          if(maxIndex != parent){
              int temp = nums[parent];
              nums[parent] = nums[maxIndex];
              nums[maxIndex] = temp;
              sinkRecru(nums, maxIndex, heapSize);
          }
          return nums;                            
      }   
  ```



### 计数排序

- 适合最大值与最小值相差不大的数组。

- 将数组元素作为下标进行统计元素出现的次数，计数数组大小为`max-min+1`。

- 将最小值min作为偏移量，即`nums[i] - min`为数组下标记录元素出现的次数。

- O(N+K),O(N+K),稳定

  ```java
  public static int[] countSort(int[] nums) {
          if(nums.length == 1 || nums == null){
              return nums;
          }
          int min = nums[0];
          int max = nums[0];
          for(int i = 1;i<nums.length;i++){
              if(nums[i] > max){
                  max = nums[i];
              }
              if(nums[i] < min){
                  min = nums[i];
              }
          }
          
          int n = max - min + 1;
          int[] countNums = new int[n];
  
          for(int i = 0 ; i < nums.length ; i ++){
              countNums[nums[i] - min] = countNums[nums[i] - min] + 1;
          }
  
          int k = 0;
          for(int i = 0;i<n;i++){
              for( int j = countNums[i];j>0;j--){
                  nums[k] = i + min;
                  k = k + 1;
              }
          }
          return nums;
      }
  ```



### 桶排序

- 将数组进行拆分，与计数排序类似将`min`作为偏移量。将`max-min`作为整个区间。
- 将区间分成不同的桶，设置`bucketSize`大小，默认为5，则桶的个数为 `(max-min)/bucketSize + 1`

- 桶定义为：`ArrayList<LinkedList<Integer>> bucket = new **ArrayList**<>(bucketNum);`，其中桶也可以为数组：`int[bucketNum]`

- 将每个桶进行排序，可以用内置排序算法。

- 不同区间桶进行合并到原数组。

- O(N+K),O(N+K),稳定

  ```java
  public static int[] bucketSort(int [] nums) {
          int n = nums.length;
          int min = nums[0];
          int max = nums[0];
          int bucketSize = 5;
  
          for(int i = 0; i<n;i++){
              if(nums[i] > max){
                  max = nums[i];
              }
              if(nums[i] < min){
                  min = nums[i];
              }
          }
          // 加一防止有余数。
          int bucketNum = (max - min) / bucketSize + 1;
          ArrayList<LinkedList<Integer>> bucket = new ArrayList<>(bucketNum);
          for(int i = 0; i < bucketNum;i++){
              bucket.add(new LinkedList<>());
          }
  
          // 根据数字大小减去偏移量/buckSize定位哪个桶并添加到该桶中。
          for( int i = 0; i<n;i++){ 
              bucket.get( (nums[i] - min)/bucketSize ).add(nums[i] - min);
          }
  
          // 每个桶排序。
          for(int i = 0; i<bucketNum;i++){
              Collections.sort(bucket.get(i));
          }
  
          // 合并到原数组。
          int k = 0;
          for(int i =0;i<bucketNum;i++){
              for(Integer element:bucket.get(i)){
                  nums[k] = element + min;
                  k = k + 1;
              }
          }
          return nums;
                                          
      }
  ```

  

### 基数排序

- 将`min`作为偏移量。将`max-min`作为整个区间。

- 计算最大的位数:`(max - min)`的位数。

- 设置10个桶：`ArrayList<LinkedList<Integer>> bucket = new **ArrayList**<>(10);`

- 按照最低位数字开始排序，即从各位到最高位进行排序。获取每个位数的数字：` int base = (int) Math.**pow**(10, i-1); int lastBase = (nums[j] - min) / base % 10;`

- 每排序完一位，复制到源数组，将每个桶清除。

- O(N*K),O(N+K)，稳定

  ```java
  public static int[] baseSort(int[] nums) {
          int n = nums.length;
          int min = nums[0];
          int max = nums[0];
          for( int i = 0; i < n;i++){
              if(nums[i] > max){
                  max = nums[i];
              }
              if(nums[i] < min){
                  min = nums[i];
              }
          }
  
          int maxNum = max - min;
          int digit = 1;
          while(maxNum / 10 > 0){
              digit = digit + 1;
              maxNum = maxNum / 10;
          }
          
          //初始化筒
          ArrayList<LinkedList<Integer>> bucket = new ArrayList<>(10);
          for( int i = 0; i< 10;i++){
              bucket.add(new LinkedList<Integer>());
          }
  
  
          for(int i = 1;i<=digit;i++){
              for(int j = 0; j<n;j++){
                  int base = (int) Math.pow(10, i-1);
                  int lastDigit = (nums[j] - min) / base % 10; //倒数第i位数
                  bucket.get(lastDigit).add(nums[j] - min);
              }
  
              int k = 0;
              for(int j = 0; j<10;j++){
                  for(Integer element:bucket.get(j)){
                      nums[k] = element + min;
                      k = k + 1;
                  }
                  bucket.get(j).clear();
              }
  
          }
          return nums;                                     
      }
  ```

  

## 1. 合并区间

> 以数组 `intervals` 表示若干个区间的集合，其中单个区间为 `intervals[i] = [starti, endi]` 。请你合并所有重叠的区间，并返回 一个不重叠的区间数组，该数组需恰好覆盖输入中的所有区间 。

- 思路

  - 首先将intervals数组按照左值进行排序。` Arrays.sort(intervals, (a,b) -> Integer.compare(a[0],b[0]));`
  - 挨个合并`intervals`中的区间：当前的左值<= `temp`左值进行合并
    - `temp`右值大，合并区间为 `temp`
    - 当前右值大，合并区间右值为当前右值。

  ```java
   public int[][] merge(int[][] intervals) {
          // 使用lambda函数重写compare方法
          Arrays.sort(intervals, (a,b) -> Integer.compare(a[0],b[0]));
          ArrayList<int[]> res = new ArrayList<>();
          int i = 0;
          int j = 0;
          int [] temp = new int[2];
          while(i<intervals.length){
              temp = intervals[i].clone();
              j = i+1;
              while(j<intervals.length && intervals[j][0] <= temp[1]){
                  if(temp[1] < intervals[j][1]){
                      temp[1] = intervals[j][1];
                  }
                  j = j + 1;
              }
              res.add(temp);
              i = j;
          }
  
          return res.toArray(new int[res.size()][2]);
      }
  ```



# 二维数组

## 2. 二维数组旋转90度

> 给你一幅由 `N × N` 矩阵表示的图像，其中每个像素的大小为 4 字节。请你设计一种算法，将图像旋转 90 度。不占用额外内存空间能否做到？

### 思路一

- 针对每个点的旋转公式为,当前坐标为 `[i,j]`，则目标坐标为 `[j,n-1-i]`。

- 循环顺序为，从最外层的正方形外壳开始循环。然后依次缩小外壳：N -> N-2*i 

  ```java
  public static void rotate_matrix(int[][] matrix) {
          int n = matrix.length;
          // 外层第几层进行旋转
          for(int i = 0;n-2*i>0;i++){
              // 内层进行外壳旋转。
              for(int j = i;j<n-i-1;j++){
                  int target_i = j;
                  int target_j = n-1-i;
  
                  int current = matrix[i][j];
                  int target = matrix[target_i][target_j];
                  
                  // 注意循环终止条件，即当两个都相等时才终止，即旋转到初始点。但是此时初始点数字未更新，须在循环外更新。
                  while(target_i != i || target_j != j){
                      matrix[target_i][target_j] = current;
                      int c = target_j;
                      target_j = n-1-target_i;
                      target_i = c;
                      current = target;
                      target = matrix[target_i][target_j];
                      
                  }
                  matrix[i][j] = current;
              }
  
          }                                  
      }
  ```

### 思路二

- 针对每个点的旋转公式为,当前坐标为 `[i,j]`，则目标坐标为 `[j,n-1-i]`。没旋转四次到原地，即一个循环。`[i,j]`->`[j,n-1-i]`->`[n-1-i,n-1-j]`->`[n-1-j,i]`->`[i,j]`

- 循环顺序为，从最外层的正方形外壳开始循环。然后依次缩小外壳：N -> N-2*i 

  ```java
  public static void rotate_matrix(int[][] matrix) {
          int n  = matrix.length;
          int temp;
  
          for(int i = 0; n-2*i>0;i++){
              for(int j = i; j< n- 1 - i;j++){
                  temp = matrix[i][j];
                  matrix[i][j] = matrix[n-1-j][i];
                  matrix[n-1-j][i] = matrix[n-1-i][n-1-j];
                  matrix[n-1-i][n-1-j] = matrix[j][n-1-i];
                  matrix[j][n-1-i] = temp;
              }
          }
                                          
      }
  ```



### 思路三

- 先进行水平翻转：`[i,j]` <-> `[n-1-i,j]`,行遍历一半。

- 在进行对角翻转: `[i,j]` <-> `[j,i]`, 遍历倒三角。

  ```java
  public static void rotate_matrix_3(int[][] matrix) {
          int n = matrix.length;
          int temp;
  
          // 水平旋转
          for(int i = 0; i<n/2;i++){
              for(int j = 0; j<n;j++){
                  temp = matrix[i][j];
                  matrix[i][j] = matrix[n-1-i][j];
                  matrix[n-1-i][j] = temp; 
              }
          }
  
          // 对角旋转
          for(int i = 0; i< n;i++){
              for(int j = 0;j<i;j++){
                  temp = matrix[i][j];
                  matrix[i][j] = matrix[j][i];
                  matrix[j][i] = temp;
              }
          }                                     
      }
  ```

  



## 3. 零矩阵

> 编写一种算法，若M × N矩阵中某个元素为0，则将其所在的行与列清零。

- 分别用一个 `row`数组和 `col`数组记录是否当前行和列清空。

  ```java
   public static void solution(int[][] matrix) {
          int[] row = new int[matrix.length];
          int[] col = new int[matrix[0].length];
          for(int i = 0; i<row.length;i++){
              for(int j = 0; j<col.length;j++){
                  if(matrix[i][j] == 0){
                      row[i] = 1;
                      col[j] = 1;
                  }
              }
          }
                                 
          for(int i = 0; i<row.length;i++){
              if(row[i] == 1){
                  for(int j = 0; j<col.length;j++){
                      matrix[i][j] = 0;
                  }
              }
          }
  
          for(int j = 0;j<col.length;j++){
              if(col[j] == 1){
                  for(int i = 0;i<row.length;i++){
                      matrix[i][j] = 0;
                  }
              }
          }
      }
  ```

## 4. 对角遍历

> 给你一个大小为 `m x n` 的矩阵 `mat` ，请以对角线遍历的顺序，用一个数组返回这个矩阵中的所有元素。

- 首先所有的元素都进入 新矩阵 `int[] res = new int[row*col];`

- 从第一个数开始 即 `i=0;j=0`。每两个循环为一个大循环：即向右上遍历和向左下遍历。

  - 右上遍历结束后。判断 `j`是否越界。
    - `j` 越界：`i = i + 2;j = j - 1;` (m > n会出现的情况。)
    - `j` 未越界: `i = i + 1;`
  - 左下遍历结束后，先判断 `i` 是否越界。
    - `i` 越界：`i = i - 1;j=j+2;`（m<n会出现的情况) 
    - `i` 未越界: `j = j + 1`

  ```java
  public static int[] solution(int[][] mat) {
          int row = mat.length;
          int col = mat[0].length;
          int i = 0;
          int j = 0;
          int k = 0;
          int[] res = new int[row*col];
  
          while(k<row*col){
              // 往左上走
              while(i>=0 && j<col){
                  res[k] = mat[i][j];
                  i = i- 1;
                  j = j + 1;
                  k = k + 1;
              }
              
              if(j<col){
                  i = i + 1;
              }
              else{
                  i = i + 2;
                  j = j - 1;
              }
              
              // 往右下走
              while(i<row&&j>=0){
                  res[k] = mat[i][j];
                  i = i +1;
                  j = j - 1;
                  k = k + 1;
              }
              
              if(i<row){
                  j = j +1;
              }
              else{
                  i = i - 1;
                  j = j + 2;
              }
          }
  
          return res;
                                          
      }
  ```

  

# 字符串

## 5. 最长公共前缀

> 写一个函数来查找字符串数组中的最长公共前缀。

- 模拟寻找过程

- 初始化 `sub`为第一个字符串

- 遍历数组，寻找当前字符与 `sub`的最大前缀并更新 `sub`

- O(M*N),O(1)

  - 注意 `String.subString(int start,int end)`函数为**左闭右开**。

  ```java
  public static String solution(String[] strs) {
          String sub = strs[0];
          for(int i =0; i<strs.length;i++){
              sub = findSub(sub, strs[i]);
          }
          return sub;
      }
  
      public static String findSub(String s1,String s2) {
          int i = 0;
          int j = 0;
          while(i<s1.length() && j <s2.length()){
              if(s1.charAt(i) == s2.charAt(j)){
                  i = i + 1;
                  j = j + 1;
              }
              else{
                  break;
              }
          }
          return s1.substring(0,i);
      }
  ```
  
  

## 6. 最长回文子串

> 给你一个字符串 `s`，找到 `s` 中最长的回文子串。回文子串例子:abba
>
> 如果字符串的反序与原始字符串相同，则该字符串称为回文字符串。

- 动态规划

  - O(N^2^),O(N^2^)

- 判断当前字串 `str[i,j]`是否为回文串的条件: 如果 `str[i] == str[j] && str[i+1,j-1]`则是。否则不是。因此可以根据此条件写出状态转移方程。

- 创建 `n*n`的矩阵：

  1. 矩阵的每一位代表: `[i,j]`的字串是否为回文串。
  2. 由于是字串，因此`j>i`。并初始化`i<j`的位置为1。
  3. 再遍历上对角线找出矩阵中元素为1并且间隔最大的则为结果。

  ```java
  public static String solution(String str) {
          int[][] palinMaxtrix = new int[str.length()][str.length()];
          for(int i = 0;i<palinMaxtrix.length;i++){
              for(int j = 0; j<=i;j++){
                  palinMaxtrix[i][j] = 1;
              }
          } 
          // 倒三角从下往上进行动态规划更新状态
          for(int i = str.length()-1;i>=0;i--){
              for(int j = i+1;j<str.length();j++){
                  if(str.charAt(i) == str.charAt(j)){
                      if(palinMaxtrix[i+1][j-1] == 1){
                          palinMaxtrix[i][j] = 1;
                      }
                      else{
                          palinMaxtrix[i][j] = 0;
                      }
                  }
                  else{
                      palinMaxtrix[i][j] = 0;
                  }
              }
          }
  
          int start = 0;
          int end = 0;
          int max = 0;
          for(int i = 0;i<str.length();i++){
              for(int j = i;j<str.length();j++){
                  if(palinMaxtrix[i][j] == 1 && (j-i) > max){
                      max = j - i;
                      start = i;
                      end = j;
                  }
              }
          }
  
          return str.substring(start, end+1);  
      }
  ```



## 7. 最长回文子序列

> 给你一个字符串 `s` ，找出其中最长的回文子序列，并返回该序列的长度。
>
> 子序列定义为：不改变剩余字符顺序的情况下，删除某些字符或者不删除任何字符形成的一个序列。
>
> **示例 1：**
>
> ```
> 输入：s = "bbbab"
> 输出：4
> 解释：一个可能的最长回文子序列为 "bbbb" 。
> ```

- `dp_matrix = N * N`
  - **最长回文子串中**每一项`dp_matrix[i][j]`代表字符串`s[i:j+1]`是否为回文子串
  - **最长回文子序列中**每一项`dp_matrix[i][j]`代表字符串`s[i:j+1]`最长回文子序列的长度
- 如果 `s[i] == s[j]`，则 `dp_matrix[i][j] == dp_matrix[i+1][j-1] + 2`
- 否则，则`dp_matrix[i][j] = max(dp_matrix[i+1][j],matrix[i][j-1])`

```python
class Solution(object):
    def longestPalindromeSubseq(self, s):
        """
        :type s: str
        :rtype: int
        """
        n = len(s)
        dp = [[0 for _ in range(n)] for _ in range(n)]

        for i in range(n-1,-1,-1):
            for j in range(i,n):
                if(i == j):
                    dp[i][j] = 1
                else:
                    if(s[i] == s[j]):
                        dp[i][j] = dp[i+1][j-1] + 2
                    else:
                        dp[i][j] = max(dp[i+1][j],dp[i][j-1])
        return dp[0][n-1]
```



## 8. 最长回文串

> 给定一个包含大写字母和小写字母的字符串 `s` ，返回 *通过这些字母构造成的 **最长的 回文串*** 的长度。
>
> 在构造过程中，请注意 **区分大小写** 。比如 `"Aa"` 不能当做一个回文字符串。
>
> **示例 1:**
>
> ```
> 输入:s = "abccccdd"
> 输出:7
> 解释:
> 我们可以构造的最长的回文串是"dccaccd", 它的长度是 7。
> ```

- 给定的字符中选取字符构造回文串而不是保持原来顺序的子串
- 因此需要记录每个字符的次数，保留偶数的个数，从剩下中选取一个放在最中间即为最大方案。

```python
class Solution(object):
    def longestPalindrome(self, s):
        """
        :type s: str
        :rtype: int
        """
        cnt = {}
        for c in s:
            cnt[c] = cnt.get(c,0) + 1
        remain_flag = 0
        max_l = 0
        for c,num in cnt.items():
            if(num % 2 == 0):
                max_l = max_l + num
            else:
                max_l = max_l + num - 1
                remain_flag = 1
        max_l = max_l + remain_flag
        return max_l
```



## 9. 最长不重复子串

> 给定一个字符串 `s` ，请你找出其中不含有重复字符的 **最长 子串** 的长度。
>

- 用一个集合记录当前窗口中的字符；当**不重复**的时候，**向右边移动窗口**。

- 如果**出现重复**，**移动左边的窗口**直到当前右窗口指向的字符不在窗口中。

  ```python
  class Solution(object):
      def lengthOfLongestSubstring(self, s):
          """
          :type s: str
          :rtype: int
          """
          left = 0
          right = 0
          max_l = 0
          window_s = set()
          n = len(s)
  
          while(right < n):
              if(s[right] not in window_s):
                  window_s.add(s[right])
                  right = right + 1
                  max_l = max(max_l,len(window_s))
              else:
                  while(s[right] in window_s):
                      window_s.remove(s[left])
                      left = left + 1
          return max_l
  ```

  

## 10.最长公共子序列

> 给定两个字符串 `text1` 和 `text2`，返回这两个字符串的最长 **公共子序列** 的长度。如果不存在 **公共子序列** ，返回 `0` 。
>
> 一个字符串的 **子序列** 是指这样一个新的字符串：它是由原字符串在不改变字符的相对顺序的情况下删除某些字符（也可以不删除任何字符）后组成的新字符串。
>
> - 例如，`"ace"` 是 `"abcde"` 的子序列，但 `"aec"` 不是 `"abcde"` 的子序列。
>
> 两个字符串的 **公共子序列** 是这两个字符串所共同拥有的子序列。

- `dp = (N+1)*(N+1)`，每个位置的值`dp[i][j]` 代表 `text1[:i]`与`text2[:j]`之间的最长公共子序列
- 如果 `text1[i] == text2[j]`,则 `dp[i][j] == dp[i-1][j-1]+1`
- 否则`dp[i][j] == max(dp[i-1][j],dp[i][j-1])`
- 需要注意遍历时的 `i,j`对于字符的索引位置是大1的。

```python
class Solution(object):
    def longestCommonSubsequence(self, text1, text2):
        """
        :type text1: str
        :type text2: str
        :rtype: int
        """
        m = len(text1)
        n = len(text2)
        dp_matrix = [ [0 for _ in range(n+1)] for _ in range(m+1)]
        for i in range(1,m+1):
            for j in range(1,n+1):
                if(text1[i-1] == text2[j-1]):
                    dp_matrix[i][j] = dp_matrix[i-1][j-1] + 1
                else:
                    dp_matrix[i][j] = max(dp_matrix[i][j-1],dp_matrix[i-1][j])
        return dp_matrix[len(text1)][len(text2)]
```



## 11. 翻转字符串里的单词

> 给你一个字符串 s ，请你反转字符串中 单词 的顺序。
>
> 单词 是由非空格字符组成的字符串。s 中使用至少一个空格将字符串中的 单词 分隔开。
>
> 返回 单词 顺序颠倒且 单词 之间用单个空格连接的结果字符串。
>
> 注意：输入字符串 s中可能会存在前导空格、尾随空格或者单词间的多个空格。返回的结果字符串中，单词间应当仅用单个空格分隔，且不包含任何额外的空格。
>

- 首先找到非空字符的 `start` 与 `end`位置。

- 遍历 `start`与 `end`之间字符，将不为空的字符组成的单词加入双端队列(模拟栈)

- `String.join` 方法添加空格。

  ```java
   public static String solution(String s) {
  
          Deque<String> stack = new ArrayDeque<>();
          StringBuilder word = new StringBuilder();
          int start = 0;
          int end = s.length() - 1;
          while(start < s.length() && s.charAt(start) == ' '){
              start = start + 1;
          }
          while(end>=0 && s.charAt(end) == ' '){
              end =  end - 1;
          }
          while(start <= end){
              if(s.charAt(start) != ' '){
                  word.append(s.charAt(start));
                  start = start + 1;
              }
              else{
                  if(word.length() != 0){
                     stack.push(word.toString());
                  }
                  start = start + 1;
                  word.delete(0, word.length());
              }
          }
  
          stack.push(word.toString());
  
          return String.join(" ", stack);
          
      }
  ```



## **字符串匹配算法：KMP**

- **模式串的next**数组：每个元素代表 如果当前元素和主串不匹配时候指针 `j`回溯的位置。

- `next[j]`的值与 `0~j-1`的字串的相同前缀与后缀的长度,设置 `next[0] == - 1`

  - 从第一位开始比较 `j=1`,`k=next[j-1]`。

  - 每次比较 `s[k] == s[j-1]`即当前位置元素与前一个元素的 `next`值是否相等，相等则将`k+1`并赋值到当前 `next`。或者 `k==-1`即回溯到了第一位数。

  - 不相等则进行回溯 `k = next[k]`

- 主串与模式子串进行匹配，若当前元素不匹配，将模式串的指针 `j`回溯到 `next[j]`的位置。

  - 当前 两字符串 `i` `j` 两位置元素相等时，或者 `j==-1`，即 `j`回溯到了第一个位置且与 `i`位置元素不相等，则两字符串都向前进。

  - 否则，进行回溯 `j=next[j]`

- 循环结束后如果 `j >= length` , 返回 `i-j`

- 否则返回 -1.

- O(N+M)

  ```java
   public static int[] getNextpos(String s) {
          int[ ] next = new int[s.length()];
  
          int j = 1;
          next[0] = -1;
          int k = next[j-1];
  
          while(j<next.length){
              if( k == -1 || s.charAt(j-1) == s.charAt(k)){
                  k = k + 1;
                  next[j] = k;
                  j = j + 1;
              }
              else{
                  k = next[k];
              }
          }
          return next;                                
      }
  
  
      public static int kmp(String str,String pattern) {
          int[] next = getNextpos(pattern);
          int i = 0;
          int j = 0;
          while(i < str.length() && j < pattern.length()){
              if(j == -1 || str.charAt(i) == pattern.charAt(j)){
                  i = i + 1;
                  j = j + 1;
              }
              else{
                  j = next[j];
              }
          }
          if(j>=pattern.length()){
              return i - j;
          }
          else{
              return - 1;
          }
                                          
      }
      
  
  
  ```

  

# 双指针技巧

## 12. 双指针情景一 运动方向相反：翻转字符串

> 编写一个函数，其作用是将输入的字符串反转过来。输入字符串以字符数组 s 的形式给出。
>
> 不要给另外的数组分配额外的空间，你必须原地修改输入数组、使用 O(1) 的额外空间解决这一问题。



- 使用双指针，`i` `j` 分别代表头指针与尾指针。

- 交换两个指针的元素，并指针向中间进行扫描。

  ```java
  public static char[] solution(char[] s){
          int i = 0;
          int j = s.length - 1;
          char temp;
          while(i<j){
              temp = s[i];
              s[i] = s[j];
              s[j] = temp;
              i = i +1 ;
              j = j + 1 ;
          }
          return s;
      }
  ```



## 13. 数组拆分 I

> 给定长度为 2n 的整数数组 nums ，你的任务是将这些数分成 n 对, 例如 (a1, b1), (a2, b2), ..., (an, bn) ，使得从 1 到 n 的 min(ai, bi) 总和最大。
>
> 返回该 最大总和 。

- 通过计数排序对 `nums`数组进行排序。

- 对排序后的 `nums`数组的偶数下标元素进行求和。

  ```java
  public static int[] sort_Count(int[] nums){
          int min = nums[0];
          int max = nums[0];
          for(int element:nums){
              if(element < min){
                  min = element;
              }
              if(element > max){
                  max = element;
              }
          }
          int n = max - min + 1;
          int count[] = new int[n];
  
          for(int element:nums){
              count[element - min] = count[element - min] + 1 ;
          }
  
          int k = 0;
          for(int i = 0; i< n;i++){
              for(int j = count[i];j>0;j--){
                  nums[k] = i + min;
                  k = k + 1;
              }
          }
          return nums;
      }
      public static int solution(int[] nums){
          sort_Count(nums);
          int sum = 0;
          for(int i = 0; i<nums.length;i=i+2){
              sum = sum +  nums[i];
          }
          return sum;
  
      }
  ```



## 14. **两数之和II - 输入有序数组**

> 给你一个**  ** 开始的整数数组 numbers ，该数组已按 **非递减顺序排列**  ，请你从数组中找出满足相加之和等于目标数 target 的两个数。如果设这两个数分别是 `numbers[index1]` 和 `numbers[index2]` ，则 `1 <= index1 < index2 <= numbers.length` 。
>
> 以长度为 2 的整数数组 `[index1, index2]` 的形式返回这两个整数的下标 `index1` 和 `index2`。
>
> 你可以假设每个输入 只对应唯一的答案 ，而且你 不可以 重复使用相同的元素。
>
> 你所设计的解决方案必须只使用常量级的额外空间。 

### 暴力解

- 双指针搜索

- 找到 `num[i] + nums[j] == target` 则跳出循环

- O(N^2^),O(1)

  ```java
  public static int[] solution(int[] nums,int target){
          int i = 0;
          int j = i + 1;
          for(i = 0;i<nums.length;i++){
              for(j = i + 1;j<nums.length;j++){
                  if(nums[i] + nums[j] == target){
                      break;
                  }
              }
              if(j<nums.length){
                  break;
              }
          }
          return new int[]{i+1,j+1};
      }
  ```

  


### 二分查找

- 先固定一个值，即寻找另一个数的target值：`targe-nums[i]`

- 使用二分查找寻找该target值。

- O(N*log(N)),O(1)

  - 注意二分查找的循环终止条件为 `while(left<=right)`
  - `left==right时需要判断一次该值是否是目标值`

  

  ```java
  public static int[] solution1(int[] nums,int target){
          for(int i = 0; i<nums.length;i++){
              int j_target = target - nums[i];
              int j_start = i + 1;
              int j_end = nums.length - 1;
  
              while(j_start <= j_end){
                  int mid = (j_start + j_end) / 2;
                  if(nums[mid] >j_target){
                      j_end = mid - 1;
                  }
                  else if(nums[mid] < j_target){
                      j_start = mid + 1;
                  }
                  else{
                      return new int[]{i+1,mid+1};
                  }
              }
          }
  
          return new int[]{-1,-1};
      }
  ```



### **双指针**

- `int i = 0;int j = nums.length-1`

- 当两个指针的元素的和 大于 `target`，则右指针向前移。

- 当两个指针的元素之和 小于 `target`，则左指针向后移。

- 由于数组有序，可以缩小搜索范围，且这样做不会导致跳过了目标值：

  - 初始化的 `i` 一定是在目标索引 `i_target`的左侧或者相等
  - 初始化的 `j` 一定是在目标索引 `j_target`的右侧或者相等    
  - 当 `i`先到达 `i_target`，此时的和一定大于 `target`，`j`还在 `j_target`的右侧
  - 当 `j` 先到达 `j_target`，此时的和一定小于 `target`，`i`还在 `i_target`的左侧

- O(N),O(1)

  ```java
  public static int[] solution3(int[] nums,int target){
          int i = 0;
          int j = nums.length-1;
          while(i<j){
              if(nums[i] + nums[j] == target){
                  return new int[]{i+1,j+1};
              }
              else if(nums[i] + nums[j] > target){
                  j = j - 1;
              }
              else{
                  i = i + 1;
              }
          }
          return new int[]{-1,-1};
      }
  ```




##  15. [移除元素](https://leetcode.cn/problems/remove-element/): 运动方法相同，不同速度(不同指针意义不同，在不同的情况下移动。)

> 给你一个数组 `nums` 和一个值 `val`，你需要 **原地** 移除所有数值等于 `val` 的元素，并返回移除后数组的新长度。

- 原地去除，因此不能通过新开辟数组存储元素。

- 设置一个扫描指针(快)，和一个新数组末尾指针(慢)。

- 快指针依次遍历，直到遇到目标值将值赋值给慢指针，慢指针移动。

  ```java
   public static int solution(int[] nums,int val) {
  
          int scan = 0;
          int new_pos = 0;
  
          for(scan = 0;scan<nums.length;scan++){
              if(nums[scan] != val){
                  nums[new_pos] = nums[scan];
                  new_pos = new_pos + 1;
              }
          }
          return new_pos;                               
      }
  ```

  

## 16. **最大连续1的个数**

> 给定一个二进制数组 `nums` ， 计算其中最大连续 `1` 的个数。  

- 首先设置慢指针为-1;

- 快指针一次扫描数组，当遇到当前元素

  - 为1时：比较 `scan - start`是否大于最大个数
  - 不为1时: `start = scan`

  ```java
  public static int solution(int[] nums) {
          int start = -1;
          int scan = 0;
  
          int num = 0;
          for(scan=0;scan<nums.length;scan++){
              if(nums[scan] == 1 ){
                  if(scan - start > num){
                      num = scan - start;
                  }
              }
              else{
                  start = scan;
              }
          }
          return num;
                                         
      }
  ```

  

## 17. **长度最小子数组**

> 给定一个含有 `n` 个正整数的数组和一个正整数 `target` 。
>
> 找出该数组中满足其总和大于等于 target 的长度最小的 连续子数组 `[numsl, numsl+1, ..., numsr-1, numsr]` ，并返回其长度。如果不存在符合条件的子数组，返回 `0` 。
>

### 思路一：前缀和+二分查找

1. 先求得 前缀和数组 `sums[n+1]`，每个元素代表 `[0,i-1]`的和。
2. 遍历数组，通过二分法 **遍历查找找到** 以**每个元素为起始点**的长度最小的满足要求的子数组，子数组的和为：`sums[end] - sums[start]`,由于 `nums`数组有序，因此可以用二分查找进行查找。

   1. **注意当前子数组的长度为 `mid - i`。**   

3. O(N*log(N))，O(N)

   ```java
   public static int solution1(int target,int[] nums) {
       
           int minLen = nums.length + 1;
           int[] sums = new int[nums.length+1];
           // 前缀和，因此需要记录最后到最后一位的和。
           sums[0] = 0;
           for(int i = 1; i<nums.length+1;i++){
               sums[i] = sums[i-1] + nums[i-1];
           }
           
           // 前缀和为有序数组，可以使用二分查找。
           for(int i = 0; i<nums.length;i++){
               int start = i+1;
               int end = nums.length;
               while(start<=end){
                   int mid = (start + end) / 2;
                   if(sums[mid] - sums[i] >= target){
                       minLen = Math.min(minLen,mid-i);
                       end = mid - 1;
                   }
                   else{
                       start = mid + 1;
                   }
               }   
           }
           if(minLen <= nums.length){
               return minLen;
           }
           return 0;   
       } 
   ```

   



### **思路二：滑动窗口**

- 窗口指针 `start`，`end`

- 先找到以当前 `start`开始的**子数组和** 大于 `target`的 `end` 指针的位置，先找到第一个窗口。

- 寻找**该子数组的子数组是否满足要求**:挨个移动 `start`指针，并减去其元素判断是否满足`sum>=target`要求

  - 满足，则更新最小结果，移动前窗口 `start`
  - 不满足，移动后窗口：向后移动 `end` 。
  - **注意当前子数组大小为 `end-start+1`**

- O(N)

  

  ```java
  public static int solution(int target,int[] nums) {
          int start = 0;
          int end = 0;
  
          int sum = 0;
          int minlen = nums.length + 1;
  
          for(end = 0 ; end<nums.length; end++){
              sum = sum + nums[end];
  
              while(sum >= target){
                  minlen = Math.min(minlen,end-start+1);
                  sum = sum - nums[start];
                  start = start + 1;
              }
          }
  
          if(minlen<= nums.length){
              return minlen;
          }
          return 0;
      }   
  ```

  

# 总结

## 18. 杨辉三角

> 给定一个非负整数 *`numRows`，*生成「杨辉三角」的前 *`numRows`* 行。
>
> 在「杨辉三角」中，每个数是它左上方和右上方的数的和。

![img](https://s2.loli.net/2023/09/14/OnVWHQstKhUcbuT.gif)

- 初始化第一行为 `[1]`

- 其余每行，除了第一个数和最后一个数 == 1，其余为 `res[row-1][num] + res[row-1][num-1]`

  ```java
  public static List<List<Integer>> solution(int numRows) {
          List<List<Integer>> res = new ArrayList<>(numRows);
          ArrayList<Integer> firstRow = new ArrayList<>();
          firstRow.add(1);
          res.add(0, firstRow);
  
          for(int row = 1 ;row<numRows;row++){
              ArrayList<Integer> temp = new ArrayList<>(null);
              for(int num = 0;num<=row;num++){
                  if(num==0 || num == row){
                      temp.add(num,1);
                  }
                  else{
                      temp.add(num,res.get(row-1).get(num)+ res.get(row-1).get(num-1));
                  }
              }
              res.add(row, temp);
          }
          return res;                 
      }
  ```

  

  ## 杨辉三角 II
  
  > 给定一个非负索引 `rowIndex`，返回「杨辉三角」的第 `rowIndex` 行。
  >
  > 在「杨辉三角」中，每个数是它左上方和右上方的数的和。

​	![img](https://s2.loli.net/2023/09/14/zkIQOTXayU5MGBt.gif)

### 思路一

- 根据杨辉三角的性质求出前 `rowIndex`行。

- 返回最后一行。

- O(N^2^),O(N^2^)

  ```java
  public static List<Integer> solution2(int rowIndex) {
          List<List<Integer>> tirangle = new ArrayList<>();
  
          for(int row=0;row<=rowIndex;row++){
              List<Integer> temp = new ArrayList<>();
              for(int i = 0 ;i<=row;i++){
                  if( i == 0 || i ==row){
                      temp.add(1);
                  }
                  else{
                      temp.add(tirangle.get(row-1).get(i) + tirangle.get(row-1).get(i-1));
                  }
              }
              tirangle.add(temp);
          }
  
          return tirangle.get(rowIndex);
      }
  ```



### 思路二

- 由于只需要最后一行的杨辉三角值，因此前面存储的 `List`不是必须保留的，可以覆盖掉。降低空间复杂度。

- 因此**外层的 `ArrayList`** 可以设置为 一维的只保留上一行的数据用于计算。

- 里层的 `ArrayList`还是用于计算更新每一行的数据，再将每一行的数据赋值给外层的  `ArrayList`

- O(N^2^),O(N)

  ```java
  public static List<Integer> solution3(int rowIndex) {
          List<Integer> pre = new ArrayList<>();
          for(int row = 0 ; row<=rowIndex ; row++){
              List<Integer> cur = new ArrayList<>();
              for(int i = 0; i<=row ; i++){
                  if(i == 0 || i == row){
                      cur.add(1);
                  }
                  else{
                      cur.add(pre.get(i) + pre.get(i-1));
                  }
              }
              pre = cur;
          }
  
          return pre;
                                        
      }
  ```



### 思路三

- 利用组合数学公式：

  - ![image-20230915165155151](https://s2.loli.net/2023/09/15/DxAhmvc5NeoQ8zs.png)
- O(N)


  - ```java
    public static List<Integer> solution4(int rowIndex) {
            List<Integer> res = new ArrayList<>();
            for(int i = 0; i<=rowIndex;i++){
                if(i==0){
                    res.add(1);
                }
                else{
                    res.add((int) ((long)res.get(i-1) * (rowIndex - i + 1) / i));
                }
            }
            return res;
                                            
        }
    ```

    

## 19. 翻转字符串中的单词



> 给定一个字符串 `s` ，你需要反转字符串中每个单词的字符顺序，同时仍保留空格和单词的初始顺序。
>
> ```
> 输入：s = "Let's take LeetCode contest"
> 输出："s'teL ekat edoCteeL tsetnoc"
> ```

- 由于Java中的 `String`类是不可修改的，因此只能开辟额外的存储空间。

- 赋值给 `StringBuilder`类后变为可修改的字符串对象，可以进行原地翻转。

  - 设置 `start`,`end`指针，找到`end` 指向单词结尾后的空格或者长度 `n`

  - 进行在`StringBuilder`上原地反转。

  - 找到`end`指向下一个非空元素，并更新 `start`

  - ```java
    public static String solution2(String s) {
            StringBuilder res = new StringBuilder(s);
            int start = 0;
            int end = 0;
            while( end < res.length() ){
                while( end < res.length() && res.charAt(end) != ' '){
                    end = end + 1;
                }
                int i = start;
                int j = end-1;
                while(i<=j){
                    char temp = res.charAt(i);
                    res.setCharAt(i, res.charAt(j));
                    res.setCharAt(j, temp);
                    i = i + 1;
                    j = j - 1;
                }
                while(end < res.length() && res.charAt(end) == ' '){
                    end = end + 1;
                }
                start = end;
            }   
            return res.toString();                             
        }
    ```




## 20. **寻找旋转排序数组中的最小值**

> 已知一个长度为 n 的数组，预先按照升序排列，经由 1 到 n 次 旋转 后，得到输入数组。例如，原数组 nums = [0,1,2,4,5,6,7] 在变化后可能得到：
> 若旋转 4 次，则可以得到 [4,5,6,7,0,1,2]
> 若旋转 7 次，则可以得到 [0,1,2,4,5,6,7]
> 注意，数组 [a[0], a[1], a[2], ..., a[n-1]] 旋转一次 的结果为数组 [a[n-1], a[0], a[1], a[2], ..., a[n-2]] 。
>
> 给你一个元素值 互不相同 的数组 nums ，它原来是一个升序排列的数组，并按上述情形进行了多次旋转。请你找出并返回数组中的 最小元素 。
>
> 你必须设计一个时间复杂度为 O(log n) 的算法解决此问题。

- 二分法。

  - `start = 0; end = n - 1;`
  - 未旋转的情况，直接返回 `nums[0]`
  - 通过二分法遍历，`nums[mid] < nums[0]`则更新 `min`

  ```java
  public static int solution(int[] nums) {
          int start = 0;
          int end = nums.length - 1;
          int min = 0;
          if(nums[0] <= nums[end]){
              return nums[0];
          }
           
          while(start <= end){
              int mid = (start + end)/2;
              if(nums[mid] >= nums[0]){
                  start = mid + 1;
              }
              else{
                  min = nums[mid];
                  end = mid - 1;
              }
          }
  
          return nums[start];
      }
  ```

  

### **同理也可以找该种数组最大值**

- `start = 0; end = n - 1;`
- 未旋转的情况，直接返回 `nums[0]`
- 通过二分法遍历，`nums[mid] >= nums[0]` 则更新 `max`

```java
public static int solution1(int[] nums) {
        int start = 0;
        int end = nums.length - 1;
        int max = 0;
        if(nums[0] <= nums[end]){
            return nums[0];
        }
         
        while(start <= end){
            int mid = (start + end)/2;
            if(nums[mid] >= nums[0]){
                max = mid;
                start = mid + 1;
            }
            else{
                end = mid - 1;
            }
        }

        return nums[max + 1];                            
    }
```



## 21. **删除排序数组中的重复项**

> 给你一个 **升序排列** 的数组 nums ，请你 **原地** 删除重复出现的元素，使每个元素 只出现一次 ，返回删除后数组的新长度。元素的 相对顺序 应该保持 一致 。然后返回 nums 中唯一元素的个数。
>
> 考虑 nums 的唯一元素的数量为 k ，你需要做以下事情确保你的题解可以被通过：
>
> 更改数组 nums ，使 nums 的前 k 个元素包含唯一元素，并按照它们最初在 nums 中出现的顺序排列。nums 的其余元素与 nums 的大小不重要。
> 返回 k 。
>
> 输入：nums = [1,1,2]
> 输出：2, nums = [1,2,_]
> 解释：函数应该返回新的长度 2 ，并且原数组 nums 的前两个元素被修改为 1, 2 。不需要考虑数组中超出新长度后面的元素。

### **快慢指针**

- 设置两个指针 `left`，`right`，用于比较元素是否重复。一个直指向不重复元素最后一位的指针 `end`,`right`为快指针用于扫描，`end`为慢指针。

  - 初始化 `left = 0；end = 0；right = left+1;`

  - 循环条件 `while(right < n)`,并比较 `left` 与 `right`的值

    - 不相等: `right`值赋值给 `end+1`，`end`，`right`分别向后移动，`left`指向 `right`

    - 相等: 向后移动 `right` 直到 `left`与`right`值不相等

  
    ```java
    public static int solution(int[] nums) {
            int left = 0;
            int end = 0;
            int right = 1;
    
    
            while(right < nums.length){
                if(nums[right] != nums[left]){
                    end = end + 1;
                    nums[end] = nums[right];
    
                    left = right;
                    right = right + 1;
                }
                else{
                    while(right < nums.length && nums[right] == nums[left]){
                        right = right + 1;
                    }
                }
            }
            return end + 1;
        }
    ```
  
    
  

## 22. **移动零**

> 给定一个数组 `nums`，编写一个函数将所有 `0` 移动到数组的末尾，同时保持非零元素的相对顺序。
>
> **请注意** ，必须在不复制数组的情况下原地对数组进行操作。
>
> 输入: `nums = [0,1,0,3,12]`
> 输出: `[1,3,12,0,0]`

### 快慢指针

- `pos`指针指向下一个需要填放的位置，`fast`指针遍历数组

- `判断 nums[fast] == 0`

  - 等于：`fast = fast + 1` 向后继续移动。
  - 不等于: 说明需要将该元素与 `pos`指向的位置进行互换。并且 `pos = pos + 1; fast = fast + 1;`

  ```java
   public static void solution(int[] nums) {
          int fast = 0;
          int pos = 0;
  
          while(fast < nums.length){
              if(nums[fast] == 0){
                  fast = fast + 1;
              }
              else{
                  int temp = nums[pos];
                  nums[pos] = nums[fast];
                  nums[fast] = temp;
                  pos = pos + 1;
                  fast = fast + 1;
              }
          }
      }
  ```




# Hot 100

## 1. **两数之和**

> 给定一个整数数组 `nums` 和一个整数目标值 `target`，请你在该数组中找出 **和为目标值** *`target`* 的那 **两个** 整数，**并返回它们的数组下标。**
>
> 你可以假设每种输入只会对应一个答案，并且你不能使用两次相同的元素。
>
> 你可以按任意顺序返回答案。

- 由于返回的顺序没有限制，可以交换两个顺序
- 第二个数的索引可以保存到一个字典中
  - 遍历数组，检查剩下的数在不在字典中，**存在则返回对应的索引**
  - **不存在则将该数以及索引存到字典中**

```python
class Solution(object):
    def twoSum(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[int]
        """
        map = {}

        for idx,num in enumerate(nums):
            if(target-num in map):
                return [idx,map[target-num]]
            map[num] = idx
        return []
```



## 2. **字母异位词分组**

> 给你一个字符串数组，请你将 **字母异位词** 组合在一起。可以按任意顺序返回结果列表。
>
> **字母异位词** 是由重新排列源单词的所有字母得到的一个新单词。
>
>  
>
> **示例 1:**
>
> ```
> 输入: strs = ["eat", "tea", "tan", "ate", "nat", "bat"]
> 输出: [["bat"],["nat","tan"],["ate","eat","tea"]]
> ```
>
> **示例 2:**
>
> ```
> 输入: strs = [""]
> 输出: [[""]]
> ```
>
> **示例 3:**
>
> ```
> 输入: strs = ["a"]
> 输出: [["a"]]
> ```

- 如何通过dict保存**字母异位词**：对**每个单词进行排序后组合**作为key

```python
class Solution(object):
    def groupAnagrams(self, strs):
        """
        :type strs: List[str]
        :rtype: List[List[str]]
        """
        map_str = {}
        for s in strs:
            s1 = "".join(sorted(s))
            if s1 in map_str:
                map_str[s1].append(s)
            else:
                map_str[s1] = [s]
        
        return [v for v in map_str.values()]
```



## 3.**最长连续序列**

> 给定一个未排序的整数数组 `nums` ，找出数字连续的最长序列（不要求序列元素在原数组中连续）的长度。
>
> 请你设计并实现时间复杂度为 `O(n)` 的算法解决此问题。
>
>  **示例 1：**
>
> ```
>输入：nums = [100,4,200,1,3,2]
> 输出：4
> 解释：最长数字连续序列是 [1, 2, 3, 4]。它的长度为 4。
> ```
> 
> **示例 2：**
>
> ```
>输入：nums = [0,3,7,2,5,8,4,6,0,1]
> 输出：9
> ```
> 
> **示例 3：**
>
> ```
>输入：nums = [1,0,1,2]
> 输出：3
> ```

- 算法为O(N)则每个元素只能遍历一次,即**只从一个连续序列的开始才会进入更新循环**，否则不进入。因为不是开始的数的话，一定有比他大的序列。

- 不能重复遍历开头的数，因此从去重后的集合开始遍历

  - 每次通过判断 `num-1`在不在集合中判断当前数是不是开头的位置
  - 如果是开头的位置，则进行遍历直到找到最大的连续序列。
  - 这样的话只会遍历一次每个数，因为都是从每个独立序列的开头开始遍历的。中间数不会重复遍历

  ```python
  class Solution:
      def longestConsecutive(self, nums: List[int]) -> int:
          num_set= set(nums)
          max_seq = 0
  
          for num in num_set:
              if( num-1 not in num_set ):
                  num_temp = num
                  len_temp = 0
                  while(num_temp in num_set):
                      num_temp = num_temp + 1
                      len_temp = len_temp + 1
                  max_seq = max(max_seq,len_temp)
  
          return max_seq
  ```

  

## 4. 移动零

## 5.**盛最多水的容器**

> 给定一个长度为 `n` 的整数数组 `height` 。有 `n` 条垂线，第 `i` 条线的两个端点是 `(i, 0)` 和 `(i, height[i])` 。
>
> 找出其中的两条线，使得它们与 `x` 轴共同构成的容器可以容纳最多的水。
>
> 即找到两个数之差 * 两数距离 的最大值
>
> 返回容器可以储存的最大水量。
>
> **说明：**你不能倾斜容器。

- 核心思想: 已计算得到两个边界的 最大盛水量后，中间比最小边界小的柱子不用遍历，一定比外面遍历过的情况小。

  - 因此双指针每次只需要从最小的一边开始移动，直到移动到比自己大的一根柱子。
  - 然后再比较两边谁更小并计算更新结果

  ```python
  class Solution(object):
      def maxArea(self, height):
          """
          :type height: List[int]
          :rtype: int
          """
          #
          lf = 0
          rt = len(height)-1
          res = (rt-lf) * min(height[lf],height[rt])
          while(lf < rt):
              if(height[lf] < height[rt]):
                  temp = height[lf]
                  while(height[lf] <= temp ):
                      lf = lf + 1
                  res = max(res,(rt-lf) * min(height[lf],height[rt]))
              else:
                  temp = height[rt]
                  while(height[rt] <= temp and lf < rt):
                      rt = rt - 1
                  res = max(res,(rt-lf) * min(height[lf],height[rt]))
          return res
  ```



## 6. **三数之和**

> 给你一个整数数组 `nums` ，判断是否存在三元组 `[nums[i], nums[j], nums[k]]` 满足 `i != j`、`i != k` 且 `j != k` ，同时还满足 `nums[i] + nums[j] + nums[k] == 0` 。请你返回所有和为 `0` 且不重复的三元组。
>
> **注意：**答案中不可以包含重复的三元组。
>
> **示例 1：**
>
> ```
> 输入：nums = [-1,0,1,2,-1,-4]
> 输出：[[-1,-1,2],[-1,0,1]]
> 解释：
> nums[0] + nums[1] + nums[2] = (-1) + 0 + 1 = 0 。
> nums[1] + nums[2] + nums[4] = 0 + 1 + (-1) = 0 。
> nums[0] + nums[3] + nums[4] = (-1) + 2 + (-1) = 0 。
> 不同的三元组是 [-1,0,1] 和 [-1,-1,2] 。
> 注意，输出的顺序和三元组的顺序并不重要。
> ```
>
> **示例 2：**
>
> ```
> 输入：nums = [0,1,1]
> 输出：[]
> 解释：唯一可能的三元组和不为 0 。
> ```
>
> **示例 3：**
>
> ```
> 输入：nums = [0,0,0]
> 输出：[[0,0,0]]
> 解释：唯一可能的三元组和为 0 。
> ```

- **不重复的核心是三个数按照顺序记录**:`a,b,c`
- 当地一个数确定时，**两数之和**也确定了，因此可以采用双指针进行 **有序数组两数之和**判定
  - 不能出现重复，需要**跳过重复的** `a`
  - 当找到符合一个要求的两数之和后，需要将 **左右指针一起往中间移动且要跳过重复的数**

```python
class Solution(object):
    def threeSum(self, nums):
        """
        :type nums: List[int]
        :rtype: List[List[int]]
        """
        if(len(nums) == 3):
            if(sum(nums) == 0):
                return [nums]
            else:
                return []
        nums = sorted(nums)
        n = len(nums)
        res = []

        for i in range(n):
            if(i>0 and nums[i] == nums[i-1]):
                continue
            left = i + 1
            right = n -1
            target = -nums[i]

            while(left < right):
                if(nums[left]+nums[right] > target):
                    right = right - 1
                elif(nums[left]+nums[right] < target):
                    left = left + 1
                else:
                    res.append([nums[i],nums[left],nums[right]])
                    lf = nums[left]
                    rt = nums[right]

                    while(left < right and nums[left] == lf and nums[right] == rt):
                        left = left + 1
                        right = right - 1
            
        return resf
```



## 7. **接雨水**

> 给定 `n` 个非负整数表示每个宽度为 `1` 的柱子的高度图，计算按此排列的柱子，下雨之后能接多少雨水。

- **当前位置能接到的最大雨水，取决于左右两边最大值中最小值**

- **动态规划**: 维护两个数组，分别记录**当前位置及其左边的最大值**和**当前位置及其右边的最大值**

  ```python
  class Solution(object):
      def trap(self, nums):
          """
          :type height: List[int]
          :rtype: int
          """
          n = len(nums)
          lt_nums = [0] * n 
          rt_nums = [0] * n 
  
          lt_nums[0] = nums[0]
          rt_nums[-1] = nums[-1]
          res = 0
  
          for i in range(1,n):
              j = n-1-i 
              lt_nums[i] = max(lt_nums[i-1],nums[i])
              rt_nums[j] = max(rt_nums[j+1],nums[j])
  
          for i in range(n):
              res = res + (min(lt_nums[i],rt_nums[i]) - nums[i])
          return res
  
  ```

  

- 双指针: 由于需要记录 **当前位置左边的最大值 与 当前位置右边的最大值** 并且取两者的最小值。

  - 对于左边的数来说，**左边的最大值是能通过左指针确定**

  - 右边的数来说，**右边的最大值能够通过右指针确定**

  - 比较这两个最大值谁更小

    - 若是左边小，则左边的数能够确定最小雨水，因为右边的最大值 >= 此时记录的右指针经过的最大值
    - 右边同理，交叉进行更新

    ```python
    class Solution(object):
        def trap(self, nums):
            """
            :type height: List[int]
            :rtype: int
            """
            n = len(nums)
            left = 0
            right = n - 1
    
            lt_max = nums[0]
            rt_max = nums[-1]
    
            res = 0
            while(left <= right):
                
                if(lt_max < rt_max):
                    res = (lt_max-nums[left]) + res
                    left = left + 1
                    lt_max = max(nums[left],lt_max)
                else:
                    res = (rt_max-nums[right]) + res
                    right = right - 1
                    rt_max = max(rt_max,nums[right])
            return res
    ```

    

## 8. 无重复字符的最长子串

## 9. **找到字符串中所有字母异位词** ⚠️

> 给定两个字符串 `s` 和 `p`，找到 `s` 中所有 `p` 的 **异位词** 的子串，返回这些子串的起始索引。不考虑答案输出的顺序。
>
>  
>
> **示例 1:**
>
> ```
> 输入: s = "cbaebabacd", p = "abc"
> 输出: [0,6]
> 解释:
> 起始索引等于 0 的子串是 "cba", 它是 "abc" 的异位词。
> 起始索引等于 6 的子串是 "bac", 它是 "abc" 的异位词。
> ```
>
>  **示例 2:**
>
> ```python
> 输入: s = "abab", p = "ab"
> 输出: [0,1,2]
> 解释:
> 起始索引等于 0 的子串是 "ab", 它是 "ab" 的异位词。
> 起始索引等于 1 的子串是 "ba", 它是 "ab" 的异位词。
> 起始索引等于 2 的子串是 "ab", 它是 "ab" 的异位词。
> ```

- 滑动窗口:维护一个**字典窗口**记录当前窗口中**字符出现的次数**，如果窗口字典与`p`字典相同则为异位词。
- 每次将窗口向右移动一个单位，**更新字典中的元素**

```python
class Solution(object):
    def findAnagrams(self, s, p):
        """
        :type s: str
        :type p: str
        :rtype: List[int]
        """
        left = 0
        right = len(p)-1
        n = len(s)
        res = []

        base_str = {}
        for ch in p:
            base_str[ch] = base_str.get(ch,0)+1
        wind = {}
        for ch in s[left:right+1]:
            wind[ch] = wind.get(ch,0) + 1

        while(right < n):
            if(wind == base_str):
                res.append(left)
            
            wind[s[left]] = wind[s[left]] - 1
            if(wind[s[left]] == 0):
                wind.pop(s[left])
            left = left + 1
            right = right + 1
            if(right < n):
                wind[s[right]] = wind.get(s[right],0) + 1
        return res
```



## 10. **和为k的子数组** ⚠️

> 给你一个整数数组 `nums` 和一个整数 `k` ，请你统计并返回 *该数组中和为 `k` 的子数组的个数* 。
>
> 子数组是数组中元素的连续非空序列。
>
> **示例 1：**
>
> ```
> 输入：nums = [1,1,1], k = 2
> 输出：2
> ```
>
> **示例 2：**
>
> ```
> 输入：nums = [1,2,3], k = 3
> 输出：2
> ```

- ⚠️注:**不能使用滑动窗口**，滑动窗口针对的情况是，窗口变大或者变小，**窗口内的某种性质是单调变化的**。比如这个题，如果 nums[i] 都是正整数的话，那么窗口范围变大时，窗口内和是递增的，窗口范围变小时，窗口内和是递减的,这种情况下是可以使用滑动窗口的,但是题中数可正可负，不能使用滑动窗口
- **前缀和 + 哈希**：
  - 子数组`[i,j]`的和可以看作两个前缀和之差：`prefix[j] - prefix[i-1]`
  - 得到前缀和后，题目变为:求一个数组(前缀和数组)中两个数之差为k的个数：**类似于两数之和的做法**，使用哈希存储之前存在过的前缀和。
  - **注意一定要保存第一个数的前缀和为0，即保留以第一个数为起点的子数组**

```python
class Solution(object):
    def subarraySum(self, nums, k):
        """
        :type nums: List[int]
        :type k: int
        :rtype: int
        """
        res = 0
        prefix_sum = [0] * (len(nums) + 1)

        for i in range(1,len(nums)+1):
            prefix_sum[i] = nums[i-1] + prefix_sum[i-1]
        
        prefix_sum_cnt = {}

        for prefix_cur in prefix_sum:
            if(prefix_cur-k in prefix_sum_cnt):
                res = res + prefix_sum_cnt.get(prefix_cur-k)

            prefix_sum_cnt[prefix_cur] = prefix_sum_cnt.get(prefix_cur,0) + 1

        return res
```



##  11. **滑动窗口中的最大值** ⚠️

> 给你一个整数数组 `nums`，有一个大小为 `k` 的滑动窗口从数组的最左侧移动到数组的最右侧。你只可以看到在滑动窗口内的 `k` 个数字。滑动窗口每次只向右移动一位。
>
> 返回 *滑动窗口中的最大值* 
>
> **示例 1：**
>
> ```
> 输入：nums = [1,3,-1,-3,5,3,6,7], k = 3
> 输出：[3,3,5,5,6,7]
> 解释：
> 滑动窗口的位置                最大值
> ---------------               -----
> [1  3  -1] -3  5  3  6  7       3
>  1 [3  -1  -3] 5  3  6  7       3
>  1  3 [-1  -3  5] 3  6  7       5
>  1  3  -1 [-3  5  3] 6  7       5
>  1  3  -1  -3 [5  3  6] 7       6
>  1  3  -1  -3  5 [3  6  7]      7
> ```
>
> **示例 2：**
>
> ```
> 输入：nums = [1], k = 1
> 输出：[1]
> ```

- **每次找到窗口内的最大值需要 o(k),需要减小这个过程的时间**

### 大根堆：`heapq.heapify(),heapq.heappush(),heapq.heappop()`

- 通过堆来存储窗口中的最大值，每次取最大值时直接取堆的根元素即可:O(1)
- 每次移动窗口时:
  - 右边元素加入堆，O(log(n))
  - 移除掉根元素在窗口外的元素
  - 每个元素最多只会被加入和移除一次，因此内循环的时间复杂度降低到O(log(n))
  - 涉及到移除元素，所以需要存储元素的下标
- O(log(n))，O(n)

```python
class Solution:
    def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:

        #初始化堆,python堆为默认小根堆
        wind_heap = [(-nums[i],i) for i in range(k)]
        heapq.heapify(wind_heap)
        res = [-wind_heap[0][0]]

        for r in range(k,len(nums)):
            #此时窗口为 [r-k+1,r]
            heapq.heappush(wind_heap,(-nums[r],r))\
            
            #移除不在窗口内的最大值
            while(wind_heap[0][1] <= r-k):
                heapq.heappop(wind_heap)
            res.append(-wind_heap[0][0])
        
        return res
```



### 双端单调队列: `collections.dque(), dque.append(),dque.appendleft(),dque.pop(),dque.popleft()`

- 双端单调队列指的是整个队列 队首和队尾都可以出入元素，不过**队首是最大元素**，且**整个队列单调递减**

- 移动窗口时
  - 右边元素加入时，**仍然需要保持队列单调**：通过`pop`和`append`进行操作
  
  - 同时pop出不在窗口中的元素
  
  - 涉及到移除元素，所以需要存储元素的下标
  
    
  
- 每个元素只会加入和移除一次，总共O(N),O(k)

```python
class Solution:
    def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:

        #初始化单调递减队列
        wind_que = collections.deque()
        for i in range(k):
            while(wind_que and nums[wind_que[-1]] <= nums[i]):
                wind_que.pop()
            wind_que.append(i)
        res = [nums[wind_que[0]]]

        for i in range(k,len(nums)):
            #加入右窗口到合适位置，保持单调递减队列
            while(wind_que and nums[i] >= nums[wind_que[-1]]):
                wind_que.pop()
            wind_que.append(i)

            #移除左边不在窗口中的位置:r-k+1,r
            while(wind_que[0] <= i-k):
                wind_que.popleft()
            
            res.append(nums[wind_que[0]])
        
        return res
```



## 12. 最小覆盖子串

> 给你一个字符串 `s` 、一个字符串 `t` 。返回 `s` 中涵盖 `t` 所有字符的最小子串。如果 `s` 中不存在涵盖 `t` 所有字符的子串，则返回空字符串 `""` 。
>
> **注意：**
>
> - 对于 `t` 中重复字符，我们寻找的子字符串中该字符数量必须不少于 `t` 中该字符数量。
> - 如果 `s` 中存在这样的子串，我们保证它是唯一的答案。
>
> **示例 1：**
>
> ```
> 输入：s = "ADOBECODEBANC", t = "ABC"
> 输出："BANC"
> 解释：最小覆盖子串 "BANC" 包含来自字符串 t 的 'A'、'B' 和 'C'。
> ```
>
> **示例 2：**
>
> ```
> 输入：s = "a", t = "a"
> 输出："a"
> 解释：整个字符串 s 是最小覆盖子串。
> ```
>
> **示例 3:**
>
> ```
> 输入: s = "a", t = "aa"
> 输出: ""
> 解释: t 中两个字符 'a' 均应包含在 s 的子串中，
> 因此没有符合条件的子字符串，返回空字符串。
> ```



## 13.**最大子数组和 / 删除一次得到子数组最大和**

## 14. 合并区间

## 15.**轮转数组**

> 给定一个整数数组 `nums`，将数组中的元素向右轮转 `k` 个位置，其中 `k` 是非负数。
>
> **示例 1:**
>
> ```
> 输入: nums = [1,2,3,4,5,6,7], k = 3
> 输出: [5,6,7,1,2,3,4]
> 解释:
> 向右轮转 1 步: [7,1,2,3,4,5,6]
> 向右轮转 2 步: [6,7,1,2,3,4,5]
> 向右轮转 3 步: [5,6,7,1,2,3,4]
> ```
>
> **示例 2:**
>
> ```
> 输入：nums = [-1,-100,3,99], k = 2
> 输出：[3,99,-1,-100]
> 解释: 
> 向右轮转 1 步: [99,-1,-100,3]
> 向右轮转 2 步: [3,99,-1,-100]
> ```

### 循环覆盖法

- 数组元素向右移动存在一个规律即：**从一个元素出发，移动后一定能够再次回到原位置**
- 从一个位置开始，先将 `(i+k)%n`位置的元素进行保存，然后将上一个数 `i` 放入 `(i+k)%n`，最后更新上一个数
- 回到原位置后，根据一直保存的上一个数 更新原位置的值，此时
  - 没有遍历完: 起始位置 + 1继续循环
  - 否则已经遍历完成

```python
class Solution(object):
    def rotate(self, nums, k):
        """
        :type nums: List[int]
        :type k: int
        :rtype: None Do not return anything, modify nums in-place instead.
        """
        n = len(nums)
        k = k % n 
        count = 0
        idx = 0

        while(count < n):
            prenum = nums[idx]
            nextidx = (idx + k) % n

            while(nextidx != idx):
                temp = nums[nextidx]
                nums[nextidx] = prenum
                prenum = temp
                nextidx = (nextidx + k) % n
                count = count + 1
            
            nums[nextidx] = prenum
            idx = idx + 1

            count = count + 1
    
```



### **通过数组反转实现**

- 往右轮转最终实现的效果是:
  - 末尾的k的数 -> 开头的k位置
  - 开头的n-k个数 -> 末尾的n-k个位置
  - k=3, [1,2,3,4,5,6,7] -> [5,6,7,1,2,3,4]
- 等价于
  - 先反转数组
  - 反转[0:k]，反转[k:]

```python
class Solution(object):
    def rotate(self, nums, k):
        """
        :type nums: List[int]
        :type k: int
        :rtype: None Do not return anything, modify nums in-place instead.
        """
    
        def reverse(nums,left,right):
            while(left < right):
                temp = nums[right]
                nums[right] = nums[left]
                nums[left] = temp 
                left = left + 1
                right = right - 1
                
        k = k % len(nums)
        left = 0
        right = len(nums)-1
        reverse(nums,left,right)
        
        left = 0
        right = k-1
        reverse(nums,left,right)

        left = k
        right = len(nums)-1
        reverse(nums,left,right)
```



## 16.除自身以外数组的乘积

```
给你一个整数数组 nums，返回 数组 answer ，其中 answer[i] 等于 nums 中除 nums[i] 之外其余各元素的乘积 。

题目数据 保证 数组 nums之中任意元素的全部前缀元素和后缀的乘积都在  32 位 整数范围内。

请 不要使用除法，且在 O(n) 时间复杂度内完成此题。

 

示例 1:

输入: nums = [1,2,3,4]
输出: [24,12,8,6]
示例 2:

输入: nums = [-1,1,0,-3,3]
输出: [0,0,9,0,0]
```

### O(1) + O(N)

- 核心思路: **当前位置的结果** = **前序数组乘积** * **后序数组乘积**
- 不使用数组存储，通过两次遍历更新结果
  - 第一次遍历，获取到 `res[i] *= p[i-1]` 此时`p`代表当前位置之前的乘积
  - 第二次遍历，更新 `res[j] *= p[j+1] `，此时`p`代表当前位置之后的乘积

```python
class Solution(object):
    def productExceptSelf(self, nums):
        """
            :type nums: List[int]
            :rtype: List[int]
        """
        res = [1] * len(nums)
        p = 1

        for i in range(len(nums)):
            res[i] = res[i] * p
            p = p * nums[i]
        
        p = 1
        for j in range(len(nums)-1,-1,-1):
            res[j] = res[j] * p 
            p = p*nums[j]
        
        return res
       
```



## 17.**缺失的第一个正数** ⚠️

> 给你一个未排序的整数数组 `nums` ，请你找出其中没有出现的最小的正整数。
>
> 请你实现时间复杂度为 `O(n)` 并且只使用常数级别额外空间的解决方案。
>
> **示例 1：**
>
> ```
> 输入：nums = [1,2,0]
> 输出：3
> 解释：范围 [1,2] 中的数字都在数组中。
> ```
>
> **示例 2：**
>
> ```
> 输入：nums = [3,4,-1,1]
> 输出：2
> 解释：1 在数组中，但 2 没有。
> ```
>
> **示例 3：**
>
> ```
> 输入：nums = [7,8,9,11,12]
> 输出：1
> 解释：最小的正数 1 没有出现。
> ```

- 数组长度为N，最理想情况数字范围为 `1~N`，此时最小未出现的数为 `N+1`

- 因此对于数组中在 `1~N`范围内的数字，将其放到对应的 `i-1`的位置上

  - 对于数组中的数字，如果在 `1~N`中，则进行交换。
  - 此时新数也可能在`1~N`中，继续交换直到不在 `1~N`中
  - 如果该交换的数字已经在对应的位置上：`nums[i] == nums[nums[i]-1]`
    - 重复数字
    - 已经交换的数字

  ```python
  class Solution(object):
      def firstMissingPositive(self, nums):
          """
          :type nums: List[int]
          :rtype: int
          """
          n = len(nums)  
  
          for i in range(n):
              while(nums[i] >= 1 and nums[i] <= n and nums[i] != nums[nums[i]-1]):
                  temp = nums[nums[i]-1]
                  nums[nums[i]-1] = nums[i]
                  nums[i] = temp
  
          for i in range(n):
              if(nums[i] != i+1):
                  return i+1
          return n+1   
  ```



## 18.不同路径

- 因为只能**向下**和**向右**移动
- 所以在`[i,j]`位置的路径个数等于，上面位置的路径个数加上左边路径的位置个数，即状态转移方程为:
  - `dp[i][j] = dp[i-1][j] + dp[i][j-1]`
- 初始化:对于第一行和第一类的元素，路径都是1，因为只能向下和向右移动

```python
class Solution:
    def uniquePaths(self, m: int, n: int) -> int:
        dp = [[0 for _ in range(n)] for _ in range(m)]
        for i in range(m):
            dp[i][0] = 1

        for j in range(n):
            dp[0][j] = 1
        
        for i in range(1,m):
            for j in range(1,n):
                dp[i][j] = dp[i-1][j] + dp[i][j-1]
        
        return dp[m-1][n-1]
        
```

## 19.最小路径和

> 给定一个包含非负整数的 `*m* x *n*` 网格 `grid` ，请找出一条从左上角到右下角的路径，使得路径上的数字总和为最小。
>
> **说明：**每次只能向下或者向右移动一步。

- 因为只能向右和向下移动
- 对于一个位置`[i,j]`的最小路径取决于左边和上边位置的最小值 加上当前位置上的数字。即状态转移方程为:
  - `dp[i][j] = nums[i][j] + min(dp[i-1][j],dp[i][j-1])`
- 初始化:对于第一行和第一列的元素，其最小数值和为前缀和

```python
class Solution:
    def minPathSum(self, grid: List[List[int]]) -> int:
        m = len(grid)
        n = len(grid[0])

        pre = 0
        for i in range(m):
            grid[i][0] += pre
            pre = grid[i][0]
        
        pre = 0
        for j in range(n):
            grid[0][j] += pre
            pre = grid[0][j]
        
        for i in range(1,m):
            for j in range(1,n):
                grid[i][j] = grid[i][j] + min(grid[i-1][j],grid[i][j-1])
        
        return grid[m-1][n-1]
```

## 20.最长回文子串/子序列

## 21.最长公共子序列

## 22.**编辑距离** ⚠️

> 给你两个单词 `word1` 和 `word2`， *请返回将 `word1` 转换成 `word2` 所使用的最少操作数* 。
>
> 你可以对一个单词进行如下三种操作：
>
> - 插入一个字符
> - 删除一个字符
> - 替换一个字符
>
>  
>
> **示例 1：**
>
> ```
> 输入：word1 = "horse", word2 = "ros"
> 输出：3
> 解释：
> horse -> rorse (将 'h' 替换为 'r')
> rorse -> rose (删除 'r')
> rose -> ros (删除 'e')
> ```

- 如果想要得到 `word1[:i]` `word2[:j]`的最小编辑距离：
  - 已知 `word1[:i-1]` `word2[:j]`的编辑距离为` a`，需要 `a+1`，即 `word2[:j]` 变成 `word1[:i-1]`后插 入一个单词即可变成`word1[:i]`
  - 已知 `word1[:i]` `word2[:j-1]`的编辑距离为` b`，需要 `b+1`，即 `word1[:i]` 变成 `word2[:j-1]`后插 入一个单词即可变成`word2[:j]`
  - 已知 `word1[:i-1]` `word2[:j-1]`的编辑距离为` c`
    - `word1[i] == word[j]`：`c`
    - `word1[i] != word[j]`：`c+1`，即最后一个不同的字符交换即可

```python
class Solution(object):
    def minDistance(self, word1, word2):
        """
        :type word1: str
        :type word2: str
        :rtype: int
        """
        m = len(word1)
        n = len(word2)

        dp = [[0 for _ in range(n+1)] for _ in range(m+1)]

        #初始化行/列
        for i in range(m+1):
            dp[i][0] = i 
        
        for j in range(n+1):
            dp[0][j] = j
        
        for i in range(1,m+1):
            for j in range(1,n+1):
                if(word1[i-1] == word2[j-1]):
                    dp[i][j] = min(dp[i-1][j-1],dp[i-1][j]+1,dp[i][j-1]+1)
                else:
                    dp[i][j] = min(dp[i-1][j-1]+1,dp[i-1][j]+1,dp[i][j-1]+1)
        
        return dp[m][n]
```



## 23. 爬楼梯

> 假设你正在爬楼梯。需要 `n` 阶你才能到达楼顶。
>
> 每次你可以爬 `1` 或 `2` 个台阶。你有多少种不同的方法可以爬到楼顶呢？

- 由于一次只能爬1个或者2个台阶；因此当前位置的 方法数量 = 前一位置 + 前第二个位置；即状态转移方程为:
  - `dp[i] = dp[i-1] + dp[i-2]`

```python
class Solution(object):
    def climbStairs(self, n):
        """
        :type n: int
        :rtype: int
        """
        dp2 = 0
        dp1 = 1

        res = 0
        for i in range(n):
            res = dp1+dp2
            temp = dp1
            dp1 = res
            dp2 = temp
        
        return res
```



## 24. 杨辉三角/杨辉三角II

## 25. **打家劫舍** ⚠️

> 你是一个专业的小偷，计划偷窃沿街的房屋。每间房内都藏有一定的现金，影响你偷窃的唯一制约因素就是相邻的房屋装有相互连通的防盗系统，**如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警**。
>
> 给定一个代表每个房屋存放金额的非负整数数组，计算你 **不触动警报装置的情况下** ，一夜之内能够偷窃到的最高金额。
>
>  
>
> **示例 1：**
>
> ```
> 输入：[1,2,3,1]
> 输出：4
> 解释：偷窃 1 号房屋 (金额 = 1) ，然后偷窃 3 号房屋 (金额 = 3)。
>      偷窃到的最高金额 = 1 + 3 = 4 。
> ```

- 抽象为求一个数组的子数组和的最大值，且该子数组不能是相邻元素。
- 因为数组中数字是非负数，因此最终要取得的最大值一定会加到 **最后一位** 或者 **倒数第二位**，即最终要求的是这两个的一个最大值。
  - 即徐要求 `dp[n] = max(dp[n-2]+nums[n],dp[n-1])`，而该问题可以分解成子问题,`dp[n-1],dp[n-2]`
  - 状态转移方程为 `dp[i] = max(dp[i-2] + nums[i], dp[i-1])`
  - **初始化的时候**: `dp2 = nums[0],dp1 = max(nums[0],nums[1])`，因为`dp`代表的是**相邻两个位置能取得的最大值**

```python
class Solution(object):
    def rob(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        n = len(nums)
        if(n <= 2):
            return max(nums)
        
        dp1 = max(nums[0],nums[1])
        dp2 = nums[0]
        res = dp1

        for num in nums[2:]:
            res = max(dp2 + num, dp1)
            temp = dp1
            dp1 = res
            dp2 = temp
        return res
```





## 26. **完全平方数**

> 给你一个整数 `n` ，返回 *和为 `n` 的完全平方数的最少数量* 。
>
> **完全平方数** 是一个整数，其值等于另一个整数的平方；换句话说，其值等于一个整数自乘的积。例如，`1`、`4`、`9` 和 `16` 都是完全平方数，而 `3` 和 `11` 不是。

- 对于一个数，可以确定能够组成他的完全平方数，即小于 `math.sqrt(n)`的数，这些数字组成一个 **选择列表**
- 遍历方案列表后，最终的结果是求每种方案的最小值。而每种方案可以拆分成一个**子问题，即求去掉这个完全平方数后数字**
- **状态转移方程**:  $dp[i] = \min_{j=1}^{math.sqrt(n)}(dp[i-j^2] + 1)$，每个位置代表的是当前这个数 `i` 能够被 **选择列表** 中的完全平方和组成的最小个数。

- 初始化, `dp[0] = 0`, $i<j^2$ 的话，dp[i]取最大值`n+1`。整体初始化 `dp[i] = n+1; dp[0]=0`
- 从`1~n`遍历每个数，
  - 每个数字`i`需要根据状态转移方程取得最小值

```python
class Solution(object):
    def numSquares(self, n):
        """
        :type n: int
        :rtype: int
        """
        dp_nums = int(math.sqrt(n))
        dp = [n+1]*(n+1)
        dp[0] = 0

        for i in range(1,n+1):
            dp_temp = n+1
            for j in range(1,dp_nums+1):
                if(i>=j*j):
                    dp_temp = min(dp_temp,dp[i-j*j]+1)
            dp[i] = dp_temp
        
        return dp[n]

```



## 27. **零钱兑换**

> 给你一个整数数组 `coins` ，表示不同面额的硬币；以及一个整数 `amount` ，表示总金额。
>
> 计算并返回可以凑成总金额所需的 **最少的硬币个数** 。如果没有任何一种硬币组合能组成总金额，返回 `-1` 。
>
> 你可以认为每种硬币的数量是无限的。
>
>  
>
> **示例 1：**
>
> ```
> 输入：coins = [1, 2, 5], amount = 11
> 输出：3 
> 解释：11 = 5 + 5 + 1
> ```

- 与完全平方数和思路类似，等价于求 `dp[amount]`
- 这个可以**遍历 所有方案后选择最小的组合**，遍历每种方案时，分解成了子问题求解，即状态转移方程:
  - $dp[i] = \min_{0}^{len(coins)-1}(dp[i-coins[j]])$
- 初始化: `dp[i] = amount+1;dp[0]=0`

```python
class Solution(object):
    def coinChange(self, coins, amount):
        """
        :type coins: List[int]
        :type amount: int
        :rtype: int
        """
        dp = [amount+1]*(amount+1)
        print(len(dp))
        dp[0] = 0

        for a in range(1,amount+1):

            dp_temp = amount+1
            for coin in coins:
                if(a >= coin):
                    dp_temp = min(dp_temp,dp[a-coin]+1)
            dp[a] = dp_temp
        if(dp[amount] == amount+1):
            return -1
        return dp[amount]
```



## 28. **单词拆分**

```python
给你一个字符串 s 和一个字符串列表 wordDict 作为字典。如果可以利用字典中出现的一个或多个单词拼接出 s 则返回 true。

注意：不要求字典中出现的单词全部都使用，并且字典中的单词可以重复使用。

示例 1：
输入: s = "leetcode", wordDict = ["leet", "code"]
输出: true
解释: 返回 true 因为 "leetcode" 可以由 "leet" 和 "code" 拼接成。
示例 2：

输入: s = "applepenapple", wordDict = ["apple", "pen"]
输出: true
解释: 返回 true 因为 "applepenapple" 可以由 "apple" "pen" "apple" 拼接成。
     注意，你可以重复使用字典中的单词。
```

- 需要求以最后位置结尾的字符是否满足能够被字典中的单词拼出，设为`dp[n-1]`
- `dp[i]`代表以 `i`位置为结尾的字符串是否满足要求。
- 假设前面的每个位置结尾的子字符的满足情况已知，则当前结束位置为`i`的字符是否满足情况取决于 ；即状态转移方程，`dp[i] == 1`当且仅当以下条件满足其中一个：
  -  遍历之前每一个存储的`dp`值，如果`dp[j]`满足要求，则判断`s[j+1:i+1]`的字符是否满足要求
  - `s[:i+1]`是否在单词表中
- 初始化`dp[i] = 0`

```python
class Solution(object):
    def wordBreak(self, s, wordDict):
        """
        :type s: str
        :type wordDict: List[str]
        :rtype: bool
        """
        n = len(s)
        dp = [False] * n

        for i in range(n):
            #求dp[i]
            dp_temp = False
            for j in range(i):
                if(dp[j] == 1):
                    if(s[j+1:i+1] in wordDict):
                        dp_temp = True
            if(s[:i+1] in wordDict):
                dp_temp = True
            dp[i] = dp_temp
        
        return dp[n-1]
```



## 29. **最长递增子序列** ⚠️

> 给你一个整数数组 `nums` ，找到其中最长严格递增子序列的长度。
>
> **子序列** 是由数组派生而来的序列，删除（或不删除）数组中的元素而不改变其余元素的顺序。例如，`[3,6,2,7]` 是数组 `[0,3,1,6,2,2,7]` 的子序列。
>
> **示例 1：**
>
> ```
> 输入：nums = [10,9,2,5,3,7,101,18]
> 输出：4
> 解释：最长递增子序列是 [2,3,7,101]，因此长度为 4 。
> ```
>
> **示例 2：**
>
> ```
> 输入：nums = [0,1,0,3,2,3]
> 输出：4
> ```
>
> **示例 3：**
>
> ```
> 输入：nums = [7,7,7,7,7,7,7]
> 输出：1
> ```

- `dp[i]`代表当前位置数为结尾的 **最大递增子序列的长度**，
- 之前的所有 `dp[:i]`已知的前提下,当前的`dp[i]`可以通过之前的值得到，
  - 即遍历之前的数，找到小于当前数的位置`j`，此时潜在的 `_dp[i] = dp[j]+1`
  - 最后去 `dp`数组的最大值，即找到 **每个位置为结尾的最大递增子序列** 的最大值
  - 取所有潜在值的最大值即可，O(N^2),O(N)
- 初始化 `dp[i] = 1`

```python
class Solution(object):
    def lengthOfLIS(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        n = len(nums)
        dp = [1] * n
        

        for i in range(1,n):
            #求解dp[i]
            temp = 1
            for j in range(i):
                if(nums[i] > nums[j]):
                    temp = max(temp,dp[j]+1)
            dp[i] = temp
        return max(dp)
        
```











# 面试 

## 1. 最大子数组和

> 给你一个整数数组 `nums` ，请你找出一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。
>
> **子数组**是数组中的一个连续部分。
>
>  
>
> **示例 1：**
>
> ```
> 输入：nums = [-2,1,-3,4,-1,2,1,-5,4]
> 输出：6
> 解释：连续子数组 [4,-1,2,1] 的和最大，为 6 。
> ```
>
> **示例 2：**
>
> ```
> 输入：nums = [1]
> 输出：1
> ```
>
> **示例 3：**
>
> ```
> 输入：nums = [5,4,-1,7,8]
> 输出：23
> ```

- 动态规划

  - 初始化为第一个数字

  - **当前位置为结尾的最大和** = max(**前一个位置为结尾的最大和** + **当前位置数**，**当前位置数**)

    ```python
    class Solution(object):
        def maxSubArray(self, nums):
            """
            :type nums: List[int]
            :rtype: int
            """
            dp0 = nums[0]
            maxres = nums[0]
    
            for num in nums[1:]:
                dp0 = max(num,num+dp0)
                maxres = max(maxres,dp0)
    
            return maxres  
    ```

    

## 2. 删除一次得到子数组最大和

> 给你一个整数数组，返回它的某个 **非空** 子数组（连续元素）在执行一次可选的删除操作后，所能得到的最大元素总和。换句话说，你可以从原数组中选出一个子数组，并可以决定要不要从中删除一个元素（只能删一次哦），（删除后）子数组中至少应当有一个元素，然后该子数组（剩下）的元素总和是所有子数组之中最大的。
>
> 注意，删除一个元素后，子数组 **不能为空**。
>
>  
>
> **示例 1：**
>
> ```
> 输入：arr = [1,-2,0,3]
> 输出：4
> 解释：我们可以选出 [1, -2, 0, 3]，然后删掉 -2，这样得到 [1, 0, 3]，和最大。
> ```
>
> **示例 2：**
>
> ```
> 输入：arr = [1,-2,-2,3]
> 输出：3
> 解释：我们直接选出 [3]，这就是最大和。
> ```
>
> **示例 3：**
>
> ```
> 输入：arr = [-1,-1,-1,-1]
> 输出：-1
> 解释：最后得到的子数组不能为空，所以我们不能选择 [-1] 并从中删去 -1 来得到 0。
>      我们应该直接选择 [-1]，或者选择 [-1, -1] 再从中删去一个 -1。
> ```

- 一维动态规划:两个变量分别记录: **前一个位置为结尾的最大和** 以及 **前一个位置为结尾的最多删除一个元素的最大和**
  - 状态转移方程：
    - **当前位置为结尾的最多删除一个元素的最大和**：比较**删除当前值**，**删除前面的某个值**，**当前值** 三种情况谁更大。
    - **当前位置为结尾的最大和** =  max(**前一个位置为结尾的最大和**+num,num)
  - 因此需要先更新下面的状态转移方程。
  - 每次比较: **当前位置为结尾的最多删除一个元素的最大和** ，**当前位置的数大小** 来更新结果。



```python
class Solution:
    def maximumSum(self, arr: List[int]) -> int:
        '''
            每次两种选择:
            删除的元素在前面
            删除的元素就是arr[i]
        '''
        presum_full = arr[0]
        presum = arr[0]

        res = arr[0]

        for i in range(1,len(arr)):
            
            presum = max(presum_full,presum+arr[i],arr[i])
            presum_full = max(presum_full+arr[i],arr[i])
            res = max(res,presum)

        return res
```









# 面试150题

## 23. 合并两个有序数组

> 给你两个按 **非递减顺序** 排列的整数数组 `nums1` 和 `nums2`，另有两个整数 `m` 和 `n` ，分别表示 `nums1` 和 `nums2` 中的元素数目。
>
> 请你 **合并** `nums2` 到 `nums1` 中，使合并后的数组同样按 **非递减顺序** 排列。
>
> **注意：**最终，合并后数组不应由函数返回，而是存储在数组 `nums1` 中。为了应对这种情况，`nums1` 的初始长度为 `m + n`，其中前 `m` 个元素表示应合并的元素，后 `n` 个元素为 `0` ，应忽略。`nums2` 的长度为 `n` 。
>
> 示例 1：
>
> 输入：nums1 = [1,2,3,0,0,0], m = 3, nums2 = [2,5,6], n = 3
> 输出：[1,2,2,3,5,6]
> 解释：需要合并 [1,2,3] 和 [2,5,6] 。
> 合并结果是 [1,2,2,3,5,6] ，其中斜体加粗标注的为 nums1 中的元素。

### 思路一：双指针

- 中间存储数组：`merged[m+n]`

- 遍历 `nums1` 与 `nums2`谁小就放入中间数组

- 复制到 `nums1`中

- T:O(M+N),S:O(M+N)

-  

  ```java
  public static void solution(int[] nums1,int m,int[] nums2,int n) {
          int i = 0;
          int j = 0;
          int[] merged = new int[m+n];
          int k = 0;
          while(i < m && j < n){
              // nums 小
              if(nums1[i] <= nums2[j]){
                  merged[k] = nums1[i];
                  k = k + 1;
                  i = i + 1;
              }
              // nums2小
              else{
                  merged[k] = nums2[j];
                  k = k + 1;
                  j = j + 1;
              }
          }
          while( i < m){
              merged[k] = nums1[i];
              k = k + 1;
              i = i + 1;
          }
          while( j < n){
              merged[k] = nums2[j];
              k = k + 1;
              j = j + 1;
          }
          k = 0;
          while(k<m+n){
              nums1[k] = merged[k];
              k = k + 1;
          }
      }
  ```

  



### 思路二：**逆向双指针**

- 双指针需要一个额外的 `merged[m+n]`数组是为了防止直接加入到 `nums1`中会被覆盖。

- `nums1`数组后面有空余的位置，从大到小遍历，即从后插入不会被覆盖。

- T:O(M+N),S:O(1)

- ```java
  public static void solution1(int[] nums1,int m,int[] nums2,int n) {
          int i = m-1;
          int j = n-1;
          int k = m+n-1;
          
          while(i >= 0 && j >= 0){
              if(nums1[i] >= nums2[j]){
                  nums1[k] = nums1[i];
                  k = k - 1;
                  i = i - 1;
              }
              else{
                  nums1[k] = nums2[j];
                  k = k - 1;
                  j = j - 1;
              }
          }
  
          while( j >= 0){
              nums1[k] = nums2[j];
              k = k - 1;
              j = j - 1;
          }
      }
  ```

## 24. 移除元素

> 给你一个数组 `nums` 和一个值 `val`，你需要 **[原地](https://baike.baidu.com/item/原地算法)** 移除所有数值等于 `val` 的元素，并返回移除后数组的新长度。
>
> 不要使用额外的数组空间，你必须仅使用 `O(1)` 额外空间并 **[原地 ](https://baike.baidu.com/item/原地算法)修改输入数组**。
>
> 元素的顺序可以改变。你不需要考虑数组中超出新长度后面的元素。
>
> 
>
> 输入：nums = [3,2,2,3], val = 3
> 输出：2, nums = [2,2]
> 解释：函数应该返回新的长度 2, 并且 nums 中的前两个元素均为 2。你不需要考虑数组中超出新长度后面的元素。例如，函数返回的新长度为 2 ，而 nums = [2,2,3,3] 或 nums = [2,2,0,0]，也会被视作正确答案。

- 遍历 `fast` 指针，判断是否为需要移除的元素

  -  `nums[fast] == val`:向后移动 `fast`指针

  -  `nums[fast] != val`：将当前位置元素移动到 `pos`处，并向后移动 `fast`指针

- ```java
  public static int solution(int[] nums,int val) {
          int pos = 0;
          int fast = 0;
          while(fast<nums.length){
              if(nums[fast] != val){
                  nums[pos] = nums[fast];
                  pos = pos + 1;
                  fast = fast + 1;
              }
              else{
                  fast = fast + 1;
              }
          } 
          return pos;                          
      }
  ```

## 25. 删除有序数组的重复项

> 给你一个 **非严格递增排列** 的数组 `nums` ，请你**[ 原地](http://baike.baidu.com/item/原地算法)** 删除重复出现的元素，使每个元素 **只出现一次** ，返回删除后数组的新长度。元素的 **相对顺序** 应该保持 **一致** 。然后返回 `nums` 中唯一元素的个数。
>
> 考虑 `nums` 的唯一元素的数量为 `k` ，你需要做以下事情确保你的题解可以被通过：
>
> - 更改数组 `nums` ，使 `nums` 的前 `k` 个元素包含唯一元素，并按照它们最初在 `nums` 中出现的顺序排列。`nums` 的其余元素与 `nums` 的大小不重要。
> - 返回 `k` 。
>
> 示例 1：
>
> 输入：nums = [1,1,2]
> 输出：2, nums = [1,2,_]
> 解释：函数应该返回新的长度 2 ，并且原数组 nums 的前两个元素被修改为 1, 2 。不需要考虑数组中超出新长度后面的元素。

- 判断是否重复: 由于数组是有序数组，因此判断当前位置和前一个位置的值是否相等即可判定是否重复

- 如果不重复，则原地覆盖慢指针位置的值；移动快指针，慢指针。

- 如果重复，移动快指针。

- ```python
  class Solution(object):
      def removeDuplicates(self, nums):
          """
          :type nums: List[int]
          :rtype: int
          """
          fast = 1
          slow = 1
          n = len(nums)
          k = 1
  
          while(fast < n):
              if(nums[fast] != nums[fast-1]):
                  nums[slow] = nums[fast]
                  k = k + 1
                  slow = slow + 1
                  fast = fast + 1
              else:
                  fast = fast + 1
          return k
  
  ```
  
  



## 26. 删除有序数组的重复项Ⅱ

> 给你一个有序数组 `nums` ，请你**[ 原地](http://baike.baidu.com/item/原地算法)** 删除重复出现的元素，使得出现次数超过两次的元素**只出现两次** ，返回删除后数组的新长度。
>
> 不要使用额外的数组空间，你必须在 **[原地 ](https://baike.baidu.com/item/原地算法)修改输入数组** 并在使用 O(1) 额外空间的条件下完成。
>
> 输入：nums = [1,1,1,2,2,3]
> 输出：5, nums = [1,1,2,2,3]
> 解释：函数应返回新长度 length = 5, 并且原数组的前五个元素被修改为 1, 1, 2, 2, 3。 不需要考虑数组中超出新长度后面的元素。

### 快慢指针

- 快指针遍历整个数组

  - 在找到重复三次的位置继续往前
  - 未重复的复制，将快指针所指位置的值复制到慢指针的位置。
  - 在填充之前，需要保存 `fast-2` 的值防止被覆盖

  慢指针指向下一个需要填充的位置

  ```python
  class Solution(object):
      def removeDuplicates(self, nums):
          """
          :type nums: List[int]
          :rtype: int
          """
          n = len(nums)
          fast = 2
          slow = 2
          flag_num = nums[fast-2]
          k = 2
          if(n < 3):
              return n
          while(fast < n):
              if(nums[fast] == flag_num):
                  fast = fast + 1
              else:
                  flag_num = nums[fast-1]
                  nums[slow] = nums[fast]
                  slow = slow + 1
                  fast = fast + 1
                  k = k + 1
          return k
  ```

  

## 27. **寻找重复数** ⚠️

> 给定一个包含 `n + 1` 个整数的数组 `nums` ，其数字都在 `[1, n]` 范围内（包括 `1` 和 `n`），可知至少存在一个重复的整数。
>
> 假设 `nums` 只有 **一个重复的整数** ，返回 **这个重复的数** 。
>
> 你设计的解决方案必须 **不修改** 数组 `nums` 且只用常量级 `O(1)` 的额外空间。

### 二分法

- 重复的数字一定在`1~n`内，因此可以通过二分`1~n`寻找这个数
- 核心判定条件，对于二分的一个数字，如果遍历数组内
  - 小于等于该数字的个数 <= 该数字，则重复数在右侧
  - 否则在左侧或者等于该数字

- 否则重复的数在mid的右边

```python
class Solution(object):
    def findDuplicate(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        n = len(nums)
        left = 1
        right = n
        while(left < right):
            mid = (left + right) // 2
            cnt = 0
            for num in nums:
                if(num <= mid):
                    cnt = cnt + 1
            if(cnt <= mid):
                left = mid + 1
            else:
                right = mid
        return left
```



### 双指针

- 由于数字的范围是 `1-n`,总共有`n+1`个位置，则一定存在重复数，且限定只有一个数字重复。
- 将每个数字看作是一个节点，并指向下一个数的位置；则一定**存在两个位置的数相同，即指向同一个位置**，此时存在环。
  - 两个数分别从第一个位置和第二个位置出发，在环中某个位置相遇。
  - `a` 代表从开始位置到 **同一个位置** 的距离，`L`代表环的长度，`b`代表 **同一个位置** 到 **相遇位置的距离**，`c`代表 **相遇位置** 返回到 **指向的同一个位置**的距离
  - `2(a+b) = a + b + kL; c+b=L`  ---> `a = (k-1)L + c`
  - 慢指针 从第一个位置出发 和 快指针从相遇位置出发会在 环的起点相遇。
- 两个核心点
  - 指针从第一个数的指针 和 第二个数的指针开始遍历，直到**两个指针指向**同一个数
  - 慢指针从`0`开始遍历，快指针从指向位置开始。相遇点即为重复数。

```python
class Solution(object):
    def findDuplicate(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        slow = nums[0]
        fast = nums[nums[0]]

        while(slow != fast):
            slow = nums[slow]
            fast = nums[nums[fast]]
        
        slow = 0
        while(slow != fast):
            slow = nums[slow]
            fast = nums[fast]
        
        return slow
```





## 多数元素

> 给定一个大小为 `n` 的数组 `nums` ，返回其中的多数元素。多数元素是指在数组中出现次数 **大于** `⌊ n/2 ⌋` 的元素。
>
> 你可以假设数组是非空的，并且给定的数组总是存在多数元素。
>
> **示例 1：**
>
> ```
> 输入：nums = [3,2,3]
> 输出：3
> ```
>
> **示例 2：**
>
> ```
> 输入：nums = [2,2,1,1,1,2,2]
> 输出：2
> ```

- 字典存储：O(n)，O(n)

- 排序取值; O(n*log(n))

- 投票方法: O(N), O(1)。核心思想:众数的个数比其他数都多，如果众数为1，其他数为-1，则最终的和大于0

  - 一次遍历数组，如果 `cnt == 0`，则取**当前数为候选众数**
  - 如果当前数 等于 候选数，则 `cnt+1`,否则`cnt-1`

- 最终得到的候选数即为所求

  ```python
  class Solution(object):
      def majorityElement(self, nums):
          """
          :type nums: List[int]
          :rtype: int
          """
          cnt = 0
          maj = 0
  
          for num in nums:
              if(cnt == 0):
                  maj = num
                  cnt = cnt + 1
              else:
                  if(num == maj):
                      cnt = cnt  + 1
                  else:
                      cnt = cnt - 1
          
          return maj
          
  ```




## 28.  对角线上不同值的数量差

> 给你一个下标从 `0` 开始、大小为 `m x n` 的二维矩阵 `grid` ，请你求解大小同样为 `m x n` 的答案矩阵 `answer` 。
>
> 矩阵 `answer` 中每个单元格 `(r, c)` 的值可以按下述方式进行计算：
>
> - 令 `topLeft[r][c]` 为矩阵 `grid` 中单元格 `(r, c)` 左上角对角线上 **不同值** 的数量。
> - 令 `bottomRight[r][c]` 为矩阵 `grid` 中单元格 `(r, c)` 右下角对角线上 **不同值** 的数量。
>
> 然后 `answer[r][c] = |topLeft[r][c] - bottomRight[r][c]|` 。
>
> 返回矩阵 `answer` 。
>
> **矩阵对角线** 是从最顶行或最左列的某个单元格开始，向右下方向走到矩阵末尾的对角线。
>
> 如果单元格 `(r1, c1)` 和单元格 `(r, c) `属于同一条对角线且 `r1 < r` ，则单元格 `(r1, c1)` 属于单元格 `(r, c)` 的左上对角线。类似地，可以定义右下对角线。

- 朴素做法:模拟整个计算过程，遍历每个位置。计算对应的**左上角的不同元素个数**，以及**右下角的不同元素个数**

  ```python
  class Solution:
      def differenceOfDistinctValues(self, grid: List[List[int]]) -> List[List[int]]:
          m  = len(grid)
          n = len(grid[0])
  
          answer = [[0 for _ in range(n)] for _ in range(m)]
          for i in range(m):
              for j in range(n):
                  leftup = set()
                  rightdown =set()
                  k = i-1
                  v = j-1
                  while(k>=0 and v >= 0):
                      leftup.add(grid[k][v])
                      k = k - 1
                      v = v - 1
                  k = i+1
                  v = j+1 
                  while(k<m and v<n):
                      rightdown.add(grid[k][v])
                      k = k + 1
                      v = v + 1
                  answer[i][j] = abs(len(leftup) - len(rightdown))
          # print(answer)
          return answer
  ```

- 先提前将每个位置的 左上角不重复个数 和右下角不重复个数先保留下来

- 第一行第一列往右下角遍历保留 左上角不重复个数

- 最后一行最后一列往左上角遍历保留 右下角不重复个数

  ```python
  class Solution:
      def differenceOfDistinctValues(self, grid: List[List[int]]) -> List[List[int]]:
          m  = len(grid)
          n = len(grid[0])
  
          answer = [[0 for _ in range(n)] for _ in range(m)]
  
          # 第一列
          for i in range(m):
              j = 0
              upleft = set()
              while( i < m  and j < n):
                  answer[i][j] = len(upleft)
                  upleft.add(grid[i][j])
                  i = i + 1
                  j = j + 1
  
          # 第一行
          for j in range(1,n):
              i = 0
              upleft = set()
              while(i<m and j < n):
                  answer[i][j] = len(upleft)
                  upleft.add(grid[i][j])
                  i = i + 1
                  j = j + 1
          
          for i in range(m):
              j = n-1
              rightdown = set()
              while(i>=0 and j>=0):
                  answer[i][j] = abs(answer[i][j] - len(rightdown))
                  rightdown.add(grid[i][j])
                  i = i - 1
                  j = j - 1
          for j in range(n-1):
              i = m-1
              rightdown = set()
              while(i>=0 and j>=0):
                  answer[i][j] = abs(answer[i][j] - len(rightdown))
                  rightdown.add(grid[i][j])
                  i = i - 1
                  j = j - 1
          
          # print(answer)
          return answer
  ```



## 29.轮转数组









#  每日一题

### 1. 对角线上不同值的数量差

> 给你一个下标从 `0` 开始、大小为 `m x n` 的二维矩阵 `grid` ，请你求解大小同样为 `m x n` 的答案矩阵 `answer` 。
>
> 矩阵 `answer` 中每个单元格 `(r, c)` 的值可以按下述方式进行计算：
>
> - 令 `topLeft[r][c]` 为矩阵 `grid` 中单元格 `(r, c)` 左上角对角线上 **不同值** 的数量。
> - 令 `bottomRight[r][c]` 为矩阵 `grid` 中单元格 `(r, c)` 右下角对角线上 **不同值** 的数量。
>
> 然后 `answer[r][c] = |topLeft[r][c] - bottomRight[r][c]|` 。
>
> 返回矩阵 `answer` 。
>
> **矩阵对角线** 是从最顶行或最左列的某个单元格开始，向右下方向走到矩阵末尾的对角线。
>
> 如果单元格 `(r1, c1)` 和单元格 `(r, c) `属于同一条对角线且 `r1 < r` ，则单元格 `(r1, c1)` 属于单元格 `(r, c)` 的左上对角线。类似地，可以定义右下对角线。

- 朴素做法:模拟整个计算过程，遍历每个位置。计算对应的**左上角的不同元素个数**，以及**右下角的不同元素个数**

  ```python
  class Solution:
      def differenceOfDistinctValues(self, grid: List[List[int]]) -> List[List[int]]:
          m  = len(grid)
          n = len(grid[0])
  
          answer = [[0 for _ in range(n)] for _ in range(m)]
          for i in range(m):
              for j in range(n):
                  leftup = set()
                  rightdown =set()
                  k = i-1
                  v = j-1
                  while(k>=0 and v >= 0):
                      leftup.add(grid[k][v])
                      k = k - 1
                      v = v - 1
                  k = i+1
                  v = j+1 
                  while(k<m and v<n):
                      rightdown.add(grid[k][v])
                      k = k + 1
                      v = v + 1
                  answer[i][j] = abs(len(leftup) - len(rightdown))
          # print(answer)
          return answer
  ```

- 先提前将每个位置的 左上角不重复个数 和右下角不重复个数先保留下来

- 第一行第一列往右下角遍历保留 左上角不重复个数

- 最后一行最后一列往左上角遍历保留 右下角不重复个数

  ```python
  class Solution:
      def differenceOfDistinctValues(self, grid: List[List[int]]) -> List[List[int]]:
          m  = len(grid)
          n = len(grid[0])
  
          answer = [[0 for _ in range(n)] for _ in range(m)]
  
          # 第一列
          for i in range(m):
              j = 0
              upleft = set()
              while( i < m  and j < n):
                  answer[i][j] = len(upleft)
                  upleft.add(grid[i][j])
                  i = i + 1
                  j = j + 1
  
          # 第一行
          for j in range(1,n):
              i = 0
              upleft = set()
              while(i<m and j < n):
                  answer[i][j] = len(upleft)
                  upleft.add(grid[i][j])
                  i = i + 1
                  j = j + 1
          
          for i in range(m):
              j = n-1
              rightdown = set()
              while(i>=0 and j>=0):
                  answer[i][j] = abs(answer[i][j] - len(rightdown))
                  rightdown.add(grid[i][j])
                  i = i - 1
                  j = j - 1
          for j in range(n-1):
              i = m-1
              rightdown = set()
              while(i>=0 and j>=0):
                  answer[i][j] = abs(answer[i][j] - len(rightdown))
                  rightdown.add(grid[i][j])
                  i = i - 1
                  j = j - 1
          
          # print(answer)
          return answer
  ```



### 2. [k-avoiding 数组的最小总和](https://leetcode.cn/problems/determine-the-minimum-sum-of-a-k-avoiding-array/)

> 给你两个整数 `n` 和 `k` 。
>
> 对于一个由 **不同** 正整数组成的数组，如果其中不存在任何求和等于 k 的不同元素对，则称其为 **k-avoiding** 数组。
>
> 返回长度为 `n` 的 **k-avoiding** 数组的可能的最小总和。
>
> **示例 1：**
>
> ```
> 输入：n = 5, k = 4
> 输出：18
> 解释：设若 k-avoiding 数组为 [1,2,4,5,6] ，其元素总和为 18 。
> 可以证明不存在总和小于 18 的 k-avoiding 数组。
> ```
>
> **示例 2：**
>
> ```
> 输入：n = 2, k = 6
> 输出：3
> 解释：可以构造数组 [1,2] ，其元素总和为 3 。
> 可以证明不存在总和小于 3 的 k-avoiding 数组。 
> ```



# 面试

## 1.最大子数组和

> 给你一个整数数组 `nums` ，请你找出一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。
>
> **子数组**是数组中的一个连续部分。
>
>  
>
> **示例 1：**
>
> ```
> 输入：nums = [-2,1,-3,4,-1,2,1,-5,4]
> 输出：6
> 解释：连续子数组 [4,-1,2,1] 的和最大，为 6 。
> ```
>
> **示例 2：**
>
> ```
> 输入：nums = [1]
> 输出：1
> ```
>
> **示例 3：**
>
> ```
> 输入：nums = [5,4,-1,7,8]
> 输出：23
> ```

- 动态规划 + 前缀和

- `dp[i]`代表的是以 `i` 位置为**结尾的最大子数组和**

  - 因此每次动态规划的转移方程为: `dp[i] = (max(dp[i-1]) + nums[i],nums[i]) `
  - 即比较以**上一个位置为结尾的最大子数组**和加**当前数** 和 **当前数**单独作为子数组谁更大。
  - 只需要保存**上一个位置为结尾的最大子数组之和**即可， O(N)+O(1)

  ```python
  class Solution:
      def maxSubArray(self, nums: List[int]) -> int:
          pre = 0
          max_res= nums[0]
          for num in nums:
              pre = max(pre+num,num)
              max_res = max(max_res,pre)
          
          return max_res
  ```



## 2.删除一次得到子数组最大和



 
