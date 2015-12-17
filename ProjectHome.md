# Android File Dialog #

The standard API does not include file dialog to select files or directories. This library is a file dialog that currently supports file selection and creation. In the near future it will also support directory selection - This functionality was added by cirelli.mauricio and commited to trunk as FileDialogWithDirectories.java. His comment:

"I have implemented these funcionalities.
You can pass a parameter to the activity to make it show only files whose extension matches a list of extensions.
Also you can select a directory."

The dialog is an Activity. It just returns you the path to the file that user has chosen. It doesn't create nor open files, the library only returns paths in such form "/sdcard/temp/file.txt". You are free to make whatever you like with the given path.

![http://android-file-dialog.googlecode.com/svn/wiki/printscreen1.png](http://android-file-dialog.googlecode.com/svn/wiki/printscreen1.png)



# Usage principles #

The dialog is created like this:

```
Intent intent = new Intent(getBaseContext(), FileDialog.class);
		intent.putExtra(FileDialog.START_PATH, "/sdcard");
		
		//can user select directories or not
		intent.putExtra(FileDialog.CAN_SELECT_DIR, true);
		
		//alternatively you can set file filter
		//intent.putExtra(FileDialog.FORMAT_FILTER, new String[] { "png" });
		
		startActivityForResult(intent, REQUEST_SAVE);
```

Here "/sdcard" is the start path where the file dialog will open.
REQUEST\_SAVE is your integer constant, it will then be transmitted to onActivityResult;

CAN\_SELECT\_DIR - possibility to select directories
FORMAT\_FILTER - files filter. If file name ends with one of filters the file will be displayed.

When user selects a file or just goes back not selected anything, the activity closes, calling the standard onActivityResult method:

```
	public synchronized void onActivityResult(final int requestCode,
		int resultCode, final Intent data) {

		if (resultCode == Activity.RESULT_OK) {

			if (requestCode == REQUEST_SAVE) {
				System.out.println("Saving...");
			} else if (requestCode == REQUEST_LOAD) {
				System.out.println("Loading...");
			}
			
			String filePath = data.getStringExtra(FileDialog.RESULT_PATH);

		} else if (resultCode == Activity.RESULT_CANCELED) {
			Logger.getLogger(AccelerationChartRun.class.getName()).log(
					Level.WARNING, "file not selected");
		}

	}
```

The result code will be Activity.RESULT\_OK if user has selected something and Activity.RESULT\_CANCELED if he just pressed the BACK button not selecting anything. You can retrieve the filepath from the intent:

```
	data.getStringExtra(FileDialog.RESULT_PATH);
```

If for some reason the words startActivityForResult and onActivityResult are not familiar to you, just read documentation about them. In short, an activity is created and when it exits a handler is called. In this handler you can get the filepath.

You can add one more parameter to the Intent: SelectionMode.MODE\_OPEN or SelectionMode.MODE\_CREATE
```
intent.putExtra(FileDialog.SELECTION_MODE, SelectionMode.MODE_OPEN);
```
SelectionMode.MODE\_OPEN will make the NEW button disabled. So user can only select existing files

# Recent changes #

Bug fixes:

[Issue 2](https://code.google.com/p/android-file-dialog/issues/detail?id=2):	File dialog crashes with NullPointerException
Reported by ppv....@gmail.com, Mar 10, 2011

[Issue 4](https://code.google.com/p/android-file-dialog/issues/detail?id=4):	Exception in getDirImpl if the list of files in the start directory is empty
Reported by herve.gi...@gmail.com, Aug 4, 2011

Selection mode OPEN/CREATE added

# Collaboration #

If you want to participate, to improve the library, be free to send me a message. New commiters are welcome.

# TODO #

In the near future i'm planning to add directory selection support and directories creation.

# CONTACT #
alexander.ponomarev.1

gmail

com

# APPLICATIONS THAT USE THE LIBRARY #

AcMeter - very functional application to work with accelerometer.
https://market.android.com/details?id=com.a10

EZ Drop:
https://market.android.com/details?id=co.dropper.ez.android.free


# TAGS #

File selection android

File dialog android

File menu android

Android choose file

File chooser android

Android open file dialog

Android file save dialog