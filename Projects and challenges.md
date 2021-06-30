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
```
<br/><br/>
# Challenge 1 - weConvert
### Day 19 Challenge

build an app that handles unit conversions: users will select an input unit and an output unit, then enter a value, and see the output of the conversion.

### Solutions
```Swift
//  ContentView.swift
//  Challenge day 19 - weConvert
//  Created by amor on 25.06.21.

import SwiftUI

struct ContentView: View {
    //user Value
    @State private var valueForConversion = ""
    //user choices in pickers
    @State private var unit = 0
    @State private var convertFromValue = 0
    @State private var convertToValue = 1
    @State private var fromLengthSystem = 0
    @State private var toLengthSystem = 0
    @State private var fromVolumeSystem = 0
    @State private var toVolumeSystem = 0
    //pickers arrays
    var units = ["Temp.", "Length", "Time", "Volune"]
    var lengthSystem = ["Metric", "US/Imperial", "Nautical"]
    var volumeSystem = ["Metric", "US customary", "Imperial"]
    var tempUnits = ["Celsius", "Fahrenheit", "Kelvin"]
    var lengthMetricUnits = ["mm", "cm", "meter", "km"]
    var lengthImperialUnits = ["inch", "foot", "yard", "mile"]
    var timeUnits = ["sec.", "min.", "hour", "day", "week", "year"]
    var volumeMetricUnits = ["milliliter", "centiliter", "deciliter", "liter"]
    var volumeImperialUnits = ["once", "cup", "pint", "quart", "gallon"]
    
    //convert user value to generic unit:
    //  Temperature in Celsius
    //  Length in meters
    //  Time in seconds
    //  Volune in Mililiter
    var userValueToGeneric: Double {
        let userValue = Double(valueForConversion) ?? 0.0
        var generic: Double = 0
        // formulas
        let fahrenheitToCelsius = (userValue - 32) / 1.8
        let kevinToCelsius = userValue - 237.15
        let mmToMeters = userValue / 1000
        let cmToMeters = userValue / 100
        let kmToMeters = userValue * 10
        let inchToMeter = userValue / 39.37
        let footToMeter = userValue / 3.281
        let yardToMeter = userValue / 1.094
        let mileToMeter = userValue * 1609
        let nauticToMeter = userValue  * 1852
        let minToSec = userValue * 60
        let hourToSec = userValue * 60 * 60
        let dayToSec = userValue * 60 * 60 * 24
        let weekToSec = userValue * 60 * 60 * 24 * 7
        let yearToSec = userValue * 60 * 60 * 24 * 7 * 365
        let centiliterToMililiter = userValue * 10
        let deciliterToMililiter = userValue * 100
        let literToMililiter = userValue  * 1000
        let usOnceToMililiter = userValue * 29.574
        let usCupToMililiter = userValue * 236.588
        let usPintToMililiter = userValue * 473.176
        let usQuartToMililiter = userValue * 946.353
        let usGallonToMililiter = userValue * 3785.41
        let ukOnceToMililiter = userValue * 28.413
        let ukCupToMililiter = userValue * 284.13
        let ukPintToMililiter = userValue * 568.262
        let ukQuartToMililiter = userValue * 1136.52
        let ukGallonToMililiter = userValue * 4546.09
        // return the value  according to the user's pickers choices
        if unit == 0 && convertFromValue == 0 { generic = userValue }
        if unit == 0 && convertFromValue == 1 { generic = fahrenheitToCelsius }
        if unit == 0 && convertFromValue == 2 { generic = kevinToCelsius }
        if unit == 1 && fromLengthSystem == 0 && convertFromValue == 0 { generic = mmToMeters }
        if unit == 1 && fromLengthSystem == 0 && convertFromValue == 1 { generic = cmToMeters }
        if unit == 1 && fromLengthSystem == 0 && convertFromValue == 2 { generic = userValue }
        if unit == 1 && fromLengthSystem == 0 && convertFromValue == 3 { generic = kmToMeters }
        if unit == 1 && fromLengthSystem == 1 && convertFromValue == 0 { generic = inchToMeter }
        if unit == 1 && fromLengthSystem == 1 && convertFromValue == 1 { generic = footToMeter }
        if unit == 1 && fromLengthSystem == 1 && convertFromValue == 2 { generic = yardToMeter}
        if unit == 1 && fromLengthSystem == 1 && convertFromValue == 3 { generic = mileToMeter}
        if unit == 1 && fromLengthSystem == 2 { generic = nauticToMeter }
        if unit == 2 && convertFromValue == 0 { generic = userValue }
        if unit == 2 && convertFromValue == 1 { generic = minToSec }
        if unit == 2 && convertFromValue == 2 { generic = hourToSec }
        if unit == 2 && convertFromValue == 3 { generic = dayToSec }
        if unit == 2 && convertFromValue == 4 { generic = weekToSec }
        if unit == 2 && convertFromValue == 5 { generic = yearToSec }
        if unit == 3 && fromVolumeSystem == 0 && convertFromValue == 0 { generic = userValue }
        if unit == 3 && fromVolumeSystem == 0 && convertFromValue == 1 { generic = centiliterToMililiter }
        if unit == 3 && fromVolumeSystem == 0 && convertFromValue == 2 { generic = deciliterToMililiter }
        if unit == 3 && fromVolumeSystem == 0 && convertFromValue == 3 { generic = literToMililiter }
        if unit == 3 && fromVolumeSystem == 1 && convertFromValue == 0 { generic = usOnceToMililiter }
        if unit == 3 && fromVolumeSystem == 1 && convertFromValue == 1 { generic = usCupToMililiter }
        if unit == 3 && fromVolumeSystem == 1 && convertFromValue == 2 { generic = usPintToMililiter }
        if unit == 3 && fromVolumeSystem == 1 && convertFromValue == 3 { generic = usQuartToMililiter }
        if unit == 3 && fromVolumeSystem == 1 && convertFromValue == 4 { generic = usGallonToMililiter }
        if unit == 3 && fromVolumeSystem == 2 && convertFromValue == 0 { generic = ukOnceToMililiter }
        if unit == 3 && fromVolumeSystem == 2 && convertFromValue == 1 { generic = ukCupToMililiter }
        if unit == 3 && fromVolumeSystem == 2 && convertFromValue == 2 { generic = ukPintToMililiter }
        if unit == 3 && fromVolumeSystem == 2 && convertFromValue == 3 { generic = ukQuartToMililiter }
        if unit == 3 && fromVolumeSystem == 2 && convertFromValue == 4 { generic = ukGallonToMililiter }
        
        return generic
    }
    //convert generic value to the desired format
    var genericToDesireValue: String {
        let userValue = userValueToGeneric
        var value: Double = 0
        // formulas
        let celsiusToFahrenheit = (userValue * 5/9) + 32
        let celsiusTokevin = userValue + 237.15
        let meterToMillimeter = userValue * 1000
        let meterToCentimeter = userValue * 100
        let meterToKilometer = userValue / 10
        let meterToInch = userValue * 39.37
        let meterToFoot = userValue * 3.281
        let meterToYard = userValue * 1.094
        let meterToMile = userValue / 1609
        let meterToNautic = userValue  / 1852
        let secToMin = userValue / 60
        let secToHour = userValue / 60 / 60
        let secToDay = userValue / 60 / 60 / 24
        let secToWeek = userValue / 60 / 60 / 24 / 7
        let secToYear = userValue / 60 / 60 / 24 / 7 / 365
        let mililiterToCentiliter = userValue / 10
        let mililiterToDeciliter = userValue / 100
        let mililiterToLiter = userValue  / 1000
        let mililiterToUsOnce = userValue / 29.574
        let mililiterToUsCup = userValue / 236.588
        let mililiterToUsPint = userValue / 473.176
        let mililiterToUsQuart = userValue / 946.353
        let mililiterToUsGallon = userValue / 3785.41
        let mililiterToUkOnce = userValue / 28.413
        let mililiterToUkCup = userValue / 284.13
        let mililiterToUkPint = userValue / 568.262
        let mililiterToUkQuart = userValue / 1136.52
        let mililiterToUkGallon = userValue / 4546.09
        // return the value  according to the user's pickers choices
        if unit == 0 && convertToValue == 0 { value = userValue }
        if unit == 0 && convertToValue == 1 { value = celsiusToFahrenheit }
        if unit == 0 && convertToValue == 2 { value = celsiusTokevin }
        if unit == 1 && toLengthSystem == 0 && convertToValue == 0 { value = meterToMillimeter }
        if unit == 1 && toLengthSystem == 0 && convertToValue == 1 { value = meterToCentimeter }
        if unit == 1 && toLengthSystem == 0 && convertToValue == 2 { value = userValue }
        if unit == 1 && toLengthSystem == 0 && convertToValue == 3 { value = meterToKilometer }
        if unit == 1 && toLengthSystem == 1 && convertToValue == 0 { value = meterToInch }
        if unit == 1 && toLengthSystem == 1 && convertToValue == 1 { value = meterToFoot }
        if unit == 1 && toLengthSystem == 1 && convertToValue == 2 { value = meterToYard}
        if unit == 1 && toLengthSystem == 1 && convertToValue == 3 { value = meterToMile}
        if unit == 1 && toLengthSystem == 2 { value = meterToNautic }
        if unit == 2 && convertToValue == 0 { value = userValue }
        if unit == 2 && convertToValue == 1 { value = secToMin }
        if unit == 2 && convertToValue == 2 { value = secToHour }
        if unit == 2 && convertToValue == 3 { value = secToDay }
        if unit == 2 && convertToValue == 4 { value = secToWeek }
        if unit == 2 && convertToValue == 5 { value = secToYear }
        if unit == 3 && toVolumeSystem == 0 && convertToValue == 0 { value = userValue }
        if unit == 3 && toVolumeSystem == 0 && convertToValue == 1 { value = mililiterToCentiliter }
        if unit == 3 && toVolumeSystem == 0 && convertToValue == 2 { value = mililiterToDeciliter }
        if unit == 3 && toVolumeSystem == 0 && convertToValue == 3 { value = mililiterToLiter }
        if unit == 3 && toVolumeSystem == 1 && convertToValue == 0 { value = mililiterToUsOnce }
        if unit == 3 && toVolumeSystem == 1 && convertToValue == 1 { value = mililiterToUsCup }
        if unit == 3 && toVolumeSystem == 1 && convertToValue == 2 { value = mililiterToUsPint }
        if unit == 3 && toVolumeSystem == 1 && convertToValue == 3 { value = mililiterToUsQuart }
        if unit == 3 && toVolumeSystem == 1 && convertToValue == 4 { value = mililiterToUsGallon }
        if unit == 3 && toVolumeSystem == 2 && convertToValue == 0 { value = mililiterToUkOnce }
        if unit == 3 && toVolumeSystem == 2 && convertToValue == 1 { value = mililiterToUkCup }
        if unit == 3 && toVolumeSystem == 2 && convertToValue == 2 { value = mililiterToUkPint }
        if unit == 3 && toVolumeSystem == 2 && convertToValue == 3 { value = mililiterToUkQuart }
        if unit == 3 && toVolumeSystem == 2 && convertToValue == 4 { value = mililiterToUkGallon }
        
        //this hasn't beem toaught yet, but I needed to find a solution for extra zeros in decimals
        var formatedValue: String {
            var theValue = String(value)
            while theValue.last == "0" { theValue.removeLast() }
            if theValue.last == "." { theValue.removeLast() }
            return theValue
        }
        return formatedValue
    }
    //create an abbreviation for the chosen unit of mesurment
    var abbreviation: String {
        var abbreviation = ""
        
        if unit == 0 && convertToValue == 0 { abbreviation = "° C" }
        if unit == 0 && convertToValue == 1 { abbreviation = "° F" }
        if unit == 0 && convertToValue == 2 { abbreviation = "° K" }
        if unit == 1 && toLengthSystem == 0 && convertToValue == 0 { abbreviation = " mm" }
        if unit == 1 && toLengthSystem == 0 && convertToValue == 1 { abbreviation = " cm" }
        if unit == 1 && toLengthSystem == 0 && convertToValue == 2 { abbreviation = " m" }
        if unit == 1 && toLengthSystem == 0 && convertToValue == 3 { abbreviation = " km" }
        if unit == 1 && toLengthSystem == 1 && convertToValue == 0 { abbreviation = " in" }
        if unit == 1 && toLengthSystem == 1 && convertToValue == 1 { abbreviation = " ft" }
        if unit == 1 && toLengthSystem == 1 && convertToValue == 2 { abbreviation = " yd"}
        if unit == 1 && toLengthSystem == 1 && convertToValue == 3 { abbreviation = " mi"}
        if unit == 1 && toLengthSystem == 2 { abbreviation = " nm" }
        if unit == 2 && convertToValue == 0 { abbreviation = " sec" }
        if unit == 2 && convertToValue == 1 { abbreviation = " min" }
        if unit == 2 && convertToValue == 2 { abbreviation = " h" }
        if unit == 2 && convertToValue == 3 { abbreviation = " d" }
        if unit == 2 && convertToValue == 4 { abbreviation = " w" }
        if unit == 2 && convertToValue == 5 { abbreviation = " y" }
        if unit == 3 && toVolumeSystem == 0 && convertToValue == 0 { abbreviation = " mL" }
        if unit == 3 && toVolumeSystem == 0 && convertToValue == 1 { abbreviation = " cL" }
        if unit == 3 && toVolumeSystem == 0 && convertToValue == 2 { abbreviation = " dL" }
        if unit == 3 && toVolumeSystem == 0 && convertToValue == 3 { abbreviation = " L" }
        if unit == 3 && toVolumeSystem == 1 && convertToValue == 0 { abbreviation = " oz" }
        if unit == 3 && toVolumeSystem == 1 && convertToValue == 1 { abbreviation = " c" }
        if unit == 3 && toVolumeSystem == 1 && convertToValue == 2 { abbreviation = " pt" }
        if unit == 3 && toVolumeSystem == 1 && convertToValue == 3 { abbreviation = " qt" }
        if unit == 3 && toVolumeSystem == 1 && convertToValue == 4 { abbreviation = " gal" }
        if unit == 3 && toVolumeSystem == 2 && convertToValue == 0 { abbreviation = " oz" }
        if unit == 3 && toVolumeSystem == 2 && convertToValue == 1 { abbreviation = " c" }
        if unit == 3 && toVolumeSystem == 2 && convertToValue == 2 { abbreviation = " pt" }
        if unit == 3 && toVolumeSystem == 2 && convertToValue == 3 { abbreviation = " qt" }
        if unit == 3 && toVolumeSystem == 2 && convertToValue == 4 { abbreviation = " gal" }
        
        return abbreviation
    }
    
    var body: some View {
        NavigationView {
            Form {
                //text fielf for user imput value
                TextField("Enter the value to convert", text: $valueForConversion)
                    .keyboardType(.decimalPad)
                if valueForConversion != "" {
                    Text("\(genericToDesireValue)\(abbreviation)")
                } else {
                    Text("")
                }
                //picker for TYPE of measurement
                Section(header: Text("Convert from:").textCase(nil)) {
                    Picker("Units", selection: $unit) {
                        ForEach(0 ..< units.count) {
                            Text("\(units[$0])")
                        }
                    }
                    .pickerStyle(SegmentedPickerStyle())
                    //create pickers for the INICIAL System and unit of measurement
                    //picker for Length measurements:
                    if unit == 1 {
                        //first the picker for: Systems of length measurement
                        Picker("Length System:", selection: $fromLengthSystem) {
                            ForEach(0 ..< lengthSystem.count) {
                                Text("\(lengthSystem[$0])")
                            }
                        }
                        .pickerStyle(SegmentedPickerStyle())
                        //picker if user chooses metric system
                        if fromLengthSystem == 0 {
                            Picker("Metric Units:", selection: $convertFromValue) {
                                ForEach(0 ..< lengthMetricUnits.count) {
                                    Text("\(lengthMetricUnits[$0])")
                                }
                            }
                            .pickerStyle(SegmentedPickerStyle())
                        }
                        //picker if user chooses US/Imperial system
                        if fromLengthSystem == 1 {
                            Picker("US/Imperial Units:", selection: $convertFromValue) {
                                ForEach(0 ..< lengthImperialUnits.count) {
                                    Text("\(lengthImperialUnits[$0])")
                                }
                            }
                            .pickerStyle(SegmentedPickerStyle())
                        }
                        //blanc line if user chooses nautical mile (to mantain graphic consistency)
                        if fromLengthSystem == 2 { Text("") }
                        
                        
                    //picker for volume measurements:
                    } else if unit == 3 {
                        //first the picker for: Systems of volume measurement
                        Picker("volumes System:", selection: $fromVolumeSystem) {
                            ForEach(0 ..< volumeSystem.count) {
                                Text("\(volumeSystem[$0])")
                            }
                        }
                        .pickerStyle(SegmentedPickerStyle())
                        if fromVolumeSystem == 0 {
                            //extra picker if user chooses metric system
                            Picker("Metric volumes:", selection: $convertFromValue) {
                                ForEach(0 ..< volumeMetricUnits.count) {
                                    Text("\(volumeMetricUnits[$0])")
                                }
                            }
                            .pickerStyle(SegmentedPickerStyle())
                        } else {
                            //extra picker if user chooses US/Imperial system
                            Picker("US Customary Volumes:", selection: $convertFromValue) {
                                ForEach(0 ..< volumeImperialUnits.count) {
                                    Text("\(volumeImperialUnits[$0])")
                                }
                            }
                            .pickerStyle(SegmentedPickerStyle())
                        }
                    } else {
                        //picker for Temperature or time
                        Picker("Convert From:", selection: $convertFromValue) {
                            if unit == 0 {
                                ForEach(0 ..< tempUnits.count) {
                                    Text("\(tempUnits[$0])")
                                }
                            } else if unit == 2 {
                                ForEach(0 ..< timeUnits.count) {
                                    Text("\(timeUnits[$0])")
                                }
                            }
                        }
                        .pickerStyle(SegmentedPickerStyle()).padding(.bottom, 45)
                    }
                }
                //repit the pickers for the FINAL System and unit of measurement
                Section(header: Text("Convert to:").textCase(nil)) {
                    //picker for Length measurements:
                    if unit == 1  {
                        Picker("Length System:", selection: $toLengthSystem) {
                            ForEach(0 ..< lengthSystem.count) {
                                Text("\(lengthSystem[$0])")
                            }
                        }
                        .pickerStyle(SegmentedPickerStyle())
                        //picker if user chooses metric system
                        if toLengthSystem == 0 {
                            Picker("Metric Units:", selection: $convertToValue) {
                                ForEach(0 ..< lengthMetricUnits.count) {
                                    Text("\(lengthMetricUnits[$0])")
                                }
                            }
                            .pickerStyle(SegmentedPickerStyle())
                        }
                        //picker if user chooses US/Imperial system
                        if toLengthSystem == 1 {
                            Picker("US/Imperial Units:", selection: $convertToValue) {
                                ForEach(0 ..< lengthImperialUnits.count) {
                                    Text("\(lengthImperialUnits[$0])")
                                }
                            }
                            .pickerStyle(SegmentedPickerStyle())
                        }
                        //blanc line if user chooses nautical mile (to mantain graphic consistency)
                        if toLengthSystem == 2 { Text("") }
                        
                    //pickers for volume measurements:
                    } else if unit == 3 {
                        //first the picker for: Systems of volume measurement
                        Picker("volumes System:", selection: $toVolumeSystem) {
                            ForEach(0 ..< volumeSystem.count) {
                                Text("\(volumeSystem[$0])")
                            }
                        }
                        .pickerStyle(SegmentedPickerStyle())
                        if toVolumeSystem == 0 {
                            //extra picker if user chooses metric system
                            Picker("Metric volumes:", selection: $convertToValue) {
                                ForEach(0 ..< volumeMetricUnits.count) {
                                    Text("\(volumeMetricUnits[$0])")
                                }
                            }
                            .pickerStyle(SegmentedPickerStyle())
                        } else {
                            //extra picker if user chooses US/Imperial system
                            Picker("US Customary Volumes:", selection: $convertToValue) {
                                ForEach(0 ..< volumeImperialUnits.count) {
                                    Text("\(volumeImperialUnits[$0])")
                                }
                            }
                            .pickerStyle(SegmentedPickerStyle())
                        }
                    } else {
                        //picker for Temperature or time
                        Picker("Convert From:", selection: $convertToValue) {
                            if unit == 0 {
                                ForEach(0 ..< tempUnits.count) {
                                    Text("\(tempUnits[$0])")
                                }
                            } else if unit == 2 {
                                ForEach(0 ..< timeUnits.count) {
                                    Text("\(timeUnits[$0])")
                                }
                            }
                        }
                        .pickerStyle(SegmentedPickerStyle())
                        .padding(.bottom, 44)
                    }
                }
            }
            .navigationTitle("weConvert")
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
        ZStack() {
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

