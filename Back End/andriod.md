# andriod 开发

## 文件位置

1. '/res/layout/' 下的 XML 文件 对应的是页面布局文件
1. '/res/values/strings.xml' 对应布局中默认字符串的文件
1. '/res/values/color.xml' 定义颜色的文件
1. 三列布局可以使用
   ```
    <RelativeLayout
       <TextView/>
       <TextView/>
       <TextView/>
    </RelativeLayout>
   ```

## 版本

关于 build:gradle 的版本，要引入非下载的包（应该是这么个说法），先升级到 3.1.4,然后在添加`google()`

```
buildscript {
    repositories {
        jcenter()
        google()
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:3.1.4'
    }
}

allprojects {
    repositories {
        jcenter()
        google()
    }
}
```

## 代码

### 事件监听

#### onCreate 中实现

```
@Override
	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_main);

		Button button = (Button) findViewById(R.id.button);
		button.setOnClickListener(new OnClickListener() {
			@Override
			public void onClick(View v) {
			}
		});
	}
```

#### 布局中实现

```
<!-- 布局文件 -->
 <TextView
    android:id="@+id/tv_back"
    android:onClick="goBak"/>
<!-- java -->
 public void goBak(View v) {}
```

### 实现 webview 上传图片

```
        // 重写方法，实现图片上传
        vtiWeb.setWebChromeClient(new WebChromeClient() {
            @Override
            public void onReceivedTitle(WebView view, String title) {
                super.onReceivedTitle(view, title);
//                if (!TextUtils.isEmpty(title)) mTitleView.setTitle(title);
            }

            @Override
            public void onProgressChanged(WebView view, int newProgress) {
                if(pbWebBase != null)pbWebBase.setProgress(newProgress);
                super.onProgressChanged(view, newProgress);
            }

            // For Android < 3.0
            public void openFileChooser(ValueCallback<Uri> valueCallback) {
                uploadMessage = valueCallback;
                openImageChooserActivity();
            }

            // For Android  >= 3.0
            public void openFileChooser(ValueCallback valueCallback, String acceptType) {
                uploadMessage = valueCallback;
                openImageChooserActivity();
            }

            //For Android  >= 4.1
            public void openFileChooser(ValueCallback<Uri> valueCallback, String acceptType, String capture) {
                uploadMessage = valueCallback;
                openImageChooserActivity();
            }

            // For Android >= 5.0
            @Override
            public boolean onShowFileChooser(WebView webView, ValueCallback<Uri[]> filePathCallback, WebChromeClient.FileChooserParams fileChooserParams) {
                uploadMessageAboveL = filePathCallback;
                openImageChooserActivity();
                return true;
            }

        });
```

### 实现权限自动申请

implements 包的时候，一样的方法名一样的函数，但是报错需要从 class QuestionWebAct 处重新添加必须的函数

```
<!-- 使用的是easypermissions公共包，直接在build,gradle中引入，旧的程序可能需要修改compileSdkVersion -->
dependencies {
    implementation 'pub.devrel:easypermissions:2.0.0'
}


import android.widget.Toast;
import android.support.annotation.NonNull;
import android.Manifest;
import java.util.List;
import pub.devrel.easypermissions.EasyPermissions;
<!-- 加上 EasyPermissions.PermissionCallbacks -->
public class QuestionWebAct extends AppCompatActivity implements EasyPermissions.PermissionCallbacks {
    <!-- 权限string，此处是相机与存储 -->
    private String[] permissions = {Manifest.permission.CAMERA, Manifest.permission.WRITE_EXTERNAL_STORAGE};

    private void getPermission() {
        if (EasyPermissions.hasPermissions(this, permissions)) {
            //已经打开权限
            Toast.makeText(this, "已经申请相关权限", Toast.LENGTH_SHORT).show();
        } else {
            //没有打开相关权限、申请权限
            EasyPermissions.requestPermissions(this, "需要获取您的相册、照相使用权限", 1, permissions);
        }
    }

    @Override
    public void onRequestPermissionsResult(int requestCode, @NonNull String[] permissions, @NonNull int[] grantResults) {
        super.onRequestPermissionsResult(requestCode, permissions, grantResults);
        //框架要求必须这么写
        EasyPermissions.onRequestPermissionsResult(requestCode, permissions, grantResults, this);
    }


    //成功打开权限
    @Override
    public void onPermissionsGranted(int requestCode, @NonNull List<String> perms) {

//        Toast.makeText(this, "相关权限获取成功", Toast.LENGTH_SHORT).show();

    }
    //用户未同意权限
    @Override
    public void onPermissionsDenied(int requestCode, @NonNull List<String> perms) {
        Toast.makeText(this, "请同意相关权限，否则功能无法使用", Toast.LENGTH_SHORT).show();
    }

    //在流程中调用
    protected void onCreate(Bundle savedInstanceState) {
        getPermission()
    }
}

```
### attention
在修改app的时候，发现了扫描二维码之后没有成功打开网址，一开始以为是各种包版本太低（新引用的包确实无法使用），因为方法执行上逻辑是没有问题的，最后定位到的是修改了XML布局，导致扫码成功页面的java获取不到之前的元素ID，应该造成了“APP重启”。这类似于DOM，如果之后再接触andorid需要注意全局搜索。