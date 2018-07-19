#### 冒泡排序

对一个无序的序列进行排序时，每次去当前没有排序的数据于下一个数据进行比较，大于则进行交换，然后让下一个数据成为当前数据，再于下一个数据进行比较交换，知道把当前排序序列最大的数据排到最后的位置上。则最后一个位置的数据是有序的，接着对剩下的序列进行相同的排序方式，直到所有数据都进行了排序。

```
    public static void BubbleSort(int[] arr) {
        int temp;
        int last = arr.length - 1;
        int k = arr.length - 1;
        for (int i = 0; i < arr.length; i++) {
            for (int j = 1; j < last; j++) {
                if (arr[j] < arr[j - 1]) {
                    temp = arr[j];
                    arr[j] = arr[j - 1];
                    arr[j - 1] = temp;
                    k = j;
                }
            }
            last = k;
        }
    }
```

#### 选择排序

每次找出当前排序序列中最小或最大的值，将它交换到当前序列的第一个位置，知道所有序列的最大或最小值都排序完毕。

冒泡排序比较和交换的时间复杂度都是O\($$N^2$$\)，而选择排序虽然比较时间复杂度也是O\($$N^2$$\)，但是交换的事件复杂度降为O\(N\)。

```
    public static void chooseSort(int[] arr) {
        int min;
        int temp;
        for (int i = 0; i < arr.length; i++) {
            min = i;
            for (int j = i + 1; j < arr.length; j++) {
                if (arr[min] > arr[j]) {
                    min = j;
                }
            }
            if (min != i) {
                temp = arr[min];
                arr[min] = arr[i];
                arr[i] = temp;
            }
        }
    }
```



