PHP RFC: Sortable objects
=========================

* **Version**: 1.0
* **Date**: 2018-07-19
* **Author**: Grégory Planchat <gregory@kiboko.fr>
* **Status**: In Discussion

Introduction
------------

When using collection or repository patterns we some times need to apply orders to the list, depending on criterias defined by the context.

Proposal
--------

### The `Sortable` interface

```
interface Sortable
{
    /**
     * @param Sorter $sorter
     *
     * @return iterable
     */
    public function sort(Sorter $sorter): iterable;

    /**
     * @param BidirectionalSeekableIterator $left
     * @param BidirectionalSeekableIterator $right
     */
    public function swap(BidirectionalSeekableIterator $left, BidirectionalSeekableIterator $right): void;
}
```

### The `Sorter` interface

```
interface Sorter
{
    /**
     * @param iterable|Composite|Sortable $composite
     */
    public function sort(iterable $composite): void;
}
```

### The `QuickSortStrategy` interface

```
interface QuickSortStrategy
{
    /**
     * @param BidirectionalSeekableIterator $left
     * @param BidirectionalSeekableIterator $right
     *
     * @return int
     */
    public function compare(
        BidirectionalSeekableIterator $left,
        BidirectionalSeekableIterator $right
    ): int;
}
```

### The `SplQuicksort` interface

```
class SplQuicksort implements Sorter
{
    /**
     * @param QuickSortStrategy $strategy
     */
    public function __construct(QuickSortStrategy $strategy);

    /**
     * @param iterable|Composite|Sortable $composite
     */
    public function sort(iterable $composite): void;
}
```

### The `SplCallbackSorter` class

```
class SplCallbackSorter implements Sorter
{
    /** 
     * @param callbale $callback
     */
    public function __construct(callable $callback);

    /**
     * @param iterable|Composite|Sortable $composite
     */
    public function sort(iterable $composite): void;
}
```

### The `SplNaturalSorter` class

```
class SplNaturalSorter implements Sorter
{
    /**
     * @param iterable|Composite|Sortable $composite
     */
    public function sort(iterable $composite): void;
}
```

### The `SplNaturalInsensitiveSorter` class

```
class SplNaturalInsensitiveSorter implements Sorter
{
    /**
     * @param iterable|Composite|Sortable $composite
     */
    public function sort(iterable $composite): void;
}
```

### The `SplShuffleSorter` class

```
class SplShuffleSorter implements Sorter
{
    /**
     * @param iterable|Composite|Sortable $composite
     */
    public function sort(iterable $composite): void;
}
```

Other related RFC
-----------------

### `ArrayIterator` improvements

See https://wiki.php.net/rfc/arrayiterator-improvements

### `BidirectionalIterator`

This RFC and the sorter feature depends on the [`BidirectionalIterator`](bidirectional-iterator.md), as some sorting algorithms like **Quick Sort** requires to be able to iterate in the reverse order.
