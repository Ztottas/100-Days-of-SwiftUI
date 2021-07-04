# Milestone 1 - Rock, paper and scissor

1. Each turn of the game the app will randomly pick either rock, paper, or scissors.
2. Each turn the app will either prompt the player to win or lose.
3. The player must then tap the correct move to win or lose the game.
4. If they are correct they score a point; otherwise they lose a point.
5. The game ends after 10 questions, at which point their score is shown.

### Solutions
```Swift

//  ContentView.swift
//  Milestone 1 - Rock, paper and Scissor
//  Created by amor on 03.07.21.

import SwiftUI

    extension View {
        func oneImage(name: String) -> some View {
            Image(name)
            .resizable()
            .renderingMode(.original)
            .frame(width:100, height: 100)
        }
    }

    struct ContentView: View {
        // Mile 1 - CHL 1 - Creation of an array of possibilities for random selection by the application
        // in this case in every level the array will be shufle...
        @State private var handImages = ["rock.hand", "paper.hand", "scissor.hand"].shuffled()
        // Mile 1 - CHL 2 (1/2) - The option for win or loose as a Boolean
        @State private var toWin = Bool.random()
        @State private var score = 0
        // Mile 1 - CHL 5 (1/3) - Add a value for the number of turns
        @State private var levelsPlayed = 0
        @State private var showAlert = false
        
    var body: some View {
        VStack {
            oneImage(name: handImages[0])
                .padding(10)
            Text("What hand will")
                .padding(10)
            Text(toWin ?  "Loose?" : "Win")
                .font(.largeTitle)
            HStack {
                // Mile 1 - CHL 3 -  A stack with two buttons for player choice.
                //(I chose to remove the tie option, as it doesn't fit with win or lose, and adding the tie option to the choice for scoring would be too easy)
                Button(action: {
                    self.getScore(image: self.handImages[1])
                }) {
                    oneImage(name: handImages[1])
                }
                Button(action: {
                    self.getScore(image: self.handImages[2])
                }) {
                    oneImage(name: handImages[2])
                }
            }
            Text("Your score is \(score)")
        }
        .padding()
        .alert(isPresented: $showAlert) {
            Alert(title: Text("Congradulations your final score is: \(score)"), message: Text(""), dismissButton:
                .default (Text("OK")) {
                    self.score = 0
                    self.levelsPlayed = 0
                    self.handImages.shuffle()
                    self.toWin = Bool.random()
                })
        }
    }
    // Mile 1 - CHL 4 - Depending on the player's choice, he loses or gains one point
    func getScore(image: String) {
        if image == "rock.hand" {
            if handImages[0] == "paper.hand" {
                toWin ? (score += 1) : (score -= 1)
            } else {
                toWin ? (score -= 1) : (score += 1)
            }
        } else if image == "paper.hand" {
            if handImages[0] == "scissor.hand" {
                toWin ? (score += 1) : (score -= 1)
            } else {
                toWin ? (score -= 1) : (score += 1)
            }
        } else if image == "scissor.hand" {
            if handImages[0] == "rock.hand" {
                toWin ? (score += 1) : (score -= 1)
            } else {
               toWin ? ( score -= 1) : (score += 1)
            }
        }
        // Mile 1 - CHL 5 (2/3) - Add one unit after the player answer one question.
        levelsPlayed += 1
        // Mile 1 - CHL 5 (3/3) - The game ends with an alert after 10 questions
        if self.levelsPlayed == 10 {
            self.showAlert = true
        }
        
        self.handImages.shuffle()
        // Mile 1 - CHL 2 (2/2) - The option for win or loose randomized
        self.toWin = Bool.random()
    }


