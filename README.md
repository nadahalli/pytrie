# About

This code is forked from [nadahalli's](https://github.com/nadahalli/pytrie.git) and it was originally forked from [gsakkis'](https://bitbucket.org/gsakkis/pytrie).

## Nadahalli's fix

Nadahalli fixed a bug in the original code that has to do with overlapping prefixes (shown below).

Originally:

```python
import pytrie
T = pytrie.Trie()
T[[1]] = 1
T.longest_prefix_item([1,1], (None, 0)) # correctly returns ((1,), 1)
T[[1,1,2]] = 2 # note this prefix overlaps with the first one
T.longest_prefix([1,1], None) # incorrectly returns None
T.longest_prefix_item([1,1], (None, 0)) # incorrectly returns (None, 0)
T.longest_prefix_value([1,1], (None, 0)) # incorrectly returns 0
```

After nadahalli's fix:

```python
import pytrie
T = pytrie.Trie()
T[[1]] = 1
T.longest_prefix_item([1,1], (None, 0)) # correctly returns ((1,), 1)
T[[1,1,2]] = 2 # note this prefix overlaps with the first one
T.longest_prefix([1,1], None) # correctly returns (1,)
T.longest_prefix_item([1,1], (None, 0)) # correctly returns ((1,), 1)
T.longest_prefix_value([1,1], 0) # incorrectly returns 0
```

From nadahalli's page:

>"If the trie is made from strings ('c', 'cex') and the query is 'cey', the longest matching prefix from the given set is 'c'. The earlier code did a 'greedy' match for 'cey' and traversed the trie till 'ce', found no corresponding trie entry, and returned in a KeyError. The required solution is to return 'c' instead. The patch stores the longest previouly encountered non-null values in case all future probes are futile."

## Reusing the fix

In this fork I am simply reusing that fix not to miss the correct value as shown in the last line of the example above.

