PHP RFC: Bidirectional iterator
===============================

* Version: 1.0
* Date: 2018-07-17
* Author: Grégory Planchat <gregory@kiboko.fr>
* Status: In Discussion

Introduction
------------

When using iterators in some cases you may need to iterate in the reverse order,
but the current SPL implementation does not consider this case. Currently, a very
few iterator classes provides a reverse iteration feature.

According to the docs, the following are some of the existing methods:
 * `SplDoublyLinkedList::prev()`
 * `EvStat::prev()`
 * `Judy::prev()`
 * `SQLiteResult::prev()`
 * `IntlBreakIterator::previous()`
 
In some cases (eg. with `Generators`), it is undesirable or impossible to iterate
in the reverse order.

This RFC proposes to add an interface to the SPL and an utility iterator class to
provide this feature as an opt-in to iterators.

Proposal
--------

This RFC proposes to implement the interface that will provide the reverse iterator feature
and a decorator class that helps using those type of iterators in `foreach` loops.

The majority of the existing implementations are using `prev()` method, only `ext_intl` is 
using `previous()` method name. To keep constistent and avoid BC breaks, the `prev()` method
name will be kept. 

### `BididrectionalIterator`

```
interface BidirectionalIterator extends Iterator
{
    /**
     * The mirror method for rewind(), this function will reset the cursor to the end of the iterator
     */
    void proceed( void );

    /**
     * The mirror method for next(), this function will move the cursor one step back
     */
    void prev( void );

    /**
     * This method will provide an iterator to use in foreach loops to iterate in reverse order
     */
    ReverseIterator reverse( void );
}
```

### `ReverseIterator`

```
class ReverseIterator implements Iterator
{
    public function ___construct( BidirectionalIterator $decorated );
}
```

Other related RFC
-----------------

### ArrayIterator improvements

See https://wiki.php.net/rfc/arrayiterator-improvements

Proposed PHP Version(s)
-----------------------

PHP 7.y

Impacts and BC breaks
---------------------

### In core PHP and SPL

The class `SplDoublyLinkedList` will implement the `BidirectionalIterator` interface
instead of `Iterator` interface.

#### BC break

None

### In extensions

#### In Intl

The class `IntlBreakIterator` will implement the `BidirectionalIterator` interface
instead of `Iterator` interface.

The `prev()` method will be implemented from the existing `previous()`, and the latter
will be transformed to a method alias.

##### BC break

The `previous()` method will be marked as deprecated and removed in the next major version (8.0)

#### In PECL EV

The class `EvStat` will implement the `BidirectionalIterator` interface.

##### BC break

None

#### In PECL Judy

This extension has no update since november 2013 and works with PHP 5.2 and 5.3 (no update since).

No changes to the extension to consider.

##### BC break

None

#### In PECL SQLite

This extension is for SQLite 2 and was removed in PHP 5.4.

No changes to the extension to consider.

##### BC break

None

Patches and Tests
-----------------

None yet
