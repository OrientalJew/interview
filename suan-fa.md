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

#### 直接插入排序

假设当前序列的前n个数据是有序的，接下来取第n+1个数据与该序列进行比较，插入该序列合适的位置，则有序序列扩展为n+1个，

不断重复该过程，直到序列的剩余所有数据都插入到有序序列中；

比较和交换的次数相比冒泡减少一半，比较操作比选择排序更快，因为不需要遍历每一个元素，只需要比较前n个比当前元素大的元素即可。在数据量巨大时，效率不如选择排序，因为选择排序每次遍历只需要交换一次，而插入排序需要多次错位赋值操作，来腾出合适的插入位置。

```
    public static void insertSort(int[] arr) {
        int temp;
        for (int i = 1; i < arr.length; i++) {
            temp = arr[i];

            int j = i - 1;
            for (; j >= 0; j--) {
                if (arr[j] > temp) {
                    arr[j + 1] = arr[j];
                } else {
                    break;
                }
            }
            // for循环最后还会减去1，所以此处需要补1
            arr[j+1] = temp;
        }
    }
```



