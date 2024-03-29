---
title: Perl strings
date: 2022-08-29 09:00:00 HH:MM:SS +0200
categories: [Development, Perl]
tags: [development, perl, strings]
---

### Substring

```text
substr(EXPR,OFFSET,LENGTH,REPLACEMENT)
```

With:

* `EXPR`: the string to substring
* `OFFSET`: the index to start at. If `OFFSET` is negative, starts that far back from the end of the string
* `LENGTH`: the length to substring (optional). If `LENGTH` is negative, leaves that many characters off the end of the string
* `REPLACEMENT`: replacement string (optional)

Classic way:

```perl
$string = "ABCDEFG";
substr($string, 3) # Starts at index 3 to the end, so "DEFG"
substr($string, -2) # Negative index here means "start 2 characters back from the end", so "FG"
substr($string, 3, 2) # Starts at index 3, then takes 2 characters, so "DE"
substr($string, 3, -1) # Starts at index 3, then to "1 character back from the end", so "DEF"
```

[CPAN source](https://metacpan.org/pod/perlfunc#substr-EXPR,OFFSET,LENGTH,REPLACEMENT)

### Index/rindex

`index` returns the first occurence's index of a string in another one, -1 if it does not exist. `rindex` does the same except it returns the last index.

```text
index(STR,SUBSTR,POSITION)
rindex(STR,SUBSTR,POSITION)
```

With:

* `STR`: the string to search in
* `SUBSTR`: the string to look after in the other one
* `POSITION`: the position to start the search at. If `POSITION` is omitted, starts searching from the beginning of the string

```perl
my $string = "ABCABCABC";
index($string, "A") # 0
index($string, "X") # -1
index($string, "A", 2) # 3
rindex($string, "A") # 6
```

Sources:

* [CPAN index](https://metacpan.org/pod/perlfunc#index-STR,SUBSTR,POSITION)
* [CPAN rindex](https://metacpan.org/pod/perlfunc#rindex-STR,SUBSTR,POSITION)

### Split

```perl
my $string = "a,b,c,d";
@myArray = split /,/, $string;
# myArray contains ("a", "b", "c", "d")
```

Source: [split](https://perldoc.perl.org/functions/split)

### Join

```perl
my $separator = ',';
my @array = ("d", "e", "f");
my $joined1 = join($separator, "a", "b", "c");
my $joined2 = join($separator, @array);
```

Source: [join](https://perldoc.perl.org/functions/join)
