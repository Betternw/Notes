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