---
title: Perl loops
date: 2022-08-29 09:00:00 HH:MM:SS +0200
categories: [Development, Perl]
tags: [development, perl, loops]
---

### Loops with a defined index

Old school way using a defined index:

```perl
my @array = (1..100);
my $lastIndex = $#array; # or (scalar @array) - 1
for (my $i = 0; $i <= $lastIndex; $i++){
    print("Current value is $array[$i] \n");
}
```

#### Loops without a defined index

Easier way with a defined value:

```perl
my @array = (1..100);
for my $currentValue (@array){
    print("Current value is $currentValue\n");
}
```

With non defined value (automatic value is `$_`):

```perl
my @array = (1..100);

for (@array){
    print("Current value is $_\n");
}
```

#### For each loop

Same as `for`, with or without a defined value:

```perl
my @array = (1..100);

foreach (@array){
    print("Current value is $_\n");
}

# Or
foreach my $currentValue (@array){
    print("Current value is $currentValue\n");
}
```
