# php
Covers some key language features.

## Callable's

```php
// Passing callable's arguments to a function.
function perform(callable $callback) {
    $callback();
}

perform(function() {
    echo "Hello from passed in clojure! \n";
});

// Returning callable's from a function.
function generateGreetingFn(string $name): callable {
    return function () use ($name) {
        echo "Hello, " . $name . ", we hope your doing well. \n";
    };
}

$greet = generateGreetingFn('Sam');
$greet();
```

## Iterables

```php
$names = [
    'Sam',
    'Tammy',
    'Tom',
    'Jay'
];

class NamesCollection implements IteratorAggregate {
    public function __construct(private $items) {}

    public function getIterator(): Traversable {
        return new ArrayIterator($this->items);
    }
}

// Accepts any iterable type.
function dumpAll(iterable $names)
{
    foreach ($names as $name) {
        var_dump($name);
    }
}

dumpAll($names);
dumpAll(new NamesCollection($names));
```

## Multi-Catches

```php
class NotFoundException extends Exception {}
class EmptyException extends Exception {}

/**
 *  Looks for a the first item in a collection.
 */
function findItem(iterable $collection, int $match): int|Exception {
    if (count($collection) === 0) throw new EmptyException(
        "The provided collection is empty.\n"
    );

    foreach ($collection as $item) {
        if ($item === $match) {
            return $item;
        }
    }

    throw new NotFoundException("A match could not be found\n");
}

// We can do it this way:
try {
    $collection = [];
    $found = findItem($collection, 3);
} catch (NotFoundException $e) {
    echo $e->getMessage();
} catch (EmptyException $e) {
    // Will go the this block, as the array is empty.
    echo $e->getMessage();
}

// Or even better we can do it like this:
try {
    $collection = [1, 2, 3, 4, 5];
    $found = findItem($collection, 6);
} catch (NotFoundException | EmptyException $e) {
    echo $e->getMessage();
}
```