## Sorting

### Easy sort

#### Bubble Sort

Repeatedly steps through the list to be sorted, compares each pair of adjacent items and swaps them if they are in the wrong order.

Time Complexity: O(n*n)

```ts
for (var i = arr.length-1; i > 0; i--) {
  for (var j = 1; j <= i; j++) {
    if (arr[j] < arr[j-1]) {
      var temp = arr[j];
      arr[j] = arr[j-1];
      arr[j-1] = temp;
    }
  }
}
```

*float the largest number to the right each loop*

#### Selection Sort

Go through the array and swap the lowest element with the first element. Go through the rest array and repeatly do the same thing.

```ts
for (var i = 0; i < arr.length - 1; i++) {
  var min = arr[i];
  var pos = i;
  for (var j=i+1; j< arr.length; j++) {
    if (min > arr[j]) {
      min = arr[j];
      pos = j;
    }
  }
  var temp = arr[i];
  arr[i] = arr[pos];
  arr[pos] = temp;
}

```

#### Insertion Sort

It builds the final sorted array one item at a time.

Implementation: It starts with the second element. Pick the second element to be inserted and then compare to the previous element. If the first one is bigger, move the first one to second position and second one at the first.
Then, pick the third element and check whether the second element is bigger than the third.

```ts
for (var i = 1; i < arr.length; i++) {
  var toInsert = arr[i];
  var j = i;

  for (j = i; j > 0, toInsert < arr[j-1]; j--) {
    arr[j] = arr[j-1];
  }

  arr[j] = toInsert;
}
```

Time Complexity:

`best`: O(n)

`worst`: O(n^2)

### Advanced sort

#### Merge Sort

divide and conquer type algorithm (分治): break down array into small pieces and until you have one item in each piece. Then merge together by comparing them.

```ts

// MergeSort
function mergeSort(arr) {
  const len = arr.length;

  if (len < 2) {
    return arr;
  }

  const mid = len/2;
  const leftArr = arr.slice(0, mid);
  const rightArr = arr.slice(mid);

  return merge(mergeSort(leftArr), mergeSort(rightArr));
}

// Merge
function merge(left, right) {
  const res = [];
  let i=0, j=0;
  while(i < left.length && j < right.length) {
    if (left[i] < right[j]) {
      res.push(left[i++]);
    }
    else {
      res.push(right[j++]);
    }
  }

  return res.concat(left.slice(i)).concat(right.slice(j));
}

```

Time Complexity: O(n*log(n))

#### Quick Sort

Pick a pivot and put all the items smaller than the pivot value to the left and larget than the pivot value to the right.

Repeat the above step for both left and right side of the pivot.

```ts
// select the pivot as the last element in array

function quickSort(arr, left, right) {
  if (left < right) {
    var pivot = arr[right];
    var pivotIndex = partition(arr, pivot, left, right);
    quickSort(arr, left, pivotIndex - 1);
    quickSort(arr, pivotIndex + 1, right);
  }

  return arr;
}

function partition(arr, pivot, left, right) {
  var partitionIndex = left;

  var i = left;
  while(i < right) {
    if (arr[i] < pivot) {
      swap(arr, i, partitionIndex);
      partitionIndex++;
    }
    i++;
  }

  swap(arr, partitionIndex, right);

  return partitionIndex;
}

function swap(arr, i, j) {
  var temp = arr[i];
  arr[i] = arr[j];
  arr[j] = temp;
}

```

Time Complexity:

`Best`: O(n*log(n))
`Worst`: O(n^2)

#### Heap Sort