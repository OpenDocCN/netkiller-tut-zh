# 部分 III. Android 9 Pie

## 第 35 章 Android Studio

## 卸载 Android Studio

卸载 Mac Android Studio

```

rm -Rf /Applications/Android\ Studio.app
rm -Rf ~/Library/Preferences/AndroidStudio*
rm ~/Library/Preferences/com.google.android.studio.plist
rm -Rf ~/Library/Application\ Support/AndroidStudio*
rm -Rf ~/Library/Logs/AndroidStudio*
rm -Rf ~/Library/Caches/AndroidStudio*	
rm -Rf ~/Library/Android*

```

删除 Projects

```

rm -Rf ~/AndroidStudioProjects	

```

删除 gradle

```

rm -rf ~/.gradle	

```

卸载 Android Virtual Devices(AVDs) and *.keystore.

```

rm -Rf ~/.android	

```

## 代码格式化

```

Option + Command + L		

```

## 设置兼容最低 SDK 版本

Android 9 Pie 的设置方法

打开 build.gradle 修改 minSdkVersion 项，这里的 26 表示 Android 8.0

```

apply plugin: 'com.android.application'

android {
    compileSdkVersion 28
    defaultConfig {
        applicationId "cn.netkiller.video"
        minSdkVersion 26
        targetSdkVersion 28
        versionCode 1
        versionName "1.0"
        testInstrumentationRunner "android.support.test.runner.AndroidJUnitRunner"
    }
    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
        }
    }
}

dependencies {
    implementation fileTree(dir: 'libs', include: ['*.jar'])
    implementation 'com.android.support:appcompat-v7:28.0.0'
    implementation 'com.android.support.constraint:constraint-layout:1.1.3'
    testImplementation 'junit:junit:4.12'
    androidTestImplementation 'com.android.support.test:runner:1.0.2'
    androidTestImplementation 'com.android.support.test.espresso:espresso-core:3.0.2'
}

```

## SDK Tools

```

wget https://dl.google.com/android/repository/sdk-tools-linux-4333796.zip
unzip sdk-tools-linux-4333796.zip
cd tools/

```

查看帮助信息

```

neo@ubuntu:~/tmp/tools$ bin/sdkmanager --help
Usage: 
  sdkmanager [--uninstall] [<common args>] [--package_file=<file>] [<packages>...]
  sdkmanager --update [<common args>]
  sdkmanager --list [<common args>]
  sdkmanager --licenses [<common args>]
  sdkmanager --version

With --install (optional), installs or updates packages.
    By default, the listed packages are installed or (if already installed)
    updated to the latest version.
With --uninstall, uninstall the listed packages.

    <package> is a sdk-style path (e.g. "build-tools;23.0.0" or
             "platforms;android-23").
    <package-file> is a text file where each line is a sdk-style path
                   of a package to install or uninstall.
    Multiple --package_file arguments may be specified in combination
    with explicit paths.

With --update, all installed packages are updated to the latest version.

With --list, all installed and available packages are printed out.

With --licenses, show and offer the option to accept licenses for all
     available packages that have not already been accepted.

With --version, prints the current version of sdkmanager.

Common Arguments:
    --sdk_root=<sdkRootPath>: Use the specified SDK root instead of the SDK 
                              containing this tool

    --channel=<channelId>: Include packages in channels up to <channelId>.
                           Common channels are:
                           0 (Stable), 1 (Beta), 2 (Dev), and 3 (Canary).

    --include_obsolete: With --list, show obsolete packages in the
                        package listing. With --update, update obsolete
                        packages as well as non-obsolete.

    --no_https: Force all connections to use http rather than https.

    --proxy=<http | socks>: Connect via a proxy of the given type.

    --proxy_host=<IP or DNS address>: IP or DNS address of the proxy to use.

    --proxy_port=<port #>: Proxy port to connect to.

    --verbose: Enable verbose output.

* If the env var REPO_OS_OVERRIDE is set to "windows",
  "macosx", or "linux", packages will be downloaded for that OS.

```

### 接受 License

```

$ yes|tools/bin/sdkmanager --licenses			

```

### 查看 SDK 列表

```

neo@ubuntu:~/tmp/tools$ bin/sdkmanager --list | grep "platforms;"
Warning: File /home/neo/.android/repositories.cfg could not be loaded.
  platforms;android-26 | 2       | Android SDK Platform 26  | platforms/android-26/
  platforms;android-10                                                                     | 2            | Android SDK Platform 10                                             
  platforms;android-11                                                                     | 2            | Android SDK Platform 11                                             
  platforms;android-12                                                                     | 3            | Android SDK Platform 12                                             
  platforms;android-13                                                                     | 1            | Android SDK Platform 13                                             
  platforms;android-14                                                                     | 4            | Android SDK Platform 14                                             
  platforms;android-15                                                                     | 5            | Android SDK Platform 15                                             
  platforms;android-16                                                                     | 5            | Android SDK Platform 16                                             
  platforms;android-17                                                                     | 3            | Android SDK Platform 17                                             
  platforms;android-18                                                                     | 3            | Android SDK Platform 18                                             
  platforms;android-19                                                                     | 4            | Android SDK Platform 19                                             
  platforms;android-20                                                                     | 2            | Android SDK Platform 20                                             
  platforms;android-21                                                                     | 2            | Android SDK Platform 21                                             
  platforms;android-22                                                                     | 2            | Android SDK Platform 22                                             
  platforms;android-23                                                                     | 3            | Android SDK Platform 23                                             
  platforms;android-24                                                                     | 2            | Android SDK Platform 24                                             
  platforms;android-25                                                                     | 3            | Android SDK Platform 25                                             
  platforms;android-26                                                                     | 2            | Android SDK Platform 26                                             
  platforms;android-27                                                                     | 3            | Android SDK Platform 27                                             
  platforms;android-28                                                                     | 6            | Android SDK Platform 28                                             
  platforms;android-7                                                                      | 3            | Android SDK Platform 7                                              
  platforms;android-8                                                                      | 3            | Android SDK Platform 8                                              
  platforms;android-9                                                                      | 2            | Android SDK Platform 9      			

```

### 按照 Android SDK

最新版本 Android 9 SDK

```

bin/sdkmanager "platform-tools" "platforms;android-28"			

```

按照旧版本的 Android 8 SDK

```

bin/sdkmanager "platforms;android-26"			

```

## 命令行操作

会同时生成 debug 和 release 两个包

```

./gradlew assemble

```

只生成 release 的包

```

./gradlew assembleRelease

```

## 第 36 章 AndroidManifest.xml

```

<uses-sdk
        android:minSdkVersion="26"
        android:targetSdkVersion="28" />		

```

## 开启网络

```

<uses-permission android:name="android.permission.INTERNET" />		

```

```

<?xml version="1.0" encoding="utf-8"?>
<manifest 
    package="cn.netkiller.android.myapplication">

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/AppTheme">
        <activity
            android:name=".MainActivity"
            android:label="@string/app_name">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />

            </intent-filter>
        </activity>

    </application>
    <uses-permission android:name="android.permission.INTERNET" />
</manifest>	

```

## 文件存储权限

```

	<uses-permission android:name="android.permission.MOUNT_UNMOUNT_FILESYSTEMS" />
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />		

```

## 相机权限

```

	<uses-permission android:name="android.permission.CAMERA" />
    <uses-feature android:name="android.hardware.camera" />		

```

## GPS 定位权限

```

<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
<uses-permission android:name="android.permission.CHANGE_WIFI_STATE" />
<uses-permission android:name="android.permission.INTERNET" />		

```

## 第 37 章 配置文件

## *.properties 文件

创建 properties 文件 res/config/development.properties

```

api_url=https://api.netkiller.cn/v1/
api_key=123456		

```

```

package cn.netkiller.app;

import android.content.Context;
import android.content.res.Resources;
import android.util.Log;

import java.io.IOException;
import java.io.InputStream;
import java.util.Properties;

public final class Config {
    private static final String TAG = "Config";

    public static String getKey(Context context, String name) {
        Resources resources = context.getResources();

        try {
            InputStream rawResource = resources.openRawResource(R.config.development);
            Properties properties = new Properties();
            properties.load(rawResource);
            return properties.getProperty(name);
        } catch (Resources.NotFoundException e) {
            Log.e(TAG, "Unable to find the config file: " + e.getMessage());
        } catch (IOException e) {
            Log.e(TAG, "Failed to open config file.");
        }

        return null;
    }
}		

```

```

String apiUrl = Config.getKey(this, "api_url");
String apiKey = Config.getKey(this, "api_key");		

```

## 再 AndroidManifest.xml 使用 meta-data element 定义

```

...
<application ...>
    ...
	...
    <meta-data android:name="api_url" android:value="https://api.netkiller.cn/v1/"/>
    <meta-data android:name="api_key" android:value="123456"/>
</application>		

```

```

public static String getMetaData(Context context, String name) {
    try {
        ApplicationInfo ai = context.getPackageManager().getApplicationInfo(context.getPackageName(), PackageManager.GET_META_DATA);
        Bundle bundle = ai.metaData;
        return bundle.getString(name);
    } catch (PackageManager.NameNotFoundException e) {
        Log.e(TAG, "Unable to load meta-data: " + e.getMessage());
    }
    return null;
}		

```

```

String apiUrl = getMetaData(this, "api_url");
String apiKey = getMetaData(this, "api_key");		

```

## 再 build.gradle 文件中配置 productFlavors

```

productFlavors {
    prod {
        buildConfigField 'String', 'API_URL', '"https://api.netkiller.cn/v1/"'
        buildConfigField 'String', 'API_KEY', '"123456"'
    }
}

```

引用 config 方法

```

String apiUrl = BuildConfig.API_URL;
String apiKey = BuildConfig.API_KEY;

```

## 第 38 章 UI Layout

## 切换 UI

```

setContentView(R.layout.view);			

```

### startActivity()

```

<?xml version="1.0" encoding="utf-8"?>
<manifest 
     <application android:label="Test">

		...
		...
        <activity android:name=".WriteActivity"></activity>

    </application>

</manifest>		

```

```

		Button button = (Button) findViewById(R.id.writeButton);

        button.setOnClickListener(new View.OnClickListener() {
            public void onClick(View v) {
                setContentView(R.layout.activity_write);
                Intent intent = new Intent(MainActivity.this,WriteActivity.class);
                startActivity(intent);
            }
        });		

```

### Activity 间数据传递

```

Intent it = new Intent(Activity.Main.this, Activity2.class);
Bundle bundle=new Bundle();
bundle.putString("name", "This is from MainActivity!");
it.putExtras(bundle);
startActivity(it);		

```

获取数据

```

Bundle bundle=getIntent().getExtras();
String name=bundle.getString("name");		

```

## Button

```

public class MainActivity extends AppCompatActivity {

    //我们需要自己写一个常量作为 requestCode，在请求 result 时传递进去
    public static final int REQUEST_CODE_NORMAL = 100;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        Button button = (Button) findViewById(R.id.Button);

        button.setOnClickListener(new View.OnClickListener() {
            public void onClick(View view) {
				startActivityForResult(new Intent(this,SecondActivity.class),REQUEST_CODE_NORMAL);
            }
        });	
    }

    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        if (requestCode == REQUEST_CODE_NORMAL) {
        	//获得 Result 数据并处理
            ...
            ...
        }
    }
}

```

```

public class SecondActivity extends AppCompatActivity {   

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.save);

     	Button button = (Button) findViewById(R.id.SaveButton);

        button.setOnClickListener(new View.OnClickListener() {
            public void onClick(View view) {
		        Intent intent = new Intent(this,MainResultActivity.class);
		        intent.putExtra("content",etContent.getText().toString());
		        setResult(1,intent);
		        //发送 Result 数据给请求方，然后 finish（）
		        finish();        
            }
        });	
    }
}		

```

### 启用禁用

```

myButton.setEnabled(false);			

```

### 实现 OnClickListener 接口

```

package cn.netkiller.video;

import android.content.Intent;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;

public class MainActivity extends AppCompatActivity implements View.OnClickListener {

    private Button buttonVideoView;
    private Button buttonSurfaceView;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        buttonVideoView = (Button) findViewById(R.id.buttonVideoView);
        buttonVideoView.setOnClickListener(this);

        buttonSurfaceView = (Button) findViewById(R.id.buttonSurfaceView);
        buttonSurfaceView.setOnClickListener(this);
    }

    @Override
    public void onClick(View v) {
        Intent intent;
        switch (v.getId()) {
            case R.id.buttonVideoView:
                startActivity(new Intent(MainActivity.this, VideoViewActivity.class));
                break;
            case R.id.buttonSurfaceView:

                break;
            default:
                break;
        }
    }
}			

```

## ListView

### Array

```

        String[] list = Arrays.asList("Apple", "Banana", "Orange", "Watermelon",
                "Pear", "Grape", "Pineapple", "Strawberry", "Cherry", "Mango";
        ArrayAdapter<String> adapter = new ArrayAdapter<String>(MainActivity.this, android.R.layout.simple_list_item_1,data);
        ListView listView = (ListView) findViewById(R.id.history);
        listView.setAdapter(adapter);	

```

```

    <ListView
        android:id="@+id/history"
        android:layout_width="368dp"
        android:layout_height="444dp"
        android:scrollbars="horizontal"
        tools:layout_editor_absoluteX="8dp"
        tools:layout_editor_absoluteY="59dp" />	

```

### List

```

		List<String> list = Arrays.asList("Apple", "Banana", "Orange", "Watermelon","Pear", "Grape", "Pineapple", "Strawberry", "Cherry", "Mango");
        ArrayAdapter<String> adapter = new ArrayAdapter<String>(MainActivity.this, android.R.layout.simple_list_item_1,list);
        ListView listView = (ListView) findViewById(R.id.history);
        listView.setAdapter(adapter);        		

```

### setOnItemClickListener()

```

        List<String> list = Arrays.asList("Text 文本", "URL 网址", "电话号码", "短信","开启应用", "地址", "日历", "图片", "邮箱", "GPS 坐标");
        ArrayAdapter<String> adapter = new ArrayAdapter<String>(this, android.R.layout.simple_list_item_1,list);
        final ListView listView = (ListView) findViewById(R.id.schemaList);
        listView.setAdapter(adapter);

        listView.setOnItemClickListener(new AdapterView.OnItemClickListener() {
            @Override
            public void onItemClick(AdapterView<?> parent, View view, int position, long id) {

                String text = listView.getItemAtPosition(position)+"";
                Log.e("WRITE","position="+position+", text="+text);
            }
        });	

```

### 用接口方法实现

```

public class MainActivity extends Activity implements OnItemClickListener, OnScrollListener

```

```

		List<String> list = Arrays.asList("Text 文本", "URL 网址", "电话号码", "短信","开启应用", "地址", "日历", "图片", "邮箱", "GPS 坐标");
        ArrayAdapter<String> adapter = new ArrayAdapter<String>(this, android.R.layout.simple_list_item_1,list);
        final ListView listView = (ListView) findViewById(R.id.schemaList);
        listView.setAdapter(adapter);

        listView.setOnItemClickListener(this);
        listView.setOnScrollListener(this);		

```

```

	@Override
    public void onItemClick(AdapterView<?> parent, View view, int position, long id) {
        String text = listView.getItemAtPosition(position)+"";
        Log.e("WRITE","position="+position+", text="+text);
    }		

```

```

	@Override
    public void onScrollStateChanged(AbsListView view, int scrollState) {

        switch (scrollState) {
        case SCROLL_STATE_FLING:
            Log.i("tag", "用户手指离开屏幕后，因惯性继续滑动");
            Map<String,Object>map = new HashMap<String,Object>();
            map.put("icon", R.drawable.ic_launcher);
            map.put("text", "新增加项目");
            dataList.add(map);  
            adapter.notifyDataSetChanged(); 
            break;
        case SCROLL_STATE_IDLE:
            Log.i("tag","已经停止滑动");
            break;      
        case SCROLL_STATE_TOUCH_SCROLL:
            Log.i("tag", "手指未离开屏幕，屏幕继续滑动");
            break;
        }   
    }		

```

## Switch

```

    <Switch
        android:id="@+id/switchWrite"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginStart="8dp"
        android:layout_marginTop="8dp"
        android:layout_marginEnd="8dp"
        android:layout_marginBottom="8dp"
        android:text="NDEF Message write"
        android:textOff="NDEF Message write Off"
        android:textOn="NDEF Message write On"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toStartOf="@+id/writeButton"
        app:layout_constraintHorizontal_bias="0.076"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/ndefWrite"
        app:layout_constraintVertical_bias="0.087" />	

```

```

		final Switch switchWrite = (Switch) findViewById(R.id.switchWrite);

        switchWrite.setOnCheckedChangeListener(new CompoundButton.OnCheckedChangeListener() {
            @Override
            public void onCheckedChanged(CompoundButton buttonView, boolean isChecked) {

                if(isChecked) {
                    status.setText(switchWrite.getTextOn().toString());
                } else {
                    status.setText(switchWrite.getTextOff().toString());
                }
            }
        });	

```

```

	if(switchWrite.isChecked()){

    }	

```

## GardView

## GridView

## ProgressBar

## ImageView

## Fragment

## Dialog

## Menu

## 第 39 章 Toast

## 默认样式

```

Toast.makeText(getApplicationContext(), "默认 Toast 样式", Toast.LENGTH_SHORT).show();	  

```

## 自定义样式

```

toast = Toast.makeText(getApplicationContext(),"自定义位置 Toast", Toast.LENGTH_LONG);
   toast.setGravity(Gravity.CENTER, 0, 0);
   toast.show();

```

## 带有图片的样式

```

toast = Toast.makeText(getApplicationContext(),"带图片的 Toast", Toast.LENGTH_LONG);
   toast.setGravity(Gravity.CENTER, 0, 0);
   LinearLayout toastView = (LinearLayout) toast.getView();
   ImageView imageView = new ImageView(getApplicationContext());
   imageView.setImageResource(R.drawable.icon);
   toastView.addView(imageView, 0);
   toast.show(); 		

```

## 第 40 章 Environment

```

Environment.getExternalStorageState().equals(Environment.MEDIA_MOUNTED);		

```

```

Environment.getExternalStorageDirectory().getAbsolutePath();		

```

```

        Log.i("Environment", "getRootDirectory(): "
                + Environment.getRootDirectory().toString());
        Log.i("Environment", "getDataDirectory(): "
                + Environment.getDataDirectory().toString());
        Log.i("Environment", "getDownloadCacheDirectory(): "
                + Environment.getDownloadCacheDirectory().toString());
        Log.i("Environment", "getExternalStorageDirectory(): "
                + Environment.getExternalStorageDirectory().toString());

        Log.i("Environment","getExternalStoragePublicDirectory(Environment.DIRECTORY_PICTURES): "
                        + Environment.getExternalStoragePublicDirectory(
                        Environment.DIRECTORY_PICTURES).toString());

```

## 第 41 章 Schedule 计划任务

## Time 和 TimerTask 定时刷新

```

package cn.netkiller.okhttp;

import android.os.Handler;
import android.os.Message;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.widget.TextView;

import java.text.DateFormat;
import java.text.SimpleDateFormat;
import java.util.Date;
import java.util.Timer;
import java.util.TimerTask;

public class ScheduleActivity extends AppCompatActivity {

    private TextView clock;

    final Handler handler = new Handler() {
        public void handleMessage(Message msg) {
            super.handleMessage(msg);
            switch (msg.what) {
                case 1:
                    update(msg.obj.toString());
                    break;
            }
        }

        void update(String c) {

            clock.setText(c);
        }
    };

    Timer timer = new Timer();
    TimerTask task = new TimerTask() {
        DateFormat dateFormat = new SimpleDateFormat("yyyy/MM/dd HH:mm:ss");

        public void run() {
            Message message = new Message();
            message.what = 1;
            message.obj = dateFormat.format(new Date());
            handler.sendMessage(message);
        }
    };

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_schedule);

        clock = (TextView) findViewById(R.id.clock);
        clock.setText("Today is ...");
        timer.schedule(task, 1000 * 5, 1000); //启动 timer

    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        if (timer != null) {
            timer.cancel();
            timer = null;
        }

    }

}

```

## 使用 Runnable 和 Handler 实现定时执行

原理是使用 handler.postDelayed 延迟 Runnable 的运行时间

```

package cn.netkiller.okhttp;

import android.os.Handler;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.widget.TextView;

import java.text.DateFormat;
import java.text.SimpleDateFormat;
import java.util.Date;

public class RunnableActivity extends AppCompatActivity {

    private Handler handler = new Handler();
    private Runnable runnable = new Runnable() {
        public void run() {
            this.update();
            handler.postDelayed(this, 1000);// 1000 ms = 1s 间隔 1 秒
        }

        void update() {
            DateFormat dateFormat = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
            time.setText(dateFormat.format(new Date()));
        }
    };
    private TextView time;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_runnable);

        time = (TextView) findViewById(R.id.time);
        time.setText("Start...");

        handler.postDelayed(runnable, 1000 * 5); // 5 秒后开始
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        handler.removeCallbacks(runnable);
    }

}		

```

## 第 42 章 Internationalization i18n with Android (国际化)

## 创建国际化文件

进入 Android Studio 文件菜单 File -> New -> New Resource File

| ![](img/NewResourceFile.png) |

在左侧列表中找到 Locale 点击 “>>” 按钮

| ![](img/Locale.png) |

选择国家后，点击 OK 按钮即可。

| ![](img/Locale-res.png) |

资源文件夹中已经显示出国际化文件，上面并有对应的国旗。

查看项目文件夹

```

neo@MacBook-Pro ~/AndroidStudioProjects/locale % find app/src/main/res | grep values
app/src/main/res/values-zh-rCN
app/src/main/res/values-zh-rCN/strings.xml
app/src/main/res/values
app/src/main/res/values/colors.xml
app/src/main/res/values/dimens.xml
app/src/main/res/values/styles.xml
app/src/main/res/values/strings.xml		

```

## strings.xml 文件

```

<resources>
    <string name="app_name">Netkiller</string>
    <string name="title_home">Home</string>
    <string name="title_dashboard">Dashboard</string>
    <string name="title_notifications">Notifications</string>
</resources>		

```

## 翻译语言

再 res/values/strings 目录上面单击鼠标右键，打开 Open Translations Editor 翻译编辑器。

| ![](img/OpenTranslationsEditor.png) |

单击地球图标，添加 zh-cn 语言

| ![](img/TranslationsEditor.png) |

现在就可以对照翻译语言包文件了。

## 引用国际化文件

```

String test = "Sign Up";

String test = getResources().getString(R.string.sign_up);		

```

```

R.string.browserSentence = "You are using $1%s to browse the Internet.";

String browser = getString(R.string.browserSentence, browser.getBrowser());		

```

```

TextView textView = new TextView(this);
TextView.setText(“Sign Up”);

TextView textView = new TextView(this);
textView.setText(R.string.sign_up);		

```

```

<TextView
 android:layout_width="wrap_content"
 android:layout_height="wrap_content"
 android:text="Hello World!" />

<TextView
 android:layout_width="wrap_content"
 android:layout_height="wrap_content"
 android:text="@string/hello_world" />

```

## 切换语言

```

        DisplayMetrics metrics = getResources().getDisplayMetrics();
        Configuration configuration = getResources().getConfiguration();
        configuration.setLocale(locale);
        getResources().updateConfiguration(configuration, metrics);

        //重新启动 Activity
        Intent intent = new Intent(this, MainActivity.class);
        intent.setFlags(Intent.FLAG_ACTIVITY_NEW_TASK | Intent.FLAG_ACTIVITY_CLEAR_TASK);
        startActivity(intent);

```

## 第 43 章 存储

## SharedPreferences

SharedPreferences 是 Android 中的数据存储技术，常用来存储一些轻量级的数据。

实际上 SharedPreferences 是 NoSQL 数据库，用于处理的 key-value 键值对存储，类似 Windows 系统的注册表，Linux 系统里的 Berkeley DB (bdb) 以及衍生出的 dba,mdb 这类 hash 表数据库。

### 操作模式

```

Context.MODE_PRIVATE：为默认操作模式,代表该文件是私有数据,只能被应用本身访问,在该模式下,写入的内容会覆盖原文件的内容
Context.MODE_APPEND：模式会检查文件是否存在,存在就往文件追加内容,否则就创建新文件.
Context.MODE_WORLD_READABLE 和 Context.MODE_WORLD_WRITEABLE 用来控制其他应用是否有权限读写该文件.
MODE_WORLD_READABLE：表示当前文件可以被其他应用读取.
MODE_WORLD_WRITEABLE：表示当前文件可以被其他应用写入.		

```

### 保存数据

```

        Button buttonPut = (Button) findViewById(R.id.buttonPut);

        buttonPut.setOnClickListener(new View.OnClickListener() {
            public void onClick(View view) {

                //实例化 SharedPreferences 对象
                SharedPreferences sharedPreferences = getSharedPreferences("test", Activity.MODE_PRIVATE);

                //实例化 SharedPreferences.Editor 对象
                SharedPreferences.Editor editor = sharedPreferences.edit();

                //用 putString 的方法保存数据
                editor.putString("name", "Neo");
                editor.putString("nickname", "netkiller");
                editor.putBoolean("sex", true);
                editor.putInt("age", 30);
                editor.putFloat("tall", 180.23f);
                Set<String> books = new HashSet<String>();
                books.add("Netkiller Linux 手札");
                books.add("Netkiller Java 手札");
                books.add("Netkiller Android 手札");
                editor.putStringSet("books", books);

                //提交当前数据
                editor.commit();

            }
        });

```

### 读取数据

```

        Button buttonGet = (Button) findViewById(R.id.buttonGet);

        buttonGet.setOnClickListener(new View.OnClickListener() {
            public void onClick(View view) {

                //实例化 SharedPreferences 对象
                SharedPreferences sharedPreferences = getSharedPreferences("test", Activity.MODE_PRIVATE);

                //使用 getString 方法获得 value，
                String name = sharedPreferences.getString("name", "");
                String nickname = sharedPreferences.getString("nickname", "");
                boolean sex = sharedPreferences.getBoolean("sex", false);
                int age = sharedPreferences.getInt("age", 0);
                float tall = sharedPreferences.getFloat("tall", 0f);
                Set<String> books = sharedPreferences.getStringSet("books", null);

                Log.i("SharedPreferences", String.format("%s,%s,%s,%s,%s,%s", name, nickname, sex, age, tall, books.toString()));

            }
        });

```

### 通过 key 查询数据是否存在

```

		SharedPreferences sharedPreferences = getSharedPreferences("test", Activity.MODE_PRIVATE);
		if (sharedPreferences.contains("nickname")) {
            Log.i("SharedPreferences", sharedPreferences.getString("nickname", ""));
        }else{
            Log.i("SharedPreferences", "key: nickname 不存在");
        }

```

### 删除数据

```

	SharedPreferences sharedPreferences = getSharedPreferences("test", Activity.MODE_PRIVATE);
	SharedPreferences.Editor editor = sharedPreferences.edit();

    editor.remove("nickname");
    editor.commit();

    Log.i("SharedPreferences", sharedPreferences.getAll().toString());

```

### 清空数据

```

	SharedPreferences sharedPreferences = getSharedPreferences("test", Activity.MODE_PRIVATE);
	SharedPreferences.Editor editor = sharedPreferences.edit();
    editor.clear();
    editor.commit();

    Log.i("SharedPreferences", sharedPreferences.getAll().toString());		

```

### 对象存储

对象存储，需要将对象序列化，然后反序列化

### SharedPreferences 读取物理存储文件

SharedPreferences 的数据存储再一个 xml 文件中，他的地址是：

```

//<package name>应替换成应用的包名, <name>
File xmlFile = new File("/data/data/<package name>/shared_prefs/<name>.xml");		

```

```

<?xml version='1.0' encoding='utf-8' standalone='yes' ?>
<map>
   <string name="name">陈景峰</string>
   <string name="nickname">netkiller</string>
   <int name="age" value="30" />
</map>

```

## SD Card

### SD Card 状态

```

	if (!Environment.getExternalStorageState().equals(Environment.MEDIA_MOUNTED)) {
                    TextView textView = (TextView) findViewById(R.id.textView);
                    textView.setText("SD 卡不存在，请插入 SD 卡！");
                }		

```

## 第 44 章 相机与相册

## manifest 文件

```

<?xml version="1.0" encoding="utf-8"?>
<manifest 
    package="cn.netkiller.camera">

    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/AppTheme">
        <activity android:name=".MainActivity">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>

        <provider
            android:name="android.support.v4.content.FileProvider"
            android:authorities="cn.netkiller.camera.provider"
            android:exported="false"
            android:grantUriPermissions="true">
            <meta-data
                android:name="android.support.FILE_PROVIDER_PATHS"
                android:resource="@xml/provider_paths" />
        </provider>
    </application>

</manifest>

```

provider_paths.xml 文件

```

<?xml version="1.0" encoding="utf-8"?>
<paths >
    <external-path name="external_files" path="."/>
</paths>		

```

## layout

```

<?xml version="1.0" encoding="utf-8"?>
<android.support.constraint.ConstraintLayout 

    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <TextView
        android:id="@+id/textView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Hello World!"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <Button
        android:id="@+id/buttonOpenCamera"
        android:layout_width="160dp"
        android:layout_height="wrap_content"
        android:layout_marginStart="8dp"
        android:layout_marginTop="8dp"
        android:layout_marginEnd="8dp"
        android:layout_marginBottom="8dp"
        android:text="拍照立即显示"
        app:layout_constraintBottom_toTopOf="@+id/buttonAlbum"
        app:layout_constraintEnd_toStartOf="@+id/buttonSavePhoto"
        app:layout_constraintHorizontal_bias="0.283"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/imageViewPicture"
        app:layout_constraintVertical_bias="0.0" />

    <Button
        android:id="@+id/buttonSavePhoto"
        android:layout_width="160dp"
        android:layout_height="wrap_content"
        android:layout_marginTop="8dp"
        android:layout_marginEnd="20dp"
        android:layout_marginBottom="8dp"
        android:text="拍照存储后显示"
        app:layout_constraintBottom_toTopOf="@+id/buttonAlbum"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/imageViewPicture"
        app:layout_constraintVertical_bias="0.0" />

    <ImageView
        android:id="@+id/imageViewPicture"
        android:layout_width="338dp"
        android:layout_height="366dp"
        android:layout_centerHorizontal="true"
        android:layout_marginStart="8dp"
        android:layout_marginTop="8dp"
        android:layout_marginEnd="8dp"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/textView" />

    <Button
        android:id="@+id/buttonAlbum"
        android:layout_width="352dp"
        android:layout_height="wrap_content"
        android:layout_marginStart="8dp"
        android:layout_marginEnd="8dp"
        android:layout_marginBottom="8dp"
        android:text="Album"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent" />

</android.support.constraint.ConstraintLayout>		

```

## Activity

```

package cn.netkiller.camera;

import android.Manifest;
import android.content.ContentResolver;
import android.content.ContentValues;
import android.content.Intent;
import android.content.pm.PackageManager;
import android.database.Cursor;
import android.graphics.Bitmap;
import android.graphics.BitmapFactory;
import android.net.Uri;
import android.os.Environment;
import android.os.StrictMode;
import android.provider.MediaStore;
import android.support.v4.app.ActivityCompat;
import android.support.v4.content.FileProvider;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.util.Log;
import android.view.View;
import android.widget.Button;
import android.widget.ImageView;
import android.widget.TextView;

import java.io.File;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.IOException;
import java.io.InputStream;

public class MainActivity extends AppCompatActivity implements View.OnClickListener {

    private ImageView imageViewPicture;
    private Button buttonOpenCamera;
    private Button buttonSavePhoto;
    private Button buttonAlbum;

    private static int REQUEST_CAMERA = 1;
    private static int REQUEST_PHOTO = 2;
    private static int REQUEST_ALBUM = 3;
    private File imageFile;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        imageViewPicture = (ImageView) findViewById(R.id.imageViewPicture);
        buttonOpenCamera = (Button) findViewById(R.id.buttonOpenCamera);
        buttonSavePhoto = (Button) findViewById(R.id.buttonSavePhoto);
        buttonAlbum = (Button) findViewById(R.id.buttonAlbum);

        buttonOpenCamera.setOnClickListener(this);
        buttonSavePhoto.setOnClickListener(this);
        buttonAlbum.setOnClickListener(this);

        StrictMode.VmPolicy.Builder newbuilder = new StrictMode.VmPolicy.Builder();
        StrictMode.setVmPolicy(newbuilder.build());

    }

    @Override
    public void onClick(View view) {
        Intent intent;
        switch (view.getId()) {
            case R.id.buttonOpenCamera:
                // 拍照并显示图片
                intent = new Intent(MediaStore.ACTION_IMAGE_CAPTURE);// 启动系统相机
                startActivityForResult(intent, REQUEST_CAMERA);
                break;
            case R.id.buttonSavePhoto:

                if (!Environment.getExternalStorageState().equals(Environment.MEDIA_MOUNTED)) {
                    TextView textView = (TextView) findViewById(R.id.textView);
                    textView.setText("SD 卡不存在，请插入 SD 卡！");
                }

                if (ActivityCompat.checkSelfPermission(this, Manifest.permission.WRITE_EXTERNAL_STORAGE) != PackageManager.PERMISSION_GRANTED) {
                    ActivityCompat.requestPermissions(MainActivity.this, new String[]{Manifest.permission.WRITE_EXTERNAL_STORAGE}, 1);
                    return;
                } else {

                    String dir = Environment.getExternalStorageDirectory().getPath();
                    new File(dir).mkdirs();

                    imageFile = new File(dir, "tmp.png");

                    Uri imageUri = FileProvider.getUriForFile(MainActivity.this, "cn.netkiller.camera.provider", imageFile);
                    Log.d("album", imageFile.getPath());

                    intent = new Intent(MediaStore.ACTION_IMAGE_CAPTURE);// 启动系统相机
                    intent.putExtra(MediaStore.EXTRA_OUTPUT, imageUri);
                    startActivityForResult(intent, REQUEST_PHOTO);

                }

                break;
            case R.id.buttonAlbum:

                intent = new Intent(Intent.ACTION_GET_CONTENT);
                intent.setType("image/*");
                startActivityForResult(Intent.createChooser(intent, "Select Picture"), 3);

                break;
            default:
                break;
        }
    }

    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        if (resultCode == RESULT_OK) { // 如果返回数据
            if (requestCode == REQUEST_CAMERA) { // 判断请求码是否为 REQUEST_CAMERA,如果是代表是这个页面传过去的，需要进行获取
                Bundle bundle = data.getExtras(); // 从 data 中取出传递回来缩略图的信息，图片质量差，适合传递小图片
                Bitmap bitmap = (Bitmap) bundle.get("data"); // 将 data 中的信息流解析为 Bitmap 类型
                imageViewPicture.setImageBitmap(bitmap);// 显示图片
            } else if (requestCode == REQUEST_PHOTO) {
                Log.i("photo", imageFile.getPath());
                try {
//                    InputStream inputStream = getContentResolver().openInputStream(imageUri);
                    String dir = Environment.getExternalStorageDirectory().getPath();
                    FileInputStream fileInputStream = new FileInputStream(imageFile);
                    Bitmap bitmap = BitmapFactory.decodeStream(fileInputStream);
                    fileInputStream.close();
                    imageViewPicture.setImageBitmap(bitmap);// 显示图片
                } catch (Exception e) {
                    e.printStackTrace();
                }
            } else if (requestCode == REQUEST_ALBUM) {

                Bitmap bitmap = null;

                //外界的程序访问 ContentProvider 所提供数据 可以通过 ContentResolver 接口
                ContentResolver resolver = getContentResolver();
                Uri originalUri = data.getData();        //获得图片的 uri

                try {
                    bitmap = MediaStore.Images.Media.getBitmap(resolver, originalUri);        //显得到 bitmap 图片
                } catch (IOException e) {
                    e.printStackTrace();
                }

                //获取图片的路径：
                Log.i("album", String.valueOf(originalUri));

                imageViewPicture.setImageBitmap(bitmap);
            }
        }
    }

}

```

## LED flash 做手电筒

```

<uses-permission android:name="android.permission.FLASHLIGHT" />		

```

```

<?xml version="1.0" encoding="utf-8"?>
<android.support.constraint.ConstraintLayout 

    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".FlashLightActivity">

    <Button
        android:id="@+id/buttonLight"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginStart="8dp"
        android:layout_marginTop="8dp"
        android:layout_marginEnd="8dp"
        android:layout_marginBottom="8dp"
        android:text="On"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />
</android.support.constraint.ConstraintLayout>		

```

```

package cn.netkiller.camera;

import android.app.AlertDialog;
import android.content.Context;
import android.content.DialogInterface;
import android.content.pm.PackageManager;
import android.hardware.Camera;
import android.hardware.camera2.CameraAccessException;
import android.hardware.camera2.CameraManager;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.Toast;

import java.security.Policy;

public class FlashLightActivity extends AppCompatActivity {

    private Button buttonLight;
    private CameraManager cameraManager;
    private String cameraId;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_flash_light);

        buttonLight = (Button) findViewById(R.id.buttonLight);

        Boolean isFlashAvailable = getApplicationContext().getPackageManager().hasSystemFeature(PackageManager.FEATURE_CAMERA_FLASH);

        if (!isFlashAvailable) {

            AlertDialog alert = new AlertDialog.Builder(FlashLightActivity.this).create();
            alert.setTitle("Error");
            alert.setMessage("Your device doesn't support flash light!");
            alert.setButton(DialogInterface.BUTTON_POSITIVE, "OK", new DialogInterface.OnClickListener() {
                public void onClick(DialogInterface dialog, int which) {
                    // closing the application
                    finish();
                    System.exit(0);
                }
            });
            alert.show();
            return;
        }

        cameraManager = (CameraManager) getSystemService(Context.CAMERA_SERVICE);
        try {
            cameraId = cameraManager.getCameraIdList()[0];
        } catch (CameraAccessException e) {
            e.printStackTrace();
        }

        buttonLight.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {

                try {
                    light();
                } catch (CameraAccessException e) {
                    e.printStackTrace();
                }

            }
        });
    }

    public void light() throws CameraAccessException {

        if (buttonLight.getText().equals("On")) {
            Toast.makeText(getApplicationContext(), "打开手电筒", Toast.LENGTH_SHORT).show();
            cameraManager.setTorchMode(cameraId, true);
            buttonLight.setText("Off");
        } else {
            Toast.makeText(getApplicationContext(), "关闭手电筒", Toast.LENGTH_SHORT).show();
            cameraManager.setTorchMode(cameraId, false);
            buttonLight.setText("On");
        }
    }
}

```

## 第 45 章 麦克风与录音

## 开启麦克风和 SD 卡权限

```

<?xml version="1.0" encoding="utf-8"?>
<manifest 
    package="cn.netkiller.voice">

    <uses-permission android:name="android.permission.RECORD_AUDIO" />
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/AppTheme">
        <activity android:name=".MainActivity">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest>		

```

## layout

```

<?xml version="1.0" encoding="utf-8"?>
<android.support.constraint.ConstraintLayout 

    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <TextView
        android:id="@+id/status"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="http://www.netkiller.cn"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <Button
        android:id="@+id/record"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginStart="8dp"
        android:layout_marginBottom="1dp"
        android:onClick="onClick"
        android:text="Record"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintStart_toStartOf="parent" />

    <Button
        android:id="@+id/play"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginEnd="8dp"
        android:layout_marginBottom="7dp"
        android:text="Play"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent" />

</android.support.constraint.ConstraintLayout>		

```

## Activity

```

package cn.netkiller.voice;

import android.Manifest;
import android.content.pm.PackageManager;
import android.media.MediaPlayer;
import android.media.MediaRecorder;
import android.os.Environment;
import android.support.v4.app.ActivityCompat;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.util.Log;
import android.view.View;
import android.widget.Button;
import android.widget.TextView;
import android.widget.Toast;

import java.io.IOException;
import java.text.SimpleDateFormat;
import java.util.Date;

public class MainActivity extends AppCompatActivity implements View.OnClickListener {

    private static final int RECORD_AUDIO = 10;
    private Button record;
    private Button play;
    private MediaRecorder mediaRecorder;
    private TextView status;

    private MediaPlayer mediaPlayer;
    private String filename;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        status = (TextView) findViewById(R.id.status);
        record = (Button) findViewById(R.id.record);
        play = (Button) findViewById(R.id.play);

        record.setOnClickListener(this);
        play.setOnClickListener(this);

        if (ActivityCompat.checkSelfPermission(this, Manifest.permission.WRITE_EXTERNAL_STORAGE) != PackageManager.PERMISSION_GRANTED || ActivityCompat.checkSelfPermission(this, Manifest.permission.RECORD_AUDIO) != PackageManager.PERMISSION_GRANTED) {
            ActivityCompat.requestPermissions(this, new String[]{Manifest.permission.WRITE_EXTERNAL_STORAGE, Manifest.permission.RECORD_AUDIO}, RECORD_AUDIO);
        }

    }

    @Override
    public void onClick(View v) {

        switch (v.getId()) {
            case R.id.record:
                if (record.getText().equals("Record")) {
                    this.start();
                } else {
                    this.stop();
                }
                break;
            case R.id.play:
                play();
                status.setText("播放录音");
                break;
        }
    }

    private void start() {

        record.setText("Stop");
        status.setText("开启录音，请对准话筒讲话");

        mediaRecorder = new MediaRecorder();
        mediaRecorder.setAudioSource(MediaRecorder.AudioSource.MIC);
        mediaRecorder.setOutputFormat(MediaRecorder.OutputFormat.THREE_GPP);
        mediaRecorder.setAudioEncoder(MediaRecorder.AudioEncoder.AMR_NB);

        String dir = Environment.getExternalStorageDirectory().getPath();
        String date = new SimpleDateFormat("yyyy-MM-ddhhmmss").format(new Date());

        filename = String.format("%s/%s.3gp", dir, date);

        Log.e("Voice", "voice path " + filename);

        mediaRecorder.setOutputFile(filename);

        try {
            mediaRecorder.prepare();
        } catch (IOException e) {
            e.printStackTrace();
        }
        mediaRecorder.start();

    }

    private void stop() {
        record.setText("Record");
        status.setText("录音停止");

        if (mediaRecorder != null) {
            mediaRecorder.stop();
            mediaRecorder.release();
            mediaRecorder = null;
        }
    }

    private void play() {
        this.stop();
        if (filename == null) {
            Toast.makeText(getApplicationContext(), "请先录音", Toast.LENGTH_SHORT).show();
            return;
        }
        try {

            if (mediaPlayer != null && mediaPlayer.isPlaying()) {
                mediaPlayer.reset();
            } else {
                mediaPlayer = new MediaPlayer();
                mediaPlayer.setDataSource(filename);
                mediaPlayer.prepare();
                mediaPlayer.start();
            }

        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        if (mediaPlayer != null) {
            mediaPlayer.stop();
            mediaPlayer.release();
        }
    }

}		

```

## 第 46 章 多媒体开发

## MediaPlayer

播放一段特效声音，例如铃音，可以在点击按钮德时候调用 playSound()

```

	private void playSound(){

        MediaPlayer mediaPlayer = MediaPlayer.create(FlashLightActivity.this, R.raw.alert);
        mediaPlayer.setOnCompletionListener(new MediaPlayer.OnCompletionListener() {

            @Override
            public void onCompletion(MediaPlayer mp) {
                // TODO Auto-generated method stub
                mediaPlayer.release();
            }
        });
        mediaPlayer.start();
    }		

```

## VideoView 开发

VideoView，用于播放一段视频媒体，它继承了 SurfaceView，位于"android.widget.VideoView"，是一个视频控件。

```

VideoView 也为开发人员提供了对应的方法，这里简单介绍一些常用的：

int getCurrentPosition()：获取当前播放的位置。
int getDuration()：获取当前播放视频的总长度。
isPlaying()：当前 VideoView 是否在播放视频。
void pause()：暂停
void seekTo(int msec)：从第几毫秒开始播放。
void resume()：重新播放。
void setVideoPath(String path)：以文件路径的方式设置 VideoView 播放的视频源。
void setVideoURI(Uri uri)：以 Uri 的方式设置 VideoView 播放的视频源，可以是网络 Uri 或本地 Uri。
void start()：开始播放。
void stopPlayback()：停止播放。
setMediaController(MediaController controller)：设置 MediaController 控制器。
setOnCompletionListener(MediaPlayer.onCompletionListener l)：监听播放完成的事件。
setOnErrorListener(MediaPlayer.OnErrorListener l)：监听播放发生错误时候的事件。
setOnPreparedListener(MediaPlayer.OnPreparedListener l)：：监听视频装载完成的事件。

```

上面的一些方法通过方法名就可以了解用途。和 MediaPlayer 配合 SurfaceView 播放视频不同，VideoView 播放之前无需编码装载视频，它会在 start()开始播放的时候自动装载视频。并且 VideoView 在使用完之后，无需编码回收资源。

### 播放网络视频

加入 android.permission.INTERNET 允许访问网络

```

<?xml version="1.0" encoding="utf-8"?>
<manifest 
    package="cn.netkiller.video">

    <uses-permission android:name="android.permission.INTERNET" />
    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/AppTheme">
        <activity android:name=".MainActivity">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
        <activity android:name=".VideoViewActivity"></activity>
    </application>

</manifest>

```

最简洁的布局，只有一个 VideoView

```

<?xml version="1.0" encoding="utf-8"?>
<android.support.constraint.ConstraintLayout 

    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".VideoViewActivity">

    <VideoView
        android:id="@+id/videoView"
        android:layout_width="match_parent"
        android:layout_height="236dp"
        android:layout_marginStart="8dp"
        android:layout_marginTop="8dp"
        android:layout_marginEnd="8dp"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

</android.support.constraint.ConstraintLayout>			

```

播放的文件来自 IPFS 星际文件系统

```

package cn.netkiller.video;

import android.net.Uri;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.widget.MediaController;
import android.widget.VideoView;

public class VideoViewActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_video_view);

        Uri uri = Uri.parse("http://ipfs.netkiller.cn/ipfs/QmcA1Fsrt6jGTVqAUNZBqaprMEdFaFkmkzA5s2M6mF85UC");
        VideoView videoView = (VideoView) this.findViewById(R.id.videoView);
        videoView.setMediaController(new MediaController(this));
        videoView.setVideoURI(uri);
        videoView.start();
        videoView.requestFocus();

    }
}

```

运行程序开始播放视频，点击视频会在屏幕下方弹出 MediaController 控制条

### MediaController 添加翻页事件

```

package cn.netkiller.video;

import android.net.Uri;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.view.View;
import android.widget.MediaController;
import android.widget.Toast;
import android.widget.VideoView;

public class VideoViewActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_video_view);

        final Uri uri = Uri.parse("http://ipfs.netkiller.cn/ipfs/QmcA1Fsrt6jGTVqAUNZBqaprMEdFaFkmkzA5s2M6mF85UC");
        final VideoView videoView = (VideoView) this.findViewById(R.id.videoView);
        MediaController mediaController = new MediaController(this);
        mediaController.setMediaPlayer(videoView);

        mediaController.setPrevNextListeners(
                new View.OnClickListener() {

                    @Override
                    public void onClick(View v) {
                        Toast.makeText(VideoViewActivity.this, "下一曲", Toast.LENGTH_SHORT).show();
                        videoView.setVideoURI(Uri.parse("http://ipfs.netkiller.cn/ipfs/QmUaDftnPB7zCTwTASnSAWLiXWd1L5vNGEeU585rddfVTh"));
                    }
                },
                new View.OnClickListener() {

                    @Override
                    public void onClick(View v) {
                        Toast.makeText(VideoViewActivity.this, "上一曲", Toast.LENGTH_SHORT).show();
                        videoView.setVideoURI(Uri.parse("http://ipfs.netkiller.cn/ipfs/QmbvKvj9X368WMtmkLYFuf59gSwLXYDLcdJuSiSHKPhTG4"));
                    }
                });

        videoView.setMediaController(mediaController);
        videoView.setVideoURI(uri);
        videoView.start();
        videoView.requestFocus();

    }
}

```

### 静音播放视频

```

		videoView.setOnPreparedListener(new MediaPlayer.OnPreparedListener() {

            @Override
            public void onPrepared(MediaPlayer mediaPlayer) {
                mediaPlayer.setVolume(0f, 0f);
            }
        });			

```

### 更新进度条

```

            new Thread() {
                @Override
                public void run() {
                    try {
                        while (videoView.isPlaying()) {
                            // 如果正在播放，没 0.5.毫秒更新一次进度条
                            int current = videoView.getCurrentPosition();
                            seekBar.setProgress(current);
                            sleep(500);
                        }
                    } catch (Exception e) {
                        e.printStackTrace();
                    }
                }
            }.start();			

```

### 完整的例子

```

<?xml version="1.0" encoding="utf-8"?>
<android.support.constraint.ConstraintLayout 

    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".VideoViewActivity">

    <VideoView
        android:id="@+id/videoView"
        android:layout_width="match_parent"
        android:layout_height="236dp"
        android:layout_marginStart="8dp"
        android:layout_marginTop="8dp"
        android:layout_marginEnd="8dp"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <LinearLayout
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:layout_marginStart="8dp"
        android:layout_marginEnd="8dp"
        android:layout_marginBottom="8dp"
        android:orientation="vertical"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent">

        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:orientation="horizontal">

            <TextView
                android:id="@+id/textViewTime"
                android:layout_width="wrap_content"
                android:layout_height="match_parent"
                android:layout_weight="1"
                android:text="00:00" />

            <SeekBar
                android:id="@+id/seekBar"
                android:layout_width="270dp"
                android:layout_height="wrap_content" />

            <TextView
                android:id="@+id/textViewCurrentPosition"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_weight="1"
                android:text="00:00" />
        </LinearLayout>

        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:orientation="horizontal">

            <Button
                android:id="@+id/buttonPlay"
                android:layout_width="183dp"
                android:layout_height="wrap_content"
                android:text="播放" />

            <Button
                android:id="@+id/buttonStop"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_weight="1"
                android:text="停止" />

        </LinearLayout>

    </LinearLayout>

    <TextView
        android:id="@+id/textViewStatus"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginStart="8dp"
        android:layout_marginTop="8dp"
        android:layout_marginEnd="8dp"
        android:text="        "
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/videoView" />
</android.support.constraint.ConstraintLayout>			

```

```

package cn.netkiller.video;

import android.media.MediaPlayer;
import android.net.Uri;
import android.os.Handler;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.SeekBar;
import android.widget.TextView;
import android.widget.Toast;
import android.widget.VideoView;

import java.text.SimpleDateFormat;
import java.util.Calendar;

public class VideoViewActivity extends AppCompatActivity implements View.OnClickListener {

    private VideoView videoView;
    private SeekBar seekBar;
    private Button buttonPlay;
    private TextView textViewTime;
    private TextView textViewCurrentPosition;
    private TextView textViewStatus;

    private Handler handler = new Handler();
    private Runnable runnable = new Runnable() {
        public void run() {
            if (videoView.isPlaying()) {
                int current = videoView.getCurrentPosition();
                seekBar.setProgress(current);
                textViewCurrentPosition.setText(time(videoView.getCurrentPosition()));
            }
            handler.postDelayed(runnable, 500);
        }
    };

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_video_view);

        final Uri uri = Uri.parse("http://ipfs.netkiller.cn/ipfs/QmcA1Fsrt6jGTVqAUNZBqaprMEdFaFkmkzA5s2M6mF85UC");
        videoView = (VideoView) this.findViewById(R.id.videoView);
        videoView.setVideoURI(uri);
        videoView.requestFocus();

        videoView.setOnPreparedListener(new MediaPlayer.OnPreparedListener() {

            @Override
            public void onPrepared(MediaPlayer mediaPlayer) {
                textViewTime.setText(time(videoView.getDuration()));
                textViewStatus.setText("视频加载完毕");
                buttonPlay.setEnabled(true);

            }
        });

        // 在播放完毕被回调
        videoView.setOnCompletionListener(new MediaPlayer.OnCompletionListener() {
            @Override
            public void onCompletion(MediaPlayer mp) {
                Toast.makeText(VideoViewActivity.this, "播放完成", Toast.LENGTH_SHORT).show();
            }
        });

        videoView.setOnErrorListener(new MediaPlayer.OnErrorListener() {

            @Override
            public boolean onError(MediaPlayer mp, int what, int extra) {
                // 发生错误重新播放
                play();
                Toast.makeText(VideoViewActivity.this, "播放出错", Toast.LENGTH_SHORT).show();
                return false;
            }
        });

        textViewStatus = (TextView) findViewById(R.id.textViewStatus);
        textViewStatus.setText("玩命加载中");

        textViewTime = (TextView) findViewById(R.id.textViewTime);

        seekBar = (SeekBar) findViewById(R.id.seekBar);
        // 为进度条添加进度更改事件
        seekBar.setOnSeekBarChangeListener(onSeekBarChangeListener);

        textViewCurrentPosition = (TextView) findViewById(R.id.textViewCurrentPosition);

        buttonPlay = (Button) findViewById(R.id.buttonPlay);
        buttonPlay.setEnabled(false);
        final Button buttonStop = (Button) findViewById(R.id.buttonStop);

        buttonPlay.setOnClickListener(this);
        buttonStop.setOnClickListener(this);

    }

    @Override
    public void onClick(View v) {
        switch (v.getId()) {
            case R.id.buttonPlay:
                play();
                break;
            case R.id.buttonStop:
                stop();
                break;
            default:
                break;
        }
    }

    private SeekBar.OnSeekBarChangeListener onSeekBarChangeListener = new SeekBar.OnSeekBarChangeListener() {
        // 当进度条停止修改的时候触发
        @Override
        public void onStopTrackingTouch(SeekBar seekBar) {
            // 取得当前进度条的刻度
            int progress = seekBar.getProgress();
            if (videoView.isPlaying()) {
                // 设置当前播放的位置
                videoView.seekTo(progress);
            }
        }

        @Override
        public void onStartTrackingTouch(SeekBar seekBar) {

        }

        @Override
        public void onProgressChanged(SeekBar seekBar, int progress,
                                      boolean fromUser) {

        }
    };

    protected void play() {

        if (buttonPlay.getText().equals("播放")) {
            buttonPlay.setText("暂停");
            textViewStatus.setText("请您欣赏");
            // 开始线程，更新进度条的刻度
            handler.postDelayed(runnable, 0);
            videoView.start();
            seekBar.setMax(videoView.getDuration());

        } else

        {
            buttonPlay.setText("播放");
            if (videoView.isPlaying()) {
                videoView.pause();
            }
        }

    }

    protected void stop() {
        if (videoView.isPlaying()) {
            videoView.stopPlayback();
        }
    }

    protected String time(long millionSeconds) {

        SimpleDateFormat simpleDateFormat = new SimpleDateFormat("mm:ss");
        Calendar c = Calendar.getInstance();
        c.setTimeInMillis(millionSeconds);
        return simpleDateFormat.format(c.getTime());
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        handler.removeCallbacks(runnable);
    }

}

```

## SurfaceView

## Vitamio

## 第 47 章 定位

## manifest 权限配置

```

<?xml version="1.0" encoding="utf-8"?>
<manifest 
    package="cn.netkiller.location">

    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
    <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
    <uses-permission android:name="android.permission.CHANGE_WIFI_STATE" />
    <uses-permission android:name="android.permission.INTERNET" />

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/AppTheme">
        <activity android:name=".MainActivity">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest>		

```

## layout

```

<?xml version="1.0" encoding="utf-8"?>
<android.support.constraint.ConstraintLayout 

    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <TableLayout
        android:id="@+id/tableLayout"
        android:layout_width="match_parent"
        android:layout_height="0dp"
        android:layout_marginStart="8dp"
        android:layout_marginTop="8dp"
        android:layout_marginEnd="8dp"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent">

        <TableRow
            android:layout_width="match_parent"
            android:layout_height="match_parent">

            <TextView
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="状态: " />

            <TextView
                android:id="@+id/status"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:text="TextView" />
        </TableRow>

        <TableRow
            android:layout_width="match_parent"
            android:layout_height="match_parent">

            <TextView
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="经度: " />

            <TextView
                android:id="@+id/textViewLatitude"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:text="TextView" />
        </TableRow>

        <TableRow
            android:layout_width="match_parent"
            android:layout_height="match_parent">

            <TextView
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="纬度: " />

            <TextView
                android:id="@+id/textViewLongitude"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:text="TextView" />
        </TableRow>

        <TableRow
            android:layout_width="match_parent"
            android:layout_height="match_parent">

            <TextView
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="海拔: " />

            <TextView
                android:id="@+id/textViewAltitude"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="TextView" />
        </TableRow>

        <TableRow
            android:layout_width="match_parent"
            android:layout_height="match_parent">

            <TextView
                android:id="@+id/textView4"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="速度" />

            <TextView
                android:id="@+id/textViewSpeed"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="TextView" />
        </TableRow>

        <TableRow
            android:layout_width="match_parent"
            android:layout_height="match_parent">

            <TextView
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="时间: " />

            <TextView
                android:id="@+id/textViewTime"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="TextView" />
        </TableRow>
    </TableLayout>

    <ListView
        android:id="@+id/listViewAddress"
        android:layout_width="368dp"
        android:layout_height="358dp"
        android:layout_marginStart="8dp"
        android:layout_marginTop="8dp"
        android:layout_marginEnd="8dp"
        android:layout_marginBottom="8dp"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/tableLayout" />
</android.support.constraint.ConstraintLayout>		

```

## Activity

```

package cn.netkiller.location;

import android.Manifest;
import android.content.pm.PackageManager;
import android.location.Address;
import android.location.Criteria;
import android.location.Geocoder;
import android.location.Location;
import android.location.LocationListener;
import android.location.LocationManager;
import android.location.LocationProvider;
import android.support.v4.app.ActivityCompat;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.util.Log;
import android.view.View;
import android.widget.ArrayAdapter;
import android.widget.ListView;
import android.widget.TextView;
import android.widget.Toast;

import java.io.IOException;
import java.text.SimpleDateFormat;
import java.util.ArrayList;
import java.util.Date;
import java.util.List;
import java.util.Locale;

public class MainActivity extends AppCompatActivity {

    private static final int REQUEST_PERMISSION_CODE = 12;
    private TextView textViewLatitude;
    private TextView textViewLongitude;
    private TextView textViewAltitude;
    private TextView textViewSpeed;
    private TextView textViewTime;
    private TextView status;

    private static final SimpleDateFormat dateFormat = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
    private ListView listViewAddress;
    private ArrayAdapter<String> adapter;
    private ArrayList<String> list;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        textViewLatitude = (TextView) findViewById(R.id.textViewLatitude);
        textViewLongitude = (TextView) findViewById(R.id.textViewLongitude);
        textViewAltitude = (TextView) findViewById(R.id.textViewAltitude);
        textViewSpeed = (TextView) findViewById(R.id.textViewSpeed);
        textViewTime = (TextView) findViewById(R.id.textViewTime);
        status = (TextView) findViewById(R.id.status);

        list = new ArrayList<String>();
        adapter = new ArrayAdapter<String>(MainActivity.this, android.R.layout.simple_list_item_1, list);
        listViewAddress = (ListView) findViewById(R.id.listViewAddress);
        listViewAddress.setAdapter(adapter);

        this.location();
    }

    private void loop() {

    }

    public void location() {

        //获取 LocationManager 对象
        LocationManager locationManager = (LocationManager) getSystemService(LOCATION_SERVICE);

        boolean gpsStatus = locationManager.isProviderEnabled(LocationManager.GPS_PROVIDER);
        Log.d("Location", "GPS Status: " + gpsStatus);

        boolean networkStatus = locationManager.isProviderEnabled(LocationManager.NETWORK_PROVIDER);
        Log.d("Location", "Network Status: " + networkStatus);

        //创建一个 Criteria 对象
        Criteria criteria = new Criteria();
        //设置粗略精确度
        criteria.setAccuracy(Criteria.ACCURACY_COARSE);
        //设置是否需要返回海拔信息
        criteria.setAltitudeRequired(true);
        //设置是否需要返回方位信息
        criteria.setBearingRequired(true);
        //设置是否允许付费服务
        criteria.setCostAllowed(false);
        //设置电量消耗等级
        criteria.setPowerRequirement(Criteria.POWER_HIGH);
        //设置是否需要返回速度信息
        criteria.setSpeedRequired(true);

        Log.d("Location", "Criteria: " + criteria.toString());

        //获取最符合此标准的 provider 对象
//        String currentProvider = locationManager.getProvider(LocationManager.GPS_PROVIDER).getName();

        //根据设置的 Criteria 对象，获取最符合此标准的 provider 对象
        String currentProvider = locationManager.getBestProvider(criteria, true);

        Log.d("Location", "currentProvider: " + currentProvider);
        status.setText(currentProvider);

        if (ActivityCompat.checkSelfPermission(this, Manifest.permission.ACCESS_FINE_LOCATION) != PackageManager.PERMISSION_GRANTED && ActivityCompat.checkSelfPermission(this, Manifest.permission.ACCESS_COARSE_LOCATION) != PackageManager.PERMISSION_GRANTED) {
            ActivityCompat.requestPermissions(MainActivity.this, new String[]{Manifest.permission.ACCESS_FINE_LOCATION, Manifest.permission.ACCESS_COARSE_LOCATION}, REQUEST_PERMISSION_CODE);
            return;
        } else {
            status.setText("正在获取 GPS 坐标请稍候...");
        }

        locationManager.requestLocationUpdates(currentProvider, 0, 0, locationListener);
        //根据当前 provider 对象获取最后一次位置信息
        Location location = locationManager.getLastKnownLocation(currentProvider);

        //如果位置信息不为 null，则请求更新位置信息
        if (location != null) {

            textViewLatitude.setText(location.getLatitude() + "");
            textViewLongitude.setText(location.getLongitude() + "");
            textViewAltitude.setText(location.getAltitude() + "");
            textViewSpeed.setText(location.getSpeed() + "");
            textViewTime.setText(location.getTime() + "");

            Log.d("Location", "Latitude: " + location.getLatitude());
            Log.d("Location", "location: " + location.getLongitude());

        } else {

            Log.d("Location", "Latitude: " + 0);
            Log.d("Location", "location: " + 0);

        }

    }

    //创建位置监听器
    private LocationListener locationListener = new LocationListener() {

        //位置发生改变时调用
        @Override
        public void onLocationChanged(Location location) {
            status.setText("onLocationChanged");

            //位置信息变化时触发
            Log.e("Location", "定位方式：" + location.getProvider());
            Log.e("Location", "纬度：" + location.getLatitude());
            Log.e("Location", "经度：" + location.getLongitude());
            Log.e("Location", "海拔：" + location.getAltitude());
            Log.e("Location", "时间：" + location.getTime());

            textViewLatitude.setText(location.getLatitude() + "");
            textViewLongitude.setText(location.getLongitude() + "");
            textViewAltitude.setText(location.getAltitude() + "");
            textViewSpeed.setText(location.getSpeed() + "");
            textViewTime.setText(dateFormat.format(new Date(location.getTime())) + "");

            //解析地址
            Geocoder geoCoder = new Geocoder(MainActivity.this, Locale.getDefault());

            double latitude = location.getLatitude();
            double longitude = location.getLongitude();

            List<Address> locationList = null;
            try {
                locationList = geoCoder.getFromLocation(latitude, longitude, 5);
            } catch (IOException e) {
                e.printStackTrace();
            }

//            Address address = locationList.get(0);//得到 Address 实例第一个地址
//            status.setText(address.toString());
//            String countryName = address.getCountryName();//得到国家名称，比如：中国
//            String locality = address.getLocality();//得到城市名称，比如：北京市

            list.clear();

            for (Address address : locationList) {

                for (int n = 0; address.getAddressLine(n) != null; n++) {
                    String addressLine = address.getAddressLine(n);//得到周边信息，包括街道等，i=0，得到街道名称
                    list.add(addressLine);
                    Log.i("Location", "addressLine = " + addressLine);
                    Log.d("Location", address.getCountryName() + address.getAdminArea() + address.getFeatureName());
                }
            }

            adapter.notifyDataSetChanged();

        }

        //provider 失效时调用
        @Override
        public void onProviderDisabled(String provider) {

            Log.d("Location", "onProviderDisabled");
            status.setText("onProviderDisabled");

        }

        //provider 启用时调用
        @Override
        public void onProviderEnabled(String provider) {

            Log.d("Location", "onProviderEnabled");
            status.setText("onProviderEnabled");

        }

        //状态改变时调用
        @Override
        public void onStatusChanged(String provider, int status, Bundle extras) {

            Log.d("Location", "onStatusChanged");
            //GPS 状态变化时触发
            switch (status) {
                case LocationProvider.AVAILABLE:
                    Log.e("Location", "当前 GPS 状态为可见状态");
                    break;
                case LocationProvider.OUT_OF_SERVICE:
                    Log.e("Location", "当前 GPS 状态为服务区外状态");
                    break;
                case LocationProvider.TEMPORARILY_UNAVAILABLE:
                    Log.e("Location", "当前 GPS 状态为暂停服务状态");
                    break;
            }

        }

    };

    @Override
    public void onRequestPermissionsResult(int requestCode, String[] permissions, int[] grantResults) {
        super.onRequestPermissionsResult(requestCode, permissions, grantResults);

        switch (requestCode) {
            case REQUEST_PERMISSION_CODE: {
                // If request is cancelled, the result arrays are empty.
                if (grantResults.length > 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED) {

                } else {
                    // permission denied, boo! Disable the
                    // functionality that depends on this permission.
                }
                return;
            }

        }
    }

}

```

## 第 48 章 电话

## SIM 卡状态

```

		TelephonyManager telephonyManager = (TelephonyManager)context.getSystemService(context.TELEPHONY_SERVICE);

        if(telephonyManager.getSimState() == TelephonyManager.SIM_STATE_READY){
            return true;
        }else{
            return false;
        }

```

## 通信录与拨打电话

```

<?xml version="1.0" encoding="utf-8"?>
<manifest 
    package="cn.netkiller.contacts">

    <uses-permission android:name="android.permission.READ_CONTACTS"/>
    <uses-permission android:name="android.permission.CALL_PHONE"/>

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/AppTheme">
        <activity android:name=".MainActivity">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest>  		

```

```

<?xml version="1.0" encoding="utf-8"?>
<android.support.constraint.ConstraintLayout 

    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <TextView
        android:id="@+id/textView2"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="通信录与拨打电话"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <Button
        android:id="@+id/contact"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginStart="8dp"
        android:layout_marginTop="60dp"
        android:layout_marginEnd="8dp"
        android:text="联系人"
        app:layout_constraintEnd_toStartOf="@+id/call"
        app:layout_constraintHorizontal_bias="0.478"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/phone" />

    <EditText
        android:id="@+id/phone"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginStart="16dp"
        android:layout_marginTop="64dp"
        android:layout_marginEnd="8dp"
        android:ems="10"
        android:inputType="phone"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.475"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/textView2" />

    <Button
        android:id="@+id/call"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="60dp"
        android:layout_marginEnd="52dp"
        android:text="Call"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/phone" />

</android.support.constraint.ConstraintLayout>		

```

```

package cn.netkiller.contacts;

import android.app.Activity;
import android.content.ContentResolver;
import android.content.Intent;
import android.content.pm.PackageManager;
import android.database.Cursor;
import android.net.Uri;
import android.provider.ContactsContract;
import android.support.v4.app.ActivityCompat;
import android.support.v4.content.ContextCompat;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.TextView;
import android.widget.Toast;

public class MainActivity extends AppCompatActivity implements View.OnClickListener {

    private EditText phone;
    private Button contact;
    private Button call;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        contact = (Button) findViewById(R.id.contact);
        contact.setOnClickListener(this);

        call = (Button) findViewById(R.id.call);
        call.setOnClickListener(this);

        phone = (EditText) findViewById(R.id.phone);
        phone.setText("", TextView.BufferType.EDITABLE);

        if (ActivityCompat.checkSelfPermission(this, android.Manifest.permission.READ_CONTACTS) != PackageManager.PERMISSION_GRANTED) {
            ActivityCompat.requestPermissions((Activity) this, new String[]{android.Manifest.permission.READ_CONTACTS}, 1);
        }
        if (ActivityCompat.checkSelfPermission(this, android.Manifest.permission.CALL_PHONE) != PackageManager.PERMISSION_GRANTED) {
            ActivityCompat.requestPermissions((Activity) this, new String[]{android.Manifest.permission.CALL_PHONE}, 1);
        }

    }

    @Override
    public void onClick(View v) {
        switch (v.getId()) {
            case R.id.contact:
                Toast.makeText(MainActivity.this, "获取联系人手机号码", Toast.LENGTH_SHORT).show();
                startActivityForResult(new Intent(Intent.ACTION_PICK, ContactsContract.Contacts.CONTENT_URI), 0);
                break;
            case R.id.call:
                Toast.makeText(MainActivity.this, "打电话", Toast.LENGTH_SHORT).show();

                Intent intent = new Intent();
                intent.setAction(Intent.ACTION_CALL);//ACTION_DIAL
                intent.setData(Uri.parse("tel:" + phone.getText()));
                startActivity(intent);

                break;
        }

    }

    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        if (resultCode == Activity.RESULT_OK) {
            Uri contactData = data.getData();
            Cursor cursor = getContentResolver().query(contactData, null, null, null, null);
            cursor.moveToFirst();

            //条件为联系人 ID
            String contactId = cursor.getString(cursor.getColumnIndex(ContactsContract.Contacts._ID));
            //获得 DATA 表中的电话号码，条件为联系人 ID,因为手机号码可能会有多个
            Cursor contact = getContentResolver().query(ContactsContract.CommonDataKinds.Phone.CONTENT_URI, null, ContactsContract.CommonDataKinds.Phone.CONTACT_ID + "=" + contactId, null, null);
            while (contact.moveToNext()) {
                String contactNumber = contact.getString(contact.getColumnIndex(ContactsContract.CommonDataKinds.Phone.NUMBER));

                phone.setText(contactNumber);
            }
            cursor.close();

        }
    }

}

```

```

<uses-permission android:name="android.permission.SEND_SMS" />		

```

```

	private void sendSMS(String phoneNumber,String message){
        if(PhoneNumberUtils.isGlobalPhoneNumber(phoneNumber)){  
            Intent intent = new Intent(Intent.ACTION_SENDTO, Uri.parse("smsto:"+phoneNumber));            
            intent.putExtra("sms_body", message);            
            startActivity(intent);  
        }  
    }  		

```

```

	public void sendSMS(String phoneNumber,String message){  
        android.telephony.SmsManager smsManager = android.telephony.SmsManager.getDefault();  
        //拆分短信内容，手机短信长度有限制    
        List<String> divideContents = smsManager.divideMessage(message);   
        for (String text : divideContents) {    
            smsManager.sendTextMessage(phoneNumber, null, text, sentPI, deliverPI);    
        }  
    }		

```

## 第 49 章 消息广播

安卓中有两种广播，一种是系统发出的广播信息，例如网络改变，电池的电量低等等，另一种是用户发出的广播信息。

Android 中的广播类型可以分为两种类型，标准广播和有序广播。

标准广播（Normal broadcasts）：标准广播是一种完全异步执行的广播，在广播发出之后，所有的广播接收器几乎会在同一时刻接收到这条广播消息。这种广播效率比较高，但同时也意味着它是无法被截断的。

有序广播（Ordered broadcasts）：有序广播则是一种同步执行的广播，在广播发出之后，同一时刻只会有一个广播接收器能够收到这条广播消息，当这个广播接收器中的逻辑执行完毕之后，广播才会继续传递。

```

android.intent.action.BATTERY_CHANGED	持久广播含充电状态，级别，以及其他相关的电池信息。
android.intent.action.BATTERY_LOW		显示设备的电池电量低。
android.intent.action.BATTERY_OKAY		指示电池正在低点后但没有问题。
android.intent.action.BOOT_COMPLETED	一次播出后，系统已完成启动。
android.intent.action.BUG_REPORT		显示活动报告的错误。
android.intent.action.CALL				执行呼叫由数据指定某人。
android.intent.action.CALL_BUTTON		用户按下“呼叫”按钮进入拨号器或其他适当的用户界面发出呼叫。
android.intent.action.DATE_CHANGED		日期改变。
android.intent.action.REBOOT			有设备重启。	

```

## 动态注册

```

package cn.netkiller.broadcast;

import android.content.BroadcastReceiver;
import android.content.Context;
import android.content.Intent;
import android.content.IntentFilter;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.widget.Toast;

public class MainActivity extends AppCompatActivity {

    private IntentFilter intentFilter;
    private MyBroadcastReceiver myBroadcastReceiver;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        intentFilter = new IntentFilter();
        //为过滤器添加处理规则
        intentFilter.addAction("android.net.conn.CONNECTIVITY_CHANGE");
        myBroadcastReceiver = new MyBroadcastReceiver();
        //注册广播接收器
        registerReceiver(myBroadcastReceiver, intentFilter);
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        //注销动态的广播接收器
        unregisterReceiver(myBroadcastReceiver);
    }

    //自定义内部类，继承 BroadcastReceiver
    public class MyBroadcastReceiver extends BroadcastReceiver {

        @Override
        public void onReceive(Context context, Intent intent) {
            Toast.makeText(context, "网络状态已改变", Toast.LENGTH_SHORT).show();
        }
    }
}

```

现在尝试改变网络状态，例如开启或关闭飞行模式，程序会弹出 "网络状态已改变"。

我的测试环境是 Android 9 Pie 没有加入下面的权限仍然能工作

```

<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />		

```

优化程序

```

package cn.netkiller.broadcast;

import android.content.BroadcastReceiver;
import android.content.Context;
import android.content.Intent;
import android.content.IntentFilter;
import android.net.ConnectivityManager;
import android.net.NetworkInfo;
import android.os.BatteryManager;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.widget.Toast;

public class MainActivity extends AppCompatActivity {

    private IntentFilter intentFilter;
    private MyBroadcastReceiver myBroadcastReceiver;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        intentFilter = new IntentFilter();
        //为过滤器添加处理规则
        intentFilter.addAction("android.net.conn.CONNECTIVITY_CHANGE");
        intentFilter.addAction(ConnectivityManager.CONNECTIVITY_ACTION);
        intentFilter.addAction(Intent.ACTION_BATTERY_CHANGED);
        intentFilter.addAction(Intent.ACTION_BATTERY_LOW);

        myBroadcastReceiver = new MyBroadcastReceiver();
        //注册广播接收器
        registerReceiver(myBroadcastReceiver, intentFilter);
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        //注销动态的广播接收器
        unregisterReceiver(myBroadcastReceiver);
    }

    //自定义内部类，继承 BroadcastReceiver
    public class MyBroadcastReceiver extends BroadcastReceiver {

        @Override
        public void onReceive(Context context, Intent intent) {
            ConnectivityManager connectivityManager = (ConnectivityManager) getSystemService(Context.CONNECTIVITY_SERVICE);
            NetworkInfo networkInfo = connectivityManager.getActiveNetworkInfo();
            //判断是否联网
            if (networkInfo != null && networkInfo.isConnected()) {
                Toast.makeText(context, "网络已连接", Toast.LENGTH_SHORT).show();
            } else {
                Toast.makeText(context, "网络不可用", Toast.LENGTH_SHORT).show();
            }

            int status = intent.getIntExtra(BatteryManager.EXTRA_STATUS, -1);
            boolean isCharging = status == BatteryManager.BATTERY_STATUS_CHARGING ||
                    status == BatteryManager.BATTERY_STATUS_FULL;

            if (isCharging) {
                Toast.makeText(context, "正在充电", Toast.LENGTH_SHORT).show();
            } else {
                Toast.makeText(context, "电池已经充满", Toast.LENGTH_SHORT).show();
            }

            int chargePlug = intent.getIntExtra(BatteryManager.EXTRA_PLUGGED, -1);
            boolean usbCharge = chargePlug == BatteryManager.BATTERY_PLUGGED_USB;
            boolean acCharge = chargePlug == BatteryManager.BATTERY_PLUGGED_AC;
            if (usbCharge) {
                Toast.makeText(context, "USB 充电", Toast.LENGTH_SHORT).show();
            }

        }
    }
}		

```

## 静态注册

Android Studio 选择 File - New - Other - Broadcast Receiver

```

<?xml version="1.0" encoding="utf-8"?>
<manifest 
    package="cn.netkiller.broadcast">

    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
    <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/AppTheme">
        <activity android:name=".MainActivity">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>

        <receiver
            android:name=".MyReceiver"
            android:enabled="true"
            android:exported="true">

            <intent-filter>
                <action android:name="android.intent.action.BOOT_COMPLETED" />
            </intent-filter>

        </receiver>
    </application>

</manifest>		

```

MyReceiver 集成 BroadcastReceiver 在 onReceive 中写入你的业务逻辑。

```

package cn.netkiller.broadcast;

import android.content.BroadcastReceiver;
import android.content.Context;
import android.content.Intent;
import android.widget.Toast;

public class MyReceiver extends BroadcastReceiver {

    @Override
    public void onReceive(Context context, Intent intent) {
        Toast.makeText(context, "程序已启动，接收到系统启动广播", Toast.LENGTH_SHORT).show();
    }
}

```

现在重启 Android 模拟器，启动后虽然 App 并没有进入，但是屏幕底部会看到 "程序已启动，接收到系统启动广播"

## 自定义用户消息广播

```

package cn.netkiller.broadcast;

import android.content.BroadcastReceiver;
import android.content.Context;
import android.content.Intent;
import android.content.IntentFilter;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.TextView;
import android.widget.Toast;

public class MainActivity extends AppCompatActivity {

    private IntentFilter intentFilter;
    private TextView textView;
    private MyBroadcastReceiver myBroadcastReceiver;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        intentFilter = new IntentFilter();
        intentFilter.addAction("cn.netkiller.broadcast.MESSAGE");
        myBroadcastReceiver = new MyBroadcastReceiver();
        //注册广播接收器
        registerReceiver(myBroadcastReceiver, intentFilter);

        textView = (TextView) findViewById(R.id.textView);

        Button button = (Button) findViewById(R.id.button);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                //把要发送的广播值传入 Intent 对象
                Intent intent = new Intent("cn.netkiller.broadcast.MESSAGE");
                intent.putExtra("msg", "Helloworld");
                //调用 Context 的 sendBroadcast()方法发送广播
                sendBroadcast(intent);
                textView.setText("Send");

            }
        });
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        //注销动态的广播接收器
        unregisterReceiver(myBroadcastReceiver);
    }

    //自定义内部类，继承 BroadcastReceiver
    public class MyBroadcastReceiver extends BroadcastReceiver {

        @Override
        public void onReceive(Context context, Intent intent) {

            String data = intent.getStringExtra("msg");
            Toast.makeText(context, data, Toast.LENGTH_SHORT).show();

        }
    }
}		

```

## 本地广播

上面讲的系统广播是全局的，任何 APP 都能接收到你的广播，这样就很容易引起 APP 的安全性问题。很多时候我们只想接收来自本应用程序发出的广播。

```

package cn.netkiller.broadcast;

import android.content.BroadcastReceiver;
import android.content.Context;
import android.content.Intent;
import android.content.IntentFilter;
import android.support.v4.content.LocalBroadcastManager;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.TextView;
import android.widget.Toast;

public class MainActivity extends AppCompatActivity {

    private TextView textView;
    private IntentFilter intentFilter;
    private LocalBroadcastManager localBroadcastManager;
    private MyBroadcastReceiver myBroadcastReceiver;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        //获取 LocalBroadcastManger 单 li 实例
        localBroadcastManager = localBroadcastManager.getInstance(this);

        intentFilter = new IntentFilter();
        intentFilter.addAction("cn.netkiller.broadcast.MESSAGE");
        myBroadcastReceiver = new MyBroadcastReceiver();
        //注册本地广播接收器
        localBroadcastManager.registerReceiver(myBroadcastReceiver, intentFilter);

        textView = (TextView) findViewById(R.id.textView);

        Button button = (Button) findViewById(R.id.button);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {

                textView.setText("Send");

                Intent intent = new Intent();
                intent.setAction("cn.netkiller.broadcast.MESSAGE");
                intent.putExtra("msg", "http://www.netkiller.cn");
                //发送本地广播
                localBroadcastManager.sendBroadcast(intent);
            }
        });
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        //注销本地广播接收器
        localBroadcastManager.unregisterReceiver(myBroadcastReceiver);
    }

    //自定义内部类，继承 BroadcastReceiver
    public class MyBroadcastReceiver extends BroadcastReceiver {

        @Override
        public void onReceive(Context context, Intent intent) {

            String data = intent.getStringExtra("msg");
            Toast.makeText(context, data, Toast.LENGTH_SHORT).show();

        }
    }
}

```

## 第 50 章 Service

## Service 的基本用法

### manifest 文件

```

<?xml version="1.0" encoding="utf-8"?>
<manifest 
    package="cn.netkiller.service">

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/AppTheme">
        <activity android:name=".MainActivity">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>

        <service
            android:name=".MyService"
            android:enabled="true"
            android:exported="true"></service>
    </application>

</manifest>			

```

这段代码不是手工加入的，只需在 Android Studio 中选择 File - New - Service - Service 创建 Service 会自动加入下面代码

```

<service
            android:name=".MyService"
            android:enabled="true"
            android:exported="true"></service>			

```

### 创建 Service

在 Android Studio 中选择 File - New - Service - Service 创建 Service

MyService 继承自 Service，并重写父类的 onCreate()、onStartCommand()和 onDestroy()方法

```

package cn.netkiller.service;

import android.app.Service;
import android.content.Intent;
import android.os.IBinder;
import android.util.Log;

public class MyService extends Service {
    public MyService() {
    }

    @Override
    public void onCreate() {
        super.onCreate();
        Log.d("Service", "onCreate() executed");
        Log.d("Service", "MyService thread id is " + Thread.currentThread().getId());
    }

    @Override
    public int onStartCommand(Intent intent, int flags, int startId) {
        Log.d("Service", "onStartCommand() executed");
        return super.onStartCommand(intent, flags, startId);
    }

    @Override
    public void onDestroy() {
        super.onDestroy();
        Log.d("Service", "onDestroy() executed");
    }

    @Override
    public IBinder onBind(Intent intent) {
        // TODO: Return the communication channel to the service.
        throw new UnsupportedOperationException("Not yet implemented");
    }
}

```

onCreate() Service 创建的时候执行，已经创建的 Service 不会再执行

onStartCommand() 任何时候，只要执行 startService(intent); 便会执行

onDestroy() 停止的时候执行

### Layout 代码

在布局文件中加入了两个按钮，一个用于启动 Service，一个用于停止 Service

```

<?xml version="1.0" encoding="utf-8"?>
<android.support.constraint.ConstraintLayout 

    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <LinearLayout
        android:layout_width="368dp"
        android:layout_height="229dp"
        android:layout_marginStart="8dp"
        android:layout_marginTop="8dp"
        android:layout_marginEnd="8dp"
        android:layout_marginBottom="8dp"
        android:orientation="vertical"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.0"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.0">

        <Button
            android:id="@+id/startService"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="Start service" />

        <Button
            android:id="@+id/stopService"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="Stop service" />
    </LinearLayout>

</android.support.constraint.ConstraintLayout>			

```

### Activity 代码

```

package cn.netkiller.service;

import android.content.Intent;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.util.Log;
import android.view.View;
import android.widget.Button;

public class MainActivity extends AppCompatActivity implements View.OnClickListener {

    private Button startService;
    private Button stopService;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        startService = (Button) findViewById(R.id.startService);
        stopService = (Button) findViewById(R.id.stopService);
        startService.setOnClickListener(this);
        stopService.setOnClickListener(this);

        Log.d("Service", "MainActivity thread id is " + Thread.currentThread().getId());

    }

    @Override
    public void onClick(View v) {
        Intent intent;
        switch (v.getId()) {
            case R.id.startService:
                intent = new Intent(this, MyService.class);
                startService(intent);
                break;
            case R.id.stopService:
                intent = new Intent(this, MyService.class);
                stopService(intent);
                break;
            default:
                break;
        }
    }
}

```

## Service 中启动线程

```

    @Override
    public int onStartCommand(Intent intent, int flags, int startId) {

        Log.d("Service", "onStartCommand() begin");
        new Thread(new Runnable() {
            @Override
            public void run() {
                // 开始执行后台任务
                Log.d("Service", "onStartCommand() executed");
            }
        }).start();

        Log.d("Service", "onStartCommand() end");

        return super.onStartCommand(intent, flags, startId);
    }		

```

## Service 和 Activity 通信

### Layout

```

<?xml version="1.0" encoding="utf-8"?>
<android.support.constraint.ConstraintLayout 

    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <LinearLayout
        android:layout_width="368dp"
        android:layout_height="229dp"
        android:layout_marginStart="8dp"
        android:layout_marginTop="8dp"
        android:layout_marginEnd="8dp"
        android:layout_marginBottom="8dp"
        android:orientation="vertical"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.0"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.0">

        <Button
            android:id="@+id/startService"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="Start service" />

        <Button
            android:id="@+id/stopService"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="Stop service" />

        <Button
            android:id="@+id/bindService"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="Bind Service" />

        <Button
            android:id="@+id/unbindService"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="Unbind Service" />

    </LinearLayout>

</android.support.constraint.ConstraintLayout>			

```

### Service

```

package cn.netkiller.service;

import android.app.Service;
import android.content.Intent;
import android.os.Binder;
import android.os.IBinder;
import android.util.Log;

public class MyService extends Service {

    private MyBinder myBinder = new MyBinder();

    public MyService() {
    }

    @Override
    public void onCreate() {
        super.onCreate();
        Log.d("Service", "onCreate() executed");
        Log.d("Service", "MyService thread id is " + Thread.currentThread().getId());
    }

    @Override
    public int onStartCommand(Intent intent, int flags, int startId) {

        Log.d("Service", "onStartCommand() begin");
        new Thread(new Runnable() {
            @Override
            public void run() {
                // 开始执行后台任务
                Log.d("Service", "onStartCommand() executed");
            }
        }).start();

        Log.d("Service", "onStartCommand() end");

        return super.onStartCommand(intent, flags, startId);
    }

    @Override
    public void onDestroy() {
        super.onDestroy();
        Log.d("Service", "onDestroy() executed");
    }

    @Override
    public IBinder onBind(Intent intent) {
        return myBinder;
    }

    class MyBinder extends Binder {

        public void startTask() {
            new Thread(new Runnable() {
                @Override
                public void run() {
                    // 执行具体的任务
                    Log.d("Service", "startTask()");
                }
            }).start();
        }

    }
}			

```

### Activity

```

package cn.netkiller.service;

import android.content.ComponentName;
import android.content.Intent;
import android.content.ServiceConnection;
import android.os.IBinder;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.util.Log;
import android.view.View;
import android.widget.Button;

public class MainActivity extends AppCompatActivity implements View.OnClickListener {

    private Button startService;
    private Button stopService;
    private Button bindService;
    private Button unbindService;

    private MyService.MyBinder myBinder;

    private ServiceConnection connection = new ServiceConnection() {

        @Override
        public void onServiceDisconnected(ComponentName name) {
        }

        @Override
        public void onServiceConnected(ComponentName name, IBinder service) {
            myBinder = (MyService.MyBinder) service;
            myBinder.startTask();
        }
    };

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        startService = (Button) findViewById(R.id.startService);
        stopService = (Button) findViewById(R.id.stopService);
        startService.setOnClickListener(this);
        stopService.setOnClickListener(this);

        bindService = (Button) findViewById(R.id.bindService);
        unbindService = (Button) findViewById(R.id.unbindService);
        bindService.setOnClickListener(this);
        unbindService.setOnClickListener(this);

        Log.d("Service", "MainActivity thread id is " + Thread.currentThread().getId());

    }

    @Override
    public void onClick(View v) {
        Intent intent;
        switch (v.getId()) {
            case R.id.startService:
                intent = new Intent(this, MyService.class);
                startService(intent);
                break;
            case R.id.stopService:
                intent = new Intent(this, MyService.class);
                stopService(intent);
                break;
            case R.id.bindService:
                Intent bindIntent = new Intent(this, MyService.class);
                bindService(bindIntent, connection, BIND_AUTO_CREATE);
                break;
            case R.id.unbindService:
                unbindService(connection);
                break;
            default:
                break;
        }
    }
}

```

## 第 51 章 NFC (Near field communication)

## AndroidManifest.xml 文件配置

```

<?xml version="1.0" encoding="utf-8"?>
<manifest 
    package="cn.netkiller.nfc">
    <!--<uses-sdk android:minSdkVersion="14"/>-->
    <uses-permission android:name="android.permission.NFC" />
    <!-- 要求当前设备必须要有 NFC 芯片 -->
    <uses-feature
        android:name="android.hardware.nfc"
        android:required="true" />

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="NFC 初始化工具"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/AppTheme">
        <activity
            android:name=".MainActivity"
            android:launchMode="singleTop">

            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>

            <intent-filter>
                <action android:name="android.nfc.action.NDEF_DISCOVERED" />
                <category android:name="android.intent.category.DEFAULT" />
                <data android:mimeType="text/plain" />

                <!--<data android:mimeType="text/plain" />-->
                <!--<data android:mimeType="*/*" />-->
            </intent-filter>
            <intent-filter>
                <action android:name="android.nfc.action.TECH_DISCOVERED" />
            </intent-filter>

            <intent-filter>
                <action android:name="android.nfc.action.TAG_DISCOVERED" />
                <category android:name="android.intent.category.DEFAULT" />
            </intent-filter>

        </activity>

    </application>

</manifest>		

```

## Loyout 文件

```

<?xml version="1.0" encoding="utf-8"?>
<android.support.constraint.ConstraintLayout 

    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <TextView
        android:id="@+id/ndefMessage"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:layout_marginStart="8dp"
        android:layout_marginTop="20dp"
        android:layout_marginEnd="8dp"
        android:text="message"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="1.0"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/textView4" />

    <TextView
        android:id="@+id/textView4"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginStart="8dp"
        android:layout_marginTop="16dp"
        android:layout_marginEnd="8dp"
        android:text="NDEF Message : "
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.03"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <TableLayout
        android:id="@+id/tableLayout"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:layout_marginStart="10dp"
        android:layout_marginTop="24dp"
        android:layout_marginEnd="8dp"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.0"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/ndefMessage">

        <TableRow
            android:layout_width="match_parent"
            android:layout_height="match_parent">

            <TextView
                android:id="@+id/textView"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="NFC UID" />

            <TextView
                android:id="@+id/uid"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="UID"
                tools:layout_editor_absoluteX="146dp"
                tools:layout_editor_absoluteY="71dp" />
        </TableRow>

        <TableRow
            android:layout_width="match_parent"
            android:layout_height="match_parent">

            <TextView
                android:id="@+id/textView2"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="NFC Tag" />

            <TextView
                android:id="@+id/nfcTag"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="tag"
                tools:layout_editor_absoluteX="179dp"
                tools:layout_editor_absoluteY="83dp" />
        </TableRow>

        <TableRow
            android:layout_width="match_parent"
            android:layout_height="match_parent">

            <TextView
                android:id="@+id/textView3"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="NFC Size" />

            <TextView
                android:id="@+id/size"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="size"
                tools:layout_editor_absoluteX="179dp"
                tools:layout_editor_absoluteY="150dp" />
        </TableRow>

        <TableRow
            android:layout_width="match_parent"
            android:layout_height="match_parent">

            <TextView
                android:id="@+id/textView5"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="NDEF Type" />

            <TextView
                android:id="@+id/schema"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="schema"
                tools:layout_editor_absoluteX="168dp"
                tools:layout_editor_absoluteY="257dp" />
        </TableRow>

        <TableRow
            android:layout_width="match_parent"
            android:layout_height="match_parent">

        </TableRow>

        <TableRow
            android:layout_width="match_parent"
            android:layout_height="match_parent">

            <TextView
                android:id="@+id/textView7"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="NDEF charset" />

            <TextView
                android:id="@+id/charset"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="charset"
                tools:layout_editor_absoluteX="163dp"
                tools:layout_editor_absoluteY="304dp" />

        </TableRow>

        <TableRow
            android:layout_width="match_parent"
            android:layout_height="match_parent">

            <TextView
                android:id="@+id/textView8"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="NDEF Lang" />

            <TextView
                android:id="@+id/language"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="lang"
                tools:layout_editor_absoluteX="163dp"
                tools:layout_editor_absoluteY="331dp" />
        </TableRow>

        <TableRow
            android:layout_width="match_parent"
            android:layout_height="match_parent">

            <TextView
                android:id="@+id/textView6"
                android:layout_width="79dp"
                android:layout_height="wrap_content"
                android:text="Status" />

            <TextView
                android:id="@+id/status"
                android:layout_width="289dp"
                android:layout_height="wrap_content"
                android:text="status"
                tools:layout_editor_absoluteX="90dp"
                tools:layout_editor_absoluteY="404dp" />

        </TableRow>
    </TableLayout>

    <TextView
        android:id="@+id/textView9"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginStart="8dp"
        android:layout_marginTop="48dp"
        android:layout_marginEnd="8dp"
        android:text="NDEF Message write :"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.0"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/tableLayout" />

    <TextView
        android:id="@+id/ndefWrite"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:layout_marginStart="8dp"
        android:layout_marginTop="8dp"
        android:layout_marginEnd="8dp"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="1.0"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/textView9" />

    <Switch
        android:id="@+id/switchWrite"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginStart="8dp"
        android:layout_marginTop="28dp"
        android:layout_marginEnd="8dp"
        android:text="NDEF Message write"
        android:textOff="NDEF Message write Off"
        android:textOn="NDEF Message write On"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.497"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/ndefWrite" />

</android.support.constraint.ConstraintLayout>		

```

## Activity 文件

```

package cn.netkiller.nfc;

import android.nfc.FormatException;
import android.nfc.NdefRecord;
import android.support.annotation.NonNull;
import android.support.design.widget.BottomNavigationView;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;

import android.app.PendingIntent;
import android.content.Intent;
import android.nfc.NdefMessage;
import android.nfc.NfcAdapter;

import android.nfc.Tag;
import android.nfc.tech.Ndef;
import android.os.Parcelable;
import android.view.MenuItem;
import android.view.View;
import android.widget.Button;
import android.widget.CompoundButton;
import android.widget.Switch;
import android.widget.TextView;

import java.io.IOException;
import java.io.UnsupportedEncodingException;
import java.math.BigInteger;
import java.util.UUID;

public class MainActivity extends AppCompatActivity {

    private NfcAdapter nfcAdapter;
    private PendingIntent pendingIntent;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        final TextView status = (TextView) findViewById(R.id.status);

        nfcAdapter = NfcAdapter.getDefaultAdapter(this);

        if (nfcAdapter == null) {
            System.out.println("**** NFC ERROR ****");
            status.setText("NFC is not available.");
            return;
        } else if (!nfcAdapter.isEnabled()) {
            status.setText("请开启系统 NFC 功能");
        }

        status.setText("Start...");

        pendingIntent = PendingIntent.getActivity(this, 0, new Intent(this, getClass()).addFlags(Intent.FLAG_ACTIVITY_SINGLE_TOP), 0);

        final Switch switchWrite = (Switch) findViewById(R.id.switchWrite);

        switchWrite.setOnCheckedChangeListener(new CompoundButton.OnCheckedChangeListener() {
            @Override
            public void onCheckedChanged(CompoundButton buttonView, boolean isChecked) {

                if (isChecked) {
                    status.setText(switchWrite.getTextOn().toString());
                } else {
                    status.setText(switchWrite.getTextOff().toString());
                }
            }
        });
    }

    @Override
    protected void onResume() {
        super.onResume();
        nfcAdapter.enableForegroundDispatch(this, pendingIntent, null, null);
    }

    @Override
    protected void onPause() {
        super.onPause();
        nfcAdapter.disableForegroundDispatch(this);
    }

    //当窗口的创建模式是 singleTop 或 singleTask 时调用，用于取代 onCreate 方法
    //当 NFC 标签靠近手机，建立连接后调用
    @Override
    public void onNewIntent(Intent intent) {
        super.onNewIntent(intent);

        TextView status = (TextView) findViewById(R.id.status);
        TextView type = (TextView) findViewById(R.id.nfcTag);
        TextView size = (TextView) findViewById(R.id.size);
        TextView uidTextView = (TextView) findViewById(R.id.uid);
        TextView ndefMessage = (TextView) findViewById(R.id.ndefMessage);
        TextView schema = (TextView) findViewById(R.id.schema);
        TextView charset = (TextView) findViewById(R.id.charset);
        TextView language = (TextView) findViewById(R.id.language);
        TextView ndefWrite = (TextView) findViewById(R.id.ndefWrite);
        Switch switchWrite = (Switch) findViewById(R.id.switchWrite);

        Tag tag = intent.getParcelableExtra(NfcAdapter.EXTRA_TAG);

        if (switchWrite.isChecked()) {

            UUID uuid = UUID.randomUUID();
            try {
                write(uuid.toString(), tag);
                ndefWrite.setText(uuid.toString());
            } catch (IOException e) {
                e.printStackTrace();
            } catch (FormatException e) {
                e.printStackTrace();
            }

        }

        if (NfcAdapter.ACTION_NDEF_DISCOVERED.equals(intent.getAction())) {
            status.setText("Read NDEF Message...");

            byte[] uid = tag.getId();
            BigInteger n = new BigInteger(uid);
            String hex = n.toString(16);
            uidTextView.setText(hex);

            Ndef ndef = Ndef.get(tag);
            String log = ndef.getType() + "\n 最大数据容量：" + ndef.getMaxSize() + " bytes\n\n";
            System.out.println(log);
            type.setText(ndef.getType());
            size.setText(ndef.getMaxSize() + " bytes");

            Parcelable[] rawMessages = intent.getParcelableArrayExtra(NfcAdapter.EXTRA_NDEF_MESSAGES);
            if (rawMessages != null) {
                NdefMessage[] messages = new NdefMessage[rawMessages.length];
                for (int i = 0; i < rawMessages.length; i++) {
                    messages[i] = (NdefMessage) rawMessages[i];
                }
                byte[] payload = messages[0].getRecords()[0].getPayload();

                try {

                    String tagId = new String(messages[0].getRecords()[0].getType());
                    schema.setText(tagId);

                    String encoding = ((payload[0] & 128) == 0) ? "UTF-8" : "UTF-16";
                    charset.setText(encoding);

                    int languageCodeLength = payload[0] & 0x3f;
                    String languageCode = new String(payload, 1, languageCodeLength, "US-ASCII");

//                    String lang = new String(payload, 1, payload[0] & 0063, "US-ASCII");
                    language.setText(languageCode);

//                    String text = new String(messages[0].getRecords()[0].getPayload());
                    String text = new String(payload, languageCodeLength + 1, payload.length - languageCodeLength - 1, encoding);
                    ndefMessage.setText(text);

                } catch (UnsupportedEncodingException e) {
                    e.printStackTrace();
                }

            }

        } else {
            status.setText("NOT NDEF Messages tag");
        }
    }

    private void write(String text, Tag tag) throws IOException, FormatException {
        NdefRecord[] records = {createRecord(text)};
        NdefMessage message = new NdefMessage(records);
        // Get an instance of Ndef for the tag.
        Ndef ndef = Ndef.get(tag);
        // Enable I/O
        ndef.connect();
        // Write the message
        ndef.writeNdefMessage(message);
        // Close the connection
        ndef.close();
    }

    private NdefRecord createRecord(String text) throws UnsupportedEncodingException {
        String lang = "en";
        byte[] textBytes = text.getBytes();
        byte[] langBytes = lang.getBytes("US-ASCII");
        int langLength = langBytes.length;
        int textLength = textBytes.length;
        byte[] payload = new byte[1 + langLength + textLength];

        // set status byte (see NDEF spec for actual bits)
        payload[0] = (byte) langLength;

        // copy langbytes and textbytes into payload
        System.arraycopy(langBytes, 0, payload, 1, langLength);
        System.arraycopy(textBytes, 0, payload, 1 + langLength, textLength);

        NdefRecord recordNFC = new NdefRecord(NdefRecord.TNF_WELL_KNOWN, NdefRecord.RTD_TEXT, new byte[0], payload);

        return recordNFC;
    }

}		

```

## 第 52 章 OkHttp - An HTTP & HTTP/2 client for Android and Java applications

[`square.github.io/okhttp/`](http://square.github.io/okhttp/)

## Gradle

再 app/build.gradle 文件中增加依赖包

```

implementation 'com.squareup.okhttp3:okhttp:3.11.0'		

```

app/build.gradle

```

neo@MacBook-Pro ~/AndroidStudioProjects/okhttp % cat app/build.gradle

apply plugin: 'com.android.application'

android {
    compileSdkVersion 28
    defaultConfig {
        applicationId "cn.netkiller.okhttp"
        minSdkVersion 28
        targetSdkVersion 28
        versionCode 1
        versionName "1.0"
        testInstrumentationRunner "android.support.test.runner.AndroidJUnitRunner"
    }
    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
        }
    }
}

dependencies {
    implementation fileTree(dir: 'libs', include: ['*.jar'])
    implementation 'com.android.support:appcompat-v7:28.0.0'
    implementation 'com.android.support:design:28.0.0'
    implementation 'com.android.support.constraint:constraint-layout:1.1.3'
    testImplementation 'junit:junit:4.12'
    androidTestImplementation 'com.android.support.test:runner:1.0.2'
    androidTestImplementation 'com.android.support.test.espresso:espresso-core:3.0.2'
    implementation 'com.squareup.okhttp3:okhttp:3.11.0'
}

```

## AndroidManifest.xml 开启网络访问权限

在 app/src/main/AndroidManifest.xml 文件中增加

```

<uses-permission android:name="android.permission.INTERNET" />		

```

否则 okhttp 无法访问网络，添加后的效果如下。

```

neo@MacBook-Pro ~/AndroidStudioProjects/okhttp % cat app/src/main/AndroidManifest.xml 
<?xml version="1.0" encoding="utf-8"?>
<manifest 
    package="cn.netkiller.okhttp">
    <uses-permission android:name="android.permission.INTERNET" />
    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/AppTheme">
        <activity
            android:name=".MainActivity"
            android:label="@string/app_name">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest>		

```

## okhttp 默认是 HTTPS 开启 HTTP

如果你尝试使用 http 链接 OkHttp3 就抛出异常: CLEARTEXT communication to " + host + " not permitted by network security policy

开发过程中由于 https ssl 需要购买证书，费用较高，通常测试环境我们仍然使用 http 下面方法是开启 http 模式，

创建文件 res/xml/network_security_config.xml 内容如下

```

neo@MacBook-Pro ~/AndroidStudioProjects/okhttp % cat app/src/main/res/xml/network_security_config.xml 
<?xml version="1.0" encoding="utf-8"?>
<network-security-config>
    <domain-config cleartextTrafficPermitted="true">
        <domain includeSubdomains="true">localhost</domain>
    </domain-config>
</network-security-config>		

```

再 app/src/main/AndroidManifest.xml 文件中增加 android:networkSecurityConfig="@xml/network_security_config"

```

<?xml version="1.0" encoding="utf-8"?>
<manifest 
    package="cn.netkiller.okhttp">

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/AppTheme">
        <activity
            android:name=".MainActivity"
            android:label="@string/app_name"
            android:networkSecurityConfig="@xml/network_security_config">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

    <uses-permission android:name="android.permission.INTERNET" />
</manifest>		

```

## GET

```

	OkHttpClient client = new OkHttpClient();

    Request request = new Request.Builder()
            .url("http://www.netkiller.cn")
            .build();

    Response response = client.newCall(request).execute();
    if (!response.isSuccessful()) {
        throw new IOException("服务器端错误: " + response);
    }

    Headers responseHeaders = response.headers();
    for (int i = 0; i < responseHeaders.size(); i++) {
        System.out.println(responseHeaders.name(i) + ": " + responseHeaders.value(i));
    }

    System.out.println(response.body().string());		

```

## POST

### POST Form Data

from 数据如下

```

key=value&other=data			

```

```

		String url = "https://api.netkiller.cn/oauth/token";

		OkHttpClient client = new OkHttpClient();

        RequestBody formBody = new FormBody.Builder()
                .add("grant_type", "password")
                .add("username", "netkiller")
                .add("password", "123456")
                .build();

        Request request = new Request.Builder()
                .url(url)
                .post(formBody)
                .build();

        Response response = client.newCall(request).execute();
        if (!response.isSuccessful()) {
            throw new IOException("服务器端错误: " + response);
        }

```

### POST RAW JSON

POST RAW Json 数据，数据的形态这样的

```

{"key":"value"}

```

```

public static final MediaType JSON = MediaType.parse("application/json; charset=utf-8");

OkHttpClient client = new OkHttpClient();

String post(String url, String json) throws IOException {
  RequestBody body = RequestBody.create(JSON, json);
  Request request = new Request.Builder()
      .url(url)
      .post(body)
      .build();
  Response response = client.newCall(request).execute();
  return response.body().string();
}		

```

### 数据流提交

```

OkHttpClient client = new OkHttpClient();
final MediaType MEDIA_TYPE_TEXT = MediaType.parse("text/plain");
final String postBody = "Hello World";

RequestBody requestBody = new RequestBody() {
    @Override
    public MediaType contentType() {
        return MEDIA_TYPE_TEXT;
    }
    @Override
    public void writeTo(BufferedSink sink) throws IOException {
        sink.writeUtf8(postBody);
    }
    @Override
    public long contentLength() throws IOException {
        return postBody.length();
    }
};

Request request = new Request.Builder()
        .url("https://www.google.com")
        .post(requestBody)
        .build();
Response response = client.newCall(request).execute();
if (!response.isSuccessful()) {
    throw new IOException("服务器端错误: " + response);
}
System.out.println(response.body().string());			

```

## http header 相关设置

### 设置 HTTP 头

添加 Http 头

```

Request request = new Request.Builder()
        .url(url)
        .addHeader("CLIENT", "AD")
        .addHeader("USERID", "343")
        .build();		

```

覆盖 HTTP 头

```

        Request request = new Request.Builder()
                .header("Authorization", "replace this text with your token")
                .url(url)
                .build();		

```

```

public class Headers {
   public static void main(String[] args) throws IOException {
    OkHttpClient client = new OkHttpClient();

    Request request = new Request.Builder()
            .url("http://www.netkiller.cn")
            .header("User-Agent", "Apple Mac")
            .addHeader("Accept", "text/html")
            .build();

    Response response = client.newCall(request).execute();
    if (!response.isSuccessful()) {
        throw new IOException("服务器端错误: " + response);
    }

    System.out.println(response.header("Server"));
    System.out.println(response.headers("Set-Cookie"));
   }
}		

```

### Cookie 管理

```

OkHttpClient mHttpClient = new OkHttpClient.Builder().cookieJar(new CookieJar() {
    private final HashMap<String, List<Cookie>> cookieStore = new HashMap<>();
    @Override
    public void saveFromResponse(HttpUrl url, List<Cookie> cookies) {
        cookieStore.put(url.host(), cookies);
    }
    @Override
    public List<Cookie> loadForRequest(HttpUrl url) {
        List<Cookie> cookies = cookieStore.get(url.host());
        return cookies != null ? cookies : new ArrayList<Cookie>();
    }
}).build();			

```

### 禁用缓存

```

Request request = new Request.Builder()
    .cacheControl(new CacheControl.Builder().noCache().build())
    .url(url)
    .build();			

```

### 设置缓存 max-age

```

// 等同于 nocache			
Request request = new Request.Builder()
    .cacheControl(new CacheControl.Builder()
        .maxAge(0, TimeUnit.SECONDS)
        .build())
    .url("https://www.netkiller.cn")
    .build();

// 设置缓存 365 天
Request request = new Request.Builder()
    .cacheControl(new CacheControl.Builder()
        .maxStale(365, TimeUnit.DAYS)
        .build())
    .url("https://www.netkiller.cn")
    .build();    	

```

### 强制缓存

```

Request request = new Request.Builder()
    .cacheControl(new CacheControl.Builder()
        .onlyIfCached()
        .build())
    .url("https://www.netkiller.cn/helloworld.txt")
    .build();
Response forceCacheResponse = client.newCall(request).execute();
if (forceCacheResponse.code() != 504) {
  // The resource was cached! Show it.
} else {
  // The resource was not cached.
}			

```

## HTTP Base Auth

```

        OkHttpClient client = new OkHttpClient.Builder().authenticator(
                new Authenticator(){
                    public Request authenticate(Route route, Response response) {
                        String credential = Credentials.basic("api","secret");
                        return response.request().newBuilder().header("Authorization", credential).build();
                    }
                }
        ).build();		

```

## HttpUrl.Builder 组装 URL 地址参数

使用字符串拼接 URL 地址特别容易出错

```

String url = "https://www.netkiller.cn/article?username="+ username + "&category="+ category;		

```

较好的处理方式是使用 HttpUrl.Builder

```

		HttpUrl.Builder builder = HttpUrl.parse("https://www.netkiller.cn/article").newBuilder();
        builder.addQueryParameter("username", "netkiller");
        builder.addQueryParameter("category", "android");
        String url = builder.build().toString();

        Log.d("okhttp", url);		

```

输出结果

```

https://www.netkiller.cn/article?username=netkiller&category=android		

```

## Android Activity Example

```

package cn.netkiller.okhttp;

import android.os.Handler;
import android.os.Looper;
import android.os.Message;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.util.Log;
import android.widget.TextView;

import java.io.IOException;

import okhttp3.Call;
import okhttp3.Callback;
import okhttp3.OkHttpClient;
import okhttp3.Request;
import okhttp3.Response;

public class HttpActivity extends AppCompatActivity {

    TextView textView;
    private Handler mHandler;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_http);

        textView = (TextView) findViewById(R.id.textView);

        mHandler = new Handler(Looper.getMainLooper()) {
            @Override
            public void handleMessage(Message msg) {
                textView.setText((String) msg.obj);
            }
        };

        OkHttpClient client = new OkHttpClient();
        Request request = new Request.Builder().url("https://api.netkiller.cn/member/json").build();
        client.newCall(request).enqueue(new Callback() {
            @Override
            public void onFailure(Call call, IOException e) {
                Log.d("okhttp", "Connect timeout. " + e.getMessage());
            }

            @Override
            public void onResponse(Call call, Response response) throws IOException {
                String text = response.body().string();
                Log.d("okhttp", "HTTP Code " + response.code() + " TEXT " + text);

                Message msg = new Message();
                msg.what = 0;
                msg.obj = text;
                mHandler.sendMessage(msg);

            }
        });

    }
}

```

## Android Oauth2 + Jwt example

Oauth 返回数据，通过 Gson 的 fromJson 存储到下面类中。

```

package cn.netkiller.okhttp.pojo;

public class Oauth {
    private String access_token;
    private String token_type;
    private String refresh_token;
    private String expires_in;
    private String scope;
    private String jti;

    public String getAccess_token() {
        return access_token;
    }

    public void setAccess_token(String access_token) {
        this.access_token = access_token;
    }

    public String getToken_type() {
        return token_type;
    }

    public void setToken_type(String token_type) {
        this.token_type = token_type;
    }

    public String getRefresh_token() {
        return refresh_token;
    }

    public void setRefresh_token(String refresh_token) {
        this.refresh_token = refresh_token;
    }

    public String getExpires_in() {
        return expires_in;
    }

    public void setExpires_in(String expires_in) {
        this.expires_in = expires_in;
    }

    public String getScope() {
        return scope;
    }

    public void setScope(String scope) {
        this.scope = scope;
    }

    public String getJti() {
        return jti;
    }

    public void setJti(String jti) {
        this.jti = jti;
    }

    @Override
    public String toString() {
        return "Oauth{" +
                "access_token='" + access_token + '\'' +
                ", token_type='" + token_type + '\'' +
                ", refresh_token='" + refresh_token + '\'' +
                ", expires_in='" + expires_in + '\'' +
                ", scope='" + scope + '\'' +
                ", jti='" + jti + '\'' +
                '}';
    }
}	

```

Activity 文件

```

package cn.netkiller.okhttp;

import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.util.Log;
import android.widget.TextView;

import com.google.gson.Gson;

import java.io.IOException;

import cn.netkiller.okhttp.pojo.Oauth;
import okhttp3.Authenticator;
import okhttp3.Call;
import okhttp3.Callback;
import okhttp3.Credentials;
import okhttp3.FormBody;
import okhttp3.OkHttpClient;
import okhttp3.Request;
import okhttp3.RequestBody;
import okhttp3.Response;
import okhttp3.Route;

public class Oauth2jwtActivity extends AppCompatActivity {

    private TextView token;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_oauth2jwt);

        token = (TextView) findViewById(R.id.token);

        try {
            get();
        } catch (IOException e) {
            e.printStackTrace();
        }

    }

    public static Oauth accessToken() throws IOException {

        OkHttpClient client = new OkHttpClient.Builder().authenticator(
                new Authenticator() {
                    public Request authenticate(Route route, Response response) {
                        String credential = Credentials.basic("api", "secret");
                        return response.request().newBuilder().header("Authorization", credential).build();
                    }
                }
        ).build();

        String url = "https://api.netkiller.cn/oauth/token";

        RequestBody formBody = new FormBody.Builder()
                .add("grant_type", "password")
                .add("username", "blockchain")
                .add("password", "123456")
                .build();

        Request request = new Request.Builder()
                .url(url)
                .post(formBody)
                .build();

        Response response = client.newCall(request).execute();
        if (!response.isSuccessful()) {
            throw new IOException("服务器端错误: " + response);
        }

        Gson gson = new Gson();
        Oauth oauth = gson.fromJson(response.body().string(), Oauth.class);
        Log.i("oauth", oauth.toString());
        return oauth;
    }

    public void get() throws IOException {

        OkHttpClient client = new OkHttpClient.Builder().authenticator(
                new Authenticator() {
                    public Request authenticate(Route route, Response response) throws IOException {
                        return response.request().newBuilder().header("Authorization", "Bearer " + accessToken().getAccess_token()).build();
                    }
                }
        ).build();

        String url = "https://api.netkiller.cn/misc/compatibility";

        Request request = new Request.Builder()
                .url(url)
                .build();

        client.newCall(request).enqueue(new Callback() {
            @Override
            public void onFailure(Call call, IOException e) {
                call.cancel();
            }

            @Override
            public void onResponse(Call call, Response response) throws IOException {

                final String myResponse = response.body().string();

                runOnUiThread(new Runnable() {
                    @Override
                    public void run() {
                        Log.i("oauth", myResponse);
                        token.setText(myResponse);
                    }
                });

            }
        });
    }

	public void post() throws IOException {

        OkHttpClient client = new OkHttpClient.Builder().authenticator(
                new Authenticator() {
                    public Request authenticate(Route route, Response response) throws IOException {
                        return response.request().newBuilder().header("Authorization", "Bearer " + accessToken().getAccess_token()).build();
                    }
                }
        ).build();

        String url = "https://api.netkiller.cn/history/create";

        String json = "{\n" +
                "  \"assetsId\": \"5bced71c432c001c6ea31924\",\n" +
                "  \"title\": \"添加信息\",\n" +
                "  \"message\": \"信息内容\",\n" +
                "  \"status\": \"录入\"\n" +
                "}";

        final MediaType JSON = MediaType.parse("application/json; charset=utf-8");
        RequestBody body = RequestBody.create(JSON, json);

        Request request = new Request.Builder()
                .url(url)
                .post(body)
                .build();

        client.newCall(request).enqueue(new Callback() {
            @Override
            public void onFailure(Call call, IOException e) {
                call.cancel();
            }

            @Override
            public void onResponse(Call call, Response response) throws IOException {

                final String myResponse = response.body().string();

                runOnUiThread(new Runnable() {
                    @Override
                    public void run() {

//                        Gson gson = new Gson();
//                        Oauth oauth = gson.fromJson(myResponse, Oauth.class);
//                        Log.i("oauth", oauth.toString());

                        token.setText(myResponse);
                    }
                });

            }
        });
    }

}

```

上面的例子演示了，怎样获取 access token 以及 HTTP 基本操作 GET 和 POST，再 Restful 接口中还可能会用到 PUT, DELETE 等等，原来相同，这里就不在演示。

## HTTP/2

首先确认你的服务器是支持 HTTP2，HTTP2 配置方法可以参考 《Netkiller Web 手札》

下面是我的例子仅供参考，Nginx 开启 http2 代理后面的 Spring Cloud 接口。

```

server {
    listen       80;
    listen 443 ssl http2;
    server_name  api.netkiller.cn;

    ssl_certificate ssl/netkiller.cn.crt;
    ssl_certificate_key ssl/netkiller.cn.key;
    ssl_session_cache shared:SSL:20m;
    ssl_session_timeout 60m;

    charset utf-8;
    access_log  /var/log/nginx/api.netkiller.cn.access.log;

    error_page  497              https://$host$uri?$args;

    if ($scheme = http) {
        return 301 https://$server_name$request_uri;
    }

    location / {
		proxy_pass   http://127.0.0.1:8000;
        proxy_http_version 1.1;
		proxy_set_header    Host    $host;
		proxy_set_header    X-Forwarded-For $proxy_add_x_forwarded_for;	
    }

    #error_page  404              /404.html;

    # redirect server error pages to the static page /50x.html
    #
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }

    # deny access to .htaccess files, if Apache's document root
    # concurs with nginx's one
    #
    #location ~ /\.ht {
    #    deny  all;
    #}
}		

```

安卓程序如下

```

		String url = "https://api.netkiller.cn/member/json";

        OkHttpClient client = new OkHttpClient();
        Request request = new Request.Builder().url(url).build();
        client.newCall(request).enqueue(new Callback() {
            @Override
            public void onFailure(Call call, IOException e) {
                Log.d("okhttp", "Connect timeout. " + e.getMessage());
            }

            @Override
            public void onResponse(Call call, Response response) throws IOException {
                String text = response.body().string();
                Log.d("okhttp", "HTTP Code " + response.code() + " TEXT " + text);
                Log.d("okhttp", "Protocol: " + response.protocol());
            }
        });

```

运行结果

```

D/okhttp: HTTP Code 200 TEXT {"status":false,"reason":"","code":0,"data":{"id":null,"username":"12322228888","mobile":"12322228888","password":"123456","wechat":"微信 ID","role":"Organization","captcha":null,"createDate":"2018-10-25 09:25:23","updateDate":null}}
    Protocol: h2

```

输出 h2 表示当前接口与服务器连接是通过 http2 完成。

## 第 53 章 EventBus

[`greenrobot.org/eventbus`](http://greenrobot.org/eventbus)

在 EventBus 中主要有以下三个成员：

```

Event：事件，可以自定义为任意对象，类似 Message 类的作用；
Publisher：事件发布者，可以在任意线程、任意位置发布 Event，已发布的 Evnet 则由 EventBus 进行分发；
Subscriber：事件订阅者，接收并处理事件，需要通过 register(this)进行注册，而在类销毁时要使用 unregister(this)方法解注册。每个 Subscriber 可以定义一个或多个事件处理方法，其方法名可以自定义，但需要添加@Subscribe 的注解，并指明 ThreadMode（不写默认为 Posting）。	

```

## 添加 EventBus 依赖到项目 Gradle 文件

Gradle:

```

implementation 'org.greenrobot:eventbus:3.1.1'		

```

完整的例子

```

apply plugin: 'com.android.application'

android {
    compileSdkVersion 28
    defaultConfig {
        applicationId "cn.netkiller.eventbus"
        minSdkVersion 26
        targetSdkVersion 28
        versionCode 1
        versionName "1.0"
        testInstrumentationRunner "android.support.test.runner.AndroidJUnitRunner"
    }
    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
        }
    }
}

dependencies {
    implementation fileTree(dir: 'libs', include: ['*.jar'])
    implementation 'com.android.support:appcompat-v7:28.0.0'
    implementation 'com.android.support.constraint:constraint-layout:1.1.3'
    testImplementation 'junit:junit:4.12'
    androidTestImplementation 'com.android.support.test:runner:1.0.2'
    androidTestImplementation 'com.android.support.test.espresso:espresso-core:3.0.2'
    implementation 'org.greenrobot:eventbus:3.1.1'
}

```

## 快速开始一个演示例子

操作 EventBus 只需四个步骤

```

1\. 注册事件

EventBus.getDefault().register( this );

2\. 取消注册

EventBus.getDefault().unregister( this );

3\. 订阅事件

	@Subscribe(threadMode = ThreadMode.MAIN)
    public void onMessageEvent(MessageEvent event) {
        Toast.makeText(this, event.message, Toast.LENGTH_SHORT).show();
    }

4\. 发送数据

EventBus.getDefault().post(new MessageEvent("Helloworld"));

```

### 创建 MessageEvent 类

```

package cn.netkiller.eventbus.pojo;

public class MessageEvent {
    public final String message;

    public MessageEvent(String message) {
        this.message = message;
    }
}

```

### Layout

```

<?xml version="1.0" encoding="utf-8"?>
<android.support.constraint.ConstraintLayout 

    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <TextView
        android:id="@+id/textView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Hello World!"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginStart="8dp"
        android:layout_marginTop="8dp"
        android:layout_marginEnd="8dp"
        android:text="Button"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/textView" />

</android.support.constraint.ConstraintLayout>			

```

### Activity

```

package cn.netkiller.eventbus;

import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.view.View;
import android.widget.Toast;

import org.greenrobot.eventbus.EventBus;
import org.greenrobot.eventbus.Subscribe;
import org.greenrobot.eventbus.ThreadMode;

import cn.netkiller.eventbus.pojo.MessageEvent;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        EventBus.getDefault().register(this);

        findViewById(R.id.button).setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                EventBus.getDefault().post(new MessageEvent("Hello everyone!"));
            }
        });

    }

    @Override
    protected void onDestroy() {
        super.onDestroy();

        //取消注册 , 防止 Activity 内存泄漏
        EventBus.getDefault().unregister(this);
    }

    @Subscribe(threadMode = ThreadMode.MAIN)
    public void onMessageEvent(MessageEvent event) {
        Toast.makeText(this, event.message, Toast.LENGTH_SHORT).show();
    }
}			

```

## Sticky Events

Sticky Events 粘性事件可以理解为 Message 做了持久化，直到 Message 被消费为止。无需注册即可发送 Message。

下面的例子：在 MainActivity 发送事件，在 StickyActivity 里注册并且接收事件

```

A. MainActivity 发送事件：

EventBus.getDefault().postSticky(new MessageEvent("http://www.netkiller.cn"));

B. StickyActivity 接收事件 

1\. 注册

EventBus.getDefault().register( this );

2\. 事件接收

	@Subscribe(threadMode = ThreadMode.MAIN, sticky = true)
    public void onMessageEvent(MessageEvent event) {
        Toast.makeText(this, event.message, Toast.LENGTH_SHORT).show();
    }

3\. 取消注册

EventBus.getDefault().unregister( this ) ;	

```

### MainActivity

Layout

```

<?xml version="1.0" encoding="utf-8"?>
<android.support.constraint.ConstraintLayout 

    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <TextView
        android:id="@+id/textView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Hello World!"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginStart="8dp"
        android:layout_marginTop="8dp"
        android:layout_marginEnd="8dp"
        android:text="Button"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/textView" />

</android.support.constraint.ConstraintLayout>			

```

MainActivity

```

package cn.netkiller.eventbus;

import android.content.Intent;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.view.View;
import android.widget.Toast;

import org.greenrobot.eventbus.EventBus;
import org.greenrobot.eventbus.Subscribe;
import org.greenrobot.eventbus.ThreadMode;

import cn.netkiller.eventbus.pojo.MessageEvent;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        findViewById(R.id.button).setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                EventBus.getDefault().postSticky(new MessageEvent("Hello everyone!"));
                startActivity(new Intent(MainActivity.this, StickyActivity.class));
            }
        });

    }

}

```

### StickyActivity

```

<?xml version="1.0" encoding="utf-8"?>
<android.support.constraint.ConstraintLayout 

    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".StickyActivity">

</android.support.constraint.ConstraintLayout>			

```

StickyActivity

```

package cn.netkiller.eventbus;

import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.widget.Toast;

import org.greenrobot.eventbus.EventBus;
import org.greenrobot.eventbus.Subscribe;
import org.greenrobot.eventbus.ThreadMode;

import cn.netkiller.eventbus.pojo.MessageEvent;

public class StickyActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_sticky);

        EventBus.getDefault().register(this);
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        EventBus.getDefault().unregister(this);
    }

    @Subscribe(threadMode = ThreadMode.MAIN, sticky = true)
    public void onMessageEvent(MessageEvent event) {
        Toast.makeText(this, event.message, Toast.LENGTH_SHORT).show();
    }

}

```

### MessageEvent

```

package cn.netkiller.eventbus.pojo;

public class MessageEvent {
    public final String message;

    public MessageEvent(String message) {
        this.message = message;
    }
}			

```

### 删除粘性事件

```

MessageEvent stickyEvent = EventBus.getDefault().getStickyEvent(MessageEvent.class);

// Better check that an event was actually posted before
if(stickyEvent != null) {
	// "Consume" the sticky event
	EventBus.getDefault().removeStickyEvent(stickyEvent);
	// Now do something with it
}			

```

## 线程模型

EventBus 有五种线程模型（ThreadMode）

```

Posting：直接在事件发布者所在线程执行事件处理方法；
Main：直接在主线程中执行事件处理方法（即 UI 线程），如果发布事件的线程也是主线程，那么事件处理方法会直接被调用，并且未避免 ANR，该方法应避免进行耗时操作；
MainOrdered：也是直接在主线程中执行事件处理方法，但与 Main 方式不同的是，不论发布者所在线程是不是主线程，发布的事件都会进入队列按事件串行顺序依次执行；
BACKGROUND：事件处理方法将在后台线程中被调用。如果发布事件的线程不是主线程，那么事件处理方法将直接在该线程中被调用。如果发布事件的线程是主线程，那么将使用一个单独的后台线程，该线程将按顺序发送所有的事件。
Async：不管发布者的线程是不是主线程，都会开启一个新的线程来执行事件处理方法。如果事件处理方法的执行需要一些时间，例如网络访问，那么就应该使用该模式。为避免触发大量的长时间运行的事件处理方法，EventBus 使用了一个线程池来有效地重用已经完成调用订阅者方法的线程以限制并发线程的数量。  后面会通过代码展示五种 ThreadMode 的工作方式。

```

```

<?xml version="1.0" encoding="utf-8"?>
<android.support.constraint.ConstraintLayout 

    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".ThreadModeActivity">

    <Button
        android:id="@+id/buttonSend"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginStart="8dp"
        android:layout_marginTop="8dp"
        android:layout_marginEnd="8dp"
        android:text="Send"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <Button
        android:id="@+id/buttonThread"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginStart="8dp"
        android:layout_marginTop="8dp"
        android:layout_marginEnd="8dp"
        android:text="Send Thread"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/buttonSend" />
</android.support.constraint.ConstraintLayout>		

```

```

package cn.netkiller.eventbus;

import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.util.Log;
import android.view.View;

import org.greenrobot.eventbus.EventBus;
import org.greenrobot.eventbus.Subscribe;
import org.greenrobot.eventbus.ThreadMode;

public class ThreadModeActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_thread_mode);

        EventBus.getDefault().register(this);

        findViewById(R.id.buttonSend).setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Log.d("EventBus Thread : ", Thread.currentThread().getName());
                EventBus.getDefault().post("http://www.netkiller.cn");
            }
        });

        findViewById(R.id.buttonThread).setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                new Thread(new Runnable() {
                    @Override
                    public void run() {
                        Log.d("EventBus Thread : ", Thread.currentThread().getName());
                        EventBus.getDefault().post("http://www.netkiller.cn");

                    }
                }).start();

            }
        });

    }

    @Subscribe(threadMode = ThreadMode.POSTING)
    public void onMessageEventPostThread(String event) {
        Log.d("EventBus PostThread", "Message： " + event + "  thread: " + Thread.currentThread().getName());
    }

    @Subscribe(threadMode = ThreadMode.MAIN)
    public void onMessageEventMainThread(String event) {
        Log.d("EventBus MainThread", "Message： " + event + "  thread: " + Thread.currentThread().getName());
    }

    @Subscribe(threadMode = ThreadMode.MAIN_ORDERED)
    public void onEventMainOrdered(String event) {
        Log.d("EventBus MainOrdered", "Message: " + event + " thread:" + Thread.currentThread().getName());
    }

    @Subscribe(threadMode = ThreadMode.BACKGROUND)
    public void onMessageEventBackgroundThread(String event) {
        Log.d("EventBus BackgroundThread", "Message： " + event + "  thread: " + Thread.currentThread().getName());
    }

    @Subscribe(threadMode = ThreadMode.ASYNC)
    public void onMessageEventAsync(String event) {
        Log.d("EventBus Async", "Message： " + event + "  thread: " + Thread.currentThread().getName());
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        EventBus.getDefault().unregister(this);
    }
}

```

在 main 线程中发布消息

```

D/EventBus Thread :: main
D/EventBus MainThread: Message： http://www.netkiller.cn  thread: main
D/EventBus PostThread: Message： http://www.netkiller.cn  thread: main
D/EventBus Async: Message： http://www.netkiller.cn  thread: pool-1-thread-1
D/EventBus BackgroundThread: Message： http://www.netkiller.cn  thread: pool-1-thread-2
D/EventBus MainOrdered: Message: http://www.netkiller.cn thread:main

```

在线程中发布消息

```

D/EventBus Thread :: Thread-2
D/EventBus BackgroundThread: Message： http://www.netkiller.cn  thread: Thread-2
D/EventBus PostThread: Message： http://www.netkiller.cn  thread: Thread-2
D/EventBus Async: Message： http://www.netkiller.cn  thread: pool-1-thread-2
D/EventBus MainOrdered: Message: http://www.netkiller.cn thread:main
D/EventBus MainThread: Message： http://www.netkiller.cn  thread: main

```

## 配置 EventBus

上面章节中的例子 EventBus 实例中采用默认方式

```

EventBus.getDefault().register(this);		

```

这种方式的获取到的 EventBus 的都是默认属性，有时候并不能满足我们的要求，这时候我们可以通过 EventBusBuilder 来个性化配置 EventBus 的属性。

```

// 创建默认的 EventBus 对象，相当于 EventBus.getDefault()。

EventBus installDefaultEventBus()：
// 添加由 EventBus“注释预处理器生成的索引
EventBuilder addIndex(SubscriberInfoIndex index)：
// 默认情况下，EventBus 认为事件类有层次结构（订户超类将被通知）
EventBuilder eventInheritance(boolean eventInheritance)：
// 定义一个线程池用于处理后台线程和异步线程分发事件
EventBuilder executorService(java.util.concurrent.ExecutorService executorService)：
// 设置忽略订阅索引，即使事件已被设置索引，默认为 false
EventBuilder ignoreGeneratedIndex(boolean ignoreGeneratedIndex)：
// 打印没有订阅消息，默认为 true
EventBuilder logNoSubscriberMessages(boolean logNoSubscriberMessages)：
// 打印订阅异常，默认 true
EventBuilder logSubscriberExceptions(boolean logSubscriberExceptions)：
// 设置发送的的事件在没有订阅者的情况时，EventBus 是否保持静默，默认 true
EventBuilder sendNoSubscriberEvent(boolean sendNoSubscriberEvent)：
// 发送分发事件的异常，默认 true
EventBuilder sendSubscriberExceptionEvent(boolean sendSubscriberExceptionEvent)：
// 在 3.0 以前，接收处理事件的方法名以 onEvent 开头，方法名称验证避免不是以此开头，启用严格的方法验证（默认：false）
EventBuilder strictMethodVerification(java.lang.Class<?> clazz)
// 如果 onEvent***方法出现异常，是否将此异常分发给订阅者（默认：false）
EventBuilder throwSubscriberException(boolean throwSubscriberException)

```

我的实例参考

```

EventBus eventBus = EventBus.builder().eventInheritance(true)
    .ignoreGeneratedIndex(false)
    .logNoSubscriberMessages(true)
    .logSubscriberExceptions(false)
    .sendNoSubscriberEvent(true)
    .sendSubscriberExceptionEvent(true)
    .throwSubscriberException(false)
    .strictMethodVerification(true)
    .build();
eventBus.register(this);		

```

## 事件优先级

priority 数值越大优先级又高

```

	// MainActivity
	@Subscribe(priority = 2)
    public void onMessageEvent(MessageEvent event) {
        Toast.makeText(this, event.message, Toast.LENGTH_SHORT).show();
    }

	// SecondActivity
	@Subscribe(priority = 1)
    public void onMessageSecondEvent(MessageEvent event) {
        Toast.makeText(this, event.message, Toast.LENGTH_SHORT).show();
    }	

```

时间拦截，MainActivity 收到信息后调用 EventBus.getDefault().cancelEventDelivery(event); 之后所有订阅将收不到信息。

```

	// MainActivity
	@Subscribe(priority = 2)
    public void onMessageEvent(MessageEvent event) {
        Toast.makeText(this, event.message, Toast.LENGTH_SHORT).show();
        EventBus.getDefault().cancelEventDelivery(event);
    }

	// SecondActivity
	@Subscribe(priority = 1)
    public void onMessageSecondEvent(MessageEvent event) {
        Toast.makeText(this, event.message, Toast.LENGTH_SHORT).show();
    }

```

## 捕获异常事件

在 init() 中加入你的业务逻辑，根据需要，在特定的情况下使用 throw new Exception("异常信息"); 抛出异常。异常会被 hrowableFailureEvent(ThrowableFailureEvent event) 捕获到。

```

package cn.netkiller.eventbus;

import android.content.Intent;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.util.Log;
import android.view.View;
import android.widget.Toast;

import org.greenrobot.eventbus.EventBus;
import org.greenrobot.eventbus.Subscribe;
import org.greenrobot.eventbus.ThreadMode;
import org.greenrobot.eventbus.util.AsyncExecutor;
import org.greenrobot.eventbus.util.ThrowableFailureEvent;

import cn.netkiller.eventbus.pojo.MessageEvent;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        EventBus.getDefault().register(this);

        findViewById(R.id.button).setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {

                AsyncExecutor.create().execute(
                        new AsyncExecutor.RunnableEx() {
                            @Override
                            public void run() throws Exception {
                                init();
                                EventBus.getDefault().post(new MessageEvent("Hello everyone!"));
                            }
                        }
                );
            }
        });
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        EventBus.getDefault().unregister(this);
    }

    @Subscribe(threadMode = ThreadMode.MAIN)
    public void onMessageEvent(MessageEvent event) {
        Toast.makeText(this, event.message, Toast.LENGTH_SHORT).show();
    }

    public void init() throws Exception {
        // ...
        throw new Exception("实际发送异常");
    }

    @Subscribe(threadMode = ThreadMode.MAIN)
    public void hrowableFailureEvent(ThrowableFailureEvent event) {
        Log.d("EventBus", "hrowableFailureEvent: " + event.getThrowable().getMessage());
        Toast.makeText(this, event.getThrowable().getMessage(), Toast.LENGTH_SHORT).show();
    }

}

```

## 第 54 章 设计模式

## 单例模式

```

package cn.netkiller.voice;

import android.media.MediaRecorder;
import android.os.Environment;
import android.util.Log;

import java.io.File;
import java.io.IOException;
import java.text.SimpleDateFormat;
import java.util.Date;

public class Audio {

    private boolean isRecord = false;

    private MediaRecorder mediaRecorder;
    private String filename;

    private Audio() {

    }

    private static Audio instance;

    public synchronized static Audio getInstance() {
        if (instance == null)
            instance = new Audio();
        return instance;
    }

    public String getFilename() {
        return filename;
    }

    public void start() {

        if (mediaRecorder == null) {

            String path = Environment.getExternalStorageDirectory().getPath();
            String folder = new SimpleDateFormat("yyyy-MM-dd").format(new Date());
            String name = new SimpleDateFormat("hhmmss").format(new Date());
            new File(path, folder).mkdirs();

            filename = String.format("%s/%s/%s.3gp", path, folder, name);
            Log.e("Voice", "voice path " + filename);

            try {

                mediaRecorder = new MediaRecorder();
                mediaRecorder.setAudioSource(MediaRecorder.AudioSource.MIC);
                mediaRecorder.setOutputFormat(MediaRecorder.OutputFormat.DEFAULT);
                mediaRecorder.setAudioEncoder(MediaRecorder.AudioEncoder.DEFAULT);
                mediaRecorder.setOutputFile(filename);
                mediaRecorder.prepare();
                mediaRecorder.start();

                isRecord = true;

            } catch (IOException ex) {
                ex.printStackTrace();

            }
        }

    }

    public void stop() {
        if (mediaRecorder != null && isRecord) {
            System.out.println("stopRecord");
            isRecord = false;
            mediaRecorder.stop();
            mediaRecorder.release();
            mediaRecorder = null;
        }
    }

}	

```

## 第 55 章 

## java.net.UnknownServiceException: CLEARTEXT communication to 192.168.0.185 not permitted by network security policy

okhttp 默认使用 https 链接服务器，如果使用 http 会抛出现上面的异常

```

if (!Platform.get().isCleartextTrafficPermitted(host)) {
      throw new RouteException(new UnknownServiceException(
          "CLEARTEXT communication to " + host + " not permitted by network security policy"));
}		

```

创建文件 res/xml/network_security_config.xml 内容如下

```

neo@MacBook-Pro ~/AndroidStudioProjects/okhttp % cat app/src/main/res/xml/network_security_config.xml 
<?xml version="1.0" encoding="utf-8"?>
<network-security-config>
    <base-config cleartextTrafficPermitted="true" />
</network-security-config>	

```

再 app/src/main/AndroidManifest.xml 文件中增加 android:networkSecurityConfig="@xml/network_security_config"

```

<?xml version="1.0" encoding="utf-8"?>
<manifest 
    package="cn.netkiller.okhttp">

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/AppTheme">
        <activity
            android:name=".MainActivity"
            android:label="@string/app_name"
            android:networkSecurityConfig="@xml/network_security_config">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

    <uses-permission android:name="android.permission.INTERNET" />
</manifest>		

```

## Caused by: android.os.NetworkOnMainThreadException

主线程不能访问网络，在访问网络的代码前面添加如下代码即可：

```

StrictMode.ThreadPolicy policy= new StrictMode.ThreadPolicy.Builder().permitAll().build();
StrictMode.setThreadPolicy(policy);		

```

或者写在 setContentView(R.layout.activity_main); 后面

另一种方式是在线程中执行

```

       new Thread(new Runnable() {
            @Override
            public void run() {

                try {
                    String json = get("http://192.168.0.185:8080/member/json");
                } catch (IOException e) {
                    e.printStackTrace();
                }

            }
        }).start();		

```