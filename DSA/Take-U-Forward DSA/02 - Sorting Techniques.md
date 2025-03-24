<font color="#7f7f7f"><em>Friday, 01-11-2024 - 15:28</em></font>
#programming #dsa 

### What is sorting?
Sorting is the process of arranging data elements in a specific order (ascending or descending) to enhance data accessibility, improve search efficiency, and enable better data organization.
### Selection Sort
Selects the smallest (or largest) element in each pass and places it in its correct position.

- Concept: Select minimum number through the array and swap positions.
![[SelectionSort| 1000 | center]]
###### Implementation
```java
public static void selectionSort(int[] array){
	for (int i = 0; i < array.length - 1; i++){  // No need to check last position
		int min_index = i;  // In case current position already holds the min value
		
		for (int j = i + 1; j < array.length; j++){
			if (array[j] < array[min_index]){
				min_index = j;
			}
		}
		
		int temp = array[i];
		array[i] = array[min_index];
		array[min_index] = temp;
	}
}
```

<u>TIME COMPLEXITY</u>: O(N^2)

---
### Bubble Sort
Repeatedly compares and swaps adjacent elements and checking if they are in the wrong order, until the largest elements "bubble" to the end.

- Concept: Pushes the maximum to the last position by adjacent swaps.
![[BubbleSort| 1000 | center]]
###### Implementation
```java
public static void bubbleSort(int[] array){	
	for (int i = 0; i < array.length - 1; i++){
		int boolean swapped = false;
		
		for(int j = 0; j < array.length - 1 - i; j++){
			if (array[j] > array[j+1]){
				int temp = array[j];
				array[j] = array[j+1];
				array[j+1] = temp;

				swapped = true;
			}
		}
		if (!swapped) break; // Array is fully sorted - no swaps happened
	}
}
```

<u>TIME COMPLEXITY</u>: O(N^2), with the swapped optimisation if its already sorted => O(N)

---
### Insertion Sort
Builds a sorted portion of the list by repeatedly inserting the next unsorted element into its correct position (all elements to its left are smaller and to the right are larger) by shifting the **larger** elements to the right. 

- Concept: Takes an element and puts it in its correct order through shifting the other elements to the right until it finds the correct place for the current one.
![[InsertionSort | 1000 | center]]
###### Implementation
```java
public static void insertionSort(int[] array){
	for(int i = 1; i < array.length; i++){
		int key = array[i];
		int j = i - 1;

		while (j>=0 && array[j] > key){
			array[j+1] = array[j];
			j--;
		}

		array[j+1] = key;
	}
}
```

<u>TIME COMPLEXITY</u>: O(N^2)

---
### Merge Sort
Divides the array into smaller halves recursively, then merges the sorted halves back together, achieving efficient and stable sorting.

![[MergeSort Concept| 1000 | center]]
![[MergeSort Merge| 1000 | center]]
###### Implementation
```java
public static void merge(int[] array, int low, int middle, int high){
	ArrayList<Integer> aux = new ArrayList<>();
	int left = low;
	int right = middle + 1;

	// Does the sorting until one of the two parts ends
	while (low <= middle && right <= high){
		if (array[left] <= array[right]){
			aux.add(array[left]);
			left++;
		}
		else {
			aux.add(array[right]);
			right++;
		}
	}

	// If elements on the left half are still left
	while (low <= middle){
		aux.add(array[left]);
		left++;
	}

	// If elements on the right half are still left
	while (right <= high){
		aux.add(array[right]);
		right++;
	}

	// Transfers all elements from auxiliar to original array
	for (int i = low; i <= high; i++){
		array[i] = aux.get(i - low);
	}
}

public static void mergeSort(int[] array, int low, int high){
	if (low >= high) { return; }

	int middle = (low + high) / 2;
	mergeSort(array, low, middle);
	mergeSort(array, middle + 1, high);
	merge(array, low, middle, high);
}
```

<u>TIME COMPLEXITY</u>: We're diving the original array of size n by n/2 each time each means N -> N/2 -> N/4 ... and for the merging on the worst case scenario we need N steps to sort the array => O(N LOG(N)).

<u>SPACE COMPLEXITY</u>: Since we're allocating space for an auxiliar array of size N, the other operations are done using pointers (integers) => O(N).

---
### Recursive Bubble Sort
Uses the bubble sort logic in a recursive manner, sorting by comparing and swapping adjacent elements through recursive calls.
###### Implementation
```java
public static void recursiveBubbleSort(int[] array, int length){
	// Base Case
	if (length == 1){ return; }

	boolean swapped = false;
	for (int i = 0; i < length - 1; i++){
		if (array[i] > array[i+1]){
			int temp = array[i];
			array[i] = array[i+1];
			array[i+1] = temp;

			swapped = true;
		}
	}

	if (!swapped){ return; }
	recursiveBubbleSort(array, length - 1);
}
```

<u>TIME COMPLEXITY</u>: O(N^2) -> without boolean variable

<u>SPACE COMPLEXITY</u>: O(1)

---
### Recursive Insertion Sort
Implements insertion sort recursively by inserting each element into its correct position within the sorted portion of the list.
```java
public static void recursiveInsertionSort(int[] array, int i){
	// Base Case
	if (i == array.length){ return; }

	int key = array[i];
	int j = i - 1;
	while (j>=0 array[j] > key){
		array[j+1] = array[j];
		j--;
	}

	array[j+1] = key;
	recursiveInsertionSort(array, i + 1);
}
```

<u>TIME COMPLEXITY</u>: O(N^2)

<u>SPACE COMPLEXITY</u>: O(1)

---
### Quick Sort
Partitions/Divides the array around a pivot element, placing it in its correct position, and recursively sorts the two halves knowing the left half only contains numbers equal or smaller than the pivot and the right one numbers higher than the pivot.

![[QuickSort| 1000 | center]]
###### Implementation
```java
public static void partition(int[] array, int low, int high){
	int pivot = array[low];
	int i = low + 1;
	int j = high;

	while (i <= j){
		while (i <= high && array[i] <= pivot){ i++; }
		while (j >= low  && array[j] > pivot){ j--; }

		if (i < j){
			int temp = array[i];
			array[i] = array[j];
			array[j] = temp;
		}
	}

	array[low] = array[j];
	array[j] = pivot;
	return j;
}

public static void quickSort(int[] array, int low, int high){
	if (low < high){
		int partitionIndex = partition(array, low, high);
		quickSort(array, low, partitionIndex - 1);
		quickSort(array, partitionIndex + 1, high);
	}
}
```

<u>TIME COMPLEXITY</u>: We're diving the original array of size n by n/2 each time each means N -> N/2 -> N/4 ... and for the merging on the worst case scenario we need N steps to sort the array => O(N LOG(N)).

<u>SPACE COMPLEXITY</u>: O(1)