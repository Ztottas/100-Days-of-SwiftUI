# Day 20 / [100 Days of SwiftUI](https://www.hackingwithswift.com/100/swiftui)

## Stacks

<img src="https://user-images.githubusercontent.com/86367196/123831617-cdc0e700-d904-11eb-8397-3173f717b890.png" width="718" object-fit="cover">

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

<img src="https://user-images.githubusercontent.com/86367196/123831728-e7fac500-d904-11eb-93c6-5687251f4544.png" width="179" object-fit="cover">


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

<img src="https://user-images.githubusercontent.com/86367196/123831813-fe088580-d904-11eb-88c5-e02f22815c4b.png" width="179" object-fit="cover">

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

<img src="https://user-images.githubusercontent.com/86367196/123831862-0a8cde00-d905-11eb-8e44-5e6bc90cb704.png" width="538" object-fit="cover">

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

<img src="https://user-images.githubusercontent.com/86367196/123831917-19739080-d905-11eb-82f7-d53de42a9398.png" width="359" object-fit="cover">

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

<img src="https://user-images.githubusercontent.com/86367196/123831976-285a4300-d905-11eb-883c-b3fcf676c055.png" width="359" object-fit="cover">

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
