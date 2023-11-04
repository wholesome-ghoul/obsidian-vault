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
