# Lab 1: Permissions

**Objectives**
- Lab 1.1: Creating and Access App Permissions
- Lab 1.2: Configuring Permissions Among Different Apps

**Lab Scenario**

In this lab, you will perform two tasks. The first task is one that accesses the Android system permission to read an SMS on a device and the other one creates permission in one application and accesses it in another.

## Lab 1.1: Creating and Access App Permissions

In this exercise, you will create a simple application that uses the Android permission **android.permission.READ_SMS** which is used to read SMS (Short Messages) from Android devices.

The application reads all the SMS from any Android device and displays the sender, the message text, and the date in a list view of an activity.

1. Open Android Studio to create a new app. Click **File -> New -> New Project**
2. Fill out the following information:
     - Application Name: **ReadSMS**
     - Company Domain: **androidatc.com**
3. Click **Next**
4. For the **"Target Android Devices"**, select **"Telephone and Tablet"** with API 26 level, and the click **Next**
5. Select **Empty Activity**, and the click **Next**
6. Type the file name of your main activity and the layout names as follows, and the click **Finish**
     - Activity Name: **ReadSMS**
     - Layout Name: **activity_read_sms**
7. Open the **activity_read_sms** in "Design" mode.
8. Delete the "Hello World" TextView.
9. Open file "activity_read_sms" in "Text" mode, and then change the activity layout of activity_read_sms.xml from ConstraintLayout to RelativeLayout by replacing the gray highlighted part of the following code with RelativeLayout.
