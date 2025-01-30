# Sorting

1.  Consider the following simple table of enrolments, sorted by course code:

    | course   | name | program |
    | -------- | ---- | ------- |
    | COMP1927 | Jane | 3970    |
    | COMP1927 | John | 3978    |
    | COMP1927 | Pete | 3978    |
    | MATH1231 | John | 3978    |
    | MATH1231 | Adam | 3970    |
    | PSYC1011 | Adam | 3970    |
    | PSYC1011 | Jane | 3970    |

    Now we wish to sort it by student name, so that we can easily see what courses each student is studying. Show an example of what the final array would look like if

    1.  we used a *stable* sorting algorithm

        | course   | name | program |
        | -------- | ---- | ------- |
        | MATH1231 | Adam | 3970    |
        | PSYC1011 | Adam | 3970    |
        | COMP1927 | Jane | 3970    |
        | PSYC1011 | Jane | 3970    |
        | COMP1927 | John | 3978    |
        | MATH1231 | John | 3978    |
        | COMP1927 | Pete | 3978    |
    2.  we used an *unstable* sorting algorithm

        | course   | name | program |
        | -------- | ---- | ------- |
        | PSYC1011 | Adam | 3970    |
        | MATH1231 | Adam | 3970    |
        | PSYC1011 | Jane | 3970    |
        | COMP1927 | Jane | 3970    |
        | COMP1927 | John | 3978    |
        | MATH1231 | John | 3978    |
        | COMP1927 | Pete | 3978    |

2.  Compare the performance of bubble sort and insertion sort on the following types of input:

    -   Sorted (e.g., $[1,2,3,…,n]$​)
        -   Bubble sort: $\mathcal{O}(n)$ linear pass with no swaps
        -   Insertion sort: $\mathcal{O}(n)$ each of the $n$ element gets inserted at the end of the sorted sub-array in constant time
    -   Reverse-sorted (e.g., $[n,n−1,n−2,…,1]$​)
        -   Bubble sort: $\mathcal{O}(n^2)$ we have $n-1$ swaps to get the largest item to the end of the array, then $n-2$ swaps for the next largest, and so on. So we have a triangular sum of $\frac{n(n-1)}{2}$ swaps, hence $n^2$ is the dominant term.
        -   Insertion sort: $\mathcal{O}(n^2)$ the first item to get inserted into the sorted sub-array doesn't require any comparisons. The next element requires $1$ comparison, then $2$, and so on until the final element requires $n-1$ comparisons. Again we have a triangular sum and $n^2$ is the dominant term.
    -   Sorted, except the first and last numbers are swapped (e.g., $[n,2,3,…,n−1,1]$​)
        -   Bubble sort: $\mathcal{O}(n^2)$ because it does $n-1$ swaps in the first iteration to get $n$ to the end, then for every other iteration it does 1 swap each time to shuffle $1$ forward once each time, repeating this ~$n$ times.
        -   Insertion sort:  $\mathcal{O}(n)$ because it does $n$ iterations where each time at most 2 comparisons and 1 swap are made, except for the final iteration where $n-1$ swaps are made, but $2\times \mathcal{O}(n)$ is still $\mathcal{O}(n)$. 

3.  Merge sort is typically implemented as follows:
    ```c
    void mergeSort(int A[], int lo, int hi) {
        if (lo >= hi) return;
        
        int mid = (lo + hi) / 2;
        mergeSort(A, lo, mid);
        mergeSort(A, mid + 1, hi);
        merge(A, lo, mid, hi);
    }
    
    void merge(int A[], int lo, int mid, int hi) {
        int nitems = hi - lo + 1;
        int *tmp = malloc(nitems * sizeof(int));
        
        int i = lo;
        int j = mid + 1;
        int k = 0;
        
        // scan both segments into tmp
        while (i <= mid && j <= hi) {
            if (A[i] <= A[j]) {
                tmp[k++] = A[i++];
            } else {
                tmp[k++] = A[j++];
            }
        }
        
        // copy items from unfinished segment
        while (i <= mid) tmp[k++] = A[i++];
        while (j <= hi)  tmp[k++] = A[j++];
        
        // copy items from tmp back to main array
        for (i = lo, k = 0; i <= hi; i++, k++) {
            A[i] = tmp[k];
        }
        free(tmp);
    }
    ```

    Suppose that at a particular point during the execution of the `mergeSort` function, the array `A` looks like this:

    ```
    { 1, 4, 5, 6, 7, 2, 3, 4, 7, 9 }
    ```

    Show how the call `merge(A, 0, 4, 9)` would merge the elements of the two sorted subarrays into a single sorted array.

    Setup before the loop:

    ```
    merge(A, 0, 4, 9):
       A = { 1, 4, 5, 6, 7, 2, 3, 4, 7, 9 }
             ^              ^       
             i              j
     tmp = { _, _, _, _, _, _, _, _, _, _ }
             ^
             k
    ```

    After first iteration of the loop:

    ```
       A = { 1, 4, 5, 6, 7, 2, 3, 4, 7, 9 }
                ^           ^       
                i           j
     tmp = { 1, _, _, _, _, _, _, _, _, _ }
                ^
                k
    ```

    After the second iteration:

    ```
       A = { 1, 4, 5, 6, 7, 2, 3, 4, 7, 9 }
                ^              ^       
                i              j
     tmp = { 1, 2, _, _, _, _, _, _, _, _ }
                   ^
                   k
    ```

    and so on...

    ```
       A = { 1, 4, 5, 6, 7, 2, 3, 4, 7, 9 }
                ^                 ^       
                i                 j
     tmp = { 1, 2, 3, _, _, _, _, _, _, _ }
                      ^
                      k
    ```

    ```
       A = { 1, 4, 5, 6, 7, 2, 3, 4, 7, 9 }
                   ^              ^       
                   i              j
     tmp = { 1, 2, 3, 4, _, _, _, _, _, _ }
                         ^
                         k
    ```

    ```
       A = { 1, 4, 5, 6, 7, 2, 3, 4, 7, 9 }
                   ^                 ^       
                   i                 j
     tmp = { 1, 2, 3, 4, 4, _, _, _, _, _ }
                            ^
                            k
    ```

    ```
       A = { 1, 4, 5, 6, 7, 2, 3, 4, 7, 9 }
                      ^              ^       
                      i              j
     tmp = { 1, 2, 3, 4, 4, 5, _, _, _, _ }
                               ^
                               k
    ```

    ```
       A = { 1, 4, 5, 6, 7, 2, 3, 4, 7, 9 }
                         ^           ^       
                         i           j
     tmp = { 1, 2, 3, 4, 4, 5, 6, _, _, _ }
                                  ^
                                  k
    ```

    ```
       A = { 1, 4, 5, 6, 7, 2, 3, 4, 7, 9 }
                            ^        ^       
                            i        j
     tmp = { 1, 2, 3, 4, 4, 5, 6, 7, _, _ }
                                     ^
                                     k
    ```

    The main loop ends now since $i > mid$. We then copy over the leftover elements from $j$ to $hi$.

    ```
       A = { 1, 4, 5, 6, 7, 2, 3, 4, 7, 9 }
                            ^              ^       
                            i              j
     tmp = { 1, 2, 3, 4, 4, 5, 6, 7, 7, 9 }
                                           ^
                                           k
    ```

    Then copy everything from $tmp$ into $A$ and return. So the final state of $A$ looks like `{ 1, 2, 3, 4, 4, 5, 6, 7, 7, 9 }`.

5.  An important operation in the quicksort algorithm is partition, which takes an element called the pivot, and reorganises the elements in the subarray `A[lo..hi]` such that all elements to the left of the pivot in the subarray are less than or equal to the pivot, and all elements to the right of the pivot in the subarray are greater than the pivot.

    Here is one possible implementation of partition:

    ```c
    int partition(int A[], int lo, int hi) {
        int pivot = A[lo];
    
        int l = lo + 1;
        int r = hi;
        while (true) {
            while (l < r && A[l] <= pivot) l++;
            while (l < r && A[r] >= pivot) r--;
            if (l == r) break;
            swap(A, l, r);
        }
    
        if (pivot < A[l]) l--;
        swap(A, lo, l);
        return l;
    }
    ```

    Show how the call `partition(A, 0, 9)` would partition each of these arrays. What index is returned by each of these calls?

    1.  `{ 5, 3, 9, 6, 4, 2, 9, 8, 1, 7 }`

        ```
        A after partition = { 4, 3, 1, 2, 5, 6, 9, 8, 9, 7 } returns index 4
        ```
    2.  `{ 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 }`

        ```
        A after partition = { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 } returns index 0
        ```
    3.  `{ 9, 8, 7, 6, 5, 4, 3, 2, 1, 0 }`

        ```
        A after partition = { 0, 8, 7, 6, 5, 4, 3, 2, 1, 9 } returns index 9
        ```

6.  Consider the following shell sort algorithm (based on Sedgewick's implementation):

    ```c
    void shellSort(int a[], int lo, int hi)
    {
        int h;
        for (h = 1; h <= (hi - lo) / 9; h = 3 * h + 1);
    
        for (; h > 0; h /= 3) {
            for (int i = lo + h; i <= hi; i++) {
                int val = a[i];
                int j = i;
                while (j >= lo + h && val < a[j - h]) {
                    a[j] = a[j - h];
                    j -= h;
                }
                a[j] = val;
            }
        }
    }
    ```

    Describe how this would sort an array containing the first 100 positive integers sorted in descending order (i.e. `{100, 99, 98, 97, 96, 95, ... , 1}`).

    ### Answer

    *   After the loop on line 4, $h = 13$​
    *   In the main loop on line 6, $h$ will be 13 then 4 then 1
        *   In the first iteration, insertion sort is performed on subsets of the array made up of every 13th elements. E.g. one subset will be indices $\set{0, 13, 26, 39, ...}$, another subset is $\set{1, 14, 27, 40, ...}$​, and so on.
        *   In the second iteration, insertion sort is performed on subsets of the array made up of every 4th elements. E.g. one subset will be indices $\set{0, 4, 8, 12, ...}$, another subset is $\set{1, 5, 9, 13, ...}$​, and so on.
        *   In the final iteration, normal insertion sort is performed on the entire subarray. We can expect fewer moves here than in normal insertion sort because the sortedness of the array would've been improved through the previous 2 passes.
