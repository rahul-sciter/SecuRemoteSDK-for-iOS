# SecuRemoteSDK for Apple iOS

# Table of Contents

- [Prerequisites](#prerequisites)
- [Introduction](#introduction)
- [Installation](#installation)
- [Examples](#examples)
- [SecuRemote API](#securemote-api)

## Prerequisites

* Mac computer with Xcode 7 and above.
* iPhone (4S or newer) or iPad (3rd generation or newer) to run the application.
* SecuRemote Account - You can [Sign Up Here](https://portal.securemote.com/).
* One or more SR Modules.

## Introduction

A **SecuRemote** iOS SDK provides a wide range of both utility and client facing features that will save lots of time and make development more easier. If you know basics of iOS and Objective C then you can build your own Bluetooth 4.0 Low Energy (aka Smart or LE) app using **SecuRemote** iOS SDK.

**SecuRemote** iOS SDK is built purely on Apple Core Bluetooth technologies and is 100 % Bluetooth Compatible.

Learn more:

- Go through [Bluetooth SIG](https://www.bluetooth.com/develop-with-bluetooth) to get knowledge of various Bluetooth Technologies.
- Go through [Apple Developer Bluetooth](https://developer.apple.com/bluetooth).
- Go through basic [Understanding Core Bluetooth](http://www.raywenderlich.com/52080/introduction-core-bluetooth-building-heart-rate-monitor)

## Installation

**SecuRemote** SDK comes to you as a **framework** rather than a **static library + headers** like it used to. This greatly simplifies setup, as you only need to include a single *SecuRemoteSDK.framework* file in your project to get started:

1. Drag and drop **SecuRemoteSDK.framework** file into your Xcode project. It will automatically show up in your project navigator and will be added to *"Linked Frameworks and Libraries"* section in project settings. Make sure to check the ‘Copy items if needed’ checkbox.

2. Use `#import <SecuRemoteSDK/SecuRemoteSDK.h>` in your implementation files.

   **Note:** in Swift, you need to add `#import <SecuRemoteSDK/SecuRemoteSDK.h>` to an *Objective-C Bridging Header* instead:

   * right-click on the "Support Files" group in the navigator, and choose "New File…"
   * pick a "Header File" from the "iOS - Source" section, and save it as "ObjCBridge.h"
   * add the `#import <SecuRemoteSDK/SecuRemoteSDK.h>` line to the newly created file
   * select your project in the navigator and go to "Build Settings"
   * look for the "Objective-C Bridging Header" setting and set it to `${PROJECT_NAME}/ObjCBridge.h`

3. Create Central Manager Object and its protocol.

   * Add the SRCentralManagerDelegate protocol
   * Create object of SRCentralManager in your view controller

```
// Write below code in your view controller.h file 
#import <UIKit/UIKit.h>
#import <SecuRemoteSDK/SecuRemoteSDK.h>
@interface ViewController : UIViewController<SRCentralManagerDelegate>
{
    SRCentralManager *srManagerObj;
} 
@end

// Write below code in viewDidLoad of your view controller.m file 
srManagerObj = [[SRCentralManager alloc]initWithDelegate:self queue:nil];

```

**Note:** Use srManagerObj object wherever you want in your view controller and set SRCentralManager property to self to get delegate method callback.
 
## Examples

**SecuRemote** SDK contains a lot of examples like Motion Sensor, Water Sensor, Accelerometer, Temperature, Solenoid, Light Control, Switches etc. It gives inspiration to help you to get familiar with SR Modules in your app. Examples are completely open source and cover most important areas of SR Modules and its configurations. 

<img src="Screenshots/DeviceList.png" alt="Home Screen" width="230">
<img src="Screenshots/Devkit Operations.png" alt="SR Modules Operations" width="230">
<img src="Screenshots/Motor.png" alt="Motor Operation Demo" width="230">
<img src="Screenshots/Motion Sensor.png" alt="Motor Sensor Demo" width="230">
<img src="Screenshots/Relays.png" alt="Relay Operation Demo" width="230">
<img src="Screenshots/Light Control.png" alt="Light Control Demo" width="230">
<img src="Screenshots/Temperature Sensor.png" alt="Temperature Sensor Demo" width="230">
<img src="Screenshots/Accelerometer.png" alt="Accelerometer Demo" width="230">
<img src="Screenshots/Water Sensor.png" alt="Water Sensor Demo" width="230">

## SecuRemote API

There are various APIs available in SRCentralManager class. User can directly use them and generate various operations on SR Modules. Here is the brief description of important APIs:

* **setSecurityParams** - It takes parameters like SR#, Security Token and Crypto Keys and assign to SR Module. After assigning these values to your SR Module, it will get ready for further communication.

```
/*!
 *  @method setSecurityParams:
 *
 *  @discussion set setSecurityParams.
 *
 *  @param serialNo             pass valid serial # of SR Module.
 *  @param securityToken        pass security token of SR Module.Its length must be 10.
 *  @param key1                 valid key for data encryption/decryption.Its length must be 32.
 *  @param key2                 valid key for data encryption/decryption.Its length must be 32.
 *  @param key3                 valid key for data encryption/decryption.Its length must be 32.
 *
 *  @see didOperationFailWithError  If valid parameter not passed will call this method.
 */

-(void)setSecurityParams:(NSString *)serialNo securityToken:(NSString *)securityToken key1:(NSString *)key1 key2:(NSString *)key2 key3:(NSString *)key3;
```
Sample request invocation looks like this:
```
    NSString *strSrNo           = @"DEVKIT-332689003";
    NSString *strSecureToken    = @"N4BWTXX6FZ";
    NSString *strKey1           = @"37EA565EEDAA73CBEDFEXX56936D5C42";
    NSString *strKey2           = @"EA3D6BFD5D3EA6C44353DAB9CB6E8C5A";
    NSString *strKey3           = @"DB7B72AA6C3FC52349BFA98A5297EB6B";
    
    [srManagerObj setSecurityParams:strSrNo securityToken:strSecureToken key1:strKey1 key2:strKey2 key3:strKey3];
```
* **initWithDelegate** - It is same as default Central Manager method. This delegate will receive central role events.
* **scanForPeripheralsWithServices**- It is same as default Central Manager method.It starts scanning for peripherals that are advertising any of the services listed in <i>service UUIDs</i>.
* **connectPeripheral** - It initiates a connection to peripheral device.
* **discoverServices** - It discovers available service(s) on the peripheral.
* **discoverCharacteristics** - It discovers the specified characteristic(s) of <i>service</i>
* **readValueForCharacteristic** - It reads the characteristic value for specified <i>characteristic</i>
* **writeValueforCharacteristic** - It writes <i>value</i> to specified <i>characteristic</i>.
* **setNotifyValue** - It enables or disables notifications/indications for the characteristic value of specified <i>characteristic</i>.
* **disconnectSRDevice** - It cancels an active or pending connection to <i>peripheral</i>.
* **readRSSIValue** - It retrieves the current RSSI of the link for connected peripheral.
* **stopScan** - It stops scanning for peripherals or SR Devices.
* **authorizeBLEChannel** - It creates and authorizes session based on security level set on SR Module. If it returns success, you are ready to perform any of the operation. It needs to add two delegate methods **didAuthorizeBLEChannel** and **didAuthorizeBLEChannelFailed** for success or failure. It needs to call after establishing connection with SR Module.

Sample request invocation looks like this:
```
- (void)centralManager:(CBCentralManager *)central didConnectPeripheral:(CBPeripheral *)peripheral
{
    connectedPeripheral = peripheral;
    NSLog(@"connected peripheral :- %@",peripheral);
    [srManagerObj authorizeBLEChannel];
}
```

* **isSRDeviceConnected** - It returns TRUE or FALSE based on device is connected or not.
```
/*!
 *  @method isSRDeviceConnected:
 *
 *  @discussion return flag (TRUE or FALSE) if device is connected or not.
 *
 */
-(BOOL)isSRDeviceConnected;
```

* **getSRDeviceInfo** - It retrieves basics information of connected peripheral like H/W and S/W version, manufacturer name, model number etc.
```
/*!
 *  @method getSRDeviceInfo:
 *
 *  @discussion return dictionary with keys H/W version, S/W version, manufacture name, Model and other information.
 *
 */
-(NSDictionary *)getSRDeviceInfo;
```
* **getSRDeviceSecurityInfo** - It retrieves basics information of connected peripheral like name,serial no,security level, security keys etc.
```
/*!
 *  @method getSRDeviceSecurityInfo:
 *
 *  @discussion return dictionary with keys peripheral,name,serial no,security level and other information.
 *
 */
-(NSDictionary *)getSRDeviceSecurityInfo;
```

* **getCharacteristicProperty** - It retrieves property of characteristic like Readable,Notifiable or Writable.
```
/*!
 *  @method getCharacteristicProperty:
 *
 *  @discussion return dictionary whether characteristic isRead,isNotify or isWrite.
 *
 */
-(NSMutableDictionary *)getCharacteristicProperty:(CBCharacteristic *)characteristic;
```
* **setSRDeviceConfiguration** - It sets device configuration like motion sensor,humidity and other information.
```
/*!
 *  @method setSRDeviceConfiguration:
 *
 *  @discussion set device configuration.
 *
 *  @param isMotion                Whether or not Motion should be enabled.
 *  @param isHumidity              Whether or not Humidity should be enabled.
 *  @param txPower                 Tx power should be Proximity-1, Low-2, Medium-3, High-4
 *  @param antennaType             Antenna type should be chip=0, ufl-1,
 *  @param networkConfiguration    Network Configuration should be Join All-0,
                                   Connect With Co-ordinator-1, Connect With Router-2
 *
 *  @see  didWriteConfiguration:
 */

-(void)setSRDeviceConfiguration:(BOOL)isMotion isHumidity:(BOOL)isHumidity txPower:(int)txPower antennaType:(int)antennaType networkConfiguration:(int)networkConfiguration;
```
* **getSRDeviceConfiguration** - It retrieves various configuration params of SR Module.
```
/*!
 *  @method getSRDeviceConfiguration:
 *
 *  @discussion get device configuration like motion sensor,humidity and other information.
 *
 *  @see  didReadConfiguration:
 */
-(void)getSRDeviceConfiguration;
```
* **operateRelay** - It operates relay of SR Module.
```
/*!
 *  @method operateRelay:
 *
 *  @discussion operate relay.
 *
 *  @param isMomentary Pass true for Relay Momentary and false for Relay Maintain
 *
 *  @see  didRelayOperated:
 */
-(void)operateRelay:(BOOL)isMomentary;
```
* **operateSolenoid** - It operates solenoid of SR Module.
```
/*!
 *  @method operateSolenoid:
 *
 *  @discussion operate solenoid.
 *
 *  @see  didSolenoidOperated:
 */
-(void)operateSolenoid;
```
* **operateMotor** - It operates motor of SR Module.
```
/*!
 *  @method operateMotor:
 *
 *  @discussion operate motor.
 *
 *  @param isForward Pass true for motor's forward operation and false for reverse operation.
 *
 *  @see  didMotorOperated:
 */
-(void)operateMotor:(BOOL)aBoolIsForward;
```
* **operateLight** - It operates light of SR Module.
```
/*!
 *  @method operateLight:
 *
 *  @discussion operate light.
 *
 *  @param aBoolIsOn Pass true for switch on light and false to switch off.
 *
 *  @see  didLightOperated:
 */
-(void)operateLight:(BOOL)aBoolIsOn;
```
* **setLightIntensity** - It sets intensity of SR Module's light.
```
/*!
 *  @method setLightIntensity:
 *
 *  @discussion set light intensity.
 *
 *  @param aIntIntensity  Pass integer value between 0 to 100 to set intensity.
 *
 *  @see  didChangedLightIntensity:
 */
-(void)setLightIntensity:(int)aIntIntensity;
```
* **getSwitchStatus** - It retrieves the status of SR Module's switch.
```
/*!
 *  @method getSwitchStatus:
 *
 *  @discussion get switch status.
 *
 *  @see  didGetSwitchStatus:
 */
-(void)getSwitchStatus;
```
* **getTemperatureSensor** - It retrieves the temperature of SR Module's sensor.
```
/*!
 *  @method getTemperatureSensor:
 *
 *  @discussion get temperature sensor.
 *
 *  @see  didGetTemperatureSensor:
 */
-(void)getTemperatureSensor;
```
* **getAccelerometer** - It retrieves the accelerometer value of SR Module.
```
/*!
 *  @method getAccelerometer:
 *
 *  @discussion get accelerometer.
 *
 *  @see  didGetAccelerometer:
 */
-(void)getAccelerometer;
```
* **getMotionSensor** - It retrieves the motion sensor value of SR Module.
```
/*!
 *  @method getMotionSensor:
 *
 *  @discussion get motion sensor.
 *
 *  @see  didGetMotionSensor:
 */
-(void)getMotionSensor;
``` 
* **getWaterSensor** - It retrieves the water sensor value of SR Module.
```
/*!
 *  @method getWaterSensor:
 *
 *  @discussion get water sensor.
 *
 *  @see  didGetWaterSensor:
 */
-(void)getWaterSensor;
``` 
* **getBatteryLevel** - It retrieves the battery level value of SR Module.
```
/*!
 *  @method getBatteryLevel:
 *
 *  @discussion get battery level.
 *
 *  @see  didGetBatteryLevel:
 */
-(void)getBatteryLevel;
``` 
* **getHumidity** - It retrieves the humidity value of SR Module.
```
/*!
 *  @method getHumidity:
 *
 *  @discussion get humidity.
 *
 *  @see  didGetHumidity:
 */
-(void)getHumidity;
``` 

