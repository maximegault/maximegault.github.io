---
title: Perl references
date: 2022-08-30 09:00:00 HH:MM:SS +0200
categories: [Development, Perl]
tags: [development, perl, references]
---

### References

A reference is a scalar that hosts the location of another value (that can be another scalar, an hash or an array).

> References are the best way to pass an array or a hash to a `sub`.
{: .prompt-info }

#### Create a reference

To create a reference, just add a `'\'` before an existing item:

```perl
my $scalar;
my @array;
my %hash;
my $referenceScalar = \$scalar;
my $referenceArray = \@array;
my $referenceHash = \%hash;

# Named sub
sub MySub { return "Foo"; }
my $referenceSub = \&MySub;
# "Anonymous" sub reference
my $referenceAnonymousSub = { return "Bar"; };
```

#### Dereference

Just add the type's prefix before the reference, ie. `$`, `@`, `%` or `&`:

```perl
my $scalar;
my @array;
my %hash;
my $referenceScalar = \$scalar;
my $referenceArray = \@array;
my $referenceHash = \%hash;
my $referenceAnonymousSub = sub { return "Bar" };
my $dereferencedScalar = $$referenceScalar;
my @dereferencedArray = @$referenceArray;
my %dereferencedHash = %$referenceHash;
# Call this sub
&$referenceAnonymousSub();
```

#### Know the reference's type

With the keyword `ref`. Ie. with previous example:

```perl
print ref($referenceScalar), "\n";
print ref($referenceArray), "\n";
print ref($referenceHash), "\n";
print ref($referenceAnonymousSub), "\n";
```

Output is:

```text
SCALAR
ARRAY
HASH
CODE
```
