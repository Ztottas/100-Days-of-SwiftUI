# GuessTheFlag: Wrap up challenges


1. Add an @State property to store the user’s score, modify it when they get an answer right or wrong, then display it in the alert.
2. Show the player’s current score in a label directly below the flags.
3. When someone chooses the wrong flag, tell them their mistake in your alert message – something like “Wrong! That’s the flag of France,” for example.

## Solutions
```Swift
//  ContentView.swift
//  Project 02 - GuessTheFlag
//  Created by amor on 28.06.21.

import SwiftUI

struct ContentView: View {
    @State private var countries = ["Estonia", "France", "Germany", "Ireland", "Italy", "Nigeria", "Poland", "Russia", "Spain", "UK", "US"].shuffled()
    @State private var correctAnswer = Int.random(in: 0...2)
    
    @State private var showingScore = false
    @State private var scoreTitle = ""
    
    //.CHALLENGE 1 (1/4) - create a @State property for player score
    @State private var score = 0
    
    //.CHALLENGE 3 (1/3) - Extra @State property for extra message if player choose wrong flag
    @State private var wrongFlagMessage = ""
    
    
    var body: some View {
        ZStack {
            LinearGradient(gradient: Gradient(colors: [Color.black, Color.blue]), startPoint: .top, endPoint: .bottom)
                .edgesIgnoringSafeArea(.all)
                
            VStack(spacing: 30) {
                VStack {
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
                        Image(countries[number])
                            //"render the original image pixels rather than trying to recolor them as a button"
                            .renderingMode(.original)
                            //crop images with capsule format
                            .clipShape(Capsule())
                            //create overlay in: Capsule()format + stroke(color, width)
                            .overlay(Capsule().stroke(Color.black, lineWidth: 1))
                            .shadow(color: .white, radius: 1)
                    }
                }
                
                //.CHALLENGE 2 - showing players score below the flags
                Text("Your current score is: \(score)")
                    .foregroundColor(.white)
                    
                Spacer()
            }
        }
        .alert(isPresented: $showingScore, content: {
            //.CHALLENGE 1 (4/4) - modify alert message to present players current score
            //.CHALLENGE 3 (2/3) - modify alert message to present extra message if player choose wrong flag
            Alert(title: Text(scoreTitle), message: Text("\(wrongFlagMessage)Your score is \(score)"), dismissButton: .default(Text("Continue")) {
                askQuestion()
            })
        })
    }
    
    func flagTapped(_ number: Int) {
        if number == correctAnswer {
            scoreTitle = "Correct"
            //.CHALLENGE 1 (2/4) - modify score when answer is correct
            score += 1
            //.CHALLENGE 3 (3/4) - modyfy "wrongFlagMessage" to be empty
            wrongFlagMessage = ""
        } else {
            scoreTitle = "Wrong"
            //.CHALLENGE 1 (3/4) - modify score when answer is wrong
            score -= 1
            //.CHALLENGE 3 (4/4) - modyfy "wrongFlagMessage" to have a message instead of being empty
            wrongFlagMessage = "That is the flag of \(countries[number]),\n"
        }
        showingScore = true
    }
    
    func askQuestion() {
        countries.shuffle()
        correctAnswer = Int.random(in: 0 ..< 3)
    }
}
