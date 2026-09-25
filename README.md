# SCANITY-Kalsel-Android1
Database Sigra Calya Community (SCANITY) Chapter Kalimantan Selatan
SCANITY-Kalsel-Android1/
├── .github/
│   └── workflows/
│       └── build-apk.yml
├── app/
│   ├── build.gradle
│   └── src/
│       └── main/
│           ├── AndroidManifest.xml
│           ├── java/com/scanity/kalsel/
│           │   └── MainActivity.java
│           └── res/
│               ├── drawable/
│               │   └── scanity_logo.png
│               ├── layout/
│               │   └── activity_main.xml
│               └── values/
│                   ├── colors.xml
│                   └── themes.xml
├── build.gradle
├── settings.gradle
└── gradle.properties
pluginManagement {
    repositories {
        google()
        mavenCentral()
        gradlePluginPortal()
    }
}

dependencyResolutionManagement {
    repositoriesMode.set(RepositoriesMode.FAIL_ON_PROJECT_REPOS)
    repositories {
        google()
        mavenCentral()
    }
}

rootProject.name = "SCANITY-Kalsel"
include(":app")
plugins {
    id 'com.android.application' version '8.6.1' apply false
}
plugins {
    id 'com.android.application'
}

android {
    namespace 'com.scanity.kalsel'
    compileSdk 35

    defaultConfig {
        applicationId "com.scanity.kalsel"
        minSdk 23
        targetSdk 35
        versionCode 2
        versionName "2.0"
    }
}

dependencies {
    implementation 'androidx.appcompat:appcompat:1.7.0'
    implementation 'com.google.android.material:material:1.12.0'
    implementation 'androidx.recyclerview:recyclerview:1.3.2'
}
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android">

    <application
        android:theme="@style/Theme.SCANITYKalsel"
        android:label="SCANITY Kalsel"
        android:icon="@drawable/scanity_logo"
        android:allowBackup="true">

        <activity
            android:name=".MainActivity"
            android:exported="true">

            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>

        </activity>

    </application>

</manifest>
package com.scanity.kalsel;

import android.app.AlertDialog;
import android.os.Bundle;
import android.content.Context;
import android.content.SharedPreferences;
import android.view.View;
import android.widget.*;

import androidx.appcompat.app.AppCompatActivity;

import java.util.ArrayList;

public class MainActivity extends AppCompatActivity {

    LinearLayout container;
    ArrayList<String> anggota = new ArrayList<>();
    SharedPreferences prefs;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        setContentView(R.layout.activity_main);

        container = findViewById(R.id.container);

        prefs = getSharedPreferences("scanity_data", Context.MODE_PRIVATE);

        tampilkanMenu();
    }

    private void tampilkanMenu() {

        container.removeAllViews();

        TextView title = new TextView(this);
        title.setText("SCANITY KALIMANTAN SELATAN");
        title.setTextSize(24);
        title.setPadding(20, 30, 20, 30);

        container.addView(title);

        Button anggotaBtn = new Button(this);
        anggotaBtn.setText("DATA ANGGOTA");
        container.addView(anggotaBtn);

        Button tambahBtn = new Button(this);
        tambahBtn.setText("TAMBAH ANGGOTA");
        container.addView(tambahBtn);

        Button korwilBtn = new Button(this);
        korwilBtn.setText("DATA KORWIL");
        container.addView(korwilBtn);

        Button adminBtn = new Button(this);
        adminBtn.setText("ADMIN");
        container.addView(adminBtn);

        anggotaBtn.setOnClickListener(v -> tampilkanAnggota());

        tambahBtn.setOnClickListener(v -> tambahAnggota());

        korwilBtn.setOnClickListener(v -> tampilkanKorwil());

        adminBtn.setOnClickListener(v -> tampilkanAdmin());
    }

    private void tampilkanAnggota() {

        container.removeAllViews();

        TextView title = new TextView(this);
        title.setText("DATA ANGGOTA SCANITY");
        title.setTextSize(22);
        title.setPadding(20, 20, 20, 20);

        container.addView(title);

        String data = prefs.getString("anggota", "");

        if (data.isEmpty()) {

            TextView kosong = new TextView(this);
            kosong.setText("Belum ada data anggota.");
            kosong.setTextSize(18);

            container.addView(kosong);

        } else {

            String[] daftar = data.split("\\|");

            for (String anggotaData : daftar) {

                TextView item = new TextView(this);

                item.setText(anggotaData);
                item.setTextSize(17);
                item.setPadding(20, 20, 20, 20);

                container.addView(item);
            }
        }

        Button kembali = new Button(this);
        kembali.setText("KEMBALI");

        container.addView(kembali);

        kembali.setOnClickListener(v -> tampilkanMenu());
    }

    private void tambahAnggota() {

        LinearLayout form = new LinearLayout(this);
        form.setOrientation(LinearLayout.VERTICAL);
        form.setPadding(30, 10, 30, 10);

        EditText nama = new EditText(this);
        nama.setHint("Nama Lengkap");

        EditText nomor = new EditText(this);
        nomor.setHint("Nomor Anggota");

        EditText kendaraan = new EditText(this);
        kendaraan.setHint("Sigra / Calya");

        EditText plat = new EditText(this);
        plat.setHint("Nomor Polisi");

        EditText korwil = new EditText(this);
        korwil.setHint("Korwil");

        form.addView(nama);
        form.addView(nomor);
        form.addView(kendaraan);
        form.addView(plat);
        form.addView(korwil);

        new AlertDialog.Builder(this)
                .setTitle("Tambah Anggota")
                .setView(form)
                .setPositiveButton("SIMPAN", (dialog, which) -> {

                    String data =
                            "ID: " + nomor.getText().toString()
                            + "\nNama: " + nama.getText().toString()
                            + "\nKendaraan: " + kendaraan.getText().toString()
                            + "\nNo Polisi: " + plat.getText().toString()
                            + "\nKorwil: " + korwil.getText().toString();

                    String lama = prefs.getString("anggota", "");

                    if (!lama.isEmpty()) {
                        lama += "|";
                    }

                    lama += data;

                    prefs.edit()
                            .putString("anggota", lama)
                            .apply();

                    Toast.makeText(
                            this,
                            "Anggota berhasil ditambahkan",
                            Toast.LENGTH_SHORT
                    ).show();

                })
                .setNegativeButton("BATAL", null)
                .show();
    }

    private void tampilkanKorwil() {

        new AlertDialog.Builder(this)
                .setTitle("KORWIL SCANITY KALSEL")
                .setItems(
                        new String[]{
                                "Tatanko",
                                "Babam",
                                "Banam",
                                "Balat"
                        },
                        null
                )
                .show();
    }

    private void tampilkanAdmin() {

        new AlertDialog.Builder(this)
                .setTitle("ADMIN SCANITY")
                .setMessage(
                        "Username: admin\n" +
                        "Password awal: scanity2026"
                )
                .setPositiveButton("OK", null)
                .show();
    }
}
<?xml version="1.0" encoding="utf-8"?>

<ScrollView
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <LinearLayout
        android:id="@+id/container"
        android:orientation="vertical"
        android:padding="20dp"
        android:layout_width="match_parent"
        android:layout_height="wrap_content">

    </LinearLayout>

</ScrollView>
