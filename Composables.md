# Building UI with Jetpack Compose

---

### Tools used to build UI in Compose

Jetpack Compose offers a powerful and declarative approach to building UIs for your Android apps. Unlike traditional XML layouts, Compose uses composable functions to define the UI structure. These composables can be combined to create complex and dynamic layouts. This article explores some of the fundamental layout components available in Jetpack Compose, along with their commonly used parameters:

#### Surface:

The Surface composable provides a foundation for your UI elements. It acts as a container with a material design background and can hold other composables. You can customize its appearance using the following parameters:

- modifier: A Modifier instance to apply various styling options like size, color, padding, etc.
- color: Defines the background color of the surface.
- contentColor: Sets the default content color for child composables placed within the surface.
- shape: Defines the shape of the surface using a Shape instance.
- elevation: Controls the elevation (shadow effect) of the surface.

Here's an example with some parameters applied:
```kotlin
Surface(
  modifier = Modifier.fillMaxSize(),
  color = MaterialTheme.colors.primary,
  contentColor = Color.White,
  shape = RoundedCornerShape(8.dp),
  elevation = 4.dp
) {
  // Other UI elements here
}
```

#### Box:

The Box composable stacks its child composables on top of each other. By default, they are positioned at the top-left corner. You can control their placement and appearance using the following parameters:

- modifier: A Modifier instance for styling.
- contentAlignment: Controls the horizontal and vertical alignment of child composables within the box.
- children: A lambda containing the composables to be stacked inside the box.

  Here's an example with content alignment applied:
  ```kotlin
  Box(modifier = Modifier.fillMaxSize(), contentAlignment = Alignment.Center) {
    Text("Hello", modifier = Modifier.background(Color.LightGray))
    Image(imageVector = Icons.Android, modifier = Modifier.align(Alignment.BottomEnd))
  }
  ```

#### Column:

The Column composable arranges its child composables vertically, one below the other. You can control the alignment of the children (start, center, end) and specify spacing between them using the following parameters:

- modifier: A Modifier instance for styling.
- verticalArrangement: Defines the vertical alignment of child composables (top, center, bottom, spaced).
- contentSpacing: Sets the vertical spacing between child composables.

Here's an example with vertical arrangement and spacing:
```kotlin
Column(modifier = Modifier.fillMaxWidth(), verticalArrangement = Arrangement.SpaceEvenly, contentSpacing = 16.dp) {
  Text("Title")
  Button(onClick = { /* Action */ }) {
    Text("Click Me")
  }
}
```

#### Row:

The Row composable arranges its child composables horizontally, one next to the other. Similar to Column, you can control alignment and spacing using the following parameters:

- modifier: A Modifier instance for styling.
- horizontalArrangement: Defines the horizontal alignment of child composables (start, center, end, spaced).
- contentSpacing: Sets the horizontal spacing between child composables.

Here's an example of using Row:
```kotlin
Row(modifier = Modifier.fillMaxWidth(), horizontalAlignment = Alignment.SpaceBetween, contentSpacing = 16.dp) {
  Icon(Icons.Star)
  Text("Rating: 4.5")
}
```

#### LazyColumn & LazyRow:

For very large lists, LazyColumn and LazyRow provide performant ways to render only visible items. They efficiently load and unload items as the user scrolls, improving memory usage and rendering speed. This is an replacement component for RecyclerView which was used previously in XML.

Here's an example of using LazyColumn:
```kotlin
LazyColumn {
  items(items = myList) { item ->
    Text(text = item.name)
  }
}
```

#### Scaffold:

The Scaffold composable is a pre-built layout that simplifies creating common app screen structures. It provides slots for placing a top app bar (TopAppBar), a bottom app bar (BottomAppBar), floating action button (FloatingActionButton), and the main content area.

Here's an example of using Scaffold:
```kotlin
Scaffold(
  topBar = { TopAppBar(title = "My App") },
  content = { Column(modifier = Modifier.fillMaxSize()) {
      Text("This is the content area")
    }
  }
)
```

#### Icon:

The Icon composable displays an icon from a vector drawable resource or a content descriptor. You can customize its size, tint color, and content using various parameters.

Here's an example of using Icon:
```kotlin
Icon(Icons.Star, tint = Color.Yellow)
```

#### TextField:

The TextField composable is used to create single-line text input fields. It allows users to enter and edit text. You can customize its appearance, behavior, and validation using various parameters like value, onValueChange, label, and modifier.

Here's an example of using TextField:
```kotlin
var text = ""

TextField(value = text, onValueChange = { text = it }, label = { Text("Enter Name") })
```

#### Button:

The Button composable represents a clickable button that triggers an action when pressed. You can customize its appearance (text, color, shape) and behavior (click listener) using various parameters like onClick, text, colors, and modifier.

Here's an example of a basic button:
```kotlin
Button(onClick = { /* Action */ }) {
  Text("Click Me")
}
```

#### ImageButton:

The ImageButton composable combines an Image and a Button composable. It displays an image and triggers an action when pressed. You can customize the image using parameters like imageVector or contentDescription and control button behavior using parameters like onClick and modifier.

Here's an example of an ImageButton:
```kotlin
ImageButton(onClick = { /* Action */ }) {
  Icon(Icons.Star, tint = Color.Yellow)
}
```

These are just a few of the layout components available in Jetpack Compose. By combining these building blocks and utilizing modifiers, you can create a wide variety of UIs for your Android applications.

### Additional Notes:

- You can nest these layout composables to create more complex structures.
- Explore the official Jetpack Compose documentation for a comprehensive list of composables and their parameters: [https://developer.android.com/develop/ui/compose/layouts](https://developer.android.com/develop/ui/compose/layouts)
-  Refer to the following playlists on Youtube for further details:
-  [https://www.youtube.com/watch?v=cDabx3SjuOY&list=PLQkwcJG4YTCSpJ2NLhDTHhi6XBNfk9WiC](https://www.youtube.com/playlist?list=PLSrm9z4zp4mEWwyiuYgVMWcDFdsebhM-r)
-  [https://www.youtube.com/playlist?list=PLQkwcJG4YTCSpJ2NLhDTHhi6XBNfk9WiC](https://www.youtube.com/playlist?list=PLQkwcJG4YTCSpJ2NLhDTHhi6XBNfk9WiC)

### Important Questions on Jetpack Compose Layouts with Answers:

Here are some important questions to consider when working with Jetpack Compose layouts, along with some guidance:

#### Layout Selection:


**Question:** Which layout composable (Surface, Box, Column, Row, LazyColumn, LazyRow) is best suited for achieving the desired UI structure?
**Answer:** The choice depends on your UI requirements:
- Surface: Use it as the foundation for most UI elements, providing background and styling options.
- Box: Useful for stacking elements on top of each other with precise control over their positioning.
- Column: Ideal for vertically stacking elements like text views, buttons, etc.
- Row: Use it to arrange elements horizontally, one next to the other.
- LazyColumn/Row: For very large lists, these efficiently render only visible items, improving performance.

**Question:** How can I nest these layout composables effectively to create complex layouts?
**Answer:**  Think of layouts like building blocks. You can nest them within each other to achieve the desired structure. For example, a Column can contain multiple Rows for complex arrangements.


#### Customization:

**Question:** How can I customize the appearance of layouts using modifiers (size, color, padding, etc.)?
**Answer:**  Most layout composables accept a modifier parameter. This allows applying various Modifier instances to control size, color, padding, margin, background, etc.

**Question:** How can I control the alignment and spacing of child elements within a layout?
**Answer:** Specific layout composables offer parameters for alignment and spacing:
- Column: Use verticalArrangement (top, center, bottom, spaced) and contentSpacing for vertical alignment and spacing between child elements.
- Row: Similar to Column, use horizontalArrangement and contentSpacing for horizontal control.
- Box: You can use contentAlignment to control the horizontal and vertical alignment of child elements within the box.

#### Performance:

**Question:** When should I use LazyColumn or LazyRow for lists to optimize performance?
**Answer:**  Always use LazyColumn or LazyRow for very large lists. They render only visible items on the screen, improving performance and memory usage compared to rendering the entire list at once.

**Question:** How can I ensure efficient rendering of my layouts, especially for complex UIs?
**Answer:** Pay attention to the nesting of layouts and avoid unnecessary complexity. Profile your UI to identify performance bottlenecks. Consider using techniques like Modifier.lazy for certain modifiers to optimize rendering.


#### Material Design:

**Question:** How can I leverage pre-built Material Design layouts (Scaffold, BottomAppBar) to simplify common UI patterns?
**Answer:** These pre-built layouts provide a foundation for common app screen structures. Use Scaffold to easily add a top app bar, bottom app bar, floating action button, and content area.

**Question:** How can I customize the look and feel of these pre-built layouts to match my app's theme?
**Answer:**  Pre-built layouts often accept parameters for customization. For example, Scaffold allows specifying the content of the app bar or bottom bar. You can also use modifiers on these layouts for further styling.


#### Accessibility:

**Question:** How can I ensure my layouts are accessible to users with disabilities?
**Answer:** Follow accessibility guidelines when designing layouts. Ensure proper content hierarchy, use semantic elements (like headings, buttons), and consider factors like color contrast and text size.

**Question:** Are there specific considerations for layout structure and content placement for accessibility?
**Answer:** Yes. Use clear and logical structure for screen readers to navigate. Place labels near their corresponding input fields. Ensure sufficient color contrast for text elements.


#### Advanced Layouts:

**Question:** How can I create custom layouts using composables to achieve unique UI requirements?
**Answer:** Jetpack Compose allows creating custom composables. You can combine existing composables and logic to achieve unique UI layouts that meet specific needs.

**Question:** How can I handle complex layout logic and interactions within composables?
**Answer:** For complex layouts, consider using state management solutions like Jetpack Compose State or other libraries to manage data and update the UI based on user.
