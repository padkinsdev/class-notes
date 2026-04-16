# Joins (Sort-Merge Join)

## Merge-join
- Essentially a merge sort on two relations. The two relations must themselves be sorted beforehand
- Another name for sort-merge join
- When duplicate keys appear in a relation, some additional logic is necessary
- Essentially just joining two relations on shared keys, i.e. `a -> 3` and `a -> A` will be joined to `a -> 3,A`
- Defined as `select * from r, s where r.a1 = s.a1`
- Sort-merge joins are superior to hash joins in that they require less memory, specifically `b_r + b_s` block transfers and some seeks depending on memory size
- Sort-merge join is still generally pretty effective even if you have to sort the input relations first

## Joins summary
- Nested-loops join can be used irrespective of the join condition
- Index nested-loops join only applies if an appropriate index exists and work pretty well if the index on S is a primary index
- Hash joins are only for equi-joins and are ideal for large relations

## Group by and aggregation
- `select A, count(B) from R group by A`
- Create a hash table and group together tuples by key value, i.e. `(1,a)` and `(1,b)` will be grouped together as `[(1,a), (1,b)]`
- If the end goal is to count the number of times a key appears then storing the grouped tuples is not necessary. However, if you want to, say, count *distinct* B, then keeping the actual tuple groupings is necessary
- Instead of blindly iterating through the relation, this can be achieved via sorting of the relation such that repetitions of the same key will be naturally grouped together

## Duplicate elimination
- Best done by sorting and then eliminating by iterating and checking if the current key is the same as the previous key

## Sorting
- If the relation is small enough then an in-memory quicksort will work well
- If the relation is too large to fit in memory, an external sort-merge is optimal. This would be performed by reading as much of the relation as possible, quicksorting it, storing it, then repaeting until the end of the relation. FInally, a sort-merge join can be used to combine the chunks