#GC Objective-C Style Guide

This documents the coding convention for GC iOS team.

**Table of Contents**  *generated with [DocToc](http://doctoc.herokuapp.com/)*

- [Whitespace](#whitespace)
- [Organization](#organization)
- [Comments](#comments)
- [Literals](#literals)
- [Declarations](#declarations)
- [Private Properties](#private-properties)
- [Expressions](#expressions)
- [Control Structures](##control-structures)
    - [if-else structures](#if-else-structures)
    - [Case Statements](#case-statements)
    - [Ternary operator](#ternary-operator)
- [Exceptions and Error Handling](#exceptions-and-error-handling)
- [Blocks](#blocks)
- [Singletons](#singletons)
- [Code Naming](#code-naming)
    - [Good](#good)
    - [Bad](#bad)
- [Method Signatures](#method-signatures)
    - [Use lower camel case](#use-lower-camel-case)
    - [Start with action](#start-with-action)
    - [Getters](#getters)
    - [Return Type Spacing](#return-type-spacing)
    - [Parameter Keywords](#parameter-keywords)
    - [Be Descriptive](#be-descriptive)
    - [Avoid And (With Exceptions)](#avoid-and-with-exceptions)
    - [Signature Spacing](#signature-spacing)
    - [Init Methods](#init-methods)
    - [Dealloc Methods](#-dealloc-methods)
    - [Init and dealloc](#init-and-dealloc)
 [Classes](#classes)
- [Namespace prefixes](#namespace-prefixes)
- [Booleans](#booleans)
- [Properties](#properties)
    - [Pointer Spacing](#pointer-spacing)
    - [@synthesize and @dynamic](#@synthesize-and-@dynamic)
    - [@property](#@property)
- [Enums](#enums)
- [Categories](#categories)
- [xib Files](#xib-files)
- [Image naming](#image-naming)
- [Golden Path](#golden-path)
- [More on Brackets](#more-on-brackets)
- [Boyscout / Girl Guide](#boyscout--girl-guide)
- [Copyrights](#copyrights)
- [Proper Code Nutrition](#proper-code-nutrition)
    - [Integers](#integers)
    - [Floats](#floats)
    - [Strings](#strings)
    - [Basic Code Principles](#basic-code-principles)
- [Testing](#testing)
- [Inspiration](#inspiration)

Whitespace
==========

* Indent using 4 spaces. Never indent with tabs. Be sure to set this preference in Xcode (Xcode defaults to 4 spaces).

* Separate imports from the rest of your file by 1 space. Optionally group imports if there are many (but try to have less dependencies). Generally strive to include frameworks first.

``` obj-c
#import <AwesomeFramework/AwesomeFramework.h>
#import <AnotherFramework/AnotherFramework.h>

#import "SomeDependency.h"
#import "SomeOtherDependency.h"

@interface MyClass
```

* Use one empty line between class extension and implementation in .m file.

``` obj-c
@interface MyClass ()

// Properties - empty line above and below

@end

@implementation MyClass

// Body - empty line above and below

@end

```

* When using pragma marks leave 1 newline before and after.

``` obj-c
- (CGSize)intrinsicContentSize {
    return CGSizeMake(12, 12);
}

#pragma mark - Private

- (void)setup {
    [self addGestureRecognizer:[[NSClickGestureRecognizer alloc] initWithTarget:self action:@selector(clicked:)]];
}
```

* When doing math use a single space between operators. Unless that operator is unary in which case don't use a space.

```obj-c
NSInteger index = rand() % 50 + 25; // arc4random_uniform(50) should be used insted of `rand() % 50`, but doesn't illustrate the principle well
index++;
index += 1;
index--;
```

* When doing logic, a single space should follow the `if` and a single space should preceed the `{`

``` obj-c
if (alpha + beta <= 0) && (kappa + phi > 0) {
}
```

* Colon-aligning method invocation should often be avoided.  There are cases where a method signature may have >= 3 colons and colon-aligning makes the code more readable. Please do **NOT** however colon align methods containing blocks because Xcode's indenting makes it illegible.

Good:

```objc
// blocks are easily readable
[UIView animateWithDuration:1.0 animations:^{
    // something
} completion:^(BOOL finished) {
    // something
}];
```

Bad:

```objc
// colon-aligning makes the block indentation wacky and hard to read
[UIView animateWithDuration:1.0
                 animations:^{
                     // something
                 }
                 completion:^(BOOL finished) {
                     // something
                 }];
```

* Whitespace should in *all* cases be used to aid readability. Readability is highly subjective, so here are some rough guides:
  * Use new lines to delimit chunks of related code (approx 4-5 lines). If more than 4-5 lines are grouped, consider refactoring those lines into another method. 
    * By grouping related lines of code it naturally starts to show where the method can be refactored into smaller reusable units
  * One blank line is generally sufficient.
  * Avoid extraneous new lines between nested sets of parenthesis.
  * Avoid blank lines at the end of methods. (Consider delimiting the final return value with one though.)

Good:
  
```objc
- (void)awakeFromNib {
    UIStoryboard *signatureStoryboard = [UIStoryboard storyboardWithName:@"BBPopoverSignature" bundle:nil];
    self.signatureViewController = [signatureStoryboard instantiateViewControllerWithIdentifier:@"BBPopoverSignature"];
    self.signatureViewController.modalPresentationStyle = UIModalPresentationPopover;
    self.signatureViewController.preferredContentSize = CGSizeMake(BBPopoverSignatureWidth, BBPopoverSignatureHeight);
    self.signatureViewController.signatureImageView = self;
    
    UITapGestureRecognizer *tapRecognizer = [[UITapGestureRecognizer alloc] initWithTarget:self action:@selector(initiateSignatureCapture)];
    [self addGestureRecognizer:tapRecognizer];
}
```
Note: 

1. all the `signatureViewController`-related lines are together
2. the new line delimits the end of configuration of `signatureViewController`
3. the `tapRecognizer` instantiation and configuration is grouped, and not mixed with unrelated code
4. a new line after the opening `{` and a new line before the closing `}` are permissible. In some cases they aid readability and in others they yield an overabundance of whitespace.

Good:
```objc
- (NSAttributedString *)aboutTermsAttributedString {

    NSDictionary *attributes = nil;
    NSError *error = nil;

    NSURL *fileURL = [NSURL fileURLWithPath:[[NSBundle mainBundle] pathForResource:@"terms_of_use" ofType:@"html"]];
    NSAttributedString *attributedString = [[NSAttributedString alloc] initWithFileURL:fileURL 
                                                                               options:@{NSDocumentTypeDocumentAttribute: NSHTMLTextDocumentType, NSCharacterEncodingDocumentAttribute: @(NSUTF8StringEncoding)}
                                                                    documentAttributes:&attributes
                                                                                 error:&error];
    if (error) {
        NSLog(@"Error: unable to load about mobile string. %@", [error localizedDescription]);
        return nil;
    }

    return attributedString;
}
```
Note:

1. blank line after the opening `{` of the method helps give the local variables their own context
2. complexity of `attributedString` initialization is more readable with colon aligning
3. final return value is immediately clear thanks to the blank line above it

Good:
```objc

@interface BBProofOfLossViewController () <UITableViewDataSource, UITableViewDelegate, UITextFieldDelegate, BBAutocompletePopoverDateTextFieldDelegate, BBPopoverSignatureImageViewDelegate>

@property (strong, nonatomic) NSArray *targetItems;
@property (strong, nonatomic) BBCustomForm *customForm;

@property (weak, nonatomic) IBOutlet UILabel *nameLabel;
@property (weak, nonatomic) IBOutlet UILabel *addressLabel;
@property (weak, nonatomic) IBOutlet UILabel *fooLabel;
@property (weak, nonatomic) IBOutlet UILabel *barLabel;
@property (weak, nonatomic) IBOutlet UILabel *instanceNumberLabel;
@property (weak, nonatomic) IBOutlet UILabel *relatedNumberLabel;
@property (weak, nonatomic) IBOutlet UILabel *bazLabel;
@property (weak, nonatomic) IBOutlet UILabel *typeOfInstanceLabel;
@property (weak, nonatomic) IBOutlet UILabel *dateLabel;

@property (weak, nonatomic) IBOutlet NSLayoutConstraint *itemListHeightConstraint;

@property (weak, nonatomic) IBOutlet BBAutocompletePopoverDateTextField *formDatePopoverTextField;

@property (weak, nonatomic) IBOutlet BBPopoverSignatureImageView *witnessSignatureImageView;
@property (weak, nonatomic) IBOutlet UITextField *witnessSignatureBackgroundTextField;
@property (weak, nonatomic) IBOutlet UILabel *witnessNameLabel;
@property (weak, nonatomic) IBOutlet UILabel *witnessLocationLabel;
@property (weak, nonatomic) IBOutlet UILabel *witnessDateLabel;

@property (weak, nonatomic) IBOutlet BBPopoverSignatureImageView *submitterSignatureImageView;
@property (weak, nonatomic) IBOutlet UITextField *submitterSignatureBackgroundTextField;
@property (weak, nonatomic) IBOutlet UILabel *submitterNameLabel;
@property (weak, nonatomic) IBOutlet UILabel *submitterLocationLabel;
@property (weak, nonatomic) IBOutlet UILabel *submitterDateLabel;

@property (weak, nonatomic) IBOutlet BBPopoverSignatureImageView *authorizerSignatureImageView;
@property (weak, nonatomic) IBOutlet UITextField *authorizerSignatureBackgroundTextField;
@property (weak, nonatomic) IBOutlet UILabel *authorizerNameLabel;
@property (weak, nonatomic) IBOutlet UILabel *authorizerLocationLabel;
@property (weak, nonatomic) IBOutlet UILabel *authorizerDateLabel;

@end

```
Note:

1. new line after the `@interface` and before the `@end`
2. properties that are *not* `IBOutlet`s are grouped
3. `IBOutlet` properties are grouped by context
4. the patterns in the grouping aid readability by allowing the eye to see inconsistencies (there are none in this case)

Bad:
```objc

- (void)viewController:(BBViewController *)viewController
        finishedWithAuth:(BBAuthentication *)auth
        error:(NSError *)error {

    //pushViewController didn't work, use ugly full screen view
    [viewController dismissViewControllerAnimated:YES completion:nil];

    if (error != nil) {

        NSLog(@"Authentication failed %@", [error description]);

    } else {

        NSString *apiURL = @"https://api.example.com/v1/quux/~?format=json";

        NSURL *url = [NSURL URLWithString:apiURL];
        NSMutableURLRequest *request = [NSMutableURLRequest requestWithURL:url];

        [auth authorizeRequest:request completionHandler:^(NSError *error) {

            AFHTTPRequestOperation *operation = [[AFHTTPRequestOperation alloc] initWithRequest:request];
            operation.responseSerializer = [AFJSONResponseSerializer serializer];
            [operation setCompletionBlockWithSuccess:^(AFHTTPRequestOperation *operation, id responseObject) {

                NSDictionary *responseData = (NSDictionary *)responseObject;
                RBKQuickAlert(@"Response description", responseData.description);

            } failure:^(AFHTTPRequestOperation *operation, NSError *error) {

                RBKQuickAlert(@"Response error", error.description);

            }];

            [operation start];

        }];

    }

}
```
Note: 

1. method colons aren't aligned. (Method signature is not well suited to colon aligning)
2. Happy path is nested. (Early return is missing)
3. Lots of extra blank lines that break up readability. (Whitespace is failing to define context)
4. URL string is hardcoded (not environment-aware)
5. `NSMutableURLRequest` instantiation is spread over several lines

Bad rewritten better:

```objc

// DEV_BUILD/STAGE_BUILD/PROD_BUILD are configured in Project Build Settings Preprocessor Macros
#if DEV_BUILD
static NSString * const BBAPIBaseURLString = @"https://dev.example.com/v1/quux/~?format=json"; // dev

#elif QA_BUILD 
static NSString * const BBAPIBaseURLString = @"https://qa.example.com/v1/quux/~?format=json"; // qa

#elif STAGE_BUILD
static NSString * const BBAPIBaseURLString = @"https://stage.example.com/v1/quux/~?format=json"; // stage

#elif PROD_BUILD
static NSString * const BBAPIBaseURLString = @"https://api.example.com/v1/quux/~?format=json"; // prod

#else
static NSString * const BBAPIBaseURLString = @"https://api.example.com/v1/quux/~?format=json"; // prod

#endif

- (void)viewController:(BBViewController *)viewController finishedWithAuth:(BBAuthentication *)auth error:(NSError *)error {

    //pushViewController didn't work, use ugly full screen view
    [viewController dismissViewControllerAnimated:YES completion:nil];

    if (error) {
        NSLog(@"Authentication failed %@", [error localizedDescription]);

        // TODO: call our own completion block with this error?

	     return;
    }
    
    NSMutableURLRequest *request = [NSMutableURLRequest requestWithURL:[NSURL URLWithString:BBAPIBaseURLString]]; // mutable in case we need to adjust the request headers
    [auth authorizeRequest:request completionHandler:^(NSError *error) {
        
        // we should probably be checking the supplied error to decided if we need to do this operation or not
        
        AFHTTPRequestOperation *operation = [[AFHTTPRequestOperation alloc] initWithRequest:request];
        operation.responseSerializer = [AFJSONResponseSerializer serializer];
        [operation setCompletionBlockWithSuccess:^(AFHTTPRequestOperation *operation, id responseObject) {
            NSDictionary *responseData = (NSDictionary *)responseObject;
            
            // TODO: Handle our response data. Call our own completion block?
            
        } failure:^(AFHTTPRequestOperation *operation, NSError *error) {
            NSLog(@"Error: %@", error.localizedDescription);
            
            // TODO: call our own completion block when we have an error?
        }];
        
        [operation start];
    }];
}
```
Note: This method is obviously incomplete and may not, *architecturally* be optimal, however it can be still styled in a readable manner.

Organization
============

* use `#pragma mark -`s to categorize methods in functional groupings and protocol/delegate implementations following this general structure.

```objc
#pragma mark - Lifecycle

+ (instancetype)objectWithThing:(id)thing {}
- (instancetype)init {}
- (void)dealloc {}
- (void)viewDidLoad {}
- (void)didReceiveMemoryWarning {}

#pragma mark - Custom Accessors

- (void)setCustomProperty:(id)property {}
- (id)anotherCustomProperty {}

#pragma mark - Actions

- (IBAction)submitData:(id)sender {}

#pragma mark - Public

- (void)publicMethod {}

#pragma mark - Private

- (void)privateMethod {}

#pragma mark - Protocol conformance
#pragma mark - UITextFieldDelegate
#pragma mark - UITableViewDataSource
#pragma mark - UITableViewDelegate

#pragma mark - NSCopying

- (id)copyWithZone:(NSZone *)zone {}

#pragma mark - NSObject

- (NSString *)description {}
```

Comments
========

When they are needed, comments should be used to explain *why* a particular piece of code does something. Any comments that are used must be kept up-to-date or deleted.

Block comments should generally be avoided, as code should be as self-documenting as possible, with only the need for intermittent, few-line explanations. *Exception: comments used to generate documentation.*

Literals
========

`NSString`, `NSDictionary`, `NSArray`, and `NSNumber` literals should be used whenever creating immutable instances of those objects. Pay special care that nil values not be passed into `NSArray` and `NSDictionary` literals, as this will cause a crash.

Good:

```objc
NSArray *names = @[@"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul"];
NSDictionary *productManagers = @{@"iPhone" : @"Kate", @"iPad" : @"Kamal", @"Mobile Web" : @"Bill"};
NSNumber *shouldUseLiterals = @YES;
NSNumber *buildingZIPCode = @10018;
```

Bad:

```objc
NSArray *names = [NSArray arrayWithObjects:@"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul", nil];
NSDictionary *productManagers = [NSDictionary dictionaryWithObjectsAndKeys: @"Kate", @"iPhone", @"Kamal", @"iPad", @"Bill", @"Mobile Web", nil];
NSNumber *shouldUseLiterals = [NSNumber numberWithBool:YES];
NSNumber *buildingZIPCode = [NSNumber numberWithInteger:10018];
```

Declarations
============

* Always declare memory-management semantics even on `readonly` properties.  List the management semantics first for consistency (and to match XCode default).

Good:

```objc
@property (assign, nonatomic) NSInteger statusCode;
```
    
Bad:

```objc
@property (nonatomic) NSInteger statusCode;
```
    
* Don't use @synthesize unless the compiler requires it. Note that optional properties in protocols must be explicitly synthesized in order to exist.
* Instance variables should be prefixed with an underscore (just like when implicitly synthesized).

Good:

```objc
@synthesize statusCode = _statusCode;
```

Bad:

```objc
@synthesize statusCode;
```    
    
* Don't put a space between an object type and the protocol it conforms to.

Good: 
    
```objc
@property (weak, nonatomic) id<SGOAnalyticsDelegate> analyticsDelegate;
```

Bad:

```objc
@property (nonatomic) id <SGOAnalyticsDelegate> analyticsDelegate;
```

Private Properties
==================

Private properties should be declared in class extensions (anonymous categories) in the implementation file of a class. Named categories, used when extending an existing class (e.g. `NSString`), should not be confused with class extensions.

For example:

```objc
@interface VPDetailViewController ()

@property (strong, nonatomic) GADBannerView *googleAdView;
@property (strong, nonatomic) ADBannerView *iAdView;
@property (strong, nonatomic) UIWebView *adXWebView;

@end
```

Expressions
===========

* Don't access an ivar unless you're in `-init`, `-dealloc` or a custom accessor.
* Only use dot syntax to access properties, not to call methods

Good: 

```objc
view.frame
```

Bad: 

```objc
[view frame]
```
    
Good:

```objc
self.view.frame = CGRectMake(x, y, width, height);
[self.view removeFromSuperview];

Person *selectedPerson = [self getPersonWithId:idNumber];
self.nameLabel.text = selectedPerson.name;

- (void)setProperty:(id)newValue {
    _property = newValue;
    [self someOtherMethod];
}
```

Bad:

```objc
[[self view] setFrame:CGRectMake(x, y, width, height)];
[_view removeFromSuperview];

self.nameLabel.text = [self getPersonWithId:idNumber].name;
```

* Similarly, use the dot syntax to get or set properties instead of the method equivalent.

Good:

```objc
NSUInteger numberOfItems = [sampleArray count]; // use NSInteger or NSUInteger instead of int

sampleView.backgroundColor = [UIColor greenColor];
```

Bad:

```objc
int numberOfItems = sampleArray.count;

[sampleView setBackgroundColor:[UIColor greenColor]];
``` 

* Use object literals, boxed expressions, and subscripting over the older, grosser alternatives.
* Separate binary operands with a single space, but unary operands and casts with none:

```objc
void *ptr = &value + 10 * 3;
NewType a = (NewType)b;

for (NSInteger i = 0; i < 10; i++) {
    doCoolThings();
}
```

Control Structures
==================

if-else structures
------------------------
* Always surround `if` bodies with curly braces if there is an `else`. Single-line `if` bodies without an `else` should be on the same line as the `if`.

* All curly braces should begin on the same line as their associated statement. They should end on a new line. [1TBS](http://en.wikipedia.org/wiki/Indent_style#Variant:_1TBS "1TBS")
* Put a single space after keywords and before their parentheses.
* Return and break early.
* No spaces between parentheses and their contents.

```objc
if (!goodCondition) return;

if (condition == YES) {
    // do stuff
} else {
    // do other stuff
}
```

If the name of a BOOL property is expressed as an adjective, the property can omit the “is” prefix but specifies the conventional name for the get accessor, for example:

```objc
@property (assign, getter=isEditable) BOOL editable;
```

Case Statements
-----------------------

Braces are not required for case statements, unless enforced by the complier.  
When a case contains more than one line, braces should be added.

```objc
switch (condition) {
  case 1:
    // ...
    break;
  case 2: {
    // ...
    // Multi-line example using braces
    break;
  }
  case 3:
    // ...
    break;
  default: 
    // ...
    break;
}

```

There are times when the same code can be used for multiple cases, and a fall-through should be used.  A fall-through is the removal of the 'break' statement for a case thus allowing the flow of execution to pass to the next case value.  A fall-through should be commented for coding clarity.

```objc
switch (condition) {
  case 1:
    // ** fall-through! **
  case 2:
    // code executed for values 1 and 2
    break;
  default: 
    // ...
    break;
}

```

When using an enumerated type for a switch, 'default' is not needed.   For example:

```objc
RWTLeftMenuTopItemType menuType = RWTLeftMenuTopItemMain;

switch (menuType) {
  case RWTLeftMenuTopItemMain:
    // ...
    break;
  case RWTLeftMenuTopItemShows:
    // ...
    break;
  case RWTLeftMenuTopItemSchedule:
    // ...
    break;
}
```

Ternary operator
-----------------------
The ternary operator, `?` , should only be used when it increases clarity or code neatness. A single condition is usually all that should be evaluated. Evaluating multiple conditions is usually more understandable as an if statement, or refactored into named variables.

**For example:**
```objc
result = a > b ? x : y;
```

**Not:**
```objc
result = a > b ? x = c > d ? c : d : y;
```

Exceptions and Error Handling
=============================
* Don't use exceptions for flow control.
* Use exceptions **only** to indicate programmer error.
* To indicate errors, use an `NSError **` argument .


Blocks
======

* Blocks should have a space between their return type and name.
* Block definitions should omit their return type when possible.
* Block definitions should omit their arguments if they are void.
* Parameters in block types should be named unless the block is initialized immediately.

```objc
void (^blockName1)(void) = ^{
    // do some things
};

id (^blockName2)(id) = ^ id (id args) {
    // do some things
};
```

Singletons
==========
Singleton objects should use a thread-safe pattern for creating their shared instance.

```objc
+ (instancetype)sharedInstance {
   static id sharedInstance = nil;

   static dispatch_once_t onceToken;
   dispatch_once(&onceToken, ^{
      sharedInstance = [[self alloc] init];
   });

   return sharedInstance;
}
```

Code Naming
===========
Good
----
 * Clear, expressive, non-ambiguous names.
   Since you do far more reading of code than writing, invest the time to type an extra eight characters (and then leverage autocomplete).

Bad
---
 * Hungarian notation
 
   e.g. data type as part of name `strUserName`
   
   Exception: `const` or `enum` values should follow Apple's class-name-then-least-to-most specific pattern. e.g. `UIDeviceOrientationLandscapeRight` or `VPNavigationBarHeight_iPad`).
 * Abbreviating. Lazy naming.
 
   e.g `index` What index?
   For an array of house objects use `houseIndex`

Method Signatures
=================
Use lower camel case
--------------------

Good:

```objc
answerViewController
```

Bad:

```objc
answer_view_controller
```

Start with action
-----------------
For methods that represent an action an object takes, start the name with the action.

Good:

```objc
- (IBAction)showDetailViewController:(id)sender
```

Bad:

```objc
- (void)detailButtonTapped:(id)sender
```

Getters
-------
If the method returns an attribute of the receiver, name the method after the attribute.  The use of "get" is unnecessary, unless one or more values are returned indirection.

Good:

```objc
- (NSInteger)age
```

Bad:

```objc
- (NSInteger)calcAge
- (NSInteger)getAge
```

Return Type Spacing
-------------------
For consistency method definitions should have a space between the + or - (scope) and the return type (matching Apple's style).

Good:

```objc
- (int)age
```

Bad:

```objc
-(int)age
-(int) age
```

Parameter Keywords
------------------
Use keywords before all parameters

Good:

```objc
- (void)sendAction:(SEL)aSelector to:(id)anObject forAllCells:(BOOL)flag;
```

Bad:

```objc
- (void)sendAction:(SEL)aSelector :(id)anObject :(BOOL)flag;
```

Be Descriptive
--------------
Make the word before the argument describe the argument.

Good:

```objc
- (id)viewWithTag:(NSInteger)tag;
```

Bad:

```objc
- (id)taggedView:(NSInteger)tag;
``` 
    
**Note: this is a poor example because in general tags are an antipattern and should be avoided if at all possible**

Avoid And (With Exceptions)
---------------------------
Do not use "and" to link keywords that are attributes of the receiver.

Good:

```objc
- (NSInteger)runModalForDirectory:(NSString *)path file:(NSString *)name types:(NSArray *)fileTypes;
```

Bad:

```objc
- (NSInteger)runModalForDirectory:(NSString *)path andFile:(NSString *)name andTypes:(NSArray *)fileTypes;
```

**Exception:** if the method describes *two separate actions*, use "and" to link them:

```objc
- (BOOL)openFile:(NSString *)fullPath withApplication:(NSString *)appName andDeactivate:(BOOL)flag;
```

Signature Spacing
---------------
Method parameters should not have a space between the keyword and the parameter.

Good:

```objc
- (void)setExample:(NSString *)text;
```


Bad:

```objc
- (void)setExample: (NSString *)text;
- (void)setExample:(NSString *) text;
```

Init Methods
---------------
Init methods should return instancetype instead of id. Generally this is the one place ivars should be used instead of properties because the class may be in an inconsistent state until it is fully initialized.

```objc
- (instancetype)init {
  self = [super init];
  if (self) {
    // ...
  }
  return self;
}
```

Dealloc Methods
---------------
Dealloc methods are no longer required when using arc but in certain cases must be used to remove observers, KVO, etc.

```objc
- (void)dealloc{
    [[NSNotificationCenter defaultCenter] removeObserver:self];
}
```

Init and dealloc
----------------------
`dealloc` methods should be placed at the top of the implementation, directly after the `@synthesize` and `@dynamic` statements. `init` should be placed directly below the `dealloc` methods of any class.

Classes
=======
Class names use upper camel case, ie First word capitalized, start of new words capitalized as well.  In general there should be one class per .h/.m file.

```objc
SVSSpy
SVSWhiteSpy
SVSBlackSpy
SVSCar
SVSCarEngine
```

Namespace prefixes
==================

Descriptive names should generally avoid conflicts, however there are tangible benefits to using three character class name prefixes e.g. `FXTObjectSerialization`. Class name prefixes can be used to:

* filter visible files in the project navigator in Xcode
* allow you to ignore search results in files without your desired prefix
* convey the ancestry of the class (as sometimes classes have been reused between projects)
* distinguish between components of the app

Class name prefixes may be avoided for CoreData entity names. 

Avoid using overly simple names like "Model" "View" "Object".  

When you don't prefix and you have a namespace collision they're megahard to debug and unravel.

Booleans
========
Objective-C uses `YES` and `NO`.  Therefore `true` and `false` should only be used for CoreFoundation, C or C++ code.  Since `nil` resolves to `NO` it is unnecessary to compare it in conditions.

Good:

```objc
if (someObject) {}
if (![anotherObject boolValue]) {}
```

Bad:

```objc
if (someObject == nil) {}
if ([anotherObject boolValue] == NO) {}
```

Never compare something directly to `YES`, because `YES` is defined as `1`, and a `BOOL` in Objective-C is a `CHAR` type that is 8 bits long (so a value of `11111110` will return `NO` if compared to `YES`).

For an object pointer:

```objc
if (!someObject) {
}

if (someObject == nil) {
}
```

For a `BOOL` value:

```objc
if (isAwesome)
if (!someNumber.boolValue)
if (someNumber.boolValue == NO)
```

Bad:

```objc
if (isAwesome == YES) // Never do this.
```

If the name of a `BOOL` property is expressed as an adjective, the property’s name can omit the `is` prefix but should specify the conventional name for the getter.

For example:

```objc
@property (assign, getter=isEditable) BOOL editable;
```

Properties
==========
Pointer Spacing
---------------
The `*` should be **nearest** the variable, not the type.

Good:

```objc
NSString *text = @"foo";
```

Bad:

```objc
NSString * text = @"foo";
NSString* text = @"foo";
```

@synthesize and @dynamic
------------------------

*Note: as of Xcode 4.4/4.5 `@synthesize` should be removed with some exceptions. See (http://useyourloaf.com/blog/2012/08/01/property-synthesis-with-xcode-4-dot-4.html) "When To Supply The Instance Variable" for clarity*

`@synthesize` and `@dynamic` should go directly beneath `@implementation` where each `@synthesize` and `@dynamic` should be on a single line.

When synthesizing the instance variable, use `@synthesize var = _var;` as this prevents accidentally calling `var = blah;` when `self.var = blah;` is correct.

@property
---------

Property definitions should be used in place of ivars. When defining properties, put strong/weak/retain/assign first then nonatomic for consistency.

```objc
@property (weak, nonatomic) IBOutlet UIWebView *messageWebView;
```    

*Note: IBOutlets should be `weak` unless they are a top-level item in a xib (e.g. the top-level view is `strong`, anything beneath it is `weak`)

Info about the overhead and performance of properties vs ivars can be found [here](http://blog.bignerdranch.com/4005-should-i-use-a-property-or-an-instance-variable/).


Enums
======
Magic numbers that are integers should be stored in an enum.  If you want to use it as a type, you can make it a type:

```objc
typedef NS_ENUM(NSUInteger, VPLeftMenuTopItemType) {
    VPLeftMenuTopItemTypeMain = 0,
    VPLeftMenuTopItemTypeShows,
    VPLeftMenuTopItemTypeSchedule,
    VPLeftMenuTopItemTypeWatchLive,
    VPLeftMenuTopItemTypeMax,
};
```

In this case each successive item in the enum will have an integer value greater than the previous item.

Naming in this ^ style also makes the enum Swift-compatible. (e.g. `.Main`, `.WatchLive`)

You can also make explicit value assignments (showing older k-style constant definition):

```objc
typedef NS_ENUM(NSInteger, RBKGlobalConstants) {
    RBKPinSizeMin = 1,
    RBKPinSizeMax = 5,
    RBKPinCountMin = 100,
    RBKPinCountMax = 500,
};
```


Older k-style constant definitions should be **avoided** unless writing CoreFoundation C code (unlikely).

Bad:

```objc
enum GlobalConstants {
    kMaxPinSize = 5,
    kMaxPinCount = 500,
};
```
Categories
=========

Categories may be used to concisely segment functionality and should be named to describe that functionality.

Good:

```objc
@interface UIViewController (FXTMediaPlaying)
@interface NSString (NSStringEncodingDetection)
```

Bad:

```objc
@interface NYTAdvertisement (private)
@interface NSString (FXTAdditions)
```

xib Files
=========
xib files should always be saved in their language-specific folders.  The default folder for English is `en.lproj`.
xib file names should end in `view`, `menu` or `window` but never `controller` (as xibs are *not* controllers).

Give descriptive labels to views to make them easier to recognize, preferrably the same name as the referencing outlet if there is one.

Image naming
===========

Image names should be named consistently to preserve organization and developer sanity. They should be named as one camel case string with a description of their purpose, followed by the un-prefixed name of the class or property they are customizing (if there is one), followed by a further description of color and/or placement, and finally their state.

**For example:**

* `RefreshBarButtonItem` / `RefreshBarButtonItem@2x` and `RefreshBarButtonItemSelected` / `RefreshBarButtonItemSelected@2x`
* `ArticleNavigationBarWhite` / `ArticleNavigationBarWhite@2x` and `ArticleNavigationBarBlackSelected` / `ArticleNavigationBarBlackSelected@2x`.

Images that are used for a similar purpose should be grouped in respective groups in an Images folder or Asset Catalog.

Golden Path
===========
When coding with conditionals, the left hand margin of the code should be the "golden" or "happy" path.  That is, don't nest `if` statements.  Multiple return statements are ok.

Good:

```objc
- (void)someMethod {
    if (![someOther boolValue]) return;
    //Do something important
}
```

Bad:

```objc
- (void)someMethod {
    if ([someOther boolValue]) {;
        //Do something important
    }
}
```

More on Brackets
========
Method brackets should be at the end of the line, preceded by a single space

Good:

```objc
- (void)checkCondition {
    if (foo) {
       NSLog(@"Woof!");
    } else {
       NSLog(@"Meow!");
    }
}
```

Prevents Bugs:

```objc
if (foo) {
   NSLog(@"Moof!");
}
```

Exception for Condition Checking:

```objc
if (condition) return;
```

Note: Apple's templates sometimes have the opening bracket on a new line flush left, but generally at the end of the line.

```objc
- (UITableViewCell *)tableView:(UITableView *)aTableView cellForRowAtIndexPath:(NSIndexPath *)indexPath {

    static NSString *CellIdentifier = @"RootViewControllerCellIdentifier";
    ....
}
```

Boyscout / Girl Guide
=====================
Always leave the code in better condition than you found it.

Copyrights
=====================
Use iTechArt copyright. 

```objc
//  Copyright (c) 2015 iTechArt. All rights reserved.
```

Set the Organization in your .xcodeproj to override your default company name for new files that are created. Select the project in File Navigator, then use the File Inspector (first button) in the Utilities pane. Set the organization to Robots and Pencils Inc.

Proper Code Nutrition
=====================
Avoid magic numbers with no meaning, preferring instead a named variable or constant (see following examples).

Integers
--------
Bad:

```objc
if ([pin length] < 5)
```

What is this checking for?

Good:

```objc
if ([pin length] > RBKPinSizeMax)
```

We can see that this is checking if the pin is longer than the max allowed.

Floats
------
In a (top level) .h you can define the float:

```objc
extern const float RBKFooFloat;
```

and then in the relevant .m file assign it a value:

```objc
const float RBKFooFloat = 18.0;
```

Strings
-------
In a (top level) .h you can define the string:

```objc
extern NSString * const RBKLocationsDatabaseName;
```

and then in the relevant .m file assign it a value:

```objc
NSString * const RBKLocationsDatabaseName = @"locations.db";
```

Note: the above is a constant pointer to an `NSString` object while the following is a pointer to a constant `NSString` object.

Bad:

```objc
const NSString * RBKConstantString = @""; // pointer to constant
// which is equivalent to
NSString const * RBKConstantString = @"";
```

Basic Code Principles
-------
* Each function/method should aim to perform one action/task that reflects it's name.
* Since each function/method performs one action/task each one should be relatively short. If the code does not fit on one screen for example, you know you have a problem!
* Declare local variables as close to the code they are used in as possible.
* Always aim to reduce code nesting (ie a statement that is nested in an if, an if, a for and an if is hard for the brain to evaluate.  Refactor!).

Testing
============
* You are responsible for thoroughly testing your own code before passing off to quality assurance.

Inspiration
============

* [Robots & Pencils](https://github.com/RobotsAndPencils/objective-c-style-guide)
* [New York Times](https://github.com/NYTimes/objective-c-style-guide)
* [Raywenderlich](https://github.com/raywenderlich/objective-c-style-guide")

