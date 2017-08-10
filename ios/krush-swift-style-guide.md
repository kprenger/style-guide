# Krushtech Swift Style Guide.




## Table of Contents

* [Correctness](#correctness)
* [Naming](#naming)
  * [Protocols](#protocols)
  * [Enumerations](#enumerations)
  * [Selectors](#selectors)
  * [Generics](#generics)
  * [Class Prefixes](#class-prefixes)
  * [Language](#language)
* [Code Organization](#code-organization)
  * [Protocol Conformance](#protocol-conformance)
  * [Unused Code](#unused-code)
  * [Minimal Imports](#minimal-imports)
* [Project Organization](#project-organization)
* [Character Limit](#character-limit)
* [Spacing](#spacing)
* [Comments](#comments)
* [Classes and Structures](#classes-and-structures)
  * [Use of Self](#use-of-self)
  * [Protocol Conformance](#protocol-conformance)
  * [Computed Properties](#computed-properties)
  * [Final](#final)
* [Function Declarations](#function-declarations)
* [Closure Expressions](#closure-expressions)
* [Types](#types)
  * [Constants](#constants)
  * [Static Methods and Variable Type Properties](#static-methods-and-variable-type-properties)
  * [Optionals](#optionals)
  * [Struct Initializers](#struct-initializers)
  * [Lazy Initialization](#lazy-initialization)
  * [Type Inference](#type-inference)
  * [Syntactic Sugar](#syntactic-sugar)
* [Functions vs Methods](#functions-vs-methods)
* [Functions vs Properties](#functions-vs-properties)
* [Memory Management](#memory-management)
  * [Extending Lifetime](#extending-lifetime)
* [Access Control](#access-control)
* [Control Flow](#control-flow)
* [Golden Path](#golden-path)
  * [Failing Guards](#failing-guards)
* [Semicolons](#semicolons)
* [Parentheses](#parentheses)
* [Views](#views)
* [View Models](#view-models)
* [View Controllers](#view-controllers)
* [Credits](#credits)


## Correctness

Consider warnings to be errors. This rule informs many stylistic decisions such as not to use the `++` or `--` operators, C-style for loops, or strings as selectors.

## Naming

Use descriptive names with camel case for classes, methods, variables, etc. Type names (classes, structures, enumerations and protocols) should be capitalized, while method names and variables should start with a lower case letter.

**Acceptable:**

```swift
private let maximumWidgetCount = 100

class WidgetContainer {
  var widgetButton: UIButton
  let widgetHeightPercentage = 0.85
}
```

**Unacceptable:**

```swift
let MAX_WIDGET_COUNT = 100

class app_widgetContainer {
  var wBut: UIButton
  let wHeightPct = 0.85
}
```

Abbreviations and acronyms should generally be avoided. Following the [API Design Guidelines](https://swift.org/documentation/api-design-guidelines/#follow-case-conventions), abbreviations and initialisms that appear in all uppercase should be uniformly uppercase or lowercase. Examples:

**Preferred**

```swift
let urlString: URLString // URL is an acronym for Uniform Resource Locator
let krushURL: URL // URL is an acronym for Uniform Resource Locator
let hardwareCPU: HardwareCPU // CPU is an initialism for Central Processing Unit
let userId: UserId // Id is an abbreviation short for Identifier, not an acronym
```

**Not Preferred**

```swift
let uRLString: UrlString // URL is an acronym for Uniform Resource Locator
let krushUrl: URL // URL is an acronym for Uniform Resource Locator
let hardwareCpu: HardwareCpu // CPU is an initialism for Central Processing Unit
let userID: UserID // Id is an abbreviation short for Identifier, not an acronym or initialism
```

For functions and init methods, prefer named parameters for all arguments unless the context is very clear. Include external parameter names if it makes function calls more readable.

```swift
func dateFromString(dateString: String) -> NSDate
func convertPointAt(column column: Int, row: Int) -> CGPoint
func timedAction(afterDelay delay: NSTimeInterval, perform action: SKAction) -> SKAction!

// would be called like this:
dateFromString("2014-03-14")
convertPointAt(column: 42, row: 13)
timedAction(afterDelay: 1.0, perform: someOtherAction)
```

For methods, follow the standard Apple convention of referring to the first parameter in the method name:

```swift
class Counter {
  func combineWith(otherCounter: Counter, options: Dictionary?) { ... }
  func incrementBy(amount: Int) { ... }
}
```

### Protocols

Following Apple's API Design Guidelines, protocols names that describe what something is should be a noun. Examples: `Collection`, `WidgetFactory`. Protocols names that describe an ability should end in -ing, -able, or -ible. Examples: `Equatable`, `Resizing`.

### Enumerations

Following Apple's API Design Guidelines for Swift 3, use lowerCamelCase for enumeration values.

```swift
enum Shape {
  case rectangle
  case square
  case rightTriangle
  case equilateralTriangle
}
```


### Class Prefixes

Swift types are automatically namespaced by the module that contains them and you should not add a class prefix such as RW. If two names from different modules collide you can disambiguate by prefixing the type name with the module name. However, only specify the module name when there is possibility for confusion which should be rare.

```swift
import SomeModule

let myClass = MyModule.UsefulClass()
```

### Selectors

Selectors are Obj-C methods that act as handlers for many Cocoa and Cocoa Touch APIs. Prior to Swift 2.2, they were specified using type unsafe strings. This now causes a compiler warning. The "Fix it" button replaces these strings with the **fully qualified** type safe selector. Often, however, you can use context to shorten the expression. This is the preferred style.

**Acceptable:**

```swift
let sel = #selector(viewDidLoad)
```

**Unacceptable:**

```swift
let sel = #selector(ViewController.viewDidLoad)
```

### Generics

Generic type parameters should be descriptive, upper camel case names. When a type name doesn't have a meaningful relationship or role, use a traditional single uppercase letter such as `T`, `U`, or `V`.

**Acceptable:**

```swift
struct Stack<Element> { ... }
func writeTo<Target: OutputStream>(inout target: Target)
func max<T: Comparable>(x: T, _ y: T) -> T
```

**Unacceptable:**

```swift
struct Stack<T> { ... }
func writeTo<target: OutputStream>(inout t: target)
func max<Thing: Comparable>(x: Thing, _ y: Thing) -> Thing
```

### Language

Use US English spelling to match Apple's API.

**Acceptable:**

```swift
let color = "red"
```

**Unacceptable:**

```swift
let colour = "red"
```

## Code Organization

Use extensions to organize your code into logical blocks of functionality. Each extension should be set off with a `// MARK: -` comment to keep things well-organized.

### Protocol Conformance

 In particular, when adding protocol conformance to a model, prefer adding a separate extension for the protocol methods. This keeps the related methods grouped together with the protocol and can simplify instructions to add a protocol to a class with its associated methods.

**Acceptable:**

```swift
class MyViewcontroller: UIViewController {
  // class stuff here
}

// MARK: - UITableViewDataSource
extension MyViewcontroller: UITableViewDataSource {
  // table view data source methods
}

// MARK: - UIScrollViewDelegate
extension MyViewcontroller: UIScrollViewDelegate {
  // scroll view delegate methods
}
```

**Unacceptable:**

```swift
class MyViewcontroller: UIViewController, UITableViewDataSource, UIScrollViewDelegate {
  // all methods
}
```

Since the compiler does not allow you to re-declare protocol conformance in a derived class, it is not always required to replicate the extension groups of the base class. This is especially true if the derived class is a terminal class and a small number of methods are being overriden. When to preserve the extension groups is left to the discretion of the author.

For UIKit view controllers, consider grouping lifecycle, custom accessors, and IBAction in separate class extensions.

### Unused Code

Unused (dead) code, including Xcode template code and placeholder comments should be removed.
This includes any empty/unused UIApplicationDelegate methods.

**Unacceptable:**

```swift
override func didReceiveMemoryWarning() {
    super.didReceiveMemoryWarning()
    // Dispose of any resources that can be recreated.
}

override func numberOfSectionsInTableView(tableView: UITableView) -> Int {
    // #warning Incomplete implementation, return the number of sections
    return 1
}

override func tableView(tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
    // #warning Incomplete implementation, return the number of rows
    return Database.contacts.count
}

```


**Acceptable:**

```swift
override func tableView(tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
    return Database.contacts.count
}
```
### Minimal Imports

Keep imports minimal. For example, don't import `UIKit` when importing `Foundation` will suffice.

## Project Organization

It is beneficial to keep your file system hiearchy in-line with your project hiearchy. In order to achieve this, whenever you create a new group in Xcode, make sure that its location is selected in the file system. There is a setting in the File Inspector which allows you to select the folder location for that group.

Always try to create a group prior to any sub-files, as the Relative to Group setting will automatically create the sub-files within the appropriate folder in your file system. If you do create a group after-the-fact, you will need to perform some manual moving of files in the file system.

## Character Limit

Maintain a 120 character per line limit

## Spacing

* Indent using 4 spaces rather than tabs to conserve space and help prevent line wrapping. Be sure to set this preference in Xcode and in the Project settings.


* Method braces and other braces (`if`/`else`/`switch`/`while` etc.) always open on the same line as the statement but close on a new line.


**Acceptable:**

```swift
if user.isHappy {
    // Do something
} else {
    // Do something else
}
```

**Unacceptable:**

```swift
if user.isHappy
{
  // Do something
}
else {
  // Do something else
}
```

* There should be exactly one blank line between methods to aid in visual clarity and organization. Whitespace within methods should separate functionality, but having too many sections in a method often means you should refactor into several methods.

* Colons always have no space on the left and one space on the right. Exceptions are the ternary operator `? :` and empty dictionary `[:]`.

**Acceptable:**

```swift
class TestDatabase: Database {
    var data: [String: CGFloat] = ["A": 1.2, "B": 3.2]
}
```

**Unacceptable:**

```swift
class TestDatabase : Database {
    var data :[String:CGFloat] = ["A" : 1.2, "B":3.2]
}
```

## Comments

When they are needed, use comments to explain **why** a particular piece of code does something. Comments must be kept up-to-date or deleted.

Avoid block comments inline with code, as the code should be as self-documenting as possible. *Exception: This does not apply to those comments used to generate documentation.*


## Classes and Structures

### Which one to use?

Remember, structs have [value semantics](https://developer.apple.com/library/mac/documentation/Swift/Conceptual/Swift_Programming_Language/ClassesAndStructures.html#//apple_ref/doc/uid/TP40014097-CH13-XID_144). Use structs for things that do not have an identity. An array that contains [a, b, c] is really the same as another array that contains [a, b, c] and they are completely interchangeable. It doesn't matter whether you use the first array or the second, because they represent the exact same thing. That's why arrays are structs.

Classes have [reference semantics](https://developer.apple.com/library/mac/documentation/Swift/Conceptual/Swift_Programming_Language/ClassesAndStructures.html#//apple_ref/doc/uid/TP40014097-CH13-XID_145). Use classes for things that do have an identity or a specific life cycle. You would model a person as a class because two person objects are two different things. Just because two people have the same name and birthdate, doesn't mean they are the same person. But the person's birthdate would be a struct because a date of 3 March 1950 is the same as any other date object for 3 March 1950. The date itself doesn't have an identity.

Sometimes, things should be structs but need to conform to `AnyObject` or are historically modeled as classes already (`NSDate`, `NSSet`). Try to follow these guidelines as closely as possible.

### Example definition

Here's an example of a well-styled class definition:

```swift
class Circle: Shape {
    var x: Int, y: Int
    var radius: Double
    var diameter: Double {
        get {
            return radius * 2
        }
        set {
            radius = newValue / 2
        }
    }

    init(x: Int, y: Int, radius: Double) {
        self.x = x
        self.y = y
        self.radius = radius
    }

    convenience init(x: Int, y: Int, diameter: Double) {
        self.init(x: x, y: y, radius: diameter / 2)
    }

    func describe() -> String {
        return "I am a circle at \(centerString()) with an area of \(computeArea())"
    }

    override func computeArea() -> Double {
        return M_PI * radius * radius
    }

    private func centerString() -> String {
        return "(\(x),\(y))"
    }
}
```

The example above demonstrates the following style guidelines:

 + Specify types for properties, variables, constants, argument declarations and other statements with a space after the colon but not before, e.g. `x: Int`, and `Circle: Shape`.
 + Define multiple variables and structures on a single line if they share a common purpose / context.
 + Indent getter and setter definitions and property observers.
 + Don't add modifiers such as `internal` when they're already the default. Similarly, don't repeat the access modifier when overriding a method.


### Use of Self

For conciseness, avoid using `self` since Swift does not require it to access an object's properties or invoke its methods.

Use `self` when required to differentiate between property names and arguments in initializers, and when referencing properties in closure expressions (as required by the compiler):

```swift
class BoardLocation {
    let row: Int, column: Int

    init(row: Int, column: Int) {
        self.row = row
        self.column = column
    
        let closure = {
            print(self.row)
        }
    }
}
```

### Computed Properties

For conciseness, if a computed property is read-only, omit the get clause. The get clause is required only when a set clause is provided.

**Acceptable:**

```swift
var diameter: Double {
    return radius * 2
}
```

**Unacceptable:**

```swift
var diameter: Double {
    get {
        return radius * 2
    }
}
```

### Final

Mark classes `final` when inheritance is not intended. Example:

```swift
// Turn any generic type into a reference type using this Box class.
final class Box<T> {
    let value: T 
    init(_ value: T) {
        self.value = value
    }
}
```

## Function Declarations

Keep short function declarations on one line including the opening brace:

```swift
func reticulateSplines(spline: [Double]) -> Bool {
    // reticulate code goes here
}
```

For functions with long signatures, add line breaks at appropriate points and add an extra indent on subsequent lines:

```swift
func reticulateSplines(spline: [Double], adjustmentFactor: Double,
        translateConstant: Int, comment: String) -> Bool {
        // reticulate code goes here
}
```

## Closure Expressions

Use trailing closure syntax only if there's a single closure expression parameter at the end of the argument list. Give the closure parameters descriptive names.

**Acceptable:**

```swift
UIView.animateWithDuration(1.0) {
    self.myView.alpha = 0
}

UIView.animateWithDuration(1.0,
    animations: {
        self.myView.alpha = 0
    },
    completion: { finished in
        self.myView.removeFromSuperview()
    }
)
```

**Unacceptable:**

```swift
UIView.animateWithDuration(1.0, animations: {
    self.myView.alpha = 0
})

UIView.animateWithDuration(1.0,
    animations: {
        self.myView.alpha = 0
    }) { f in
        self.myView.removeFromSuperview()
}
```

For single-expression closures where the context is clear, use implicit returns:

```swift
attendeeList.sort { a, b in
    a > b
}
```

Chained methods using trailing closures should be clear and easy to read in context. Decisions on spacing, line breaks, and when to use named versus anonymous arguments is left to the discretion of the author. Examples:

```swift
let value = numbers.map { $0 * 2 }.filter { $0 % 3 == 0 }.indexOf(90)

let value = numbers
     .map {$0 * 2}
     .filter {$0 > 50}
     .map {$0 + 10}
```

## Types

Always use Swift's native types when available. Swift offers bridging to Objective-C so you can still use the full set of methods as needed.

**Acceptable:**

```swift
let width = 120.0                                    // Double
let widthString = (width as NSNumber).stringValue    // String
```

**Unacceptable:**

```swift
let width: NSNumber = 120.0                          // NSNumber
let widthString: NSString = width.stringValue        // NSString
```

In Sprite Kit code, use `CGFloat` if it makes the code more succinct by avoiding too many conversions.

### Constants

Constants are defined using the `let` keyword, and variables with the `var` keyword. Always use `let` instead of `var` if the value of the variable will not change. Constants should be wrapped in a `struct` that is globally accessible and contains smaller `struct`'s that pertain to the scope of the constant. To avoid bloating the `Constants` file, add new constants to the `Constants` struct using an extension.

**Tip:** A good technique is to define everything using `let` and only change it to `var` if the compiler complains!

You can define constants on a type rather than an instance of that type using type properties. To declare a type property as a constant simply use `static let`. Type properties declared in this way are generally preferred over global constants because they are easier to distinguish from instance properties. Example:

**Acceptable:**

```swift
struct Constants {
    struct Math {
        static let e  = 2.718281828459045235360287
        static let pi = 3.141592653589793238462643
    }
}

radius * Constants.Math.pi * 2 // circumference
```
**Note:** The advantage of using a case-less enumeration is that it can't accidentally be instantiated and works as a pure namespace.

**Unacceptable:**

```swift
let e  = 2.718281828459045235360287  // pollutes global namespace
let pi = 3.141592653589793238462643

radius * pi * 2 // is pi instance data or a global constant?
```

**Acceptable:**

```swift
extension Constants {
    struct Regex {
    	static let hashtag = "(^|\\s)#([A-Za-z_][A-Za-z0-9_]*)"
	static let email = "[A-Z0-9a-z._%+-]+@[A-Za-z0-9.-]+\\.[A-Za-z]{2,64}"
    }
}
```

**Unacceptable:**

```swift
struct Constants {
    struct Math {
        static let e  = 2.718281828459045235360287
        static let pi = 3.141592653589793238462643
    }
    
    struct Regex {
    	static let hashtag = "(^|\\s)#([A-Za-z_][A-Za-z0-9_]*)"
	static let email = "[A-Z0-9a-z._%+-]+@[A-Za-z0-9.-]+\\.[A-Za-z]{2,64}"
    }
}
```

### Static Methods and Variable Type Properties

Static methods and type properties work similarly to global functions and global variables and should be used sparingly. They are useful when functionality is scoped to a particular type or when Objective-C interoperability is required.

### Optionals

Declare variables and function return types as optional with `?` where a nil value is acceptable.

Use implicitly unwrapped types declared with `!` only for instance variables that you know will be initialized later before use, such as subviews that will be set up in `viewDidLoad`.

When accessing an optional value, use optional chaining if the value is only accessed once or if there are many optionals in the chain:

```swift
textContainer?.textLabel?.setNeedsDisplay()
```

Use optional binding when you need to perform more than 2 operations on the same optional variable or captured list in a closure (e.g. `[weak self]`):

```swift
if let textContainer = textContainer {
    // do many things with textContainer
}

doThingWithCompletion { [weak self] in
    guard let strongSelf = self else { return }

    // do something with the unwrapped self
}
```

When naming optional variables and properties, avoid naming them like `optionalString` or `maybeView` since their optional-ness is already in the type declaration.

For optional binding, shadow the original name when appropriate rather than using names like `unwrappedView` or `actualLabel`.

**Acceptable:**

```swift
var subview: UIView?
var volume: Double?

// later on...
if let subview = subview, volume = volume {
    // do something with unwrapped subview and volume
}
```

**Unacceptable:**

```swift
var optionalSubview: UIView?
var volume: Double?

if let unwrappedSubview = optionalSubview {
    if let realVolume = volume {
      // do something with unwrappedSubview and realVolume
    }
}
```

### Struct Initializers

Use the native Swift struct initializers rather than the legacy CGGeometry constructors.

**Acceptable:**

```swift
let bounds = CGRect(x: 40, y: 20, width: 120, height: 80)
let centerPoint = CGPoint(x: 96, y: 42)
```

**Unacceptable:**

```swift
let bounds = CGRectMake(40, 20, 120, 80)
let centerPoint = CGPointMake(96, 42)
```

Prefer the struct-scope constants `CGRect.infinite`, `CGRect.null`, etc. over global constants `CGRectInfinite`, `CGRectNull`, etc. For existing variables, you can use the shorter `.zero`.


### Lazy Initialization

Consider using lazy initialization for finer grain control over object lifetime. This is especially true for `UIViewController` that loads views lazily. You can either use a closure that is immediately called `{ }()` or call a private factory method. Example:

```swift
lazy var locationManager: CLLocationManager = makeLocationManager()

private func makeLocationManager() -> CLLocationManager {
    let manager = CLLocationManager()
    manager.desiredAccuracy = kCLLocationAccuracyBest
    manager.delegate = self
    manager.requestAlwaysAuthorization()
    return manager
}
```

**Notes:**
  - `[unowned self]` is not required here. A retain cycle is not created.
  - Location manager has a side-effect for popping up UI to ask the user for permission so fine grain control makes sense here.


### Type Inference

Prefer compact code and let the compiler infer the type for constants or variables of single instances. Type inference is also appropriate for small (non-empty) arrays and dictionaries. When required, specify the specific type such as `CGFloat` or `Int16`.

**Acceptable:**

```swift
let message = "Click the button"
let currentBounds = computeViewBounds()
var names = ["Mic", "Sam", "Christine"]
let maximumWidth: CGFloat = 106.5
```

**Unacceptable:**

```swift
let message: String = "Click the button"
let currentBounds: CGRect = computeViewBounds()
let names = [String]()
```

#### Type Annotation for Empty Arrays and Dictionaries

For empty arrays and dictionaries, use type annotation. (For an array or dictionary assigned to a large, multi-line literal, use type annotation.)

**Acceptable:**

```swift
var names: [String] = []
var lookup: [String: Int] = [:]
```

**Unacceptable:**

```swift
var names = [String]()
var lookup = [String: Int]()
```

**NOTE**: Following this guideline means picking descriptive names is even more important than before.


### Syntactic Sugar

Prefer the shortcut versions of type declarations over the full generics syntax.

**Acceptable:**

```swift
var deviceModels: [String]
var employees: [Int: String]
var faxNumber: Int?
```

**Unacceptable:**

```swift
var deviceModels: Array<String>
var employees: Dictionary<Int, String>
var faxNumber: Optional<Int>
```

## Functions vs Methods

Free functions, which aren't attached to a class or type, should be used sparingly. When possible, prefer to use a method instead of a free function. This aids in readability and discoverability.

Free functions are most appropriate when they aren't associated with any particular type or instance.

**Preferred**

```swift
let sorted = items.mergeSort()  // easily discoverable
rocket.launch()  // clearly acts on the model
```

**Not Preferred**

```swift
let sorted = mergeSort(items)  // hard to discover
launch(&rocket)
```

**Free Function Exceptions**

```swift
let tuples = zip(a, b)  // feels natural as a free function (symmetry)
let value = max(x,y,z)  // another free function that feels natural
```

## Functions vs Properties

In some cases functions with no arguments might be interchangeable with read-only properties. Although the semantics are similar, there are some stylistic conventions on when to prefer one to another.

Prefer a property over a function when the underlying algorithm:

* does not throw any Exceptions
* has a O(1) complexity
* is cheap to calculate (or caсhed on the first run)
* returns the same result over invocations

## Memory Management

Code should not create reference cycles. Analyze your object graph and prevent strong cycles with `weak` and `unowned` references. Alternatively, use value types (`struct`, `enum`) to prevent cycles altogether.

### Extending object lifetime

Extend object lifetime using the `[weak self]` and `guard let strongSelf = self else { return }` idiom. `[weak self]` is preferred to `[unowned self]` where it is not immediately obvious that `self` outlives the closure. Explicitly extending lifetime is preferred to optional unwrapping.

**Preferred**

```swift
resource.request().onComplete { [weak self] response in
    guard let strongSelf = self else { return }
    let model = strongSelf.updateModel(response)
    strongSelf.updateUI(model)
}
```

**Not Preferred**

```swift
// might crash if self is released before response returns
resource.request().onComplete { [unowned self] response in
    let model = self.updateModel(response)
    self.updateUI(model)
}
```

**Not Preferred**

```swift
// deallocate could happen between updating the model and updating UI
resource.request().onComplete { [weak self] response in
    let model = self?.updateModel(response)
    self?.updateUI(model)
}
```

## Access Control

Use `private` as the leading property specifier. The only things that should come before access control are the `static` specifier or attributes such as `@IBAction` and `@IBOutlet`.

**Acceptable:**

```swift
class TimeMachine {  
    private dynamic lazy var fluxCapacitor = FluxCapacitor()
}
```

**Unacceptable:**

```swift
class TimeMachine {  
    lazy dynamic private var fluxCapacitor = FluxCapacitor()
}
```

## Golden Path

When coding with conditionals, the left hand margin of the code should be the "golden" or "happy" path. That is, don't nest `if` statements. Multiple return statements are OK. The `guard` statement is built for this.

**Acceptable:**

```swift
func computeFFT(context: Context?, inputData: InputData?) throws -> Frequencies {

    guard let context = context else { throw FFTError.noContext }
    guard let inputData = inputData else { throw FFTError.noInputData }
    
    // use context and input to compute the frequencies
    
    return frequencies
}
```

**Unacceptable:**

```swift
func computeFFT(context: Context?, inputData: InputData?) throws -> Frequencies {

    if let context = context {
        if let inputData = inputData {
            // use context and input to compute the frequencies

            return frequencies
        } else {
            throw FFTError.noInputData
        }
    } else {
        throw FFTError.noContext
    }
}
```

When multiple optionals are unwrapped either with `guard` or `if let`, minimize nesting by using the compound version when possible. Example:

**Acceptable:**

```swift
guard let number1 = number1, number2 = number2, number3 = number3 else { fatalError("impossible") }
// do something with numbers
```

**Unacceptable:**

```swift
if let number1 = number1 {
  if let number2 = number2 {
    if let number3 = number3 {
      // do something with numbers
    }
    else {
      fatalError("impossible")
    }
  }
  else {
    fatalError("impossible")
  }
}
else {
  fatalError("impossible")
}
```

### Failing Guards

Guard statements are required to exit in some way. Generally, this should be simple one line statement such as `return`, `throw`, `break`, `continue`, and `fatalError()`. Large code blocks should be avoided. If cleanup code is required for multiple exit points, consider using a `defer` block to avoid cleanup code duplication.

## Semicolons

Swift does not require a semicolon after each statement in your code. They are only required if you wish to combine multiple statements on a single line.

Do not write multiple statements on a single line separated with semicolons.

The only exception to this rule is the `for-conditional-increment` construct, which requires semicolons. However, alternative `for-in` constructs should be used where possible.

**Acceptable:**

```swift
let swift = "not a scripting language"
```

**Unacceptable:**

```swift
let swift = "not a scripting language";
```

**NOTE**: Swift is very different from JavaScript, where omitting semicolons is [generally considered unsafe](http://stackoverflow.com/questions/444080/do-you-recommend-using-semicolons-after-every-statement-in-javascript)

## Parentheses

Parentheses around conditionals are not required and should be omitted.

**Acceptable:**

```swift
if name == "Hello" {
  print("World")
}
```

**Unacceptable:**

```swift
if (name == "Hello") {
  print("World")
}
```

## Views

All views should be independent of any models and should be as reusable as possible. View property names should make it clear what they do, but abstract enough that it remains reuseable.

**Acceptable**

```swift
class MyView: UIView {
  @IBOutlet weak var titleLabel: UILabel!
  @IBOutlet weak var subtitleLabel: UILabel!
  @IBOutlet weak var imageView: UIImageView!
}
```

**Unacceptable**
```swift
class VagueView: UIView {
  @IBOutlet weak var label1: UILabel!
  @IBOutlet weak var label2: UILabel!
}
```

If it is possible for the view to reused it should be organized in a `nib` file and instantiated in the following manner.

```swift
class MyView: UIView {
  override init(frame: CGRect) {
    super.init(frame: frame)
    let view = Bundle.main.loadNibNamed("MyView", owner: self, options: nil)?.first as! UIView
    view.frame = frame
    addSubview(view)
  }
}
```

## View Models

View models are used to direct the view what to display without giving access to the view. All view models should be abstracted by a protocol. View models may be used for different view controllers, and my be passed between view controllers as a transfer of data.

**Example**

```swift
class MyViewController: UIViewController {
  @IBOutlet weak var titleLabel: UILabel!
  @IBOutlet weak var imageView: UIImageView!
  @IBOutlet weak var inputField: UITextField!
  
  var model: MyViewModelType!
  
  func viewDidLoad() {
    titleLabel.text = model.title
    imageView.image = model.image
  }
   
  @IBAction func textFieldValueDidChange(sender: UITextField) {
    model.inputText = inputField.text ?? ""
  }
}

protocol MyViewModelType {
  var title: String { get }
  var image: UIImage { get }
  var inputText: String { get set }
}

struct MyViewModel: MyViewModelType {
  var title: String {
    return user.name
  }
  
  var image: UIImage {
    return user.profilePicture
  }
  
  var inputText: String = "" {
    didSet {
      user.name = inputText
    }
  }
}

struct User {
  var name: String
  let profilePicture: UIImage

  init(name: String, profilePicture: UIImage) {
    self.name = name
    self.profilePicture = profilePicture
  }
}

```

## View Controllers

View controller's sole responsibility is to communicate updates to the view and receive actions from the view (IBActions, UIGestureRecognizers, etc). View controllers should not contain state for the view and should not be making any data requests (HTTP requests, database queries, etc). The view controller is the middle-man between the view and view model.

## Credits

This style guide is based on the [Ray Wenderlich Swift Style Guide](https://github.com/raywenderlich/swift-style-guide).

[Functions vs Properties](#functions-vs-properties) section borrowed from [Kotlin Coding Conventions](https://kotlinlang.org/docs/reference/coding-conventions.html#functions-vs-properties).