# Day 20 / [100 Days of SwiftUI](https://www.hackingwithswift.com/100/swiftui)

## Stacks

<img src="https://user-images.githubusercontent.com/86367196/123831617-cdc0e700-d904-11eb-8397-3173f717b890.png" width="760" object-fit="cover">

#### VStack

```Swift
struct ContentView: View {
    var body: some View {
        //Vertical Stack of views
        //with a optional alignment and spacing
        VStack(alignment: .leading, spacing: 20) {
            Text("Hello, world!")
            Text("This is another text view")
        }
    }
}
```

#### HStack

```Swift
struct ContentView: View {
    var body: some View {
        //Horizontal Stack of views
	//with a optional spacing
        HStack(spacing: 20) {
            Text("Hello, world!")
            Text("This is another text view")
        }
    }
}
```

#### Spacer()

```Swift
struct ContentView: View {
    var body: some View {
        //Vertical Stack of views
        //with a optional spacing
        VStack(spacing: 20) {
            Text("First")
            Text("Second")
            //Spacer view that takes the max of the available space
	    //“more than one spacer will divide the available space…”
            Spacer()
            Text("Third")
        }
    }
}
```

#### ZStack

```Swift
struct ContentView: View {
    var body: some View {
        //Horizontal Stack of views
        //with option alignment to top
        ZStack(alignment: .top) {
            //first and most far to the background view with extra properties for differentiation..
            Text("Hello, world!").italic().bold().font(.title).foregroundColor(.blue)
            Text("This is inside a ZStack")
        }
    }
}
```
<br/><br/>
## Grid Challenge

<img src="https://user-images.githubusercontent.com/86367196/123831728-e7fac500-d904-11eb-93c6-5687251f4544.png" width="190" object-fit="cover">


```Swift
import SwiftUI

struct ContentView: View {
    var body: some View {
        VStack(spacing: 20) {
            HStack(spacing: 35) {
                Text("1").bold()
                Text("2").font(.title)
                Text("3").foregroundColor(.red)
            }
            HStack(spacing: 35) {
                Text("4").italic().font(.footnote)
                Text("5").bold().foregroundColor(.green)
                Text("6").font(.title2).underline()
            }
            HStack(spacing: 35) {
                Text("7").underline(true, color: .red)
                Text("8").fontWeight(.black)
                Text("9").foregroundColor(.blue).strikethrough(true, color: .gray).font(.largeTitle).fontWeight(.black)
            }
        }
    }
}
```

<br/><br/>
## Color and frames

<img src="https://user-images.githubusercontent.com/86367196/123831813-fe088580-d904-11eb-88c5-e02f22815c4b.png" width="190" object-fit="cover">

```Swift
import SwiftUI

struct ContentView: View {
    var body: some View {
        ZStack{
            //background over the safe area -> .edgesIgnoringSafeArea(.all)
            Color(red: 01, green: 0.8, blue: 0.3).edgesIgnoringSafeArea(.all)
            //background respecting safe area
            Color(red: 0.6, green: 0.1, blue: 0.3).opacity(0.7)
            //provide a frame as background
            Color.white.frame(width: 200, height: 100, alignment: .center)
            //Normal text view
            Text("Text")
                .foregroundColor(.yellow)
                .strikethrough(true, color: .red.opacity(0.5))
                .font(.largeTitle)
                .fontWeight(.black)
                //background for the text view
                .background(Color.gray)
        }
    }
}
```

<br/><br/>
## Gradients

<img src="https://user-images.githubusercontent.com/86367196/123831862-0a8cde00-d905-11eb-8e44-5e6bc90cb704.png" width="570" object-fit="cover">

#### LinearGradient

```Swift
import SwiftUI

struct ContentView: View {
    var body: some View {
        LinearGradient(gradient: Gradient(colors: [.red, .green]), startPoint: .top, endPoint: .bottom
	.edgesIgnoringSafeArea(.all)
    }
}
```

#### RadialGradient

```Swift
import SwiftUI

struct ContentView: View {
    var body: some View {
        RadialGradient(gradient: Gradient(colors: [.blue, .black]), center: .center, startRadius: 0, endRadius: 200)
	.edgesIgnoringSafeArea(.all)
    }
}
```

#### AngularGradient

```Swift
import SwiftUI

struct ContentView: View {
    var body: some View {
        AngularGradient(gradient: Gradient(colors: [.red, .yellow, .green, .blue, .purple, .red]), center: .center)
	.edgesIgnoringSafeArea(.all)
    }
}
```

<br/><br/>
## Buttons

<img src="https://user-images.githubusercontent.com/86367196/123831917-19739080-d905-11eb-82f7-d53de42a9398.png" width="380" object-fit="cover">

#### Simple text button

```Swift
import SwiftUI

struct ContentView: View {
    var body: some View {
	//Version for a simple text button with the action
        Button("Tap me!") {
            print("Button tapped.")
        }
    }
}
```

#### Button with Image

```Swift
import SwiftUI

struct ContentView: View {
    var body: some View {
        //Version for a button containing an image
        Button(action: {
            print("Button tapped.")
        }) {
            HStack{
                Image(systemName: "heart.fill")
                    .foregroundColor(.red)
                Text("Tap me!")
            }
        }
    }
}
```
<br/><br/>

## Alerts

<img src="https://user-images.githubusercontent.com/86367196/123831976-285a4300-d905-11eb-883c-b3fcf676c055.png" width="380" object-fit="cover">

```Swift
import SwiftUI

struct ContentView: View {
    //creating a State for the alert
    @State private var showingAlert = false
    
    var body: some View {
        //Create the button
        Button(action: {
            //action will be make the State for the alert true
            showingAlert = true
        }) {
            HStack {
                Image(systemName: "heart.fill")
                    .foregroundColor(.red)
                Text("Tap me")
            }
        }
        //alert "Observer?". With two-way binding State -> dismiss puts the State as false
        .alert(isPresented: $showingAlert) {
            Alert(title: Text("Hello SwiftUI"), message: Text("This is some detail message"), dismissButton: .default(Text("OK")))
        }
    }
}
```
<br/><br/>

# Day 23 / [100 Days of SwiftUI](https://www.hackingwithswift.com/100/swiftui)

## Behind the main SwiftUI view

<img src="https://user-images.githubusercontent.com/86367196/123932689-f642f280-d991-11eb-8811-005654ee1489.png" width="570" object-fit="cover">

#### Image 1 (left-most image)
```Swift
import SwiftUI

struct ContentView: View {
    var body: some View {
        Text("Hello, world!")
            //create a modifier to change the background colour of our view
            .background(Color.red)
    }
}
```
#### Image 2
```Swift
import SwiftUI

struct ContentView: View {
    var body: some View {
        Text("Hello, world!")
	    // Behind our view we have NOTHING we can change (the white screen is not a view)
	    // We can spread our view to occupy the white space, like this:
            .frame(maxWidth: .infinity, maxHeight: .infinity)
            .background(Color.red)
            .edgesIgnoringSafeArea(.all)
    }
}
```
#### Image 3
```Swift
import SwiftUI

struct ContentView: View {
    var body: some View {
	// Trying it out I realise there is space between views that I cannot fill, I will certainly find a logical solution in a later class
        ZStack() {
            Text("XXXXX")
                .frame(maxWidth: .infinity, maxHeight: .infinity)
                .background(Color.yellow)
            VStack() {
                Text("Hello, world!")
                    .frame(maxWidth: .infinity, maxHeight: .infinity)
                    .background(Color.red)
                    .edgesIgnoringSafeArea(.all)
                Text("Another Hello, world!")
                    .frame(maxWidth: .infinity, maxHeight: .infinity)
                    .background(Color.gray)
                    .edgesIgnoringSafeArea(.all)
            }
        }
    }
}
```
<br/><br/>
## The modifier order matters!

<img src="https://user-images.githubusercontent.com/86367196/123939343-576dc480-d998-11eb-8c3a-a765e9e6f695.png" width="570" object-fit="cover">

#### Image 1 (left-most image)

```Swift
import SwiftUI

struct ContentView: View {
    var body: some View {
        Button("Hello World") {
            print(type(of: self.body))
            // Will print on tap:
            // ModifiedContent<ModifiedContent<Button<Text>, _BackgroundModifier<Color>>, _FrameLayout>
        }
        // First adding a background color to the view
        .background(Color.red)
        // Adding a frame after
        .frame(width: 200, height: 200)  
    }
}
```

#### Image 2
```Swift
import SwiftUI

struct ContentView: View {
    var body: some View {
        Button("Hello World") {
           //do nothing
        }
        // First adding a frame size to the view
        .frame(width: 200, height: 200)
        // Adding color after
        .background(Color.red)
    }
}
```

#### Image 3
```Swift
import SwiftUI

struct ContentView: View {
    var body: some View {
        Text("Hello World")
            .padding()
            .background(Color.red)
            .padding()
            .background(Color.blue)
            .padding()
            .background(Color.green)
            .padding()
            .background(Color.yellow)
    }
}
```
<br/><br/>
## Conditional modifiers

<img src="https://user-images.githubusercontent.com/86367196/123945770-c6e6b280-d99e-11eb-84e0-1d54e09350a0.png" width="380" object-fit="cover">

```Swift
import SwiftUI

struct ContentView: View {
    @State private var useRedText = false
    
    var body: some View {
        Button("Hello world") {
            //.toggle() to change the value of the Bolean "useRedText" on tap
            useRedText.toggle()
        }
        // adding a foreground color with a ternary operator.
        // if true: make text red / if false: will make it blue
        .foregroundColor(useRedText ? .red : .blue)
    }
}
```
<br/><br/>
## Environment modifiers

<img src="https://user-images.githubusercontent.com/86367196/123959894-f7cee380-d9ae-11eb-911b-f0cb24e33e9a.png" width="380" object-fit="cover">

#### Font() as an example of an environment modifier (left-most image)
```Swift
import SwiftUI

struct ContentView: View {
    var body: some View {
        VStack() {
            Text("Gryffindor")
                // Font() is one example of an environment modifier.
                // when the child view has font() modifier, this has priority over another font() modifier in the parent view
                .font(.largeTitle)
            Text("Hufflepuff")
            Text("Ravenclaw")
            Text("Slytherin")
        }
        .font(.title)
    }
}
```

#### Blur() as an example of a regular modifier

```Swift
import SwiftUI

struct ContentView: View {
    var body: some View {
        VStack() {
            Text("Gryffindor")
                //if it is not an environment modifier
                //the modifier in the child view will be added to the one in parent view.
                .blur(radius: 1)
            Text("Hufflepuff")
            Text("Ravenclaw")
            Text("Slytherin")
        }
        .blur(radius: 1)
    }
}
```
<br/><br/>
## Views as properties

<img src="https://user-images.githubusercontent.com/86367196/123964146-7168d080-d9b3-11eb-9b43-a92cd9304bc2.png" width="190" object-fit="cover">

#### Simple properties as views
```Swift
import SwiftUI

struct ContentView: View {
    //We can make views as properties like this
    let motto1 = Text("Draco dormiens")
    let motto2 = Text("nunquam titillandus")

    var body: some View {
        VStack() {
            //Assigning those views
            motto1
                //...and even assign modifiers 
                .foregroundColor(.red)
            motto2
                .foregroundColor(.gray)
        }
    }
}
```
#### Computed properties as views
```Swift
import SwiftUI

struct ContentView: View {
    //we can also create computed properties like this
    var motto1: some View { Text("Draco dormiens")
    	.foregroundColor(.red) 
    }
    var motto2: some View { Text("nunquam titillandus")
    	.foregroundColor(.gray)
    }

    var body: some View {
        VStack() {
            motto1
            motto2
        }
    }
}
```
<br/><br/>
## Custom Modifiers

<img src="https://user-images.githubusercontent.com/86367196/123975721-d88b8280-d9bd-11eb-93c5-6074db4e0bbd.png" width="380" object-fit="cover">

#### Creating custom modifier
```Swift
import SwiftUI

// creating a struct that conforms with ViewModifier
struct myModifier: ViewModifier {
    // we create a body function with our custom content
    func body(content: Content) -> some View {
        cont
            //this is the custom content:
            .font(.largeTitle)
            .foregroundColor(.white)
            .padding()
            .background(Color.blue)
            .clipShape(RoundedRectangle(cornerRadius: 10))
    }
}

```

#### Calling custom modifier example 1
```Swift
struct ContentView: View {
    var body: some View {
        Text("Hello world")
            //we call our custom modifier
            .modifier(myModifier())
    }
}
```

#### Calling custom modifier example 1
```Swift
// we cam make an extencion on View
extension View {
    func mytitleStyle() -> some View {
        // with our personal modifier
        modifier(myModifier())
    }
}

struct ContentView: View {
    var body: some View {
        Text("Hello world")
            //and easy call it like this
            .mytitleStyle()
    }
}
```
 
#### Creating custom modifier with a Stack View
```Swift
// because custom modifiers return new objects we can create views on Stacks inside these modifiers
struct myModifier: ViewModifier {
    var text: String
    
    // we create a body function with our custom content
    func body(content: Content) -> some View {
        ZStack(alignment: .bottomTrailing) {
            content
            Text(text)
                .font(.caption)
                .foregroundColor(.white)
                .padding(5)
                .background(Color.gray)
        }
        
    }
}
// we cam make an extencio on View like this
extension View {
    func watermark(is text: String) -> some View {
        // with our personal modifier
        modifier(myModifier(text: text))
    }
}

struct ContentView: View {
    var body: some View {
        Color.blue
            .frame(width: 200, height: 200)
            .watermark(is: "Hacking with Swift")
    }
}
```
<br/><br/>
## Custom Containers

<img src="https://user-images.githubusercontent.com/86367196/123984979-6159ec80-d9c5-11eb-9358-d155c94baa46.png" width="190" object-fit="cover">

#### Creating acustom Container 
```Swift
import SwiftUI
import SwiftUI

struct GridStack<Content: View>: View {
    let rows: Int
    let columns: Int
    let content: (Int, Int) -> Content
    
    var body: some View {
        VStack() {
            ForEach(0..<rows, id: \.self) { row in
                HStack {
                    ForEach(0..<columns, id: \.self) { column in
                        self.content(row, column)
                            .padding(5)
                    }
                }
            }
        }
    }
}

struct ContentView: View {
    var body: some View {
        GridStack(rows: 4, columns: 4) { row, col in
            HStack() {
                Image(systemName: "\(row * 4 + col).square")
                    .foregroundColor(.red)
                Text("R\(row)-C\(col)")
            }
        }
    }
}
```