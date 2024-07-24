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

```
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".ReadSMS">

</androidx.constraintlayout.widget.ConstraintLayout>
```

10. The **activity_read_sms** file will be as follows:
    
```
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".ReadSMS">

</RelativeLayout>
```

11. Add three **TextView** inside the "RelativeLayout": one for the **SMS sender** details, the second for the **SMS Content** and the third for **Date details**, as illustrated in the following code.

```
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".ReadSMS">

    <TextView
        android:id="@+id/sms_origin"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignParentStart="true"
        android:layout_alignParentTop="true" />

    <TextView
        android:id="@+id/sms_body"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignParentStart="true"
        android:layout_alignParentTop="true"
        android:layout_below="@id/sms_origin"/>

    <TextView
        android:id="@+id/sms_date"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignParentStart="true"
        android:layout_alignParentTop="true"
        android:layout_below="@id/sms_body"/>

  </RelativeLayout>
```

**Note:** It is not necessary to set the location of each TextView widget because you need to refer to these widgets using their IDs only.

12. Open the **ReadSMS.kt** file as illustrated in the following figure:

     ![image](https://github.com/user-attachments/assets/75c3b8e9-93e0-4578-9719-5368383d7771)

13. Change the **ReadSMS** activity parent class to **ListActivity** as illustrated in the following code:

```
package androidatc.com

import android.app.ListActivity
import android.os.Bundle
import androidx.activity.enableEdgeToEdge
import androidx.appcompat.app.AppCompatActivity
import androidx.core.view.ViewCompat
import androidx.core.view.WindowInsetsCompat

class ReadSMS : ListActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_read_sms)
    }
}
```

14. Create a constant Uri property (variable) inside the **ReadSMS** file called **SMS** as follows:

```
val SMS = Uri.parse("content://sms")
```

15. Create a constant int property (variable) inside the **ReadSMS** Kotlin file called **PERMISSIONS_REQUEST_READ_SMS**, and give it a value of **1**. We will use this property when we request the **READ_SMS** permission ina code as follows:

```
val PERMISSIONS_REQUEST_READ_SMS = 1
```

16. Create a singleton class called **SmsColumns** inside the ReadSMS activity. This class contains four constant String values that define the details of an SMS read from the device. See below the updated codes.

```
class ReadSMS : ListActivity() {

    val SMS = Uri.parse("content://sms")
    val PERMISSIONS_REQUEST_READ_SMS = 1

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_read_sms)
    }

    object SmsColumns {
        val ID = "_ID"
        val ADDRESS = "address"
        val DATE = "date"
        val BODY = "body"
    }
}
```

17. Within the ReadSMS activity, create a private inner-class called **SmsCursorAdapter**. This new class should extend from CursorAdapter. The purpose of this class is to bind the views (widgets) to the corresponding data from the cursor.

```
    private inner class SmsCursorAdapter (context:Context, c: Cursor, autoRequery:Boolean) : CursorAdapter(context, c, autoRequery){
        override fun newView(context: Context?, cursor: Cursor?, parent: ViewGroup?): View {
            return View.inflate(context, R.layout.activity_read_sms, null)
        }

        override fun bindView(view: View?, context: Context?, cursor: Cursor?) {
            view?.findViewById<TextView>(R.id.sms_origin)?.text = cursor?.getString(cursor.getColumnIndexOrThrow(SmsColumns.ADDRESS))
            view?.findViewById<TextView>(R.id.sms_body)?.text = cursor?.getString(cursor.getColumnIndexOrThrow(SmsColumns.BODY))
            view?.findViewById<TextView>(R.id.sms_date)?.text = cursor?.getString(cursor.getColumnIndexOrThrow(SmsColumns.DATE)).toString()
        }

    }
```

18. Create a private method called **readSMS()**, and configure it to create a cursor containing the SMS data present on the device as shown below.

```
    private fun readSMS() {
        val cursor = contentResolver.query(SMS, arrayOf(SmsColumns.ID, SmsColumns.ADDRESS, SmsColumns.DATE, SmsColumns.BODY), null, null, SmsColumns.DATE + " DESC")
        val adapter = cursor?.let { SmsCursorAdapter(this, it, true) }
        listAdapter = adapter
    }
```

19. Now read the SMS data in the onCreate() method by calling readSMS(). Your onCreate() should look like below.

```
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_read_sms)
        readSMS()
    }
```

20. Run the application. You will see an error similar to the following in the console.

