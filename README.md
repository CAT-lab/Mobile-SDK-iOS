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
- [Documentation](#documentation)
  - [Authentication](#authentication)
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

<key>XCaliperAccessKey</key>
<string>{your-access-key}</string>


# Documentation

## Authentication



## Measure

## Dashboard

## Search



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
