---
title: Perl arrays
date: 2022-08-29 09:00:00 HH:MM:SS +0200
categories: [Development, Perl]
tags: [development, perl, arrays]
---

### Arrays

```perl
my @array = qw("aa" "bb" "cc");
# To get the length
my $length = @array;
$length = scalar @array;
$length = $#array + 1;
```

`$#` gives the array's last index (`-1` if the array is empty), so `$#array` in this case and so `$#array + 1` also gives its length.

> For a reference to an array, ie. `$arrayReference`, don't forget the reference's `$` to have the last index, so `$#$arrayReference`.
{: .prompt-info }

To check if an array is defined, just do an `if`:

```perl
if (@array) {
    # ...
}   
```

#### Check if a value exist in an array

```perl
if (grep { $_ eq "myValue"} @array) {}
```

#### Range value

With `..` ie. here from `1` to `100`:

```perl
my @array = (1..100);
```

#### Add an item at the end

With `push`:

```perl
my @array = qw("aa" "bb" "cc");
push @array, "dd";
# @array is ("aa", "bb", "cc", "dd")

my @anotherArray = qw("ee" "ff");
push @array, @anotherArray;
# @array is ("aa", "bb", "cc", "dd", "ee", "ff)
```

#### Remove the last item

With `pop`:

```perl
my @array = qw("aa" "bb" "cc");
# pop removes and returns the last item
my $popped = pop @array;
# $popped is "cc", @array is ("aa", "bb")
```

#### Add an item at the beginning

With `unshift`:

```perl
my @array = qw("aa" "bb" "cc");
unshift @array, "zz";
# @array is ("zz", "aa", "bb", "cc")

my @anotherArray = qw("xx" "yy");
push @array, @anotherArray;
# @array is ("xx", "yy", "zz", "aa", "bb", "cc")
```

#### Remove the first item

With `shift`:

```perl
my @array = qw("aa" "bb" "cc");
# shift removes and returns the first item
my $shifted = shift @array;
# $shifted is "aa", @array is ("bb", "cc")
```
