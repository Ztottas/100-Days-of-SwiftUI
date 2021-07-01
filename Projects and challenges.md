# Project 1 - WeSplit 
### Wrap up challenges

1. Add a header to the third section, saying “Amount per person”
2. Add another section showing the total amount for the check – i.e., the original amount plus tip value, without dividing by the number of people.
3. Change the “Number of people” picker to be a text field, making sure to use the correct keyboard type.

### Solutions
```Swift
//  ContentView.swift
//  project 1 - weSplit
//  Created by amor on 24.06.21

import SwiftUI

struct ContentView: View {
    @State private var checkAmount = ""
    @State private var numberOfPeople =  ""
    @State private var tipPercentage = 2
    
    let tipPercentages = [0, 5, 10, 15, 20]
    
    var totalPerPerson: Double {
        let orderAmount = Double(checkAmount) ?? 0
        let persons = Double(numberOfPeople) ?? 0
        let tipSelected = Double(tipPercentages[tipPercentage])
        
        let tipValue  = orderAmount / 100 * tipSelected
        let grandTotal = orderAmount + tipValue
        let amountPerPerson = grandTotal / persons
        
        return amountPerPerson
    }
    // PROJ 1 - CHL 2 (1/2) - Calculate total amount of the check
    var FullAmount: Double {
        let orderAmount = Double(checkAmount) ?? 0
        let tipSelected = Double(tipPercentages[tipPercentage])
        let tipValue  = orderAmount / 100 * tipSelected
        let billPlusTip = orderAmount + tipValue
        return billPlusTip
    }
    
    var body: some View {
        NavigationView {
            Form {
                
                Section(header: Text("Bill").textCase(nil)) {
                    TextField("Amount", text: $checkAmount)
                        .keyboardType(.decimalPad)
                    // PROJ 1 - CHL 3 - Change the "Number of people" picker to be a text field.
//                    Picker("Number of people", selection: $numberOfPeople) {
//                        ForEach(2 ..< 100) {
//                            Text("\($0) people")
//                        }
//                    }
                    TextField("Number of people", text: $numberOfPeople)
                        .keyboardType(.decimalPad)
                }
                
                Section(header: Text("Tip percentage").textCase(nil)) {
                    Picker("Tip percentage", selection: $tipPercentage) {
                        ForEach(0 ..< tipPercentages.count) {
                            Text("\(tipPercentages[$0])%")
                        }
                    }
                    .pickerStyle(SegmentedPickerStyle())
                }
                // PROJ 1 - CHL 2 (2/2) - Add another section showing total amount of the check
                Section(header: Text("Total amount for the check").textCase(nil)) {
                    Text("$\(FullAmount, specifier: "%.2f")")
                        // PROJ 3 - CHL 2 - conditional modifier, to change text view to red if user selects a 0% tip.
                        .foregroundColor(tipPercentage == 0 ? Color.red : Color.primary)
                }
                
                // PROJ 1 - CHL 1 - Add header to the third section
                //.TextCase(nil) - was the solution I found to remove upercased headers!
                Section(header: Text("Total per Person").textCase(nil)) {
                    //value.isNaN ? 0.0 : value - was the solution I found to resolve the abcence of value
                    //called "Conditional modifiers"
                    Text("$\(totalPerPerson.isNaN ? 0.0 : totalPerPerson, specifier: "%.2f")")
                }
            }
            .navigationTitle("weSplit")
        }
    }
}
```
<br/><br/>

# Project 2 - GuessTheFlag
### Wrap up challenges

1. Add an @State property to store the user’s score, modify it when they get an answer right or wrong, then display it in the alert.
2. Show the player’s current score in a label directly below the flags.
3. When someone chooses the wrong flag, tell them their mistake in your alert message – something like “Wrong! That’s the flag of France,” for example.

### Solutions
```Swift
//  ContentView.swift
//  Project 02 - GuessTheFlag
//  Created by amor on 28.06.21.

import SwiftUI

//PROJ 3 - CHL 3 (1/2) - create a FlagImage() view that renders one flag image. (Not sure what I done is what was asked...)
extension View {
    func FlagImage(name: String) -> some View {
        Image(name)
            //"render the original image pixels rather than trying to recolor them as a button"
            .renderingMode(.original)
            //crop images with capsule format
            .clipShape(Capsule())
            //create overlay in: Capsule()format + stroke(color, width)
            .overlay(Capsule().stroke(Color.black, lineWidth: 1))
            .shadow(color: .white, radius: 1)
    }
}

struct ContentView: View {
    @State private var countries = ["Estonia", "France", "Germany", "Ireland", "Italy", "Nigeria", "Poland", "Russia", "Spain", "UK", "US"].shuffled()
    @State private var correctAnswer = Int.random(in: 0...2)
    
    @State private var showingScore = false
    @State private var scoreTitle = ""
    
    // PROJ 2 - CHL 1 (1/4) - create a @State property for player score
    @State private var score = 0
    
    // PROJ 2 - CHL 3 (1/4) - Extra @State property for extra message if player choose wrong flag
    @State private var wrongFlagMessage = ""
    
    
    var body: some View {
        ZStack {
            LinearGradient(gradient: Gradient(colors: [Color.black, Color.blue]), startPoint: .top, endPoint: .bottom)
                .edgesIgnoringSafeArea(.all)
            VStack(spacing: 30) {
                VStack() {
                    Text("Tap the flag of")
                        .foregroundColor(.white)
                    Text(countries[correctAnswer])
                        .foregroundColor(.white)
                        .font(.largeTitle)
                        .fontWeight(.black)
                }
            
                ForEach(0 ..< 3) { number in
                    Button(action: {
                        flagTapped(number)
                    }) {
                        // PROJ 3 - CHL 3 (2/2) - call FlagImage()
                        FlagImage(name: countries[number])
                    }
                }
                // PROJ 2 - CHL 2 - showing players score below the flags
                Text("Your current score is: \(score)")
                    .foregroundColor(.white)
                Spacer()
            }
        }
        .alert(isPresented: $showingScore, content: {
            // PROJ 2 - CHL 1 (2/4) - modify alert message to present players current score
            // PROJ 2 - CHL 3 (2/4) - modify alert message to present extra message if player choose wrong flag
            Alert(title: Text(scoreTitle), message: Text("\(wrongFlagMessage)Your score is \(score)"), dismissButton: .default(Text("Continue")) {
                askQuestion()
            })
        })
    }
    
    func flagTapped(_ number: Int) {
        if number == correctAnswer {
            scoreTitle = "Correct"
            // PROJ 2 - CHL 1 (3/4) - modify score when answer is correct
            score += 1
            // PROJ 2 - CHL 3 (3/4) - modyfy "wrongFlagMessage" to be empty
            wrongFlagMessage = ""
        } else {
            scoreTitle = "Wrong"
            // PROJ 2 - CHL 1 (4/4) - modify score when answer is wrong
            score -= 1
            // PROJ 2 - CHL 3 (4/4) - modyfy "wrongFlagMessage" to have a message instead of being empty
            wrongFlagMessage = "That is the flag of \(countries[number]),\n"
        }
        showingScore = true
    }
    
    func askQuestion() {
        countries.shuffle()
        correctAnswer = Int.random(in: 0 ..< 3)
    }
}
```
<br/><br/>
# Project 3 - first technical project 
### Wrap up challenges

1. Create a custom ViewModifier (and accompanying View extension) that makes a view have a large, blue font suitable for prominent titles in a view.
2. Go back to project 1 and use a conditional modifier to change the total amount text view to red if the user selects a 0% tip.
3. Go back to project 2 and create a FlagImage() view that renders one flag image using the specific set of modifiers we had.

#### Solutions to challenge 1

<img src="https://user-images.githubusercontent.com/86367196/124162248-99812e00-da9e-11eb-8066-f24dba813ca4.png" width="180" object-fit="cover">

```Swift
import SwiftUI
//Create the custom modifier
struct largeTitleModifier: ViewModifier {
    func body(content: Content) -> some View {
        content
            //...with large fonte and blue font color
            .font(.largeTitle)
            .foregroundColor(Color.blue)
    }
}
//create an extencion for my "largeTitleModifier".
extension View {
    func myLargeTitle() -> some View {
        modifier(largeTitleModifier())
    }
}

struct ContentView: View {
    var body: some View {
        Text("Hello, world!")
            //implement my modifier extension
            .myLargeTitle()
    }
}
```
<br/>

#### Solutions to challenge 2

<img src="https://user-images.githubusercontent.com/86367196/124162381-aaca3a80-da9e-11eb-96a5-2eaa9a1fc751.png" width="180" object-fit="cover">

```Swift
Section(header: Text("Total amount for the check").textCase(nil)) {
    Text("$\(FullAmount, specifier: "%.2f")")
        // PROJ 3 - CHL 2 - conditional modifier, to change text view to red if user selects a 0% tip.
        .foregroundColor(tipPercentage == 0 ? Color.red : Color.primary)
}
```
<br/>

#### Solutions to challenge 3

```Swift
//PROJ 3 - CHL 3 (1/2) - create a FlagImage() view that renders one flag image. (Not sure what I done is what was asked...)
extension View {
    func FlagImage(name: String) -> some View {
        Image(name)
            //"render the original image pixels rather than trying to recolor them as a button"
            .renderingMode(.original)
            //crop images with capsule format
            .clipShape(Capsule())
            //create overlay in: Capsule()format + stroke(color, width)
            .overlay(Capsule().stroke(Color.black, lineWidth: 1))
            .shadow(color: .white, radius: 1)
    }
}
...
ForEach(0 ..< 3) { number in
    Button(action: {
        flagTapped(number)
    }) {
        // PROJ 3 - CHL 3 (2/2) - call FlagImage()
        FlagImage(name: countries[number])
    }
}
                    
