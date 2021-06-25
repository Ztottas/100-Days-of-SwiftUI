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
    //.CHALLENGE 2 (1/2) - proj. 1 - Calculate total amount of the check
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
                    //.CHALLENGE 3 - proj. 1 - Change the "Number of people" picker to be a text field.
                    //Picker("Number of people", selection: $numberOfPeople) {
                    //    ForEach(2 ..< 100) {
                    //        Text("\($0) people")
                    //    }
                    //}
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
                //.CHALLENGE 2 (2/2) - proj. 1 - Add another section showing total amount of the check
                Section(header: Text("Total amount for the check").textCase(nil)) {
                    Text("$\(FullAmount, specifier: "%.2f")")
                }
                //.CHALLENGE 1 - proj. 1 - Add header to the third section
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
