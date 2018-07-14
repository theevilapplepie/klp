# Kinda Like Perl

## Preface
Kinda Like Perl ( or KLP -- which I prefer as the name is dumb ) is my sad and amuzing attempt to get the "experience" of perl without some of the pain, plus I'll throw in some features I want because I'm writing it. #yolo

Please understand this is a hobby and it's my free time that I'm spending on this.
Constructive criticisim and dialog is always welcomed but I'm doing this mostly for myself at this point.

That being said, if for some reason this is being used by some demented individual, give me a shout! I'd love to know.

## Goals / Points

1. Shoot yourself in the foot -- The best thing about perl is that it tries to be "as simple as possible" from a coding perspective internally. It doesn't put a lot of bounds on "features" or "functions" within core at times and that weirdly loose coupling of interaction makes an INTERPRETED LANGUAGE give me some part of the feeling I get writing "C" ( minus all the pain ) and it's the most important thing I want to maintain. It's a sense of "pieces making a whole" but of your design, and not that I'm riding on someone else's code. If I don't end up getting that same feeling using KLP then I've failed. 
1. Simpler interaction with variables -- Not saying that it's _difficult_ but I notice myself having to "think" about variables _way the hell more often than I should_.. Why is that? It's already "typed".. This needs to get the hell out of my thought-path when I'm doing something.
1. Struct like concept -- Why isn't this an existing feature? Perl's usage in my experience is mostly data processing and system scripting ( and it's damn good at it ), sooooo why can't we define data models?
1. Consistency -- Just make a rule and stick to it, if the rule doesn't hold true for the desired scope then modify it and refactor for fuck sake.
1. Compatibility -- Perl is a total bitch to use in situations where you can't "control" the environment, by "control" I mean pick the version or add modules ( by that I mean XS/Compiled as you could always bundle PP, but uh... PP? Performance hit much? ). I expect this language to support "real" to machine code compilation, even if I don't support it at first. ( plus that means modules inherently can have both a "purely interpreted" and "compiled" option without even needing to change code for a speed boost ). Also, I'll probably add some crazy option where you can include C as a native thing. Maybe I'll even make it work for interpreted runs too... "He's crazy enough, He'll do it!"

## Documentation

### Syntax
WELL IT'S BASICALLY THE SAME AS PERL
( I'll put stuff in here later )

#### Operators
##### Math
##### String / Binary
##### Comparison

### Variables
Variables are used to store and manage runtime data within the language.

#### Naming

#### Data Types
A Data Type defines structure or classification for a given variable. 

There are 5 Types:

* Number - Integer of Float ( Prefix: + )
* String - Text or Binary Data ( Prefix: ^ | Creator: "" or '' )
* Array - Ordered List ( Prefix: @ | Creator: [] )
* Hash - Key/Value Store ( Prefix: % | Creator: {} )
* Reference - Alias of another variable ( Prefix: $ )

**Regarding References:**

You should always use the Reference data type when interacting with data in your code, including the use of "Creators" with the Reference type to define a variable. 

There _is_ one edge case where you _could_ self optimize by using data types non-reference data types directly. It is only valid when the variable is created within a limited scope and is not passed outside of that scope (including to subroutines/functions).
This is the _**only**_ time using data types directly is desirable as it would _slightly_ reduce overhead, potentially giving a reduction in time for internal script compilation or processing time during execution.
In **ALL** other cases References **WILL** perform and optimize better.
_I will tell you now if you think you have "hit this edge case", you haven't. Just use references. It will be better. Let the optimizations of the language do their work. They know better than you.

##### String

**Type Name** String
**Description:** Array of Characters or Bytes
**Symbol:** ^
**Creator:** "" | ''
**Internals:** Linked List of Values

###### Code Examples
**Direct Definition**
CODE
`
my ^var = Woohoo";
printn ^var;`
OUTPUT
`Woohoo`

**Indirect Definition**
CODE
`my $varone = "Woohoo";
my $vartwo = 'Woohoo';
my $varthree = $varone . $vartwo;
printn $varthree;`

OUTPUT
`WoohooWoohoo`

**Array Interaction**
Single Element
CODE
`my $varone = "Woohoo";
printn $varone[0];
`
OUTPUT
`W`

Element Range
CODE
`my $varone = "Woohoo";
printn $varone[0..2];`

OUTPUT
`Woo`

##### Array
**Type Name:** Array
**Description:** Numerically indexed List
**Symbol:** @
**Creator:** []
**Internals:** Linked List of References

###### Code Examples
**Direct Definition**

~~~~
OLD BROKEN SCRATCHPAD HERE FOR ME TO SORT OUT, BUT I WANT THIS MD IN GIT SO IT DOESN'T DISAPPEAR
== Hash
  Description: Key/Value Pair where Key is Scalar and Value is Reference
  Symbol: %
  Value Type =  HashMap

= Variable Modifiers (Prefix Symbols)
These are used on the right hand side of an assignment or during reads

Without Prefix = Value Type
\ = Set Value to Value
& = Set Value to Reference


= References


== Code Examples

=== Initialization

==== Scalar
===== Direct Creation
===== A
==== 
my $var = "wow this is fun";

==== Reference Assignment
my $var = "I love klp!";
my $newvar = $var;
$newvar = "It does cool stuff!";
printn $var;

OUTPUT:
 It does cool stuff!

==== Copying Value
my $var = "I love klp!";
my $newvar = \$var;
$newvar = "Wow!";

printn "VAR: $var";
printn "NEWVAR: $newvar";

OUTPUT:
 VAR: I love klp!
 NEWVAR: Wow!

==== 
~~~~ 
