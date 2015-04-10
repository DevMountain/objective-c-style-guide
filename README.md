# DevMountain Objective-C Style Guide

This style guide outlines the coding conventions used by mentors and teachers and taught to the iOS students at DevMountain.

We welcome feedback. The guide was informed by our developers, documents from [Apple](#apple-docs), [Ash Furrow](http://ashfurrow.com/blog/structuring-modern-objective-c), [Objective Clean](www.objclean.com), [GitHub](https://github.com/github/objective-c-conventions), and the [NYTimes Style Guide](https://github.com/NYTimes/objetive-c-style-guide). There may be slight differences, but these are excellent resources.

## Table of Contents

* [Spacing](#spacing)
* [Methods](#methods)
* [Conditionals](#conditionals)
* [Ternary Operator](#ternary-operator)
* [Variables](#variables)
* [Dot Notation Syntax](#dot-notation-syntax)
* [Imports](#imports)
* [Naming](#naming)
* [Comments](#comments)
* [Init & Dealloc](#init-and-dealloc)
* [Literals](#literals)
* [CGRect Functions](#cgrect-functions)
* [Constants](#constants)
* [Enumerated Types](#enumerated-types)
* [Private and Public Properties](#private-and-public-properties)
* [Shared Instances and Singletons](#shared-instances-and-singletons)
* [Xcode Project](#xcode-project)


## Spacing

* Indents should use 4 spaces. Never indent with tabs. Be sure to set this preference in Xcode.
* Method braces and other braces (`if`/`else`/`switch`/`while` etc.) always open on the same line as the statement but close on a new line.
* `else` should be nested between the closing bracket of the `if` and the opening bracket.
* There should always be a space after statement declarations and before braces
* There should be a space around each equal sign and (`=`) and math (`+`, `-`, `*`, `/`, `>`, `<`)

**Example:**
```objc
if (user.isHappy) {
    user.heart = happy + joy;    
} else {
    user.face = sad + cry;    
}
```
* There should be exactly one blank line between methods to aid in visual clarity and organization. Whitespace within methods should separate functionality.
  * Additional blank lines may be used above pragma marks in order to break up groups of methods.
* `@synthesize` and `@dynamic` should each be declared on new lines in the implementation.

## Methods

* In method signatures, there should be a space after the scope (`-` or `+` symbol). There should be a space between the method segments.
* There should be no space between the closing parentheses of the return type and the method name.
* There should be no space after the colon in a method signature.
* There should be a space between the parameter type and * (if a pointer) and no space after the closing parentheses of the parameter type. 
* The opening brace of the method declaration should be on the same line as the method name.
* There should not be a space before parameters passed into a method
* There should be an empty space above return statements.

**Example**:
```objc
- (BOOL)setExampleText:(NSString *)text image:(UIImage *)image {

	return YES;
}
```

```objc
[self setExampleText:text];
```

## Conditionals

* Conditional bodies should always use braces even when a conditional body could be written without braces (e.g., it is one line only) to prevent [errors](https://github.com/NYTimes/objective-c-style-guide/issues/26#issuecomment-22074256). 
* This style is more consistent with all other conditionals, and therefore more easily scannable.

**Example:**
```objc
if (!error) {
    return success;
}
```

**Not:**
```objc
if (!error)
    return success;
```

or

```objc
if (!error) return success;
```

## Ternary Operator

* The ternary operator, `?` , should only be used when it increases clarity or code neatness. A single condition is usually all that should be evaluated. 
* Evaluating multiple conditions is usually more understandable as an if statement, or refactored into named variables.
* There should be a space on either side of the ternary operator.

**Example:**
```objc
result = a > b ? x : y;
```

**Not:**
```objc
result = a > b ? x = c > d ? c : d : y;
```


## Variables

* Variables should be named as descriptively as possible. 
* Single letter variable names should be avoided except as simple counter variables in `for` loops.
* Asterisks indicating pointers belong with the variable, e.g., `NSString *text` not `NSString* text` or `NSString * text`, except in the case of constants (`NSString * const DMConstantString`).
* Property definitions should be used in place of naked instance variables whenever possible. Direct instance variable access should be avoided except in initializer methods (`init`, `initWithCoder:`, etc…), `dealloc` methods and within custom setters and getters. For more information on using Accessor Methods in Initializer Methods and dealloc, see [here](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/MemoryMgmt/Articles/mmPractical.html#//apple_ref/doc/uid/TP40004447-SW6).
* There should be a space after the comma in property declaration.

**Example:**

```objc
@interface Car: NSObject

@property (strong, nonatomic) NSString *headlight;

@end
```

**Not:**

```objc
@interface Car : NSObject {
    NSString *headlight;
}
```

#### Variable Qualifiers

When it comes to the variable qualifiers [introduced with ARC](https://developer.apple.com/library/ios/releasenotes/objectivec/rn-transitioningtoarc/Introduction/Introduction.html#//apple_ref/doc/uid/TP40011226-CH1-SW4), the qualifier (`__strong`, `__weak`, `__unsafe_unretained`, `__autoreleasing`) should be placed between the asterisks and the variable name, e.g., `NSString * __weak text`. 

## Dot Notation Syntax

Dot notation should **always** be used for accessing and mutating properties. Bracket notation is preferred in all other instances.

**Example:**
```objc
view.backgroundColor = [UIColor orangeColor];
[UIApplication sharedApplication].delegate;
```

**Not:**
```objc
[view setBackgroundColor:[UIColor orangeColor]];
UIApplication.sharedApplication.delegate;
```

## Imports

* For modules use the [@import](http://clang.llvm.org/docs/Modules.html#using-modules) syntax.
* Use the @class forward class declaration rather than importing in the header file.
* Two exceptions are when subclassing a custom class (import the superclass in the header), or acting as a controller or category (import the object class in the header).
* If there is more than one import statement, group the statements [together](http://ashfurrow.com/blog/structuring-modern-objective-c). 
* Commenting groups is optional, but recommended.

```objc
// Frameworks
@import QuartzCore;

// Models
#import "DMUser.h"

// Views
#import "DMButton.h"
#import "DMUserView.h"
```

## Naming

* Apple naming conventions should be adhered to wherever possible, especially those related to [memory management rules](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/MemoryMgmt/Articles/MemoryMgmt.html) ([NARC](http://stackoverflow.com/a/2865194/340508)).
* Long, descriptive method and variable names are good.

**Example:**

```objc
UIButton *studentButton;
```

**Not**

```objc
UIButton *sBut;
```

* A prefix (e.g., `DM`) should always be used for class names and constants, however _may_ be omitted for Core Data entity names. 
* Constants should be camel-case with all words capitalized and prefixed by the related class name for clarity. 
* DevMountain ignores the Apple recommendation that a two letter prefix (e.g., `NS`) should be [reserved for use by Apple](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/ProgrammingWithObjectiveC/DefiningClasses/DefiningClasses.html#//apple_ref/doc/uid/TP40011210-CH3-SW12). 

**Example:**

```objc
static const CGFloat DMSucessRatio = 1.0;
```

**Not:**

```objc
static const GCFloat successRatio = 1.0;
```

* Properties and local variables should be camel-case with the leading word being lowercase.
* Instance variables should be camel-case with the leading word being lowercase, and should be prefixed with an underscore. This is consistent with instance variables synthesized automatically by LLVM. 
* If LLVM can synthesize the variable automatically, then let it.

**Example:**

```objc
@synthesize descriptiveVariableName = _descriptiveVariableName;
```

**Not:**

```objc
id varnm;
```

## Comments

* When they are needed, comments should be used to explain **why** a particular piece of code does something. 
* Comments that are used must be kept up-to-date or deleted.
* Block comments should generally be avoided, as code should be as self-documenting as possible, with only the need for intermittent, few-line explanations. 
	* This does not apply to those comments used to generate documentation.

## init and dealloc

* `init` method, and any `initWith` variations should be placed at the top of the implementation, directly after the `@synthesize` and `@dynamic` statements.
* `dealloc` methods should be placed directly below the `init` methods of any class.

`init` methods should be structured like this:

```objc
- (instancetype)init {
    self = [super init]; // or call the designated initializer
    if (self) {
        // Custom initialization
    }

    return self;
}
```

## Literals

* `NSString`, `NSDictionary`, `NSArray`, and `NSNumber` literals should be used whenever creating immutable instances of those objects. 
* Pay special care that `nil` values not be passed into `NSArray` and `NSDictionary` literals, as this will cause a crash.

**Example:**

```objc
NSArray *names = @[@"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul"];
NSDictionary *productManagers = @{@"iPhone" : @"Kate", @"iPad" : @"Kamal", @"Mobile Web" : @"Bill"};
NSNumber *shouldUseLiterals = @YES;
NSNumber *buildingZIPCode = @10018;
```

**Not:**

```objc
NSArray *names = [NSArray arrayWithObjects:@"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul", nil];
NSDictionary *productManagers = [NSDictionary dictionaryWithObjectsAndKeys: @"Kate", @"iPhone", @"Kamal", @"iPad", @"Bill", @"Mobile Web", nil];
NSNumber *shouldUseLiterals = [NSNumber numberWithBool:YES];
NSNumber *buildingZIPCode = [NSNumber numberWithInteger:10018];
```

## `CGRect` Functions

When accessing the `x`, `y`, `width`, or `height` of a `CGRect`, always use the [`CGGeometry` functions](http://developer.apple.com/library/ios/#documentation/graphicsimaging/reference/CGGeometry/Reference/reference.html) instead of direct struct member access. From Apple's `CGGeometry` reference:

> All functions described in this reference that take CGRect data structures as inputs implicitly standardize those rectangles before calculating their results. For this reason, your applications should avoid directly reading and writing the data stored in the CGRect data structure. Instead, use the functions described here to manipulate rectangles and to retrieve their characteristics.

**Example:**

```objc
CGRect frame = self.view.frame;

CGFloat x = CGRectGetMinX(frame);
CGFloat y = CGRectGetMinY(frame);
CGFloat width = CGRectGetWidth(frame);
CGFloat height = CGRectGetHeight(frame);
```

**Not:**

```objc
CGRect frame = self.view.frame;

CGFloat x = frame.origin.x;
CGFloat y = frame.origin.y;
CGFloat width = frame.size.width;
CGFloat height = frame.size.height;
```

## Constants

* Constants are preferred over in-line string literals or numbers, as they allow for easy reproduction of commonly used variables and can be quickly changed without the need for find and replace. 
* Constants should be declared as `static` constants and not `#define`s unless explicitly being used as a macro.
* Optionally, the prefix `k` can be used before a constant. However, the codebase should be consistent between either `k` or the project prefix (i.e. `DM`)

**Example:**

```objc
static NSString * const DMUserFirstNameKey = @"firstName";

static const CGFloat DMUserNameHeight = 50.0;
```

**Not:**

```objc
#define DMFirstNameKey @"firstName"

#define kUserNameHeight 50
```

## Enumerated Types

* When using `enum`s, use the new fixed underlying type specification, which provides stronger type checking and code completion. 
* `enum`s should be used for UITableView sections and rows where content layout is static.

**Example:**

```objc
typedef NS_ENUM(NSInteger, DMResourcesTableViewSection) {
    DMResourcesTableViewSectionIntro,
    DMResourcesTableViewSectionStretchProblem,
    DMResourcesTableViewSectionCapstone,
};
```

## Private and Public Properties

* Public properties should be readonly with private write declaration unless otherwise necessary.
* Do not expose mutable properties in your header files.
* Use proper getter declaration and syntax for BOOL properties. 

**Example:**

```objc

// In the .h file

@interface DMResource : NSObject 

@property (strong, nonatomic, readonly) NSString *teacherDescription;
@property (nonatomic, assign, getter = isRead) BOOL read;

@end


// In the .m file

@interface DMResource ()

@property (strong, nonatomic) NSString *teacherDescription;
@property (assign, nonatomic) NSInteger readCount;

@end
```

## Shared Instances and Singletons

* Shared Instance objects should use a thread-safe pattern for creating their shared instance.
	* This will prevent [possible and sometimes frequent crashes](http://cocoasamurai.blogspot.com/2011/04/singletons-your-doing-them-wrong.html).
* A strict singleton model (forced single object) should not be used.
* A shared instance may use one of two name prefixes `shared` or `default`. 
	* A `shared` instance is one that is expected to be used globally by classes throughout the project.
	* A `default` instance is one that is used in *most* cases. However, another instance may be initialized.
	
```objc
+ (instancetype)sharedInstance {
   static id sharedInstance = nil;

   static dispatch_once_t onceToken;
   dispatch_once(&onceToken, ^{
      sharedInstance = [[[self class] alloc] init];
   });

   return sharedInstance;
}
```

## Xcode project

* The Xcode project files should be used exclusively for organization of the project library files for a given framework or project.
* Files should only be removed from the main project folder in the filesystem in the following cases:
	* A separate framework (i.e. shared data extension, WatchKit or Notification extension)
	* An external code base (i.e. imported libraries or frameworks)
	* Files for tests (i.e. project tests and extension tests should each have their own unique folders)
* Xcode groups created should *not* be reflected by folders in the filesystem. This is to avoid potential git conflicts on teams. 
* Code should be grouped by feature first, and then by by type.
* When possible, always turn on “Treat Warnings as Errors” in the target’s Build Settings and enable as many [additional warnings](http://boredzo.org/blog/archives/2009-11-07/warnings) as possible. 
* If you need to ignore a specific warning, use [Clang’s pragma feature](http://clang.llvm.org/docs/UsersManual.html#controlling-diagnostics-via-pragmas).

## Apple Docs

Apple has a number of valuable resources that informed our decisions. Also, if there is something not mentioned in this readme, it's probably in one of these:

* [The Objective-C Programming Language](http://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/ObjectiveC/Introduction/introObjectiveC.html)
* [Cocoa Fundamentals Guide](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CocoaFundamentals/Introduction/Introduction.html)
* [Coding Guidelines for Cocoa](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/CodingGuidelines.html)
* [iOS App Programming Guide](http://developer.apple.com/library/ios/#documentation/iphone/conceptual/iphoneosprogrammingguide/Introduction/Introduction.html)
