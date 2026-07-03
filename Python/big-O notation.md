# What is Big O Notation?

Big O Notation is a mathematical concept used in computer science to describe the performance or complexity of an algorithm. Specifically, it characterizes algorithms in terms of their time or space requirements relative to the input size.

# O(1) - Constant Time Complexity

O(1), also known as constant time complexity, describes an algorithm that always executes in the same amount of time, regardless of the size of the input data set.

In the worst-case scenario, O(1) algorithms will always take the same amount of time to execute, regardless of the size of the input data set. This makes them very efficient and desirable in many situations.

Example of a O(1) algorithms in pyhon:

```python
def get_first_element(arr):
    return arr[0]
```

```python
def get_middle_element(arr):
    return arr[len(arr) // 2]
```

```python
def get_last_element(arr):
    return arr[-1]
```

If the input array has 1,000,000 elements, the time taken to retrieve the first, middle, or last element will be the same as if the array had only 10 elements. The time complexity remains constant, O(1), regardless of the input size.

![alt text](NOTES/backend_studies%20(git)/Python/img/image.png)

---

# O(n) - Linear Time Complexity

O(n), also known as linear time complexity, describes an algorithm whose performance will grow linearly and in direct proportion to the size of the input data set.

In the worst-case scenario, O(n) algorithms will take time proportional to the size of the input data set. This means that if the input size doubles, the time taken to execute the algorithm will also double.

Example of a O(n) algorithms in Python:

```python
def find_max(arr):
    max_value = arr[0]
    for num in arr:
        if num > max_value:
            max_value = num
    return max_value
```

```python
def linear_search(arr, target):
    for index, value in enumerate(arr):
        if value == target:
            return index
    return -1
```

```python
def sum_elements(arr):
    total = 0
    for num in arr:
        total += num
    return total
```

If the input array has 1,000,000 elements, the time taken to find the maximum value, search for a target value, or sum all elements will be directly proportional to the size of the input array. The time complexity is O(n), meaning that as the input size increases, the time taken to execute the algorithm increases linearly.

![alt text](NOTES/backend_studies%20(git)/Python/img/image-1.png)

---

# O(n²) - Quadratic Time Complexity

O(n²), also known as quadratic time complexity, describes an algorithm whose performance is proportional to the square of the size of the input data set. This means that if the input size doubles, the time taken to execute the algorithm will quadruple.

Example of a O(n²) algorithms in Python:

```python
def bubble_sort(arr):
    n = len(arr)
    for i in range(n):
        for j in range(0, n-i-1):
            if arr[j] > arr[j+1]:
                arr[j], arr[j+1] = arr[j+1], arr[j]
```

```python
def selection_sort(arr):
    n = len(arr)
    for i in range(n):
        min_index = i
        for j in range(i+1, n):
            if arr[j] < arr[min_index]:
                min_index = j
        arr[i], arr[min_index] = arr[min_index], arr[i]
```

If the input array has 1,000 elements, the time taken to sort the array using bubble sort will be proportional to 1,000² = 1,000,000 operations. The time complexity is O(n²), meaning that as the input size increases, the time taken to execute the algorithm increases quadratically.

Notice that O(n²) algorithms can become inefficient for large input sizes, and alternative algorithms with better time complexity should be considered in such cases.

![alt text](NOTES/backend_studies%20(git)/Python/img/image-2.png)

---

# O(n*m) - Polynomial Time Complexity

This is a generalization of O(n²) and describes an algorithm whose performance is proportional to the product of the sizes of two input data sets. This means that if the size of one input set doubles, the time taken to execute the algorithm will also double, and if the size of both input sets doubles, the time taken will quadruple.

Very similar to O(n²), O(n*m) algorithms can become inefficient for large input sizes, and alternative algorithms with better time complexity should be considered in such cases.

Example of a O(n*m) algorithms in Python:

```python
def matrix_multiply(A, B):
    result = [[0 for _ in range(len(B[0]))] for _ in range(len(A))]
    for i in range(len(A)):
        for j in range(len(B[0])):
            for k in range(len(B)):
                result[i][j] += A[i][k] * B[k][j]
    return result
```

```python
def find_common_elements(arr1, arr2):
    common_elements = []
    for element1 in arr1:
        for element2 in arr2:
            if element1 == element2:
                common_elements.append(element1)
    return common_elements
```

With the matrix multiplication example, if matrix A has dimensions 100x200 and matrix B has dimensions 200x300, the time taken to multiply these matrices will be proportional to 100 * 300 = 30,000 operations. The time complexity is O(n*m), meaning that as the sizes of the input matrices increase, the time taken to execute the algorithm increases proportionally to the product of their sizes.

![alt text](NOTES/backend_studies%20(git)/Python/img/image-2.png)

(Same graphic as O(n²) because it is a generalization of O(n²))

---

# O(n³) - Cubic Time Complexity

Cubic time complexity, denoted as O(n³), describes an algorithm whose performance is proportional to the cube of the size of the input data set. This means that if the input size doubles, the time taken to execute the algorithm will increase eightfold.

That's because O(n³) algorithms have three nested loops, each iterating over the input data set. As a result, the time taken to execute the algorithm grows rapidly with increasing input size.

Example of a O(n³) algorithm in Python:

```python
def matrix_multiply_cubic(A, B):
    n = len(A)
    result = [[0 for _ in range(n)] for _ in range(n)]
    for i in range(n):
        for j in range(n):
            for k in range(n):
                result[i][j] += A[i][k] * B[k][j]
    return result
```

This function multiplies two square matrices A and B of size n x n. The three nested loops iterate over the rows and columns of the matrices, resulting in a time complexity of O(n³). As the size of the input matrices increases, the time taken to execute the algorithm increases cubically (3x).

![alt text](NOTES/backend_studies%20(git)/Python/img/image-2.png)

The same graphic can be used for O(n³) as well, as it is a generalization of O(n²) and O(n * m). The key difference is that O(n³) has three nested loops, while O(n²) has two nested loops, and O(n*m) has two nested loops with different input sizes. All of them growth closely related to the size of the input data set, but O(n³) grows much faster.

---

# O(log n) - Logarithmic Time Complexity

Logarithmic time complexity, denoted as O(log n), describes an algorithm whose performance grows logarithmically with the size of the input data set. This means that if the input size doubles, the time taken to execute the algorithm increases by a constant amount.

Example of a O(log n) algorithm in Python:

```python
def binary_search(arr, target):
    left, right = 0, len(arr) - 1
    while left <= right:
        mid = (left + right) // 2
        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    return -1
```

This function performs a binary search on a sorted array to find the index of a target value. The algorithm repeatedly divides the search interval in half, resulting in a time complexity of O(log n). As the size of the input array increases, the time taken to execute the algorithm increases logarithmically.

![alt text](NOTES/backend_studies%20(git)/Python/img/image-3.png)

This algorithm is much more efficient than linear search (O(n)) for large input sizes, as it reduces the number of comparisons needed to find the target value.

---

# O(n log n) - Linearithmic Time Complexity

Linearithmic time complexity, denoted as O(n log n), describes an algorithm whose performance grows in proportion to the product of the input size and the logarithm of the input size. This means that if the input size doubles, the time taken to execute the algorithm slightly more than doubles.

Example of a O(n log n) algorithm in Python:

```python
def merge_sort(arr):
    if len(arr) > 1:
        mid = len(arr) // 2
        left_half = arr[:mid]
        right_half = arr[mid:]

        merge_sort(left_half)
        merge_sort(right_half)

        i = j = k = 0

        while i < len(left_half) and j < len(right_half):
            if left_half[i] < right_half[j]:
                arr[k] = left_half[i]
                i += 1
            else:
                arr[k] = right_half[j]
                j += 1
            k += 1

        while i < len(left_half):
            arr[k] = left_half[i]
            i += 1
            k += 1

        while j < len(right_half):
            arr[k] = right_half[j]
            j += 1
            k += 1
```

In this function, the merge sort algorithm recursively divides the input array into halves and then merges the sorted halves back together. The time complexity of merge sort is O(n log n), as it requires O(log n) divisions and O(n) merging operations. As the size of the input array increases, the time taken to execute the algorithm grows in proportion to n log n.

The O(n log n) time complexity is more efficient than O(n²) algorithms for large input sizes, making it a preferred choice for sorting algorithms in many applications.

![alt text](NOTES/backend_studies%20(git)/Python/img/image-4.png)

In best case scenarios, O(n log n) algorithms can perform better than O(n²) algorithms, especially for large input sizes. However, they may still be less efficient than O(n) or O(log n) algorithms for smaller input sizes.

In worst case scenarios, O(n log n) algorithms can still perform better than O(n²) algorithms, but they may be less efficient than O(n) or O(log n) algorithms for larger input sizes.

---

# O(2^n) - Exponential Time Complexity

Exponential time complexity, denoted as O(2^n), describes an algorithm whose performance doubles with each additional element in the input. This means that if the input size increases by one, the time taken to execute the algorithm approximately doubles.

If the input size is n, the time taken to execute the algorithm will be proportional to 2 raised to the power of n. This results in a rapid increase in execution time as the input size grows.

Example of a O(2^n) algorithm in Python:

```python
def fibonacci(n):
    if n <= 1:
        return n
    else:
        return fibonacci(n-1) + fibonacci(n-2)
```

```python
def power_set(s):
    if len(s) == 0:
        return [[]]
    else:
        subsets = power_set(s[1:])
        return subsets + [[s[0]] + subset for subset in subsets]
```

These functions demonstrate exponential time complexity, as the number of recursive calls grows exponentially with the input size. For example, calculating the 40th Fibonacci number using this recursive approach would require approximately 2^40 = 1,099,511,627,776 function calls.

![alt text](NOTES/backend_studies%20(git)/Python/img/image-5.png)

Following this pattern, we have also O(3^n), O(4^n), and so on, which are also exponential time complexities. These algorithms become impractical for even moderately sized inputs due to their rapid growth in execution time.

---

# O(n!) - Factorial Time Complexity

Factorial time complexity, denoted as O(n!), describes an algorithm whose performance grows factorially with the size of the input data set. This means that if the input size increases by one, the time taken to execute the algorithm increases by a factor of n.

A factorial is the product of all positive integers up to a given number n. For example, 5! = 5 × 4 × 3 × 2 × 1 = 120. As a result, O(n!) algorithms can become impractical for even small input sizes due to their rapid growth in execution time.

The time complexity of O(n!) is often associated with algorithms that generate all possible permutations of a set of elements, such as the traveling salesman problem or the n-queens problem.

Example of a O(n!) algorithm in Python:

```python
def generate_permutations(arr):
    if len(arr) == 0:
        return [[]]
    else:
        permutations = []
        for i in range(len(arr)):
            current = arr[i]
            remaining = arr[:i] + arr[i+1:]
            for p in generate_permutations(remaining):
                permutations.append([current] + p)
        return permutations
```

This function generates all possible permutations of a given list of elements. The number of permutations of n elements is n!, which results in a time complexity of O(n!). As the size of the input list increases, the time taken to execute the algorithm grows factorially.

![alt text](NOTES/backend_studies%20(git)/Python/img/image-6.png)

In the best case scenario, O(n!) algorithms can still be impractical for even small input sizes due to their rapid growth in execution time. However, they may be necessary for certain problems that require generating all possible permutations or combinations of a set of elements.

For example, generating all possible arrangements of a set of 10 elements would require 10! = 3,628,800 permutations.

Or if you have to generate all possible combinations of a set of 20 elements, you would have to consider 20! = 2,432,902,008,176,640,000 combinations.