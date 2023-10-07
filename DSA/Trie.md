## Time Complexity

`m` is the length of the string used in the operation.

| Operation | Big-O |
| --------- | ----- |
| Search    | O(m)  |
| Insert    | O(m)  |
| Remove    | O(m)  |

## Edge Cases

- searching for a string in an empty trie
- inserting empty strings into a trie

## Techniques

- Sometimes preprocessing a dictionary of words into a trie, will improve the efficiency of searching for a word of length k, among n words. Searching becomes O(k) instead of O(n)
