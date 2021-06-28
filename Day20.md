# Stacks

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
  
