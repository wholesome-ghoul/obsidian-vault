---
tags: go, dsa
deck: dsa
---
# Three sum #card
<!-- 1699120559230 016fc562e5a93fecee9d56f7c16afc88 -->

<!-- AnkiFront:start -->

Given an integer array of nums, return all the triplets `[nums[i], nums[j], nums[k]]`, such that `i != j`, `i != k`, and `j != k`, and triplet sum is 0. Solution should not contain duplicates.

```go
func threeSum(nums []int) [][]int { }
```

- `3 <= nums.length <= 3000`
- `-10^5 <= nums[i] <= 10^5`

<!-- AnkiFront:end -->
<!-- AnkiBack:start -->

```go
func threeSum(nums []int) [][]int {
	triplets := [][]int{}

	tripletsSet := make(map[[3]int]bool)
	numsSet := make(map[int]bool)

	slices.Sort(nums)
	for i := range nums {
		if numsSet[nums[i]] {
			continue
		}

		numsSet[nums[i]] = true
		j := i + 1
		k := len(nums) - 1

		for j < k {
			sum := nums[i] + nums[j] + nums[k]

			if sum == 0 {
				triplet := [3]int{nums[i], nums[j], nums[k]}
				tripletsSet[triplet] = true

				j++
				k--
			} else if sum < 0 {
				j++
			} else {
				k--
			}
		}
	}

	for k := range tripletsSet {
		triplets = append(triplets, []int{k[0], k[1], k[2]})
	}

	return triplets
}
```
<!-- AnkiBack:start -->

# Sliding Window Maximum #card
<!-- 1699761700318 02cd3e74812f416bc619cacb12fb4fa5 -->

<!-- AnkiFront:start -->

Given array of ints `nums` and sliding window size `k`, moving from left to right by one position. Return max sliding window (max integer in every window)

```go
func maxSlidingWindow(nums []int, k int) []int { }
```

- `3 <= nums.length <= 3000`
- `-10^5 <= nums[i] <= 10^5`

<!-- AnkiFront:end -->
<!-- AnkiBack:start -->

```go
package sliding_window_maximum

import "fmt"

type Dequeue struct {
	first *Node
	last  *Node
}

type Node struct {
	next  *Node
	prev  *Node
	value int
}

func (d *Dequeue) push(elem int) {
	node := &Node{value: elem}

	if d.isEmpty() {
		d.first = node
		d.last = d.first
		return
	}

	node.prev = d.last
	d.last.next = node
	d.last = node
}

func (d *Dequeue) pop() int {
	value := d.last.value
	d.last = d.last.prev

	if d.last == nil {
		d.first = nil
	} else {
		d.last.next = nil
	}

	return value
}

func (d *Dequeue) popFront() int {
	value := d.first.value
	d.first = d.first.next

	if d.first == nil {
		d.last = nil
	} else {
		d.first.prev = nil
	}

	return value
}

func (d *Dequeue) isEmpty() bool {
	return d.first == nil && d.last == nil
}

func (d *Dequeue) back() int {
	return d.last.value
}

func (d *Dequeue) front() int {
	return d.first.value
}

func (d *Dequeue) print() {
	curr := d.first
	for curr != nil {
		fmt.Printf("%d ", curr.value)
		curr = curr.next
	}
}

func MaxSlidingWindow(nums []int, k int) []int {
	return maxSlidingWindow(nums, k)
}

func maxSlidingWindow(nums []int, k int) []int {
	maxNumsInWindows := make([]int, 0, len(nums)-k+1)

	indexDq := Dequeue{first: nil, last: nil}

	for i := 0; i < k; i++ {
		for !indexDq.isEmpty() && nums[i] >= nums[indexDq.back()] {
			indexDq.pop()
		}
		indexDq.push(i)
	}

	maxNumsInWindows = append(maxNumsInWindows, nums[indexDq.front()])
	for i := k; i < len(nums); i++ {
		if i-indexDq.front() >= k {
			indexDq.popFront()
		}

		for !indexDq.isEmpty() && nums[i] >= nums[indexDq.back()] {
			indexDq.pop()
		}

		indexDq.push(i)
		maxNumsInWindows = append(maxNumsInWindows, nums[indexDq.front()])
	}

	return maxNumsInWindows
}
```

<!-- AnkiBack:end -->

# Group Anagrams #card
<!-- 1699942062579 34cca8ad66907949de10f86184db4148 -->

<!-- AnkiFront:start -->

Given array of strings `strs`. Group anagrams together.

- `1 <= strs.length <= 10^4`
- `0 <= strs[i].length <= 100`
- `strs[i]` consists of lowercase English letters

```go
func groupAnagrams(strs []string) [][]string {}
```

<!-- AnkiFront:end -->
<!-- AnkiBack:start -->

```go
func groupAnagrams(strs []string) [][]string {
  anagrams := [][]string{}
	primes := map[string]int{
		"a": 2,
		"b": 3,
		"c": 5,
		"d": 7,
		"e": 11,
		"f": 13,
		"g": 17,
		"h": 19,
		"i": 23,
		"j": 29,
		"k": 31,
		"l": 37,
		"m": 41,
		"n": 43,
		"o": 47,
		"p": 53,
		"q": 59,
		"r": 61,
		"s": 67,
		"t": 71,
		"u": 73,
		"v": 79,
		"w": 83,
		"x": 89,
		"y": 91,
		"z": 101,
	}

	ints := make([]int, len(strs))
	for i, str := range strs {
		product := 1
		for _, c := range str {
			product *= primes[string(c)]
		}

		ints[i] = product
	}

	intMap := make(map[int]int)
	for i, num := range ints {
		if index, ok := intMap[num]; ok {
			anagram := append(anagrams[index], strs[i])
			anagrams[index] = anagram
		} else {
			anagrams = append(anagrams, []string{strs[i]})
			intMap[num] = len(anagrams) - 1
		}
	}

	return anagrams
}
```

<!-- AnkiBack:end -->
