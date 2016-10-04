# XCaliper SDK for iOS

This library allows you to use your XCaliper into your iOS app.

## Requirements

The only requirements to start developing with the iOS SDK are OS X Mavericks, Xcode 5 and iOS 7. 
It is recommended that you upgrade to OS X Mavericks, Xcode 5 and iOS 7 if you haven't done so already.

You will need to have access key to an existing XCaliper License in order to authenticate with the Platform. 


## Table of Contents

- [Getting Started](#getting-started)
  - [Download the iOS SDK](#download-the-ios-sdk)
  - [Run the Sample App](#run-the-sample-app)
  - [Add the SDK to Your Project](#add-the-sdk-to-your-project)
  - [Configure Xcode Project](#configure-xcode-project)
- [Documentation](#documentation)
  - [Authentication](#authentication)
  - [Connect](#connect)
  - [Measure](#meature)
  - [Dashboard](#dashboard)
  - [Search](#search)
- [Terms of Service](#terms-of-service)
- [Support](#support)
- [Credits](#credits)
- [License](#license)

## Getting Started

#### Download the iOS SDK.

You can download the latest iOS SDK release via the link below or clone it directly from this GitHub repository.

**Option 1:** Download iOS SDK v1.0.0
https://github.com/CAT-Lab/Mobile-SDK-iOS/releases/tag/v1.0.0

**Option 2:** Clone this repository from GitHub  
`git clone git@github.com:CAT-Lab/Mobile-SDK-iOS.git`


#### Run the Sample App

The iOS SDK comes with a sample iOS app that you can use to authenticate with the UP Platform with sample key.

You can find and open the PlatformTest project in `Mobile-SDK-iOS/SampleApp/Xcaliper.xcodeproj`.


#### Add the SDK to Your Project

* Drag `Xcaliper.framework` into Frameworks group inside your Xcode project or workspace.

That's it!  
Now just add `#import <Xcaliper/XcaliperSDK.h>` wherever you want to use the SDK in your project.


#### Configure Xcode Project

In Xcode, secondary-click your project's .plist file and select Open As -> Source Code.

Insert the following XML snippet into the body of your file just before the final </dict> element.

```xml
<key>XCaliperAccessKey</key>
<string>{your-access-key}</string>
```

# Documentation

## Authentication

```swift
XCManager.default().initilaize(with:self)
```

## Connect

```swift
if ( XCManager.default().startScan(5, allowDuplicates:true) == true ) {
  // Start Connecting
}
```

```swift
func manager(_ manager: XCManager!, didDiscover connections: [XCConnection]!) {
  for ( _, connection ) in connections.enumerated() {
    if ( (connection.name) != nil && connection.name!.hasPrefix("X-Caliper") && connection.rssi.int32Value > -45 ) {

      // Start Paring
      XCManager.default().pair(connection, timeout: 5)
      break
    }
  }
}

func manager(_ manager: XCManager!, didChangePeripheralStatus peripheral: CBPeripheral!) {
  
  if peripheral.state == CBPeripheralState.connected {
    // Connected
  }
}

func manager(_ manager: XCManager!, didFailToConnectWithError error: Error!) {
    // Fail to Connect
    
}
```

## Measure

```swift
let measureResource = XCMeasureResource(gender: MeasureGenderMale, age: 30, bodyHeight: 68, bodyWeight: 176)
measureResource.deviceUUID = XCManager.default().connectedConnection.identifier.uuidString

// Start Measuring
XCManager.default().service(XCService.getMeasureValue, data: nil, timeout: 5)
```


```swift
func manager(_ manager: XCManager!, didChangePeripheralStatus peripheral: CBPeripheral!) {
  if ( peripheral.state == CBPeripheralState.disconnected ) {
      // Disconnected
  }
}

func manager(_ manager: XCManager!, didUpdate service: XCServiceInterface!, for message: XCMessage!, error: Error!) {
  if ( message.service == XCService.getMeasureValue ) {
    let measureMessage:XCMeasureMessage = message as! XCMeasureMessage;
    
    // 
    measureMessage.measure // Point of Measure
    measureMessage.pressure // Pressure of Measure
    
    if ( measureMessage.status == XCMessageStatus.OK ) {
      if ( self.containerView.measureSite == MeasureSite.triceps ) {
        measureResource.measureTriceps = measureValue
      }
      else if ( self.containerView.measureSite == MeasureSite.abdominal ) {
        measureResource.measureAbdominal = measureValue
      }
      else if ( self.containerView.measureSite == MeasureSite.thigh ) {
        measureResource.measureThigh = measureValue
      }
    }
  }
}

func finishMeasure() {
    // Save
    measureResource.save()
    
    // Close to measure
    XCManager.default().closeAllServices(with: self)
}
```

## Dashboard

```swift
XCMeasureManager.default().delegate = self
XCMeasureManager.default().restoreResources()
```
```swift
let manager:XCMeasureManager = XCMeasureManager.default()
let lastResource:XCMeasureResource? = manager.lastResource
```

#### `XCMeasureResource`

```swift
var resourceUUID:String     // Universally Unique Identifier of MessureResource
var bodyWeight:CGFloat      // Body Weight (KG)
var bodyHeight:CGFloat      // Body Height (CM)
var gender:MeasureGender    // Gender
var age:Int                 // Age
var measureTriceps:CGFloat  // Measure Point of Triceps
var measureTricepsState:MeasureState  // Measure Analysis State of Triceps 
var measureAbdominal:CGFloat  // Measure Point of Abdominal
var measureAbdominalState:MeasureState  // Measure Analysis State of Abdominal
var measureThigh:CGFloat    // Measure Point of Thigh
var measureThighState:MeasureState  // Measure Analysis State of Thigh
var measureTotal:CGFloat    // Sum Point of Measures
var measureDate:Date        // Measured Datetime
var bodyFatPercentage:CGFloat // Percentage of body Fat
var bodyMass:CGFloat        // Point of Body Mass
var fatMass:CGFloat         // Weight of Fat Mass (KG)
var leanMass:CGFloat        // Weight of Lean Body Mass (KG)
var muscleMass:CGFloat      // Weight of Muscle Mass (KG)
var skeletalMuscleMass:CGFloat // Weight of Skeletal Muscle Mass (KG)
var idealBodyWeight:CGFloat // Ideal Weight of Body (KG)
var idealBodyFatPercentage:CGFloat // Ideal Percentage of body Fat
var idealFatMass:CGFloat    // Ideal Weight of Fat Mass (KG)
var idealLeanMass:CGFloat   // Ideal Weight of Lean Body Mass (KG)
var idealMuscleMass:CGFloat // Ideal Weight of Muscle Mass (KG)
var idealSkeletalMuscleMass:CGFloat // Ideal Weight of Skeletal Muscle Mass (KG)
var state:MeasureState      // Measure Analysis State
```

#### Measure State

```swift
public enum MeasureState {
    case Unknown
    case HighlyUnderfat // Highly Underfat
    case Underfat       // Underfat
    case Normal         // Normal
    case Overfat        // Overfat
    case Obese          // Obese
}
```

#### Battery Status

```swift
manager.batteryValue  Value of Battery Gage ( 1~100, 120 = Charging )
```


## Search

```swift
let searchManager:XCSearchManager = XCSearchManager()
searchManager.delegate = self

let formatter = DateFormatter()
formatter.dateFormat = "yyyy-MM-dd"

let startDate = Date(timeIntervalSinceNow: -30*24*60*60) // in a Month
let endDate = Date(timeIntervalSinceNow: 0)
let start = formatter.string(from: startDate)
let end = formatter.string(from: endDate)
searchManager.seek(start, end:end, groupByDay:true)
```

```swift
func manager(_ manager: XCSearchManager!, didSeekResources resources: [XCMeasureResource]!) {
  for i in 0..<resources.count {
    // Get Resource
    let resource:XCMeasureResource = resources[i]
    
    
  }
}
```

# Terms of Service


# Support

Contact the developer support team by sending an email to coopercaliper@gmail.com.

# Credits

### Development

**YongUk Lee**  
Software Engineer  
XCaliper

# License

Usage is provided under the Apache License (v2.0). See LICENSE for full details.
