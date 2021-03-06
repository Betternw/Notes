1. 排序算法
    * 选择排序——每次选择当前数后面数字中的最小值与当前数交换位置
    ```java
          public class Selection<T extends Comparable<T>> extends Sort<T> {

          @Override
          public void sort(T[] nums) {
              int N = nums.length;
              for (int i = 0; i < N - 1; i++) {
                  int min = i;
                  for (int j = i + 1; j < N; j++) {
                      if (less(nums[j], nums[min])) {
                          min = j;
                      }
                  }
                  swap(nums, i, min);
              }
          }
      }
    ```
    * 冒泡排序——每次两两比较然后交换。每一轮的循环数字减少一个
    ```java
    public class Bubble<T extends Comparable<T>> extends Sort<T> {

    @Override
    public void sort(T[] nums) {
        int N = nums.length;
        boolean isSorted = false;
        for (int i = N - 1; i > 0 && !isSorted; i--) {
            isSorted = true;
            for (int j = 0; j < i; j++) {
                if (less(nums[j + 1], nums[j])) {
                    isSorted = false;
                    swap(nums, j, j + 1);
                }
            }
        }}}
    ```
    * 快速排序
    ```java
    public static void quick(int[] numbers) {
    if (numbers.length > 0) // 查看数组是否为空
    {
        quickSort(numbers, 0, numbers.length - 1);
          }
      }
      public static void quickSort(int[] numbers, int low, int high) {
          if (low >= high) {
              return;
          }
          int middle = getMiddle(numbers, low, high); // 将numbers数组进行一分为二
          quickSort(numbers, low, middle - 1); // 对低字段表进行递归排序
          quickSort(numbers, middle + 1, high); // 对高字段表进行递归排序
      }
      public static int getMiddle(int[] numbers, int low, int high) {
          int temp = numbers[low]; // 数组的第一个作为中轴
          while (low < high) {
              while (low < high && numbers[high] > temp) {
                  high--;
              }
              numbers[low] = numbers[high];// 比中轴小的记录移到低端
              while (low < high && numbers[low] < temp) {
                  low++;
              }
              numbers[high] = numbers[low]; // 比中轴大的记录移到高端
          }
          numbers[low] = temp; // 中轴记录到尾
          return low; // 返回中轴的位置
      }
    ```