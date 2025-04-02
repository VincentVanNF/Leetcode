

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

  

## 27. 寻找重复数

> 给定一个包含 `n + 1` 个整数的数组 `nums` ，其数字都在 `[1, n]` 范围内（包括 `1` 和 `n`），可知至少存在一个重复的整数。
>
> 假设 `nums` 只有 **一个重复的整数** ，返回 **这个重复的数** 。
>
> 你设计的解决方案必须 **不修改** 数组 `nums` 且只用常量级 `O(1)` 的额外空间。

### 二分法

- 思想：一个在该数组中的一个数mid，如果数组中小于mid的数量 > mid；则重复的数**在mid左边（包含mid）**
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

> 给定一个整数数组 `nums`，将数组中的元素向右轮转 `k` 个位置，其中 `k` 是非负数。
>
>  
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
> ```python
> 输入：nums = [-1,-100,3,99], k = 2
> 输出：[3,99,-1,-100]
> 解释: 
> 向右轮转 1 步: [99,-1,-100,3]
> 向右轮转 2 步: [3,99,-1,-100]
> ```









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



 
