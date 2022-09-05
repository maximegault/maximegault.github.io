---
title: Perl arguments
date: 2022-09-02 09:00:00 HH:MM:SS +0200
categories: [Development, Perl]
tags: [development, perl, arguments]
---

### Arguments

Easiest way to treat arguments in Perl is with the [Getopt::Long](https://metacpan.org/pod/Getopt::Long) native module.

It works like this:

* Declaration of the target variable(s) that will contain the argument(s) value(s), ie.:

```perl
my ($targetVariable) = ();
# Or
my ($targetVariable1, $targetVariable2, @targetVariable3) = ();
```

* Argument declaration in the call to the `GetOptions` method, with the format `'argumentName[|argumentAlias]=argumentType' => \$targetVariable`
* Arguments types are:
  * `i` for an integer
  * `s` for a string
  * nothing for a flag
* Arguments can be used with `-` or `--`, ie. `-argumentName` or `--argumentName`

```perl
# First import the module
use Getopt::Long; 

# Then declare the variables
my ($targetVariable1, $targetVariable2, @targetVariable3, $doDebug) = ();
GetOptions(
    'fooArg|f=i'    => \$targetVariable1,
    'barArg=s'      => \$targetVariable2,
    'bazArg|b=s'    => \@targetVariable3,
    'debug'         => \$doDebug
);

# Check if an argument has been given
print "fooArg is mandatory!" if (!defined $targetVariable1);
# Flag usage if not given
$doDebug = 0 if (!defined $doDebug);
```

Example of a call to this script:

```bash
./myScript.pl --fooArg 10 -barArg TOTO --bazArg BAZ_1 --b BAZ_2 -bazArg BAZ_3 --debug
```
