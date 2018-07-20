PHP RFC: Collection objects
=========================

* **Version**: 1.0
* **Date**: 2018-07-19
* **Author**: Grégory Planchat <gregory@kiboko.fr>
* **Status**: In Discussion

Introduction
------------

This document describes the addition of *Collection objects*, native PHP objects aimed at containing a list of data items, indexed numerically.

*Collection objects* are **countable**, **array accessible** and **traversable** objects

The `Collection` interface
--------------------------

```
interface Collection extends Countable, Traversable, ArrayAccess
{
    function void __construct ( mixed ...$item )
    
    function self add ( mixed ...$item )

    function self remove ( mixed ...$item )

    function self removeAt ( int ...$index )

    function mixed at ( int $index )
}
```

The `MergeableCollection` interface
-----------------------------------

```
interface MergeableCollection extends Collection
{
    function self merge ( MergeableCollection ...$collection )
}
```

The `WalkableCollection` interface
----------------------------------

```
interface WalkableCollection extends Collection
{
    function self walk ( callable $callback )
}
```

The `SliceableCollection` interface
-----------------------------------

```
interface SliceableCollection extends Collection
{
    function self slice ( int $offset, int $length )
}
```

The `SpliceableCollection` interface
-----------------------------------

```
interface SpliceableCollection extends Collection
{
    function self splice ( int $offset, int $length, mixed ...$items )
}
```

The `CollectionMapper` interface
---------------------------

```
interface CollectionMapper
{
    function Collection map ( Collection ...$collection )
}
```
