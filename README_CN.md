# flutter_qq_login

Flutter集成QQ登录插件

|             | Android | iOS   |
|-------------|---------|-------|
| **Support** | SDK 19+ | 11.0+ |

<p>
  <img src="https://github.com/yechong/flutter_qq_login/blob/main/doc/images/android.gif?raw=true"
    alt="An animated image of the iOS QQ Login Plugin UI" height="400"/>
  &nbsp;&nbsp;&nbsp;&nbsp;
  <img src="https://github.com/yechong/flutter_qq_login/blob/main/doc/images/iphone.gif?raw=true"
   alt="An animated image of the Android QQ Login Plugin UI" height="400"/>
</p>

## 功能

此插件已集成QQ登录的功能：
- 判断是否已安装QQ APP `isQQInstalled()`
- 登录成功后获取的数据，其中包含 `OpenId`、`AccessToken` 等重要数据
- 获取用户信息 `getUserInfo()`

## 入门

使用此插件前，强力建议详细阅读官方文档
- [Android_SDK环境搭建](https://wiki.connect.qq.com/qq%e7%99%bb%e5%bd%95)
- [iOS_SDK环境搭建](https://wiki.connect.qq.com/ios_sdk%e7%8e%af%e5%a2%83%e6%90%ad%e5%bb%ba)

### 使用方法

```dart
import 'package:flutter_qq_login/flutter_qq_login.dart';

// 创建 FlutterQqLogin
final flutterQqLogin = FlutterQqLogin();

// 初始化，请填写QQ互联创建的APP应用ID
flutterQqLogin.init(appId: "你的APPID");

// 判断当前是否安装QQ应用
bool isInstalled = await flutterQqLogin.isInstalled();

// 调起QQ登录，登录成功后返回 OpenId、AccessToken 等重要数据
Map<String, dynamic> qqInfo = await flutterQqLogin.login();

// 获取用户信息
Map<String, dynamic> userInfo = await flutterQqLogin.getUserInfo(qqInfo['access_token'], qqInfo['openid']);

```

### 配置Android版本

配置 `android/app/build.gradle`
```
android {
    ...

    defaultConfig {
        ...
        minSdkVersion 19
        ...
    }

}

```

配置 `android/app/src/main/AndroidManifest.xml`

```
<manifest xmlns:android="http://schemas.android.com/apk/res/android">
	<!-- 新增的内容 开始 -->
	<uses-permission android:name="android.permission.INTERNET" />
	<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
	<!-- 新增的内容 结束 -->
	<application>
		...
		<!-- 新增的内容 开始 -->
		<activity
            android:name="com.tencent.tauth.AuthActivity"
            android:noHistory="true"
            android:launchMode="singleTask"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.VIEW" />
                <category android:name="android.intent.category.DEFAULT" />
                <category android:name="android.intent.category.BROWSABLE" />
                <data android:scheme="你的APPID" />
            </intent-filter>
        </activity>
        <activity
            android:name="com.tencent.connect.common.AssistActivity"
            android:configChanges="orientation|keyboardHidden"
            android:screenOrientation="behind"
            android:theme="@android:style/Theme.Translucent.NoTitleBar" />
		<!-- 新增的内容 结束 -->
		...
	</application>
	<!-- 新增的内容 开始 -->
	<queries>
		<package android:name="com.tencent.mobileqq" />
	</queries>
	<!-- 新增的内容 结束 -->
</manifest>
```


### 配置iOS版本

配置 `URL Types`
- 使用 `xcode` 打开你的 iOS 工程 `Runner.xcworkspace`
- 在 `info` 配置选项卡中的 `URL Types` 下，新增一项
    - `identifier` 填写 `tencentopenapi`
    - `URL Schemes` 填写 `tencent123456789` ，其中 `123456789` 是你的 `APPID`
    - 如下图所示：
      ![xcode配置事例](https://raw.githubusercontent.com/yechong/flutter_qq_login/main/doc/images/ios_screenshot_01.png)

配置 `LSApplicationQueriesSchemes`
- 方式一，在 `xcode` 中配置 `info`
    - 打开 `info` 配置，添加一项 `LSApplicationQueriesSchemes` ，即 `Queried URL Schemes`
    - 添加以下这些项：
        - mqqopensdknopasteboard
        - mqqapi
        - mqq
        - mqqOpensdkSSoLogin
        - mqqconnect
        - mqqopensdkdataline
        - mqqopensdkgrouptribeshare
        - mqqopensdkfriend
        - mqqopensdkapi
        - mqqopensdkapiV2
        - mqqopensdkapiV3
        - mqzoneopensdk
        - wtloginmqq
        - wtloginmqq2
        - mqqwpa
        - mqzone
        - mqzonev2
        - mqzoneshare
        - wtloginqzone
        - mqzonewx
        - mqzoneopensdkapiV2
        - mqzoneopensdkapi19
        - mqzoneopensdkapi
        - mqzoneopensdk
    - 如下图所示：
      ![xcode配置事例](https://raw.githubusercontent.com/yechong/flutter_qq_login/main/doc/images/ios_screenshot_02.png)

- 方式二，直接修改 `Info.plist`
    - 使用 `Android Studio` 打开项目工程下的 `ios/Runner/Info.plist`
    - 在 `dict` 节点下增加以下配置 (可参考文件里的配置格式)：
```
<key>LSApplicationQueriesSchemes</key>
<array>
	<string>mqqopensdknopasteboard</string>
	<string>mqqapi</string>
	<string>mqq</string>
	<string>mqqOpensdkSSoLogin</string>
	<string>mqqconnect</string>
	<string>mqqopensdkdataline</string>
	<string>mqqopensdkgrouptribeshare</string>
	<string>mqqopensdkfriend</string>
	<string>mqqopensdkapi</string>
	<string>mqqopensdkapiV2</string>
	<string>mqqopensdkapiV3</string>
	<string>mqzoneopensdk</string>
	<string>wtloginmqq</string>
	<string>wtloginmqq2</string>
	<string>mqqwpa</string>
	<string>mqzone</string>
	<string>mqzonev2</string>
	<string>mqzoneshare</string>
	<string>wtloginqzone</string>
	<string>mqzonewx</string>
	<string>mqzoneopensdkapiV2</string>
	<string>mqzoneopensdkapi19</string>
	<string>mqzoneopensdkapi</string>
	<string>mqzoneopensdk</string>
</array>
```



## LICENSE

```Copyright 2018 OpenFlutter Project

BSD 3-Clause License

Copyright 2017 German Saprykin
All rights reserved.

Redistribution and use in source and binary forms, with or without
modification, are permitted provided that the following conditions are met:

* Redistributions of source code must retain the above copyright notice, this
  list of conditions and the following disclaimer.

* Redistributions in binary form must reproduce the above copyright notice,
  this list of conditions and the following disclaimer in the documentation
  and/or other materials provided with the distribution.

* Neither the name of the copyright holder nor the names of its
  contributors may be used to endorse or promote products derived from
  this software without specific prior written permission.

THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS"
AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE
IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE
DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT HOLDER OR CONTRIBUTORS BE LIABLE
FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL
DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR
SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER
CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY,
OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE
OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.```
