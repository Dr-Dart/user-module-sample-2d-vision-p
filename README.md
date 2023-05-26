# [Sample 2D Vision Module]
[![License](https://img.shields.io/badge/License-BSD%203--Clause-blue.svg)](https://opensource.org/licenses/BSD-3-Clause)


## *Precautions*
#### Note
> __This sample code is currently private for developers participating in the Early Access Program and only works in the Dr.Dart Ecosystem.__ 

## *Overview*
"This sample is a modified TCP/IP client Template module provided from the Device Module UI templates."

### Vision Module Main View
|File|Description|
|---|---|
|Manifest.json<br>(In sample.2D.vision.module/<br>vision.module.sensopart.setting)|To use 1 basic module Main screen and 1 User Commands, 2 screens and 1 services are declared.<br> (*Note. However, in the case of the service, it will be changed so that User Command can operate even if there is only the User Command Service service in the released version.)</br>|
|index.tsx| This is where the database is initialized and MainScreen VisionSetting.tsx is imported.|
|utils/DatabaseManager.ts| Includes functions for creating and inserting into tables using DB functions in Dart API.|
|components/VisionSettings.tsx| This module contains the main screen of the application. It includes buttons for connecting to the Vision Device as a TCP/IP client and setting the mode using checkboxes. It also imports child components such as VisionJobMain, Command, and Calibration.|
|components/VisionJobMain.tsx| When connected to the camera, it retrieves the job list from the camera and displays it in a select box. Clicking the apply button applies the selected job based on the chosen job name.|
|components/Command.tsx| When the select box is clicked, it combines pre-defined 2D Vision command commands and text to display the setting and output results in a multiline format when the Send button is clicked.|
|components/CalibrationSenso.tsx| It utilizes DRL (Deep Reinforcement Learning) from drl/DRL_Calibration_run.ts to obtain seven robot coordinates and perform calibration. (*Note: When outputting using this code, the result will be in rotation Euler XYZ, representing yaw-pitch-roll 6Dof results.)|
|drl/DRL_Calibration_run.ts| It is a DRL text file. (*Note: Currently, the 2nd version of EAP has an issue with finding and reading DRL files, so the DRL code must be saved as a constant string. This DRL code is derived from https://github.com/humarobotics-france/doosan-sensopart.)|


### User Command Property View
|File|Description|
|---|---|
|UserCommandScreen/DetectionPIPscreen.tsx| This is the main screen of the Detection PIP (Picture-in-Picture) screen that is displayed in the Task Editor. It allows connecting and disconnecting. It imports child components such as VisionJob, ShootPose, VisionPose, and PickingPose.|
|components/VisionJob.tsx| When connected to the camera, it retrieves the job list from the camera and displays it in a select box. Clicking the apply button applies the selected job based on the chosen job name.|
|components/ShootPose.tsx| It sets the position for shooting in 2D Vision.|
|components/VisionPose.tsx| This component is used to obtain the initial position of the object set in 2D Vision. It triggers capturing the initial position of the object from the shooting position and saves it.|
|components/PickingPose.tsx| It is a component for determining the initial Picking position. It moves the robot upwards from the object's initial position to obtain the pose. Gripper integration is performed through another module.|
|UserCommandService/DetectionService.ts| Includes interfaces that must be implemented in Detection User Commands.|
|drl/DRL_Sub_Detection.ts| It is a DRL text file for Detection. It outputs the 2D position. (*Note: Currently, the value can be obtained through the variable = user_commands.Detection, and improvements are planned for the future.)|






Regenerate response



## *ChangeLog*


### [1.0.0] - Initial Release
* User Command feature introduced in Task Editor Module.

### [1.0.1] - Updates
#### Added
* Ability to copy and paste 'ShootPose'.
* State when the servo is on.
* Feature to update the database upon disconnect.
* Editable 'vision pose' text field.

#### Fixed
* Certain bugs related to database loading.

#### Changed
* 'Moveto' button behavior modified to 'Hold to Run'.

#### Improved
* Reduced the number of excessive database calls.

## *Usage*
#### In this Module, you can
* Perform camera connection tests.
* Change between free-run and Trigger modes.
* Load and select Vision Job lists and apply them.
* Input and combine Command commands to set up the camera and view the output results.
* Perform calibration between the camera and the robot.

#### In Task Editor Module(former Task Builder/Task Writer),
* You can add these command blocks to your task list to set up Shoot Pose, Vision Pose, and Picking Pose.
* Obtain 2D Pose through the Detection User Command.
* When the task is executed, the 2D Pose can be utilized in other commands.

## *Limitations*
#### The current sample of EAP version has the following limitations however We will update the Task Editor soon and redistribute it to the Dart-Store.


### 1. [Sensopart Vision Sample Module]
- Please refer to the Dart-developer site and Dart-API documentation for information on the APIs and code usage used in this module.
 - A Sensopart Visor camera is required, and for instructions on setting up the camera and downloading the software, please refer to the Sensopart website: https://www.sensopart.com/ko/service/downloads/143-documentation-for-visor/
 - Referring to the Sensopart manual and to briefly explain, you need to set up Sensopart's software, Senso Config, as follows:
    + Job Settings : 
        1. Press 'New' to create a job.
        2. Adjust the shutter speed to get an appropriate brightness.
        3. Focus the camera.
        4. Change to trigger mode. 
        5. This mode change can also be done from the example module.
        6. Set the internal illumination to off.
    + Detector Settings
        1. Add one Pattern matching.
        2. Appropriately set up the object.
    + Output Settings
        1. Check Ethernet enable in the Interface tab.
        2. Confirm if (IN)2006, (OUT)2005, and ASCII are selected.
        3. Move to the Telegram tab.
        4. Set the Start to 'pos,'.
        5. Set the Separator to ','.
        6. For payload, choose the Detector you added earlier.
        7. In order, add Pos X, Pos Y, Angle Z, and Detect Result. (After this, when you trigger the coordinate, the result comes in the order of [pos, Pos X, Pos Y, Angle Z, Detect Result].)
    + Start Sensor
        1. Make sure to press the 'Start Sensor' button. If you do not press it, the result will come as Fail even if you send a command.
- Since the camera is calibrated in Euler XYZ, rotation values will be displayed in XYZ when triggered. When the module receives this, it converts Euler XYZ into Euler ZYZ for display on the screen. When the robot operates and moves to the offset pose, it also converts from Euler XYZ to Euler ZYZ for movement.

2. **[DRL Generator in Dart-IDE]** 
<br>-You cannot use drl files created by DRL Generator. Currently, it should be used as a string like `drl/DRL_Calibration_run.ts`. It will be improved in the release version.

3. **[Task Editor in Dart-Platform]** 
<br>- When changing the value in the User Command Property window, the change is not reflected in the Task Editor. Normally, like other move commands, user input values should be saved through the task editor. However, this is due to a bug in Task Editor, and we will upload it to the Store as soon as we fix it. However, the first preset will properly reflect the port number you changed in module settings.
<br>- There is a problem that intermittently does not work properly when playing in Task Editor. We are aware of the problem and are fixing it.