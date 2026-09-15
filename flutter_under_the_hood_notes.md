# Flutter Under the Hood

## Core mental model

``` text
Dart / Flutter source code
        |
        | compile time
        v
   Compiled program
        |
        | runtime
        v
     build() runs
        |
        v
 Widget descriptions
        |
        v
    Widget Tree
        |
        v
    Element Tree
        |
        v
 RenderObjects (where applicable)
        |
        v
 Layout + Paint
        |
        v
      Pixels
```

> The widget tree is created at runtime. Compilation produces
> executable/target-specific code; it does not compile the widget tree
> itself.

## Widget description

A widget description is the configuration of one widget: what type it is
and how it should be configured.

``` dart
Text(
  "Dharanesh",
  style: TextStyle(
    fontSize: 24,
    fontWeight: FontWeight.bold,
  ),
)
```

Conceptually:

``` text
Text
├── text: "Dharanesh"
├── fontSize: 24
└── fontWeight: bold
```

It describes **what UI should exist and how it is configured**.

It does not contain the final screen coordinates or pixels.

## Widget Tree

The Widget Tree is the hierarchy of widget descriptions.

``` text
Scaffold
├── AppBar
└── Card
    └── Column
        ├── Text("Customer Name")
        ├── Text("Phone Number")
        └── ElevatedButton("Login")
```

Think:

> **Widget Tree = blueprint / desired UI structure.**

## Element and Element Tree

An **Element** is a persistent runtime object associated with a location
in the widget tree.

Conceptually:

``` text
Widget Tree                 Element Tree

Card                        CardElement
  |                            |
Column                      ColumnElement
  |                            |
+-- Text                    +-- TextElement
+-- Text                    +-- TextElement
+-- Button                  +-- ButtonElement
```

Elements maintain runtime structure such as parent-child relationships,
the current widget configuration associated with that location,
lifecycle information, and the location represented by the Element.

The Element Tree is maintained in memory while the app is running.

Do not think of an Element as the object that stores final screen
coordinates. Geometry belongs to the rendering/layout system.

Also, not every Element has its own RenderObject.

## Build

`build()` is a method that creates and returns widget descriptions.

``` dart
@override
Widget build(BuildContext context) {
  return Card(
    child: Text("Hello"),
  );
}
```

Think:

> **build = "What widgets should exist here right now?"**

It does not permanently store the widget tree or directly paint pixels.
Flutter reconciles the returned widget descriptions with the existing
Element tree.

## setState and rebuild

``` dart
setState(() {
  count++;
});
```

Simplified flow:

``` text
User event
    |
    v
setState()
    |
    v
State/associated Element marked dirty
    |
    v
build() runs
    |
    v
New widget descriptions
    |
    v
Flutter reconciles old and new
    |
    +-- match -> reuse/update existing Element
    |
    +-- no match -> remove old Element and create new one
    |
    v
Render/layout/paint work if required
    |
    v
Updated pixels
```

A normal state change does not mean the whole Element Tree is destroyed
and recreated.

## BuildContext

`BuildContext` is a type representing a location in the Element Tree.

``` dart
Widget build(BuildContext context) {
  ...
}
```

Think:

> **BuildContext = "Where am I in the Element Tree?"**

Technically, an `Element` implements `BuildContext`.

The context does not itself contain screen size. It gives code access to
a location from which framework APIs can find information associated
with that part of the tree.

Examples:

``` dart
MediaQuery.sizeOf(context)
Theme.of(context)
Navigator.of(context)
Scaffold.of(context)
```

Conceptually:

``` text
context
   |
   v
Element's location
   |
   v
find relevant inherited/ancestor information
   |
   v
MediaQuery / Theme / Navigator / etc.
```

## mounted

`mounted` is lifecycle information.

``` dart
if (!mounted) return;
```

Think:

> **mounted = "Is this State still attached to an Element?"**

This is especially important after asynchronous work:

``` dart
Future<void> loadData() async {
  await someOperation();

  if (!mounted) return;

  setState(() {
    // update UI
  });
}
```

The async operation can finish after the user has navigated away. The
State may then no longer be mounted.

`mounted` does not mean "currently visible on the screen." It means the
State is still attached to an Element.

## RenderObject

RenderObjects handle the physical layout and painting side of Flutter.

They deal with things such as:

-   constraints
-   size
-   position
-   layout
-   painting

Simplified:

``` text
Constraints
     |
     v
Calculate size
     |
     v
Determine layout/position
     |
     v
Paint
     |
     v
Pixels
```

Think:

> **RenderObject = "How should this UI occupy space and be painted?"**

Not every widget has a RenderObject.

## Constraints and Size

Constraints are rules/limits supplied during layout:

``` text
minWidth
maxWidth
minHeight
maxHeight
```

The child chooses a size that satisfies those constraints.

``` text
Parent
  |
  | constraints
  v
Child
  |
  | chooses
  v
Size
```

A useful simplified rule is:

> **Constraints go down. Sizes come back up.**

## LayoutBuilder

`LayoutBuilder` gives its builder callback access to the constraints
available at that location.

``` dart
LayoutBuilder(
  builder: (context, constraints) {
    return Text(
      "Hello",
      style: TextStyle(
        fontSize: constraints.maxWidth * 0.05,
      ),
    );
  },
)
```

Important:

> `LayoutBuilder` does not itself set the constraints. It receives
> constraints from its parent and lets you build based on them.

Compare:

``` text
MediaQuery
    |
    +-- "What is the overall app/window environment?"

LayoutBuilder
    |
    +-- "How much space is available HERE?"
```

## Opening a new dialog

A button that opens a dialog is different from a normal `setState()`
rebuild.

``` dart
ElevatedButton(
  onPressed: () {
    showDialog(
      context: context,
      builder: (context) {
        return AlertDialog(
          title: Text("Login"),
        );
      },
    );
  },
  child: Text("Login"),
)
```

`showDialog()` asks the Navigator to show a dialog route. The dialog
gets its own widget subtree and corresponding elements while the
existing UI remains underneath.

Conceptually:

``` text
                 Navigator
                 /                        /                  Main Route       Dialog Route
            |                 |
      Customer Card       AlertDialog
            |                 |
       Login Button       TextField
                          Buttons
```

So:

> **build describes widgets; `showDialog()`/Navigator introduces the new
> route/subtree.**

## The four key ideas

``` text
build()
   |
   +-- WHAT UI should exist?
   |
   +-- returns widget descriptions


BuildContext
   |
   +-- WHERE am I?
   |
   +-- handle to an Element's location


mounted
   |
   +-- AM I STILL ATTACHED?
   |
   +-- lifecycle status


RenderObject
   |
   +-- HOW DO I DISPLAY IT?
   |
   +-- layout + painting
```

## Final memory table

  Concept              Simple meaning
  -------------------- -----------------------------------------------------------
  Widget description   Configuration describing one piece of UI
  Widget Tree          Hierarchy of widget descriptions
  Element              Persistent runtime node/location associated with a widget
  Element Tree         Persistent runtime structure Flutter manages
  build                Method that creates/returns widget descriptions
  BuildContext         Handle representing an Element's location
  mounted              Whether the State is still attached to an Element
  RenderObject         Handles layout and painting
  Constraints          Limits supplied during layout
  Size                 Size chosen within those limits
  LayoutBuilder        Lets build code respond to local layout constraints

## Memory trick

**Build = WHAT**

**Context = WHERE**

**Mounted = ALIVE?**

**Element = PERSISTENT NODE**

**RenderObject = HOW TO DISPLAY**

**Widget = DESCRIPTION**

**Widget Tree = BLUEPRINT**

**Element Tree = RUNTIME STRUCTURE**

**Render Tree = LAYOUT + PAINT**
