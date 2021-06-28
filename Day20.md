# Day 20
## Stacks

<img src="https://user-images.githubusercontent.com/86367196/123614780-eea21300-d804-11eb-968b-76482990e3c2.jpg" height="500">

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

## Grid Challenge

<img src="https://user-images.githubusercontent.com/86367196/123612537-eea11380-d802-11eb-95e7-35cd5f8ad393.png" height="500">


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

<br/>

## Color and frames

<img src="https://user-images.githubusercontent.com/86367196/123636314-fff71980-d81c-11eb-905d-ee8395b87dc5.png" height="500">

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

<br/>

## Gradients

<img src="https://user-images.githubusercontent.com/86367196/123637909-d50dc500-d81e-11eb-9d3f-cabd17bce44a.jpg" height="500">


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
