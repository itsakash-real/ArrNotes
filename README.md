# Complete Arrays Mastery: Zero to Hero

---

## **PART 1: FUNDAMENTALS**

### **What is an Array?**

An **array** is a collection of elements of the **same data type** stored in **contiguous memory locations**. Think of it as a row of numbered boxes, where each box holds a value.

**Why use arrays?**
- Store multiple values under one variable name
- Access any element instantly using its position (index)
- Efficient memory usage
- Foundation for complex data structures

---

### **Memory Layout (Contiguous Memory)**

```
Array: [10, 20, 30, 40, 50]

Memory visualization:
Address: 1000  1004  1008  1012  1016
Value:   [10]  [20]  [30]  [40]  [50]
Index:    0     1     2     3     4
```

**Key Points:**
- Elements stored back-to-back in memory
- If first element at address 1000, and each int = 4 bytes, then:
  - arr[0] → 1000
  - arr[1] → 1004
  - arr[2] → 1008
- **Formula**: `address(arr[i]) = base_address + (i × size_of_datatype)`
- This is why accessing arr[i] is O(1) - direct calculation!

---

### **Indexing: 0-based vs 1-based**

**0-based indexing** (C++, Java, Python):
```
Array: [10, 20, 30]
Index:  0   1   2
```

**1-based indexing** (MATLAB, Lua, some mathematical contexts):
```
Array: [10, 20, 30]
Index:  1   2   3
```

**Why 0-based?**
- Matches memory offset calculation: `base_address + (index × size)`
- No subtraction needed in hardware

---

### **Static vs Dynamic Arrays**

| Feature | Static Array | Dynamic Array |
|---------|-------------|---------------|
| **Size** | Fixed at compile time | Can grow/shrink at runtime |
| **Declaration** | `int arr[5];` | `vector<int> v;` |
| **Memory** | Stack (usually) | Heap |
| **Resizing** | Not possible | Automatic (vector) |
| **Performance** | Faster access | Slight overhead for resizing |

**Static Array:**
```cpp
int arr[5] = {1, 2, 3, 4, 5};
// Size = 5, cannot change
```

**Dynamic Array (Vector):**
```cpp
vector<int> v = {1, 2, 3};
v.push_back(4);  // Now size = 4
```

---

### **Array vs Vector vs List**

| | Array | Vector | List |
|---|-------|--------|------|
| **Type** | Static | Dynamic | Dynamic |
| **Size** | Fixed | Variable | Variable |
| **Memory** | Contiguous | Contiguous | Non-contiguous (linked) |
| **Access** | O(1) | O(1) | O(n) |
| **Insertion (end)** | - | O(1) amortized | O(1) |
| **Insertion (middle)** | O(n) | O(n) | O(1) if position known |
| **Cache friendly** | Yes | Yes | No |

**When to use what:**
- **Array**: Size known, no modifications
- **Vector**: Size unknown, frequent access, occasional insertions at end
- **List**: Frequent insertions/deletions in middle

---

### **Time & Space Complexity: Basic Operations**

| Operation | Time Complexity | Space Complexity |
|-----------|----------------|------------------|
| **Access** arr[i] | O(1) | O(1) |
| **Search** (unsorted) | O(n) | O(1) |
| **Search** (sorted) | O(log n) - binary search | O(1) |
| **Insert** at end | O(1) | O(1) |
| **Insert** at beginning/middle | O(n) - shifting needed | O(1) |
| **Delete** at end | O(1) | O(1) |
| **Delete** at beginning/middle | O(n) - shifting needed | O(1) |
| **Traversal** | O(n) | O(1) |

---

## **PART 2: CORE OPERATIONS**

### **1. Traversal**

**Explanation:** Visit each element once from start to end.

**Dry Run:**
```
Array: [5, 3, 8, 1]
Step 1: Visit index 0 → 5
Step 2: Visit index 1 → 3
Step 3: Visit index 2 → 8
Step 4: Visit index 3 → 1
```

**C++ Code:**
```cpp
#include <iostream>
#include <vector>
using namespace std;

void traverseArray(int arr[], int n) {
    cout << "Array elements: ";
    for(int i = 0; i < n; i++) {
        cout << arr[i] << " ";
    }
    cout << endl;
}

// Vector version
void traverseVector(vector<int>& v) {
    cout << "Vector elements: ";
    for(int i = 0; i < v.size(); i++) {
        cout << v[i] << " ";
    }
    cout << endl;
    
    // Alternative: range-based for loop
    cout << "Using range loop: ";
    for(int x : v) {
        cout << x << " ";
    }
    cout << endl;
}

int main() {
    int arr[] = {5, 3, 8, 1};
    int n = 4;
    traverseArray(arr, n);
    
    vector<int> v = {5, 3, 8, 1};
    traverseVector(v);
    
    return 0;
}
```

**Complexity:**
- Time: O(n) - visit each element once
- Space: O(1) - no extra space

---

### **2. Insertion**

#### **Insertion at End**

**Explanation:** Add element after last element. Simple if space available.

**Dry Run:**
```
Initial: [5, 3, 8, _, _]  size=3, capacity=5
Insert 7 at end:
Result:  [5, 3, 8, 7, _]  size=4
```

**Code:**
```cpp
void insertAtEnd(int arr[], int& n, int capacity, int value) {
    if(n >= capacity) {
        cout << "Array full!" << endl;
        return;
    }
    arr[n] = value;
    n++;
}

int main() {
    int arr[10] = {5, 3, 8};
    int n = 3;
    int capacity = 10;
    
    insertAtEnd(arr, n, capacity, 7);
    
    // Print
    for(int i = 0; i < n; i++) {
        cout << arr[i] << " ";
    }
    // Output: 5 3 8 7
    
    return 0;
}
```

**Complexity:**
- Time: O(1)
- Space: O(1)

---

#### **Insertion at Beginning**

**Explanation:** Shift all elements one position right, then insert at index 0.

**Dry Run:**
```
Initial: [5, 3, 8, _]  n=3
Insert 2 at beginning:

Step 1: Shift 8 → index 3: [5, 3, _, 8]
Step 2: Shift 3 → index 2: [5, _, 3, 8]
Step 3: Shift 5 → index 1: [_, 5, 3, 8]
Step 4: Insert 2 at index 0: [2, 5, 3, 8]
```

**Code:**
```cpp
void insertAtBeginning(int arr[], int& n, int capacity, int value) {
    if(n >= capacity) {
        cout << "Array full!" << endl;
        return;
    }
    
    // Shift elements right
    for(int i = n - 1; i >= 0; i--) {
        arr[i + 1] = arr[i];
    }
    
    arr[0] = value;
    n++;
}

int main() {
    int arr[10] = {5, 3, 8};
    int n = 3;
    int capacity = 10;
    
    insertAtBeginning(arr, n, capacity, 2);
    
    for(int i = 0; i < n; i++) {
        cout << arr[i] << " ";
    }
    // Output: 2 5 3 8
    
    return 0;
}
```

**Complexity:**
- Time: O(n) - shifting n elements
- Space: O(1)

---

#### **Insertion at Position**

**Explanation:** Insert at specific index, shift elements from that position.

**Dry Run:**
```
Initial: [5, 3, 8, 9, _]  n=4
Insert 7 at index 2:

Step 1: Shift 9 → index 4: [5, 3, 8, _, 9]
Step 2: Shift 8 → index 3: [5, 3, _, 8, 9]
Step 3: Insert 7 at index 2: [5, 3, 7, 8, 9]
```

**Code:**
```cpp
void insertAtPosition(int arr[], int& n, int capacity, int pos, int value) {
    if(n >= capacity) {
        cout << "Array full!" << endl;
        return;
    }
    
    if(pos < 0 || pos > n) {
        cout << "Invalid position!" << endl;
        return;
    }
    
    // Shift elements from pos to end
    for(int i = n - 1; i >= pos; i--) {
        arr[i + 1] = arr[i];
    }
    
    arr[pos] = value;
    n++;
}

int main() {
    int arr[10] = {5, 3, 8, 9};
    int n = 4;
    int capacity = 10;
    
    insertAtPosition(arr, n, capacity, 2, 7);
    
    for(int i = 0; i < n; i++) {
        cout << arr[i] << " ";
    }
    // Output: 5 3 7 8 9
    
    return 0;
}
```

**Complexity:**
- Time: O(n) - worst case shift all elements
- Space: O(1)

---

### **3. Deletion**

#### **Delete at End**

**Explanation:** Simply reduce size by 1.

**Code:**
```cpp
void deleteAtEnd(int arr[], int& n) {
    if(n <= 0) {
        cout << "Array empty!" << endl;
        return;
    }
    n--;
}
```

**Complexity:**
- Time: O(1)
- Space: O(1)

---

#### **Delete at Beginning**

**Explanation:** Shift all elements one position left.

**Dry Run:**
```
Initial: [5, 3, 8, 9]  n=4
Delete at beginning:

Step 1: Shift 3 → index 0: [3, 3, 8, 9]
Step 2: Shift 8 → index 1: [3, 8, 8, 9]
Step 3: Shift 9 → index 2: [3, 8, 9, 9]
Step 4: Reduce size: [3, 8, 9]  n=3
```

**Code:**
```cpp
void deleteAtBeginning(int arr[], int& n) {
    if(n <= 0) {
        cout << "Array empty!" << endl;
        return;
    }
    
    // Shift elements left
    for(int i = 0; i < n - 1; i++) {
        arr[i] = arr[i + 1];
    }
    
    n--;
}
```

**Complexity:**
- Time: O(n)
- Space: O(1)

---

#### **Delete at Position**

**Code:**
```cpp
void deleteAtPosition(int arr[], int& n, int pos) {
    if(n <= 0) {
        cout << "Array empty!" << endl;
        return;
    }
    
    if(pos < 0 || pos >= n) {
        cout << "Invalid position!" << endl;
        return;
    }
    
    // Shift elements from pos+1 to end
    for(int i = pos; i < n - 1; i++) {
        arr[i] = arr[i + 1];
    }
    
    n--;
}
```

**Complexity:**
- Time: O(n)
- Space: O(1)

---

### **4. Update**

**Explanation:** Change value at given index.

**Code:**
```cpp
void updateElement(int arr[], int n, int pos, int value) {
    if(pos < 0 || pos >= n) {
        cout << "Invalid position!" << endl;
        return;
    }
    arr[pos] = value;
}
```

**Complexity:**
- Time: O(1)
- Space: O(1)

---

### **5. Searching (Linear Search)**

**Explanation:** Check each element until found or reach end.

**Dry Run:**
```
Array: [5, 3, 8, 1, 9]
Search for 8:

i=0: arr[0]=5 ≠ 8
i=1: arr[1]=3 ≠ 8
i=2: arr[2]=8 = 8 → Found at index 2!
```

**Code:**
```cpp
int linearSearch(int arr[], int n, int target) {
    for(int i = 0; i < n; i++) {
        if(arr[i] == target) {
            return i;  // Return index
        }
    }
    return -1;  // Not found
}

int main() {
    int arr[] = {5, 3, 8, 1, 9};
    int n = 5;
    
    int index = linearSearch(arr, n, 8);
    if(index != -1) {
        cout << "Found at index: " << index << endl;
    } else {
        cout << "Not found" << endl;
    }
    
    return 0;
}
```

**Complexity:**
- Time: O(n) - worst case check all elements
- Space: O(1)

---

### **6. Reversal**

**Explanation:** Swap elements from start and end, moving towards center.

**Dry Run:**
```
Initial: [5, 3, 8, 1, 9]

Step 1: Swap arr[0] and arr[4]: [9, 3, 8, 1, 5]
Step 2: Swap arr[1] and arr[3]: [9, 1, 8, 3, 5]
Step 3: Middle element arr[2] stays: [9, 1, 8, 3, 5]

Result: [9, 1, 8, 3, 5]
```

**Code:**
```cpp
void reverseArray(int arr[], int n) {
    int left = 0;
    int right = n - 1;
    
    while(left < right) {
        // Swap arr[left] and arr[right]
        int temp = arr[left];
        arr[left] = arr[right];
        arr[right] = temp;
        
        left++;
        right--;
    }
}

// Alternative: using swap function
void reverseArray2(int arr[], int n) {
    for(int i = 0; i < n / 2; i++) {
        swap(arr[i], arr[n - 1 - i]);
    }
}

int main() {
    int arr[] = {5, 3, 8, 1, 9};
    int n = 5;
    
    reverseArray(arr, n);
    
    for(int i = 0; i < n; i++) {
        cout << arr[i] << " ";
    }
    // Output: 9 1 8 3 5
    
    return 0;
}
```

**Complexity:**
- Time: O(n) - n/2 swaps
- Space: O(1)

---

### **7. Finding Max/Min**

**Explanation:** Track largest/smallest value while traversing.

**Dry Run (Finding Max):**
```
Array: [5, 3, 8, 1, 9]

max = arr[0] = 5
i=1: arr[1]=3 < 5, max stays 5
i=2: arr[2]=8 > 5, max = 8
i=3: arr[3]=1 < 8, max stays 8
i=4: arr[4]=9 > 8, max = 9

Result: max = 9
```

**Code:**
```cpp
int findMax(int arr[], int n) {
    if(n <= 0) return -1;  // Handle empty array
    
    int maxVal = arr[0];
    for(int i = 1; i < n; i++) {
        if(arr[i] > maxVal) {
            maxVal = arr[i];
        }
    }
    return maxVal;
}

int findMin(int arr[], int n) {
    if(n <= 0) return -1;
    
    int minVal = arr[0];
    for(int i = 1; i < n; i++) {
        if(arr[i] < minVal) {
            minVal = arr[i];
        }
    }
    return minVal;
}

// Find both together
pair<int, int> findMinMax(int arr[], int n) {
    if(n <= 0) return {-1, -1};
    
    int minVal = arr[0];
    int maxVal = arr[0];
    
    for(int i = 1; i < n; i++) {
        if(arr[i] < minVal) minVal = arr[i];
        if(arr[i] > maxVal) maxVal = arr[i];
    }
    
    return {minVal, maxVal};
}

int main() {
    int arr[] = {5, 3, 8, 1, 9};
    int n = 5;
    
    cout << "Max: " << findMax(arr, n) << endl;  // 9
    cout << "Min: " << findMin(arr, n) << endl;  // 1
    
    auto [minVal, maxVal] = findMinMax(arr, n);
    cout << "Min: " << minVal << ", Max: " << maxVal << endl;
    
    return 0;
}
```

**Complexity:**
- Time: O(n)
- Space: O(1)

---

## **PART 3: IMPORTANT ARRAY PATTERNS**

### **1. Two Pointer Technique**

**What is it?**
Use two pointers (indices) to traverse array from different positions, usually:
- Both start at opposite ends (left=0, right=n-1)
- Both start at beginning
- One slow, one fast

**When to use:**
- Sorted arrays
- Finding pairs with target sum
- Removing duplicates
- Reversing
- Partitioning

**Example Problem: Find pair with target sum in sorted array**

**Dry Run:**
```
Array: [1, 3, 5, 7, 9]  Target: 12

left=0, right=4: arr[0] + arr[4] = 1 + 9 = 10 < 12 → move left++
left=1, right=4: arr[1] + arr[4] = 3 + 9 = 12 = 12 → Found!
```

**Code:**
```cpp
bool twoSumSorted(int arr[], int n, int target) {
    int left = 0;
    int right = n - 1;
    
    while(left < right) {
        int sum = arr[left] + arr[right];
        
        if(sum == target) {
            cout << "Pair found: " << arr[left] << ", " << arr[right] << endl;
            return true;
        } else if(sum < target) {
            left++;  // Need larger sum
        } else {
            right--;  // Need smaller sum
        }
    }
    
    return false;
}
```

**Why it works:**
- If sum too small → left pointer moves right (increases sum)
- If sum too large → right pointer moves left (decreases sum)
- O(n) instead of O(n²) brute force

**Complexity:**
- Time: O(n)
- Space: O(1)

---

### **2. Sliding Window**

**What is it?**
Maintain a "window" (subarray) that slides through array. Window can be:
- **Fixed size**: Move by one position each time
- **Variable size**: Expand/shrink based on condition

**When to use:**
- Subarray/substring problems
- Finding max/min in subarrays
- Problems with "consecutive elements"
- "K elements" problems

**Example: Maximum sum of k consecutive elements**

**Dry Run:**
```
Array: [2, 1, 5, 1, 3, 2]  k=3

Window [2, 1, 5]: sum = 8
Window [1, 5, 1]: sum = 7
Window [5, 1, 3]: sum = 9 ← Maximum
Window [1, 3, 2]: sum = 6

Result: 9
```

**Code:**
```cpp
// Brute Force: O(n*k)
int maxSumBrute(int arr[], int n, int k) {
    int maxSum = INT_MIN;
    
    for(int i = 0; i <= n - k; i++) {
        int sum = 0;
        for(int j = i; j < i + k; j++) {
            sum += arr[j];
        }
        maxSum = max(maxSum, sum);
    }
    
    return maxSum;
}

// Sliding Window: O(n)
int maxSumSlidingWindow(int arr[], int n, int k) {
    if(n < k) return -1;
    
    // Calculate sum of first window
    int windowSum = 0;
    for(int i = 0; i < k; i++) {
        windowSum += arr[i];
    }
    
    int maxSum = windowSum;
    
    // Slide window: remove first element, add next element
    for(int i = k; i < n; i++) {
        windowSum = windowSum - arr[i - k] + arr[i];
        maxSum = max(maxSum, windowSum);
    }
    
    return maxSum;
}

int main() {
    int arr[] = {2, 1, 5, 1, 3, 2};
    int n = 6;
    int k = 3;
    
    cout << "Max sum: " << maxSumSlidingWindow(arr, n, k) << endl;
    // Output: 9
    
    return 0;
}
```

**Why it works:**
- Calculate first window sum once
- For next window: subtract element going out, add element coming in
- No recalculation!

**Complexity:**
- Time: O(n)
- Space: O(1)

---

### **3. Prefix Sum**

**What is it?**
Create auxiliary array where `prefix[i]` = sum of elements from index 0 to i.

**Formula:**
```
prefix[0] = arr[0]
prefix[i] = prefix[i-1] + arr[i]
```

**When to use:**
- Range sum queries
- Subarray sum problems
- Equilibrium index
- Count subarrays with given sum

**Example: Range sum queries**

**Dry Run:**
```
Array: [3, 1, 5, 2, 4]

Prefix: [3, 4, 9, 11, 15]
         ↑  ↑  ↑   ↑   ↑
        [0][1][2] [3] [4]

Query: Sum from index 1 to 3
Answer = prefix[3] - prefix[0] = 11 - 3 = 8
Verify: arr[1] + arr[2] + arr[3] = 1 + 5 + 2 = 8 ✓
```

**Code:**
```cpp
vector<int> buildPrefixSum(int arr[], int n) {
    vector<int> prefix(n);
    prefix[0] = arr[0];
    
    for(int i = 1; i < n; i++) {
        prefix[i] = prefix[i-1] + arr[i];
    }
    
    return prefix;
}

int rangeSum(vector<int>& prefix, int left, int right) {
    if(left == 0) {
        return prefix[right];
    }
    return prefix[right] - prefix[left - 1];
}

int main() {
    int arr[] = {3, 1, 5, 2, 4};
    int n = 5;
    
    vector<int> prefix = buildPrefixSum(arr, n);
    
    // Query: sum from index 1 to 3
    cout << "Sum[1..3]: " << rangeSum(prefix, 1, 3) << endl;
    // Output: 8
    
    // Query: sum from index 0 to 4
    cout << "Sum[0..4]: " << rangeSum(prefix, 0, 4) << endl;
    // Output: 15
    
    return 0;
}
```

**Formula for range [L, R]:**
```
sum[L..R] = prefix[R] - prefix[L-1]  (if L > 0)
          = prefix[R]                (if L == 0)
```

**Complexity:**
- Build: O(n) time, O(n) space
- Query: O(1)

---

### **4. Kadane's Algorithm**

**What is it?**
Algorithm to find maximum sum subarray in O(n) time.

**Key Idea:**
At each position, decide: 
- Continue current subarray OR
- Start new subarray from current element

**Formula:**
```
currentSum = max(arr[i], currentSum + arr[i])
maxSum = max(maxSum, currentSum)
```

**When to use:**
- Maximum subarray sum
- Maximum product subarray (variation)
- Stock buy-sell problems

**Dry Run:**
```
Array: [-2, 1, -3, 4, -1, 2, 1, -5, 4]

i=0: current=-2, max=-2
i=1: current=max(1, -2+1=-1)=1, max=1
i=2: current=max(-3, 1-3=-2)=-2, max=1
i=3: current=max(4, -2+4=2)=4, max=4
i=4: current=max(-1, 4-1=3)=3, max=4
i=5: current=max(2, 3+2=5)=5, max=5
i=6: current=max(1, 5+1=6)=6, max=6 ← Answer
i=7: current=max(-5, 6-5=1)=1, max=6
i=8: current=max(4, 1+4=5)=5, max=6

Maximum sum = 6 (subarray: [4, -1, 2, 1])
```

**Code:**
```cpp
int kadane(int arr[], int n) {
    int currentSum = arr[0];
    int maxSum = arr[0];
    
    for(int i = 1; i < n; i++) {
        currentSum = max(arr[i], currentSum + arr[i]);
        maxSum = max(maxSum, currentSum);
    }
    
    return maxSum;
}

// With subarray indices
struct Result {
    int maxSum;
    int start;
    int end;
};

Result kadaneWithIndices(int arr[], int n) {
    int currentSum = arr[0];
    int maxSum = arr[0];
    int start = 0, end = 0;
    int tempStart = 0;
    
    for(int i = 1; i < n; i++) {
        if(arr[i] > currentSum + arr[i]) {
            currentSum = arr[i];
            tempStart = i;
        } else {
            currentSum = currentSum + arr[i];
        }
        
        if(currentSum > maxSum) {
            maxSum = currentSum;
            start = tempStart;
            end = i;
        }
    }
    
    return {maxSum, start, end};
}

int main() {
    int arr[] = {-2, 1, -3, 4, -1, 2, 1, -5, 4};
    int n = 9;
    
    cout << "Maximum sum: " << kadane(arr, n) << endl;
    // Output: 6
    
    Result res = kadaneWithIndices(arr, n);
    cout << "Max sum: " << res.maxSum << endl;
    cout << "Subarray: [" << res.start << ", " << res.end << "]" << endl;
    
    return 0;
}
```

**Why it works:**
- At each position, including current element either:
  - Extends previous subarray (if it was positive)
  - Starts fresh (if previous subarray was dragging us down)

**Complexity:**
- Time: O(n)
- Space: O(1)

---

### **5. Hashing with Arrays**

**What is it?**
Use hash map (unordered_map) to store array elements for fast lookup.

**When to use:**
- Finding duplicates
- Two sum problem (unsorted)
- Frequency counting
- Subarray sum equals K

**Example: Two Sum (unsorted array)**

**Problem:** Find two numbers that add up to target.

**Approach:**
- Store elements in hash map with their indices
- For each element x, check if (target - x) exists in map

**Dry Run:**
```
Array: [2, 7, 11, 15]  Target: 9

i=0: num=2, needed=9-2=7, map={2:0}
i=1: num=7, needed=9-7=2, 2 exists in map! → Found: indices 0,1
```

**Code:**
```cpp
vector<int> twoSum(int arr[], int n, int target) {
    unordered_map<int, int> mp;  // value -> index
    
    for(int i = 0; i < n; i++) {
        int needed = target - arr[i];
        
        if(mp.find(needed) != mp.end()) {
            return {mp[needed], i};
        }
        
        mp[arr[i]] = i;
    }
    
    return {-1, -1};  // Not found
}

int main() {
    int arr[] = {2, 7, 11, 15};
    int n = 4;
    int target = 9;
    
    vector<int> result = twoSum(arr, n, target);
    
    if(result[0] != -1) {
        cout << "Indices: " << result[0] << ", " << result[1] << endl;
        cout << "Numbers: " << arr[result[0]] << ", " << arr[result[1]] << endl;
    }
    
    return 0;
}
```

**Complexity:**
- Time: O(n)
- Space: O(n)

---

### **6. Sorting-Based Techniques**

**What is it?**
Sort array first, then solve problem becomes easier.

**When to use:**
- Finding pairs/triplets
- Removing duplicates
- Merging intervals
- Meeting rooms problems

**Example: Find if pair exists with given sum**

**Approach 1: Sort + Two Pointer**

bool hasPairWithSum(int arr[], int n, int target) {
    sort(arr, arr + n);  // O(n log n)
    
    int left = 0, right = n - 1;
    
    while(left < right) {
        int sum = arr[left] + arr[right];
        if(sum == target) return true;
        else if(sum < target) left++;
        else right--;
    }
    
    return false;
}
```

**Time:** O(n log n) for sorting + O(n) for two pointer = O(n log n)  
**Space:** O(1)

**Trade-offs:**
- Sorting: O(n log n) time, O(1) space
- Hashing: O(n) time, O(n) space

Choose based on constraints!

---

## **PART 4: INTERVIEW-IMPORTANT PROBLEMS**

### **Problem 1: Rotate Array**

#### **Left Rotation by k positions**

**Problem:** Rotate array left by k positions.

**Example:**
```
Input: [1, 2, 3, 4, 5], k=2
Output: [3, 4, 5, 1, 2]
```

**Approach 1: Brute Force (Using temp array)**

```cpp
void rotateLeftBrute(int arr[], int n, int k) {
    k = k % n;  // Handle k > n
    
    int temp[n];
    
    // Copy rotated elements
    for(int i = 0; i < n; i++) {
        temp[i] = arr[(i + k) % n];
    }
    
    // Copy back
    for(int i = 0; i < n; i++) {
        arr[i] = temp[i];
    }
}
```
**Time:** O(n), **Space:** O(n)

**Approach 2: Optimized (Reversal Algorithm)**

**Key Insight:** 3 reversals!
```
[1, 2, 3, 4, 5], k=2

Step 1: Reverse first k elements: [2, 1, 3, 4, 5]
Step 2: Reverse remaining: [2, 1, 5, 4, 3]
Step 3: Reverse entire array: [3, 4, 5, 1, 2]
```

```cpp
void reverse(int arr[], int start, int end) {
    while(start < end) {
        swap(arr[start], arr[end]);
        start++;
        end--;
    }
}

void rotateLeft(int arr[], int n, int k) {
    k = k % n;
    
    // Reverse first k elements
    reverse(arr, 0, k - 1);
    
    // Reverse remaining n-k elements
    reverse(arr, k, n - 1);
    
    // Reverse entire array
    reverse(arr, 0, n - 1);
}
```
**Time:** O(n), **Space:** O(1) ✅

---

#### **Right Rotation by k positions**

```cpp
void rotateRight(int arr[], int n, int k) {
    k = k % n;
    
    // Reverse entire array
    reverse(arr, 0, n - 1);
    
    // Reverse first k elements
    reverse(arr, 0, k - 1);
    
    // Reverse remaining
    reverse(arr, k, n - 1);
}
```

**Edge Cases:**
- k = 0 → no rotation
- k = n → complete rotation = original
- k > n → use k % n
- n = 1 → return as is

---

### **Problem 2: Move Zeros to End**

**Problem:** Move all zeros to end while maintaining order of non-zero elements.

**Example:**
```
Input: [0, 1, 0, 3, 12]
Output: [1, 3, 12, 0, 0]
```

**Approach 1: Brute Force**
```cpp
void moveZeroesBrute(int arr[], int n) {
    int temp[n];
    int j = 0;
    
    // Copy non-zero elements
    for(int i = 0; i < n; i++) {
        if(arr[i] != 0) {
            temp[j++] = arr[i];
        }
    }
    
    // Fill remaining with zeros
    while(j < n) {
        temp[j++] = 0;
    }
    
    // Copy back
    for(int i = 0; i < n; i++) {
        arr[i] = temp[i];
    }
}
```
**Time:** O(n), **Space:** O(n)

**Approach 2: Optimized (Two Pointer)**

**Dry Run:**
```
[0, 1, 0, 3, 12]
 ↑
 j (position for next non-zero)

i=0: arr[0]=0, skip
i=1: arr[1]=1≠0, swap with j=0: [1, 0, 0, 3, 12], j++
i=2: arr[2]=0, skip
i=3: arr[3]=3≠0, swap with j=1: [1, 3, 0, 0, 12], j++
i=4: arr[4]=12≠0, swap with j=2: [1, 3, 12, 0, 0], j++

Result: [1, 3, 12, 0, 0]
```

```cpp
void moveZeroes(int arr[], int n) {
    int j = 0;  // Position for next non-zero
    
    // Move non-zeros to front
    for(int i = 0; i < n; i++) {
        if(arr[i] != 0) {
            swap(arr[i], arr[j]);
            j++;
        }
    }
}

int main() {
    int arr[] = {0, 1, 0, 3, 12};
    int n = 5;
    
    moveZeroes(arr, n);
    
    for(int i = 0; i < n; i++) {
        cout << arr[i] << " ";
    }
    // Output: 1 3 12 0 0
    
    return 0;
}
```
**Time:** O(n), **Space:** O(1) ✅

**Edge Cases:**
- All zeros
- No zeros
- Single element

---

### **Problem 3: Remove Duplicates from Sorted Array**

**Problem:** Remove duplicates in-place, return new length.

**Example:**
```
Input: [1, 1, 2, 2, 3, 4, 4]
Output: length=4, array=[1, 2, 3, 4, _, _, _]
```

**Approach: Two Pointer**

**Dry Run:**
```
[1, 1, 2, 2, 3, 4, 4]
 ↑
 j (position for next unique)

i=0: arr[0]=1, j=0
i=1: arr[1]=1==arr[j]=1, skip
i=2: arr[2]=2≠arr[j]=1, j++, arr[j]=2: [1, 2, 2, 2, 3, 4, 4]
i=3: arr[3]=2==arr[j]=2, skip
i=4: arr[4]=3≠arr[j]=2, j++, arr[j]=3: [1, 2, 3, 2, 3, 4, 4]
i=5: arr[5]=4≠arr[j]=3, j++, arr[j]=4: [1, 2, 3, 4, 3, 4, 4]
i=6: arr[6]=4==arr[j]=4, skip

Result: j+1=4 unique elements
```

```cpp
int removeDuplicates(int arr[], int n) {
    if(n == 0) return 0;
    
    int j = 0;  // Position for next unique element
    
    for(int i = 1; i < n; i++) {
        if(arr[i] != arr[j]) {
            j++;
            arr[j] = arr[i];
        }
    }
    
    return j + 1;  // New length
}

int main() {
    int arr[] = {1, 1, 2, 2, 3, 4, 4};
    int n = 7;
    
    int newLength = removeDuplicates(arr, n);
    
    cout << "New length: " << newLength << endl;
    cout << "Array: ";
    for(int i = 0; i < newLength; i++) {
        cout << arr[i] << " ";
    }
    // Output: New length: 4
    //         Array: 1 2 3 4
    
    return 0;
}
```
**Time:** O(n), **Space:** O(1) ✅

---

### **Problem 4: Second Largest Element**

**Problem:** Find second largest element in array.

**Example:**
```
Input: [12, 35, 1, 10, 34, 1]
Output: 34
```

**Approach 1: Sorting**
```cpp
int secondLargestSorting(int arr[], int n) {
    if(n < 2) return -1;
    
    sort(arr, arr + n);
    
    // Find second largest distinct element
    for(int i = n - 2; i >= 0; i--) {
        if(arr[i] != arr[n-1]) {
            return arr[i];
        }
    }
    
    return -1;  // All elements same
}
```
**Time:** O(n log n), **Space:** O(1)

**Approach 2: Optimized (Single Pass)**

**Dry Run:**
```
[12, 35, 1, 10, 34, 1]

largest=-∞, second=-∞

i=0: 12>-∞, second=-∞, largest=12
i=1: 35>12, second=12, largest=35
i=2: 1<35, skip
i=3: 10<35, 10<12, skip
i=4: 34<35, 34>12, second=34
i=5: 1<35, skip

Result: second=34
```

```cpp
int secondLargest(int arr[], int n) {
    if(n < 2) return -1;
    
    int largest = INT_MIN;
    int second = INT_MIN;
    
    for(int i = 0; i < n; i++) {
        if(arr[i] > largest) {
            second = largest;
            largest = arr[i];
        } else if(arr[i] > second && arr[i] != largest) {
            second = arr[i];
        }
    }
    
    return (second == INT_MIN) ? -1 : second;
}

int main() {
    int arr[] = {12, 35, 1, 10, 34, 1};
    int n = 6;
    
    cout << "Second largest: " << secondLargest(arr, n) << endl;
    // Output: 34
    
    return 0;
}
```
**Time:** O(n), **Space:** O(1) ✅

**Edge Cases:**
- n < 2
- All elements same
- Negative numbers

---

### **Problem 5: Majority Element (Boyer-Moore Algorithm)**

**Problem:** Find element that appears more than n/2 times.

**Example:**
```
Input: [2, 2, 1, 1, 1, 2, 2]
Output: 2 (appears 4 times, 4 > 7/2)
```

**Approach 1: Hashing**
```cpp
int majorityElementHash(int arr[], int n) {
    unordered_map<int, int> freq;
    
    for(int i = 0; i < n; i++) {
        freq[arr[i]]++;
        if(freq[arr[i]] > n/2) {
            return arr[i];
        }
    }
    
    return -1;
}
```
**Time:** O(n), **Space:** O(n)

**Approach 2: Boyer-Moore Voting (Optimized)**

**Key Insight:** Majority element appears > n/2 times, so if we cancel out each occurrence with different element, majority survives!

**Dry Run:**
```
[2, 2, 1, 1, 1, 2, 2]

candidate=?, count=0

i=0: 2, count=0 → candidate=2, count=1
i=1: 2, 2==2 → count=2
i=2: 1, 1≠2 → count=1
i=3: 1, 1≠2 → count=0
i=4: 1, count=0 → candidate=1, count=1
i=5: 2, 2≠1 → count=0
i=6: 2, count=0 → candidate=2, count=1

Candidate: 2
Verify: 2 appears 4 times > 7/2 ✓
```

```cpp
int majorityElement(int arr[], int n) {
    // Phase 1: Find candidate
    int candidate = arr[0];
    int count = 1;
    
    for(int i = 1; i < n; i++) {
        if(arr[i] == candidate) {
            count++;
        } else {
            count--;
        }
        
        if(count == 0) {
            candidate = arr[i];
            count = 1;
        }
    }
    
    // Phase 2: Verify candidate (not always needed if guaranteed to exist)
    count = 0;
    for(int i = 0; i < n; i++) {
        if(arr[i] == candidate) {
            count++;
        }
    }
    
    if(count > n/2) {
        return candidate;
    }
    
    return -1;  // No majority element
}

int main() {
    int arr[] = {2, 2, 1, 1, 1, 2, 2};
    int n = 7;
    
    cout << "Majority element: " << majorityElement(arr, n) << endl;
    // Output: 2
    
    return 0;
}
```
**Time:** O(n), **Space:** O(1) ✅

---

### **Problem 6: Maximum Subarray Sum (Kadane's Algorithm)**

Already covered in Part 3! Here's the code again:

```cpp
int maxSubarraySum(int arr[], int n) {
    int currentSum = arr[0];
    int maxSum = arr[0];
    
    for(int i = 1; i < n; i++) {
        currentSum = max(arr[i], currentSum + arr[i]);
        maxSum = max(maxSum, currentSum);
    }
    
    return maxSum;
}
```

---

### **Problem 7: Merge Two Sorted Arrays**

**Problem:** Merge two sorted arrays into one sorted array.

**Example:**
```
arr1: [1, 3, 5, 7]
arr2: [2, 4, 6, 8]
result: [1, 2, 3, 4, 5, 6, 7, 8]
```

**Approach: Two Pointer**

**Dry Run:**
```
arr1: [1, 3, 5, 7]  i=0
arr2: [2, 4, 6, 8]  j=0
result: []

Compare 1 vs 2: 1 smaller, result=[1], i++
Compare 3 vs 2: 2 smaller, result=[1,2], j++
Compare 3 vs 4: 3 smaller, result=[1,2,3], i++
Compare 5 vs 4: 4 smaller, result=[1,2,3,4], j++
Compare 5 vs 6: 5 smaller, result=[1,2,3,4,5], i++
Compare 7 vs 6: 6 smaller, result=[1,2,3,4,5,6], j++
Compare 7 vs 8: 7 smaller, result=[1,2,3,4,5,6,7], i++
arr1 done, add remaining: result=[1,2,3,4,5,6,7,8]
```

```cpp
vector<int> mergeSortedArrays(int arr1[], int n1, int arr2[], int n2) {
    vector<int> result;
    int i = 0, j = 0;
    
    while(i < n1 && j < n2) {
        if(arr1[i] <= arr2[j]) {
            result.push_back(arr1[i]);
            i++;
        } else {
            result.push_back(arr2[j]);
            j++;
        }
    }
    
    // Add remaining elements from arr1
    while(i < n1) {
        result.push_back(arr1[i]);
        i++;
    }
    
    // Add remaining elements from arr2
    while(j < n2) {
        result.push_back(arr2[j]);
        j++;
    }
    
    return result;
}

int main() {
    int arr1[] = {1, 3, 5, 7};
    int arr2[] = {2, 4, 6, 8};
    int n1 = 4, n2 = 4;
    
    vector<int> result = mergeSortedArrays(arr1, n1, arr2, n2);
    
    for(int x : result) {
        cout << x << " ";
    }
    // Output: 1 2 3 4 5 6 7 8
    
    return 0;
}
```
**Time:** O(n1 + n2), **Space:** O(n1 + n2)

---

### **Problem 8: Subarray with Given Sum**

**Problem:** Find if there exists a subarray with given sum (positive numbers only).

**Example:**
```
Input: [1, 4, 20, 3, 10, 5], sum=33
Output: true (subarray [20, 3, 10])
```

**Approach: Sliding Window (Variable Size)**

**Dry Run:**
```
[1, 4, 20, 3, 10, 5], target=33

left=0, right=0, sum=0

Add arr[0]=1: sum=1 < 33
Add arr[1]=4: sum=5 < 33
Add arr[2]=20: sum=25 < 33
Add arr[3]=3: sum=28 < 33
Add arr[4]=10: sum=38 > 33
  Remove arr[0]=1: sum=37 > 33
  Remove arr[1]=4: sum=33 == 33 → Found!
```

```cpp
bool subarraySum(int arr[], int n, int target) {
    int left = 0, sum = 0;
    
    for(int right = 0; right < n; right++) {
        sum += arr[right];
        
        // Shrink window while sum > target
        while(sum > target && left <= right) {
            sum -= arr[left];
            left++;
        }
        
        if(sum == target) {
            cout << "Subarray found: [" << left << ", " << right << "]" << endl;
            return true;
        }
    }
    
    return false;
}

int main() {
    int arr[] = {1, 4, 20, 3, 10, 5};
    int n = 6;
    int target = 33;
    
    if(subarraySum(arr, n, target)) {
        cout << "Subarray with sum " << target << " exists" << endl;
    } else {
        cout << "No subarray found" << endl;
    }
    
    return 0;
}
```
**Time:** O(n), **Space:** O(1) ✅

**Note:** This works only for positive numbers. For arrays with negatives, use hashing with prefix sum.

---

### **Problem 9: Leaders in Array**

**Problem:** An element is a leader if all elements to its right are smaller.

**Example:**
```
Input: [16, 17, 4, 3, 5, 2]
Output: [17, 5, 2]
(17 > all right, 5 > 2, 2 is rightmost)
```

**Approach 1: Brute Force**
```cpp
vector<int> findLeadersBrute(int arr[], int n) {
    vector<int> leaders;
    
    for(int i = 0; i < n; i++) {
        bool isLeader = true;
        
        for(int j = i + 1; j < n; j++) {
            if(arr[j] >= arr[i]) {
                isLeader = false;
                break;
            }
        }
        
        if(isLeader) {
            leaders.push_back(arr[i]);
        }
    }
    
    return leaders;
}
```
**Time:** O(n²), **Space:** O(1)

**Approach 2: Optimized (Right to Left)**

**Key Insight:** Traverse from right, track maximum seen so far.

**Dry Run:**
```
[16, 17, 4, 3, 5, 2]

From right:
i=5: arr[5]=2, maxRight=-∞, 2 is leader, maxRight=2
i=4: arr[4]=5, maxRight=2, 5>2, leader, maxRight=5
i=3: arr[3]=3, maxRight=5, 3<5, not leader
i=2: arr[2]=4, maxRight=5, 4<5, not leader
i=1: arr[1]=17, maxRight=5, 17>5, leader, maxRight=17
i=0: arr[0]=16, maxRight=17, 16<17, not leader

Leaders: [2, 5, 17] (reverse to get order: [17, 5, 2])
```

```cpp
vector<int> findLeaders(int arr[], int n) {
    vector<int> leaders;
    int maxRight = INT_MIN;
    
    // Traverse from right to left
    for(int i = n - 1; i >= 0; i--) {
        if(arr[i] > maxRight) {
            leaders.push_back(arr[i]);
            maxRight = arr[i];
        }
    }
    
    // Reverse to get left-to-right order
    reverse(leaders.begin(), leaders.end());
    
    return leaders;
}

int main() {
    int arr[] = {16, 17, 4, 3, 5, 2};
    int n = 6;
    
    vector<int> leaders = findLeaders(arr, n);
    
    cout << "Leaders: ";
    for(int x : leaders) {
        cout << x << " ";
    }
    // Output: 17 5 2
    
    return 0;
}
```
**Time:** O(n), **Space:** O(k) where k = number of leaders ✅

---

### **Problem 10: Stock Buy and Sell (Single Transaction)**

**Problem:** Find maximum profit from buying and selling once.

**Example:**
```
Input: [7, 1, 5, 3, 6, 4]
Output: 5 (buy at 1, sell at 6)
```

**Approach: Track Minimum Price**

**Key Insight:** For each day, calculate profit if we sell today. Track minimum price seen so far.

**Dry Run:**
```
[7, 1, 5, 3, 6, 4]

minPrice=∞, maxProfit=0

i=0: price=7, minPrice=7, profit=7-7=0, maxProfit=0
i=1: price=1, minPrice=1, profit=1-1=0, maxProfit=0
i=2: price=5, minPrice=1, profit=5-1=4, maxProfit=4
i=3: price=3, minPrice=1, profit=3-1=2, maxProfit=4
i=4: price=6, minPrice=1, profit=6-1=5, maxProfit=5
i=5: price=4, minPrice=1, profit=4-1=3, maxProfit=5

Maximum profit: 5
```

```cpp
int maxProfit(int prices[], int n) {
    if(n == 0) return 0;
    
    int minPrice = INT_MAX;
    int maxProfit = 0;
    
    for(int i = 0; i < n; i++) {
        minPrice = min(minPrice, prices[i]);
        int profit = prices[i] - minPrice;
        maxProfit = max(maxProfit, profit);
    }
    
    return maxProfit;
}

int main() {
    int prices[] = {7, 1, 5, 3, 6, 4};
    int n = 6;
    
    cout << "Maximum profit: " << maxProfit(prices, n) << endl;
    // Output: 5
    
    return 0;
}
```
**Time:** O(n), **Space:** O(1) ✅

**Edge Cases:**
- Prices always decreasing → profit = 0
- Single element → profit = 0
- All same price → profit = 0

---

## **PART 5: ADVANCED & TRICKY PROBLEMS**

### **Problem 1: Dutch National Flag (Sort 0s, 1s, 2s)**

**Problem:** Sort array containing only 0s, 1s, and 2s in-place.

**Example:**
```
Input: [2, 0, 2, 1, 1, 0]
Output: [0, 0, 1, 1, 2, 2]
```

**Approach: Three Pointer**

**Key Idea:**
- `low` pointer: boundary for 0s
- `mid` pointer: current element
- `high` pointer: boundary for 2s

**Rules:**
- If arr[mid] == 0: swap with low, move both pointers right
- If arr[mid] == 1: move mid right
- If arr[mid] == 2: swap with high, move high left

**Dry Run:**
```
[2, 0, 2, 1, 1, 0]
 ↑        ↑     ↑
low      mid   high

mid=0: arr[0]=2, swap with high: [0, 0, 2, 1, 1, 2], high--
       [0, 0, 2, 1, 1, 2]
        ↑     ↑  ↑
       low   mid high

mid=0: arr[0]=0, swap with low: [0, 0, 2, 1, 1, 2], low++, mid++
       [0, 0, 2, 1, 1, 2]
           ↑  ↑  ↑
          low mid high

mid=1: arr[1]=0, swap with low: [0, 0, 2, 1, 1, 2], low++, mid++
       [0, 0, 2, 1, 1, 2]
              ↑  ↑  ↑
             low mid high

mid=2: arr[2]=2, swap with high: [0, 0, 1, 1, 2, 2], high--
       [0, 0, 1, 1, 2, 2]
              ↑  ↑
             low mid,high

mid=2: arr[2]=1, mid++
       [0, 0, 1, 1, 2, 2]
              ↑     ↑
             low   mid
                   high

mid=3: arr[3]=1, mid++
       [0, 0, 1, 1, 2, 2]
              ↑        ↑
             low      mid
                      high

mid > high, stop

Result: [0, 0, 1, 1, 2, 2]
```

```cpp
void sortColors(int arr[], int n) {
    int low = 0;
    int mid = 0;
    int high = n - 1;
    
    while(mid <= high) {
        if(arr[mid] == 0) {
            swap(arr[low], arr[mid]);
            low++;
            mid++;
        } else if(arr[mid] == 1) {
            mid++;
        } else {  // arr[mid] == 2
            swap(arr[mid], arr[high]);
            high--;
            // Don't increment mid, need to check swapped element
        }
    }
}

int main() {
    int arr[] = {2, 0, 2, 1, 1, 0};
    int n = 6;
    
    sortColors(arr, n);
    
    for(int i = 0; i < n; i++) {
        cout << arr[i] << " ";
    }
    // Output: 0 0 1 1 2 2
    
    return 0;
}
```
**Time:** O(n), **Space:** O(1) ✅

---

### **Problem 2: Trapping Rain Water**

**Problem:** Given elevation map, calculate how much water can be trapped.

**Example:**
```
Input: [0, 1, 0, 2, 1, 0, 1, 3, 2, 1, 2, 1]
Output: 6

Visualization:
       ▓
   ▓███▓█▓█
 ▓█▓█▓█▓███
0123012321

Water trapped (█ = water):
Position 2: min(1, 3) - 0 = 1
Position 4: min(2, 3) - 1 = 1
Position 5: min(2, 3) - 0 = 2
Position 6: min(2, 3) - 1 = 1
Position 9: min(2, 2) - 1 = 1
Total: 6
```

**Approach: Pre-compute Left Max and Right Max**

**Key Insight:**
Water at position i = `min(leftMax[i], rightMax[i]) - height[i]`

**Steps:**
1. For each position, find max height to its left
2. For each position, find max height to its right
3. Water = min(leftMax, rightMax) - current height

```cpp
int trapRainWater(int height[], int n) {
    if(n <= 2) return 0;
    
    // Compute left max for each position
    int leftMax[n];
    leftMax[0] = height[0];
    for(int i = 1; i < n; i++) {
        leftMax[i] = max(leftMax[i-1], height[i]);
    }
    
    // Compute right max for each position
    int rightMax[n];
    rightMax[n-1] = height[n-1];
    for(int i = n-2; i >= 0; i--) {
        rightMax[i] = max(rightMax[i+1], height[i]);
    }
    
    // Calculate water trapped
    int totalWater = 0;
    for(int i = 0; i < n; i++) {
        int water = min(leftMax[i], rightMax[i]) - height[i];
        totalWater += water;
    }
    
    return totalWater;
}

// Space-optimized: Two Pointer
int trapRainWaterOptimized(int height[], int n) {
    if(n <= 2) return 0;
    
    int left = 0, right = n - 1;
    int leftMax = 0, rightMax = 0;
    int water = 0;
    while(left < right) {
        if(height[left] < height[right]) {
            if(height[left] >= leftMax) {
                leftMax = height[left];
            } else {
                water += leftMax - height[left];
            }
            left++;
        } else {
            if(height[right] >= rightMax) {
                rightMax = height[right];
            } else {
                water += rightMax - height[right];
            }
            right--;
        }
    }
    
    return water;
}

int main() {
    int height[] = {0, 1, 0, 2, 1, 0, 1, 3, 2, 1, 2, 1};
    int n = 12;
    
    cout << "Water trapped: " << trapRainWater(height, n) << endl;
    // Output: 6
    
    cout << "Water trapped (optimized): " << trapRainWaterOptimized(height, n) << endl;
    // Output: 6
    
    return 0;
}
```

**Time:** O(n), **Space:** O(n) or O(1) with two pointer ✅

---

### **Problem 3: Maximum Product Subarray**

**Problem:** Find contiguous subarray with largest product.

**Example:**
```
Input: [2, 3, -2, 4]
Output: 6 (subarray [2, 3])
```

**Key Challenge:** Negative numbers can flip min/max!

**Dry Run:**
```
[2, 3, -2, 4]

maxProd=2, minProd=2, result=2

i=1: num=3
  temp = maxProd*3 = 6
  maxProd = max(3, 6, 2*3=6) = 6
  minProd = min(3, 6, 2*3=6) = 3
  result = 6

i=2: num=-2
  temp = maxProd*(-2) = -12
  maxProd = max(-2, -12, 3*(-2)=-6) = -2
  minProd = min(-2, -12, 3*(-2)=-6) = -12
  result = 6

i=3: num=4
  temp = maxProd*4 = -8
  maxProd = max(4, -8, -12*4=-48) = 4
  minProd = min(4, -8, -12*4=-48) = -48
  result = 6

Maximum product: 6
```

```cpp
int maxProduct(int arr[], int n) {
    if(n == 0) return 0;
    
    int maxProd = arr[0];
    int minProd = arr[0];
    int result = arr[0];
    
    for(int i = 1; i < n; i++) {
        // When multiplied by negative, max becomes min and vice versa
        if(arr[i] < 0) {
            swap(maxProd, minProd);
        }
        
        maxProd = max(arr[i], maxProd * arr[i]);
        minProd = min(arr[i], minProd * arr[i]);
        
        result = max(result, maxProd);
    }
    
    return result;
}

int main() {
    int arr[] = {2, 3, -2, 4};
    int n = 4;
    
    cout << "Maximum product: " << maxProduct(arr, n) << endl;
    // Output: 6
    
    return 0;
}
```
**Time:** O(n), **Space:** O(1) ✅

---

### **Problem 4: Longest Consecutive Sequence**

**Problem:** Find length of longest consecutive sequence (elements need not be contiguous in array).

**Example:**
```
Input: [100, 4, 200, 1, 3, 2]
Output: 4 (sequence: 1, 2, 3, 4)
```

**Approach: Hash Set**

**Key Insight:** Only start counting from numbers that are sequence starts (num-1 doesn't exist).

**Dry Run:**
```
[100, 4, 200, 1, 3, 2]
Set: {100, 4, 200, 1, 3, 2}

Check 100: 99 not in set, start=100
  Check 101: not in set, length=1

Check 4: 3 in set, skip (not a start)

Check 200: 199 not in set, start=200
  Check 201: not in set, length=1

Check 1: 0 not in set, start=1
  Check 2: in set, continue
  Check 3: in set, continue
  Check 4: in set, continue
  Check 5: not in set, length=4 ← Maximum

Check 3: 2 in set, skip

Check 2: 1 in set, skip

Longest: 4
```

```cpp
int longestConsecutive(int arr[], int n) {
    if(n == 0) return 0;
    
    unordered_set<int> numSet(arr, arr + n);
    int longest = 0;
    
    for(int num : numSet) {
        // Check if it's the start of a sequence
        if(numSet.find(num - 1) == numSet.end()) {
            int currentNum = num;
            int currentLength = 1;
            
            // Count consecutive numbers
            while(numSet.find(currentNum + 1) != numSet.end()) {
                currentNum++;
                currentLength++;
            }
            
            longest = max(longest, currentLength);
        }
    }
    
    return longest;
}

int main() {
    int arr[] = {100, 4, 200, 1, 3, 2};
    int n = 6;
    
    cout << "Longest consecutive sequence: " << longestConsecutive(arr, n) << endl;
    // Output: 4
    
    return 0;
}
```
**Time:** O(n), **Space:** O(n) ✅

---

### **Problem 5: Count Inversions**

**Problem:** Count pairs (i, j) where i < j and arr[i] > arr[j].

**Example:**
```
Input: [2, 4, 1, 3, 5]
Output: 3
Inversions: (2,1), (4,1), (4,3)
```

**Approach 1: Brute Force**
```cpp
int countInversionsBrute(int arr[], int n) {
    int count = 0;
    for(int i = 0; i < n - 1; i++) {
        for(int j = i + 1; j < n; j++) {
            if(arr[i] > arr[j]) {
                count++;
            }
        }
    }
    return count;
}
```
**Time:** O(n²), **Space:** O(1)

**Approach 2: Merge Sort (Optimized)**

**Key Insight:** Count inversions while merging.

```cpp
int mergeAndCount(int arr[], int temp[], int left, int mid, int right) {
    int i = left;     // Left subarray
    int j = mid + 1;  // Right subarray
    int k = left;     // Merged array
    int invCount = 0;
    
    while(i <= mid && j <= right) {
        if(arr[i] <= arr[j]) {
            temp[k++] = arr[i++];
        } else {
            // arr[i] > arr[j], so all elements from i to mid are greater
            temp[k++] = arr[j++];
            invCount += (mid - i + 1);
        }
    }
    
    while(i <= mid) {
        temp[k++] = arr[i++];
    }
    
    while(j <= right) {
        temp[k++] = arr[j++];
    }
    
    // Copy back to original array
    for(i = left; i <= right; i++) {
        arr[i] = temp[i];
    }
    
    return invCount;
}

int mergeSortAndCount(int arr[], int temp[], int left, int right) {
    int invCount = 0;
    
    if(left < right) {
        int mid = left + (right - left) / 2;
        
        invCount += mergeSortAndCount(arr, temp, left, mid);
        invCount += mergeSortAndCount(arr, temp, mid + 1, right);
        invCount += mergeAndCount(arr, temp, left, mid, right);
    }
    
    return invCount;
}

int countInversions(int arr[], int n) {
    int temp[n];
    return mergeSortAndCount(arr, temp, 0, n - 1);
}

int main() {
    int arr[] = {2, 4, 1, 3, 5};
    int n = 5;
    
    cout << "Inversion count: " << countInversions(arr, n) << endl;
    // Output: 3
    
    return 0;
}
```
**Time:** O(n log n), **Space:** O(n) ✅

---

### **Problem 6: Kth Largest/Smallest Element**

**Problem:** Find Kth largest element efficiently.

**Example:**
```
Input: [3, 2, 1, 5, 6, 4], k=2
Output: 5 (2nd largest)
```

**Approach 1: Sorting**
```cpp
int kthLargestSort(int arr[], int n, int k) {
    sort(arr, arr + n, greater<int>());
    return arr[k-1];
}
```
**Time:** O(n log n)

**Approach 2: Min Heap (Optimized)**
```cpp
int kthLargest(int arr[], int n, int k) {
    priority_queue<int, vector<int>, greater<int>> minHeap;
    
    for(int i = 0; i < n; i++) {
        minHeap.push(arr[i]);
        
        if(minHeap.size() > k) {
            minHeap.pop();
        }
    }
    
    return minHeap.top();
}
```
**Time:** O(n log k), **Space:** O(k) ✅

---

### **Problem 7: Prefix XOR Problems**

**Problem:** Answer range XOR queries efficiently.

**Concept:** Similar to prefix sum, but with XOR.

**Property:** `a XOR a = 0`, so `prefixXOR[r] XOR prefixXOR[l-1] = XOR of range [l, r]`

```cpp
vector<int> buildPrefixXOR(int arr[], int n) {
    vector<int> prefixXOR(n);
    prefixXOR[0] = arr[0];
    
    for(int i = 1; i < n; i++) {
        prefixXOR[i] = prefixXOR[i-1] ^ arr[i];
    }
    
    return prefixXOR;
}

int rangeXOR(vector<int>& prefixXOR, int left, int right) {
    if(left == 0) {
        return prefixXOR[right];
    }
    return prefixXOR[right] ^ prefixXOR[left - 1];
}

int main() {
    int arr[] = {1, 3, 4, 8};
    int n = 4;
    
    vector<int> prefixXOR = buildPrefixXOR(arr, n);
    
    // Query: XOR from index 1 to 2
    cout << "XOR[1..2]: " << rangeXOR(prefixXOR, 1, 2) << endl;
    // 3 XOR 4 = 7
    
    return 0;
}
```

---

## **PART 6: COMMON MISTAKES & EDGE CASES**

### **1. Off-by-One Errors**

**Common Mistakes:**
```cpp
// WRONG: Missing last element
for(int i = 0; i < n-1; i++)  // Should be i < n

// WRONG: Array out of bounds
for(int i = 0; i <= n; i++)  // Should be i < n

// WRONG: Infinite loop
for(int i = 0; i < n; )  // Forgot i++

// CORRECT
for(int i = 0; i < n; i++)
```

**In Loops with Two Pointers:**
```cpp
// WRONG
while(left <= right)  // For reversal, should be left < right

// WRONG
while(left < right) {
    swap(arr[left], arr[right]);
    // Forgot to move pointers!
}

// CORRECT
while(left < right) {
    swap(arr[left], arr[right]);
    left++;
    right--;
}
```

---

### **2. Index Out of Bounds**

**Always check:**
```cpp
// Before accessing
if(i >= 0 && i < n) {
    // Safe to access arr[i]
}

// In binary search
int mid = left + (right - left) / 2;  // Prevents overflow

// When using i+1, i-1
if(i + 1 < n) {
    // Safe to access arr[i+1]
}
```

---

### **3. Overflow Issues**

**Problem:** Integer overflow in calculations.

```cpp
// WRONG: Can overflow
int sum = arr[i] + arr[j];

// CORRECT: Use long long for large numbers
long long sum = (long long)arr[i] + arr[j];

// WRONG in binary search
int mid = (left + right) / 2;  // Can overflow if left + right > INT_MAX

// CORRECT
int mid = left + (right - left) / 2;
```

---

### **4. When Array Fails**

**Arrays don't work well for:**
- **Frequent insertions/deletions in middle** → Use LinkedList
- **Need to find duplicates quickly** → Use HashSet
- **Count frequencies** → Use HashMap
- **Need sorted order always** → Use Set/Map (BST)

**Example: Finding duplicates**

```cpp
// Array approach: O(n²)
bool hasDuplicateArray(int arr[], int n) {
    for(int i = 0; i < n; i++) {
        for(int j = i + 1; j < n; j++) {
            if(arr[i] == arr[j]) return true;
        }
    }
    return false;
}

// Hash Set approach: O(n)
bool hasDuplicateSet(int arr[], int n) {
    unordered_set<int> seen;
    for(int i = 0; i < n; i++) {
        if(seen.count(arr[i])) return true;
        seen.insert(arr[i]);
    }
    return false;
}
```

---

### **5. Edge Cases Checklist**

**Always test:**
- [ ] Empty array (n = 0)
- [ ] Single element (n = 1)
- [ ] All elements same
- [ ] Sorted array (ascending/descending)
- [ ] Array with negatives
- [ ] Array with zeros
- [ ] Large values (near INT_MAX)
- [ ] k > n (for k-related problems)
- [ ] Duplicates present

---

## **PART 7: PRACTICE & MASTERY**

### **20 Beginner Problems**

1. Find sum of all elements
2. Find product of all elements
3. Count even and odd numbers
4. Find element at given index
5. Check if element exists
6. Count occurrences of element
7. Find first and last occurrence
8. Swap two elements
9. Check if array is sorted
10. Count positive, negative, and zeros
11. Replace all negative numbers with 0
12. Find difference between max and min
13. Left rotate by 1 position
14. Right rotate by 1 position
15. Separate even and odd numbers
16. Find missing number in 1 to N
17. Check if array is palindrome
18. Remove element at given position
19. Insert element in sorted array
20. Find sum of even-indexed elements

---

### **20 Intermediate Problems**

1. Two sum problem (unsorted)
2. Three sum problem
3. Container with most water
4. Product of array except self
5. Find all pairs with given difference
6. Minimum size subarray sum
7. Longest subarray with sum K
8. Count distinct elements in every window
9. First negative in every window of size k
10. Rearrange positive and negative numbers
11. Find majority element (n/3 times)
12. Maximum difference between two elements
13. Smallest subarray with sum greater than X
14. Equilibrium index
15. Maximum sum with no adjacent elements
16. Find minimum in rotated sorted array
17. Search in rotated sorted array
18. Find peak element
19. Sort array by frequency
20. Kth smallest element

---

### **10 Advanced Interview Problems**

1. **Median of two sorted arrays** - Binary search approach
2. **Longest increasing subsequence** - DP or binary search
3. **Best time to buy and sell stock (multiple transactions)** - DP
4. **Merge intervals** - Sorting + merging
5. **Jump game II (minimum jumps)** - Greedy/BFS
6. **First missing positive** - Array manipulation
7. **Sliding window maximum** - Deque
8. **Count of smaller numbers after self** - Merge sort/BIT
9. **Russian doll envelopes** - LIS variant
10. **Longest substring without repeating characters** - Sliding window + hash

---

## **REVISION CHEAT SHEET**

```
═══════════════════════════════════════════════════════════
                    ARRAY MASTERY CHEAT SHEET
═══════════════════════════════════════════════════════════

📌 CORE OPERATIONS
─────────────────
Access:     O(1)     | arr[i] = base_address + i × size
Search:     O(n)     | Linear scan (unsorted)
Insert:     O(n)     | Shift elements (worst: beginning)
Delete:     O(n)     | Shift elements (worst: beginning)
Traverse:   O(n)     | Visit each element once

═══════════════════════════════════════════════════════════

🎯 KEY PATTERNS
───────────────
1. TWO POINTER
   - Sorted array problems
   - Pair sum, remove duplicates
   - Pattern: left=0, right=n-1

2. SLIDING WINDOW
   - Subarray problems
   - Fixed/variable size window
   - Pattern: Expand right, shrink left

3. PREFIX SUM
   - Range queries O(1)
   - prefix[i] = prefix[i-1] + arr[i]
   - range[L,R] = prefix[R] - prefix[L-1]

4. KADANE'S ALGORITHM
   - Maximum subarray sum
   - current = max(arr[i], current + arr[i])

5. HASHING
   - Two sum, frequency count
   - Trade space for time

6. SORTING + TWO POINTER
   - Three sum, pair problems
   - O(n log n) + O(n)

═══════════════════════════════════════════════════════════

🔥 MUST-KNOW PROBLEMS
─────────────────────
✓ Rotate array (reversal method)
✓ Move zeros (two pointer)
✓ Remove duplicates (two pointer)
✓ Second largest (one pass)
✓ Majority element (Boyer-Moore)
✓ Max subarray sum (Kadane)
✓ Merge sorted arrays
✓ Subarray sum = K
✓ Leaders in array
✓ Stock buy-sell

═══════════════════════════════════════════════════════════

⚠️ COMMON MISTAKES
──────────────────
❌ for(i=0; i<=n; i++)      → Out of bounds
✓  for(i=0; i<n; i++)

❌ mid = (l+r)/2            → Overflow
✓  mid = l+(r-l)/2

❌ Forgetting k = k % n      → Wrong rotation
✓  Always normalize k

❌ Not handling n=0, n=1     → Edge cases
✓  Check before processing

═══════════════════════════════════════════════════════════

💡 COMPLEXITY GOALS
───────────────────
Target for interviews:
- Time:  O(n) or O(n log n)
- Space: O(1) or O(n)

Avoid: O(n²) unless required

═══════════════════════════════════════════════════════════

🧪 TESTING CHECKLIST
────────────────────
□ Empty array (n=0)
□ Single element (n=1)
□ All same elements
□ Sorted/reverse sorted
□ With duplicates
□ With negatives/zeros
□ Large values
□ Edge values (INT_MAX, INT_MIN)

═══════════════════════════════════════════════════════════
```

---

## **7-DAY ARRAY MASTERY PLAN**

### **Day 1: Foundations**
**Focus:** Basics & Core Operations

**Theory (2 hours):**
- Memory layout, indexing
- Static vs dynamic arrays
- Time complexities

**Practice (3 hours):**
- Traversal, insertion, deletion
- Linear search
- Find max/min
- Reverse array

**Problems (5):**
1. Sum of array elements
2. Find second largest
3. Rotate left by 1
4. Move zeros to end
5. Check if sorted

---

### **Day 2: Two Pointer Technique**
**Focus:** Master two pointer pattern

**Theory (1 hour):**
- When to use two pointer
- Variations (opposite ends, same direction)

**Practice (4 hours):**
- Two sum (sorted)
- Remove duplicates
- Pair with given sum
- Three sum

**Problems (6):**
1. Two sum sorted array
2. Remove duplicates sorted
3. Container with most water
4. Valid palindrome
5. Merge sorted arrays
6. Squares of sorted array

---

### **Day 3: Sliding Window**
**Focus:** Fixed & variable size windows

**Theory (1 hour):**
- Fixed vs variable window
- When to expand/shrink

**Practice (4 hours):**
- Max sum k consecutive
- Longest substring without repeat
- Minimum window substring

**Problems (5):**
1. Max sum subarray size k
2. First negative in window k
3. Count anagrams in string
4. Longest substring k distinct
5. Subarray with given sum

---

### **Day 4: Prefix Sum & Kadane**
**Focus:** Range queries & max subarray

**Theory (1 hour):**
- Prefix sum concept
- Kadane's algorithm intuition

**Practice (4 hours):**
- Range sum queries
- Equilibrium index
- Maximum subarray sum
- Maximum product subarray

**Problems (5):**
1. Range sum queries (multiple)
2. Subarray sum equals K
3. Max subarray sum (Kadane)
4. Max product subarray
5. Equilibrium index

---

### **Day 5: Hashing & Sorting**
**Focus:** Hash maps & sorting techniques

**Theory (1 hour):**
- When to use hashing vs sorting
- Trade-offs

**Practice (4 hours):**
- Two sum unsorted
- Frequency counting
- Dutch national flag
- Sort 0s 1s 2s

**Problems (6):**
1. Two sum unsorted
2. Find duplicates
3. Longest consecutive sequence
4. Sort colors (0,1,2)
5. Top K frequent elements
6. Group anagrams

---

### **Day 6: Advanced Problems**
**Focus:** Tricky interview questions

**Practice (5 hours):**
- Trapping rain water
- Stock buy-sell
- Count inversions
- Leaders in array

**Problems (5):**
1. Trapping rain water
2. Stock buy-sell (single)
3. Stock buy-sell (multiple)
4. Leaders in array
5. Next greater element

---

### **Day 7: Mock Interview & Review**
**Focus:** Timed practice & revision

**Morning (3 hours):**
- Solve 5 random problems (40 min each)
- No looking at solutions
- Focus on: Approach → Code → Test

**Afternoon (2 hours):**
- Review all mistakes
- Revise cheat sheet
- Practice explaining solutions

**Evening (2 hours):**
- Mock interview (1 hour)
- Review weak areas (1 hour)

---

## **FINAL TIPS**

### **Problem-Solving Framework**

```
1. UNDERSTAND
   - Read carefully
   - Note constraints
   - Ask clarifying questions

2. EXAMPLES
   - Draw small example
   - Edge cases
   - Expected output

3. BRUTE FORCE
   - Think simple solution first
   - State complexity

4. OPTIMIZE
   - Can we do better?
   - What pattern fits?
   - Trade space for time?

5. CODE
   - Write clean code
   - Handle edge cases
   - Use meaningful names

6. TEST
   - Run through examples
   - Check edge cases
   - Dry run logic
```

### **Interview Strategy**

1. **Think aloud** - Explain your thought process
2. **Start simple** - Brute force first, then optimize
3. **Ask questions** - Clarify requirements
4. **Test thoroughly** - Don't rush to submit
5. **Handle feedback** - Adapt if interviewer hints

### **Debugging Tips**

- Print intermediate values
- Check array bounds
- Verify loop conditions
- Test with small inputs first

---

**You now have everything you need to master arrays!** 

Practice daily, focus on understanding WHY each approach works, and don't just memorize code. Arrays are the foundation—master them, and harder data structures become easier.

**Ready to conquer arrays? Let's go! 🚀**

