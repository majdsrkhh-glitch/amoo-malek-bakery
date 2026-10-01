# amoo-malek-bakery
from pathlib import Path
import zipfile, textwrap, shutil

root = Path("/mnt/data/NanvaiiMalek_Android")
if root.exists():
    shutil.rmtree(root)

files = {
"settings.gradle": """pluginManagement {
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
rootProject.name = "NanvaiiMalek"
include(":app")
""",
"build.gradle": """plugins {
    id 'com.android.application' version '8.5.2' apply false
}
""",
"gradle.properties": """org.gradle.jvmargs=-Xmx2048m -Dfile.encoding=UTF-8
android.useAndroidX=true
""",
"app/build.gradle": """plugins {
    id 'com.android.application'
}

android {
    namespace 'com.nanvaiimalek.app'
    compileSdk 35

    defaultConfig {
        applicationId 'com.nanvaiimalek.app'
        minSdk 23
        targetSdk 35
        versionCode 1
        versionName '1.0'
    }
}

dependencies {
    implementation 'androidx.appcompat:appcompat:1.7.0'
    implementation 'com.google.android.material:material:1.12.0'
}
""",
"app/src/main/AndroidManifest.xml": """<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android">
    <application
        android:theme="@style/Theme.NanvaiiMalek"
        android:label="@string/app_name"
        android:allowBackup="true"
        android:supportsRtl="true">
        <activity
            android:name=".MainActivity"
            android:screenOrientation="portrait"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN"/>
                <category android:name="android.intent.category.LAUNCHER"/>
            </intent-filter>
        </activity>
    </application>
</manifest>
""",
"app/src/main/res/values/strings.xml": """<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="app_name">نانوایی مالک</string>
</resources>
""",
"app/src/main/res/values/styles.xml": """<?xml version="1.0" encoding="utf-8"?>
<resources>
    <style name="Theme.NanvaiiMalek" parent="Theme.Material3.DayNight.NoActionBar">
        <item name="android:fontFamily">sans</item>
        <item name="android:windowLightStatusBar">true</item>
        <item name="android:statusBarColor">#F7F1E6</item>
        <item name="android:navigationBarColor">#F7F1E6</item>
    </style>
</resources>
""",
"app/src/main/java/com/nanvaiimalek/app/MainActivity.java": """package com.nanvaiimalek.app;

import android.app.Activity;
import android.os.Bundle;
import android.graphics.Color;
import android.graphics.Typeface;
import android.view.Gravity;
import android.view.View;
import android.widget.*;

public class MainActivity extends Activity {
    int cream = Color.rgb(247,241,230);
    int dark = Color.rgb(40,38,34);
    int mustard = Color.rgb(218,164,55);

    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        showHome();
    }

    TextView text(String s, float size, boolean bold) {
        TextView t = new TextView(this);
        t.setText(s);
        t.setTextSize(size);
        t.setTextColor(dark);
        t.setGravity(Gravity.CENTER);
        if (bold) t.setTypeface(Typeface.DEFAULT, Typeface.BOLD);
        t.setPadding(24, 18, 24, 18);
        return t;
    }

    void showHome() {
        LinearLayout root = new LinearLayout(this);
        root.setOrientation(LinearLayout.VERTICAL);
        root.setGravity(Gravity.CENTER_HORIZONTAL);
        root.setPadding(24, 40, 24, 24);
        root.setBackgroundColor(cream);

        TextView logo = text("🥖", 52, false);
        root.addView(logo, new LinearLayout.LayoutParams(-1, 90));

        root.addView(text("نانوایی مالک", 30, true),
                new LinearLayout.LayoutParams(-1, 70));

        TextView subtitle = text("نان عربی تازه و گرم", 18, false);
        root.addView(subtitle);

        LinearLayout card = new LinearLayout(this);
        card.setOrientation(LinearLayout.VERTICAL);
        card.setGravity(Gravity.CENTER);
        card.setPadding(20, 20, 20, 20);
        card.setBackgroundColor(Color.WHITE);

        card.addView(text("نان عربی", 24, true));
        card.addView(text("۷٬۰۰۰ تومان", 21, true));

        Button order = new Button(this);
        order.setText("ثبت سفارش");
        order.setTextSize(17);
        order.setTextColor(Color.WHITE);
        order.setBackgroundColor(mustard);
        order.setOnClickListener(v -> showInfo());
        card.addView(order, new LinearLayout.LayoutParams(-1, 60));

        LinearLayout.LayoutParams cp = new LinearLayout.LayoutParams(-1, 250);
        cp.setMargins(0, 30, 0, 20);
        root.addView(card, cp);

        root.addView(text("📞 ۰۹۰۱۱۶۷۳۵۴۶", 17, false));
        root.addView(text("پردیس ۱، خیابان ملک، بین گلبرگ ۹ و ۱۰\nروبروی مجتمع رضوان ۳، بلوک B", 16, false));

        setContentView(root);
    }

    void showInfo() {
        new AlertDialog.Builder(this)
            .setTitle("نانوایی مالک")
            .setMessage("نان عربی\nقیمت: ۷٬۰۰۰ تومان\n\nبرای سفارش با شماره ۰۹۰۱۱۶۷۳۵۴۶ تماس بگیرید.")
            .setPositiveButton("باشه", null)
            .show();
    }
}
""",
}

# Add missing import for AlertDialog
files["app/src/main/java/com/nanvaiimalek/app/MainActivity.java"] = files["app/src/main/java/com/nanvaiimalek/app/MainActivity.java"].replace(
    "import android.app.Activity;", "import android.app.Activity;\nimport android.app.AlertDialog;"
)

for rel, content in files.items():
    p = root / rel
    p.parent.mkdir(parents=True, exist_ok=True)
    p.write_text(content, encoding="utf-8")

zip_path = Path("/mnt/data/NanvaiiMalek_Android_Project.zip")
if zip_path.exists():
    zip_path.unlink()

with zipfile.ZipFile(zip_path, "w", zipfile.ZIP_DEFLATED) as z:
    for p in root.rglob("*"):
        if p.is_file():
            z.write(p, p.relative_to(root))

print(f"پروژه آماده شد: {zip_path}")
print(f"[دانلود پروژه نانوایی مالک](sandbox:/mnt/data/NanvaiiMalek_Android_Project.zip)")
