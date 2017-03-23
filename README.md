# Read_SMS
Reading incoming SMS messages for verification (OTP) in Android

Nowadays, Applications usually use SMS texts for authentication purpose, like OTP (One-Time-Passwords), where a message is sent by the service-provider and it is automatically read by the application, verifying it. This flow helps user to save a lot of app switching from application to messenger app and then entering the authentication text to app again.

In this code, we will focus on reading the sms automatically and runtime permission of Reading SMS when we are expecting it, and responding when we receive the message.

## Demo
![Read_SMS](screenshots/Read_SMS.gif)

 ## Usage
   ### Step 1 : Add "Read_SMS" to your Android project.

   1- Open your project in Android Studio.
   2- Download the library
       (using Git Link ---> https://github.com/yash786agg/Read_SMS.git)
                                        or 
       (Download a zip File archive to unzip)
    
   3- Create a folder "Read_SMS" in your project.
   4- Copy and paste the Code to your Read_SMS folder
   5- On the root of your project directory create/modify the settings.gradle file. It should contain something like the following:

      include 'MyApp', ':Read_SMS'

   6- Go to File > Project Structure > Modules.
   7- App > Dependencies.
   8- Click on the more on the left green "+" button > Module dependency.
   9- Select "Read_SMS Library".
   
    ### Step 2 : Add Code to your Project
   
      Code for Checking the Version of Android Device for Asking the Runtime Permission.
      
        if (Build.VERSION.SDK_INT <= Build.VERSION_CODES.LOLLIPOP_MR1)
        {
            readOTP();
        }
        else
        {
            int REQUEST_CODE_ASK_MULTIPLE_PERMISSIONS = 1;
            Acess_SMS.getOTP(MainActivity.this, REQUEST_CODE_ASK_MULTIPLE_PERMISSIONS);
        }
        
         @TargetApi(Build.VERSION_CODES.M)
    @Override
    public void onRequestPermissionsResult(int requestCode, @NonNull final String[] permissions, @NonNull int[] grantResults)
    {
        switch (requestCode)
        {
            case 1:
            {
                Map<String, Integer> perms = new HashMap<>();

                perms.put(android.Manifest.permission.READ_SMS, PackageManager.PERMISSION_GRANTED);


                for (int i = 0; i < permissions.length; i++)
                {
                    perms.put(permissions[i], grantResults[i]);
                }


                if (perms.get(android.Manifest.permission.READ_SMS) == PackageManager.PERMISSION_GRANTED)
                {
                    readOTP();
                }



                    if (grantResults[0] == PackageManager.PERMISSION_DENIED)
                    {
                        boolean should = ActivityCompat.shouldShowRequestPermissionRationale(MainActivity.this, android.Manifest.permission.READ_SMS);
                        if (should)
                        {
                            AlertDialog.Builder builder = new AlertDialog.Builder(MainActivity.this, R.style.MyAlertDialogStyle);
                            builder.setTitle(R.string.permission_denied);
                            builder.setMessage(R.string.permission_auro_read_sms);
                            builder.setPositiveButton(R.string.i_am_sure, new DialogInterface.OnClickListener()
                            {
                                @Override
                                public void onClick(DialogInterface dialog, int which) {
                                    dialog.dismiss();
                                }
                            });
                            builder.setNegativeButton(R.string.re_try, new DialogInterface.OnClickListener() {
                                @Override
                                public void onClick(DialogInterface dialog, int which) {
                                    dialog.dismiss();
                                    ActivityCompat.requestPermissions(MainActivity.this, new String[]{android.Manifest.permission.READ_SMS}, MY_PERMISSIONS_REQUEST_READ_SMS);
                                }
                            });
                            builder.show();

                        }
                    }

                break;
            }

            case 2:
            {
                Map<String, Integer> perms = new HashMap<>();

                perms.put(android.Manifest.permission.READ_SMS, PackageManager.PERMISSION_GRANTED);

                for (int i = 0; i < permissions.length; i++)
                    perms.put(permissions[i], grantResults[i]);

                if(perms.get(android.Manifest.permission.READ_SMS) == PackageManager.PERMISSION_GRANTED)
                {
                    readOTP();
                }

                break;
            }

            default:
                super.onRequestPermissionsResult(requestCode, permissions, grantResults);
        }

    }
    
    private void readOTP()
    {
        SmsReceiver.bindListener(new SmsListener()
        {
            @Override
            public void messageReceived(String messageText)
            {
                edt_verify_otp.setText(messageText);
                
                //Note: "edt_verify_otp" is your Editext Object.
            }
        });
    }
    
    /**********************************************************************/
    
    Code Snippet to be Add on styles.xml File
    
    /****Below is the code for Runtime Permission Alert Dialog***************/

    <style name="MyAlertDialogStyle" parent="Theme.AppCompat.Light.Dialog.Alert">
        <!-- Used for the buttons -->
        <item name="colorAccent">#000000</item>
        <!-- Used for the title and text -->
        <item name="android:textColorPrimary">#000000</item>
        <!-- Used for the background -->
        <item name="android:background">#FFFFFF</item>
    </style>

    /****Above is the code for Runtime Permission Alert Dialog***************/
    
    /**********************************************************************/
    
    Code Snippet to be Add on strings.xml File
    
    /******************* Runtime Permission *******************/

    <string name="i_am_sure">I AM SURE</string>
    <string name="re_try">RE-TRY</string>
    <string name="permission_denied">Permission Denied</string>
    <string name="toast_for_cancelling_the_permission">Sorry you can not proceed further process</string>
    <string name="You_need_to_Accept">You need to Accept</string>
    <string name="permission_to_access_the_app">permissions to access the app</string>
    <string name="RECEIVE_SMS">RECEIVE_SMS</string>
    <string name="READ_SMS">READ_SMS</string>
    <string name="please_manually_enter_the_otp">Please Manually Enter the OTP</string>
    <string name="permission_auro_read_sms">Without this permission the app is unable to auto fill the OTP. Are you sure you want to Deny this permission.?</string>

    /******************* Runtime Permission *******************/

    
