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

# Documentation
## Syntax
WELL IT'S BASICALLY THE SAME AS PERL
( I'll put stuff in here later )

## Operators
#### Math
#### String / Binary
#### Comparison

## Variables
Variables are used to store and manage runtime data within the language.

### Data Types
A Data Type defines structure or classification for a given variable. 

There are 5 Types:

* Number - Integer of Float ( Prefix: + )
* String - Text or Binary Data ( Prefix: ^ | Creator: "" or '' )
* Array - Ordered List ( Prefix: @ | Creator: [] )
* Hash - Key/Value Store ( Prefix: % | Creator: {} )
* Reference - Alias to defined data ( Prefix: $ ) -- This is covered in it's own section

Regarding References:

>You should always use the Reference data type when interacting with data in your code, by using the "Creators" for other data types with the Reference type to define a variable. 
>
>There _is_ one edge case where you _could_ self optimize by using non-reference data types directly, given that:
>>The variable is created within a limited scope and is not passed outside of that scope (including to subroutines/functions).  
>  
>This is the _**only**_ time using non-reference data types directly _could_ be desirable as it _**could**_ potentially reduce time taken for internal script compilation and/or reduce Time and CPU for the runtime code.
>  
>  In _every other case_, References will _perform and optimize better_.  
>  I will tell you now, if you think you have hit this edge case, _**Think again.**_
>  
>  Just use references. Let the language optimizations do their thing and make the best choice, they know what they're doing for the language better than you.

#### String

**Type Name:** String  
**Description:** Array of Characters or Bytes  
**Symbol:** ^  
**Creator:** "" | ''  
**Internals:** Linked List of Values  

##### Code Examples
###### Direct Definition  
>CODE  
```
my ^var = Woohoo";
printn ^var;
```
>OUTPUT  
```
Woohoo
```

###### Indirect Definition
>CODE
```
my $varone = "Woohoo";
my $vartwo = 'Woohoo';
my $varthree = $varone . $vartwo;
printn $varthree;
```
>OUTPUT
```
WoohooWoohoo
```

###### Retrieval
Whole String  
>CODE  
```
my $varone = "Woohoo";
printn $varone;
```
>OUTPUT
```
Woohoo
```

Single Character  
>CODE  
```
my $varone = "Woohoo";
printn $varone[0];
```
>OUTPUT
```
W
```

Character Range  
>CODE  
```
my $varone = "Woohoo";
printn $varone[0..2];
```
>OUTPUT
```
Woo
```

#### Array
**Type Name:** Array  
**Description:** Numerically indexed List  
**Symbol:** @  
**Creator:** []  
**Internals:** Linked List of References  

##### Code Examples
###### Direct Definition
>CODE  
```
my @var = qw(woo hoo);
printn @var[0]
```
>OUTPUT  
```
woo
```

###### Indirect Definition
>CODE  
```
my $var = ['woo',"hoo"];
printn $var[0];
```
>OUTPUT  
```
woo
```

###### Retrieval
Single Value  
>CODE  
```
my $var = ['woo',"hoo"];
printn $var[0];
```
>OUTPUT  
```
woo
```

Length  
>CODE  
```
my $var = ['woo',"hoo"];
printn len($varone);
```
>OUTPUT  
```
2
2
```

Value Range (w/join to string)  
>CODE  
```
my $var = ['woo',"hoo"];
print $varone[0..1,"\n"];
print $varone[0..,"\n"];
```
>OUTPUT  
```
woo
hoo
woo
hoo
```

#### Hash
**Type Name:** Hash  
**Description:** String Indexed List  
**Symbol:** %  
**Creator:** {}  
**Internals:** Linked List with References  


##### Code Examples
###### Direct Definition
>CODE  
```
my %var = ( 'woo' => "hoo" );
printn %var{'woo'};
```
>OUTPUT  
```
hoo
```

###### Indirect Definition
>CODE  
```
my $var = { "woo" => 'hoo' };
printn $var{'woo'};
```
>OUTPUT  
```
hoo
```

###### Retrieval
Value from Key  
>CODE  
```
my $var = { "woo" => 'hoo' };
printn $var{'woo'};
```
>OUTPUT  
```
hoo
```

Key Listing  
>CODE  
```
my $var = { "woo" => 'hoo' };
my $keys = keys($var); # This is an array w/ Keys now :)
print $keys[..,"\n"];
```
>OUTPUT  
```
woo
```

### References
References are defined in this doc as "aliases" to defined data, however the scope and utilziation of these is far more complciated.

#### Why do I need to use references?
Within this language nothing is passed by value, everything is passed as a reference handle.

That means you cannot pass a variable or have a defined variable leave your scope ( in many cases including to builtin methods ) without it being in a reference.

The reason for this is so "by default" we are not copying around data, we are sending pointers.

#### What are references?
References are handles which have a given name and no "real" value, they're specialized pointers.

#### Reference Modifiers
There are several prefixes you can use which will modify the behaviour of a reference

"\\" - Tells assignments to set the "value" of the assignment to the "value" of the reference.  
      ( See: Usage Examples, Getting the Value from a Reference )  
      In a non-assignment context, such as passing to a method, it will create a "new" anonymous variable with a matching data type, then pass that reference forward. ( Try to avoid )

"&" - Tells assignments to set the "value" of the assignment to the reference,
      rather than assigning the reference on the variable. ( This should rarely be used )  
      In a non-assignment context, such as passing to a method, it will unset the "read only" flag on the reference and allow the destination method to perform modifications.

#### Usage Examples
##### Passing References to Methods

There is a huge language oddity you should be aware of when passing data to methods, the passed reference becomes 'read only' from the scope of the target method you're calling.

This avoids "accidental data overwrite" or otherwise unobvious stuff, but it's very simple to mark as read-write when you need to.

To mark as read-write use "&", which is the "set the value to the reference" prefix.

###### Read-Only Error Example
>CODE  
```
sub waffles($syrup =~ /\S+/) {
	printn $syrup;
	$syrup = "OMG";
	printn $syrup;
}
my $var = "omg";
waffles($var);
```
>OUTPUT  
```
omg
[!break:err@(anonymous:3) "write attempted on read-only reference" ]> line
  $syrup = "OMG";
```

###### Writable Reference Example
>CODE  
```
sub waffles($syrup =~ /\S+/) {
	printn $syrup;
	$syrup = "OMG";
	printn $syrup;
}
my $var = "omg";
waffles(&$var);
```
>OUTPUT  
```
omg
OMG
```

##### Getting the Value from a Reference
When dealing with references, especially during assignment of other variables, you may want to make a "copy" of the value rather than create a new reference to the same data.

This is simple to do, we use "\\" prefix, which is the "set the value to the value of the reference" modifier.

>CODE  
```
my $sheep = "baaaa";
my $clone = \$sheep;
$clone = "woof";
printn "The Sheep Says: $sheep";
printn "The Bad Sheep Says: $clone";
```
>OUTPUT  
```
The Sheep Says: baaaa
The Bad Sheep Says: woof
```
