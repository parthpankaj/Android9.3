# Android9.3
package com.example.pankaj.permission92;

import java.io.BufferedReader;
import java.io.DataInputStream;
import java.io.File;
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.InputStreamReader;

import android.app.Activity;
import android.content.Context;
import android.content.ContextWrapper;
import android.os.Bundle;
import android.os.Environment;
import android.view.View;
import android.view.View.OnClickListener;
import android.widget.Button;
import android.widget.EditText;
import android.widget.TextView;

public class MainActivity extends Activity implements OnClickListener{

    private String filename = "MySampleFile.txt";
    private String filepath = "MyFileStorage";
    File myInternalFile;
    File myExternalFile;
    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        ContextWrapper contextWrapper = new ContextWrapper(getApplicationContext());
        File directory = contextWrapper.getDir(filepath, Context.MODE_PRIVATE);
        myInternalFile = new File(directory , filename);

        Button saveToInternalStorage =
                (Button) findViewById(R.id.saveInternalStorage);
        saveToInternalStorage.setOnClickListener(this);

        Button readFromInternalStorage =
                (Button) findViewById(R.id.getInternalStorage);
        readFromInternalStorage.setOnClickListener(this);

        Button saveToExternalStorage =
                (Button) findViewById(R.id.saveExternalStorage);
        saveToExternalStorage.setOnClickListener(this);

        Button readFromExternalStorage =
                (Button) findViewById(R.id.getExternalStorage);
        readFromExternalStorage.setOnClickListener(this);

        //check if external storage is available and not read only
        if (!isExternalStorageAvailable() || isExternalStorageReadOnly()) {
            saveToExternalStorage.setEnabled(false);
        }
        else {
            myExternalFile = new File(getExternalFilesDir(filepath), filename);
        }

    }

    public void onClick(View v) {

        EditText myInputText = (EditText) findViewById(R.id.myInputText);
        TextView responseText = (TextView) findViewById(R.id.responseText);
        String myData = "";

        switch (v.getId()) {
            case R.id.saveInternalStorage:
                try {
                    FileOutputStream fos = new FileOutputStream(myInternalFile);
                    fos.write(myInputText.getText().toString().getBytes());
                    fos.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
                myInputText.setText("");
                responseText
                        .setText("MySampleFile.txt saved to Internal Storage...");
                break;

            case R.id.getInternalStorage:
                try {
                    FileInputStream fis = new FileInputStream(myInternalFile);
                    DataInputStream in = new DataInputStream(fis);
                    BufferedReader br =
                            new BufferedReader(new InputStreamReader(in));
                    String strLine;
                    while ((strLine = br.readLine()) != null) {
                        myData = myData + strLine;
                    }
                    in.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
                myInputText.setText(myData);
                responseText
                        .setText("MySampleFile.txt data retrieved from Internal Storage...");
                break;

            case R.id.saveExternalStorage:
                try {
                    FileOutputStream fos = new FileOutputStream(myExternalFile);
                    fos.write(myInputText.getText().toString().getBytes());
                    fos.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
                myInputText.setText("");
                responseText
                        .setText("MySampleFile.txt saved to External Storage...");
                break;

            case R.id.getExternalStorage:
                try {
                    FileInputStream fis = new FileInputStream(myExternalFile);
                    DataInputStream in = new DataInputStream(fis);
                    BufferedReader br =
                            new BufferedReader(new InputStreamReader(in));
                    String strLine;
                    while ((strLine = br.readLine()) != null) {
                        myData = myData + strLine;
                    }
                    in.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
                myInputText.setText(myData);
                responseText
                        .setText("MySampleFile.txt data retrieved from Internal Storage...");
                break;


        }
    }

    private static boolean isExternalStorageReadOnly() {
        String extStorageState = Environment.getExternalStorageState();
        if (Environment.MEDIA_MOUNTED_READ_ONLY.equals(extStorageState)) {
            return true;
        }
        return false;
    }

    private static boolean isExternalStorageAvailable() {
        String extStorageState = Environment.getExternalStorageState();
        if (Environment.MEDIA_MOUNTED.equals(extStorageState)) {
            return true;
        }
        return false;
    }

}
--------
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent" android:layout_height="fill_parent"
    android:orientation="vertical">

    <TextView android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:text="@string/hello" />

    <EditText android:id="@+id/myInputText"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:ems="10" android:lines="5"
        android:minLines="3" android:gravity="top|left"
        android:inputType="textMultiLine">

        <requestFocus />
    </EditText>

    <Button android:id="@+id/saveInternalStorage"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Save to Internal Storage" />

    <Button android:id="@+id/getInternalStorage"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Display from Internal Storage" />

    <Button android:id="@+id/saveExternalStorage"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Save to External Storage" />

    <Button android:id="@+id/getExternalStorage"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Display from External Storage" />

    <TextView android:id="@+id/responseText"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content" android:padding="5dp"
        android:text=""
        android:textAppearance="?android:attr/textAppearanceMedium" />

</LinearLayout>
