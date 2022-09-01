---
title: Perl hashes
date: 2022-08-29 09:00:00 HH:MM:SS +0200
categories: [Development, Perl]
tags: [development, perl, hashes]
---

### Hashes

#### Creation

Empty:

```perl
my %hash = ();
```

With elements, either:

```perl
my %hash = ();
$hash{'Foo'} = 1;
$hash{'Bar'} = 2;
$hash{'Baz'} = 3;
```

Or:

```perl
my %hash = (
    'Foo' => 1,
    'Bar' => 2,
    'Baz' => 3
)
```

#### Access to a value

```perl
print "Foo value is $hash{'Foo'}\n";
```

#### Access to keys or values

With `keys` or `values` keywords:

```perl
my @hashKeys = keys %hash;
my @hashValues = values %hash;
```

Another usage example:

```perl
foreach my $currentHashKey (keys %hash) {
    # ...
}
```

#### Check if a value exists

With `exists` keyword:

```perl
my $keyToTest = 'Foo';
if (exists($hash{$keyToTest})) {
    print "$keyToTest exists!\n";
}
```

#### Get the hash size

Simply by getting the scalar that belong to the keys or the values:

```perl
my @hashKeys = keys %hash;
my @hashSize = @hashKeys;
```

#### Add or remove element to an existing hash

Add an element:

```perl
my %hash = (
    'Foo' => 1,
    'Bar' => 2,
    'Baz' => 3
)
# Adding a new element
$hash{'Qux'} = 4;
```

Remove an element withe the `delete` keyword:

```perl
my %hash = (
    'Foo' => 1,
    'Bar' => 2,
    'Baz' => 3
)
# Adding a new element
delete $hash{'Bar'};
```

#### Complex elements in a hash

Multidimensional hash:

```perl
# Init with value
my %hash = {
    'a key' => {
        name => 'A name',
        intValue => 1,
        somethingElse => 'dummy'
    }
};
my $key = 'another key';
$hash{$key} = {
    name => 'Another name',
    intValue => 2,
    somethingElse => 'dummy'
};
```

Get a multidimensional hash value:

```perl
print "A name is: $hash{$key}{name}\n";
```

Set a multidimensional hash value:

```perl
$hash{$key}->{name} = 'A new name!';
```

Hash in a hash with an array element, must be elgobed in `@{}`:

```perl
my @array = (7, 8, 9);
@{$hash{$key}{subHashArray}} = @array;
foreach my $arrayItem (@{$hash{$key}{subHashArray}}) {
    print "$arrayItem\n";
}
```
