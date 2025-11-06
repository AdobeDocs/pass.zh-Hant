---
title: 頁首 — X-Device-Info
description: REST API V2 — 標題 — X-Device-Info
exl-id: 0ef25e06-86de-427a-a938-7ba3817f0d5e
source-git-commit: 42df16e34783807e1b5eb1a12ca9db92f4e4c161
workflow-type: tm+mt
source-wordcount: '1133'
ht-degree: 2%

---

# 頁首 — X-Device-Info {#header-x-device-info}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

## 概觀 {#overview}

<b>X-Device-Info</b>要求標頭包含與實際串流裝置相關的使用者端資訊（裝置、連線和應用程式），用來決定MVPD可能強制執行的平台特定規則。

## 語法 {#syntax}

<table style="table-layout:auto">
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>X-Device-Info</b>： &lt;device_info&gt;</td>
   </tr>
   <tr>
      <td>頁首型別</td>
      <td>請求標頭</td>
   </tr>
   <tr>
      <td>標準</td>
      <td>否</td>
   </tr>
</table>

## 指令 {#directives}

<b>&lt;device_information></b>

JSON元素的`Base64-encoded`值，至少包含下表標示的必要屬性。

<table style="table-layout:auto">
    <tr>
        <th style="background-color: #EFF2F7; width: 15%;">是否存在</th>
        <th style="background-color: #EFF2F7; width: 15%;">索引鍵</th>
        <th style="background-color: #EFF2F7;">說明</th>    
        <th style="background-color: #EFF2F7; width: 15%;">受限制</th>
        <th style="background-color: #EFF2F7;">可能的值</th>
    </tr>
    <tr>
        <td></td>
        <td>primaryHardwaretype</td>
        <td>裝置的主要硬體型別。</td>
        <td>&amp;amp；檢查；</td>
        <td>
            值受到限制：
            <ul>
                <li>相機</li>
                <li>DataCollectionTerminal</li>
                <li>案頭</li>
                <li>嵌入式網路模組</li>
                <li>電子閱讀器</li>
                <li>遊戲主控台</li>
                <li>GeolocationTracker</li>
                <li>眼鏡</li>
                <li>MediaPlayer</li>
                <li>行動電話</li>
                <li>支付終端機</li>
                <li>PluginModem</li>
                <li>SetTopBox</li>
                <li>TV</li>
                <li>平板電腦</li>
                <li>WirelessHotspot</li>
                <li>手錶</li>
                <li>未知</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td><i>必填</i></td>
        <td>模型</td>
        <td>裝置的型號名稱。</td>
        <td></td>
        <td>例如iPhone、SM-G930V、AppleTV等。</td>
    </tr>
    <tr>
        <td><i>必填</i></td>
        <td>版本</td>
        <td>裝置的版本。</td>
        <td></td>
        <td>例如2.0.1等。</td>
    </tr>
    <tr>
        <td></td>
        <td>製造商</td>
        <td>裝置的製造公司/組織。</td>
        <td></td>
        <td>例如三星、LG、ZTE、華為、摩托羅拉、Apple等。</td>
    </tr>
    <tr>
        <td></td>
        <td>廠商</td>
        <td>裝置的銷售公司/組織。</td>
        <td></td>
        <td>例如Apple、Samsung、LG、Google等。</td>
    </tr>
    <tr>
        <td><i>必填</i></td>
        <td>osName</td>
        <td>裝置的作業系統(OS)名稱。</td>
        <td>&amp;amp；檢查；</td>
        <td>
            值受到限制：
            <ul>
                <li>Android</li>
                <li>Chrome作業系統</li>
                <li>Linux</li>
                <li>Mac作業系統</li>
                <li>OS X</li>
                <li>OpenBSD</li>
                <li>Roku OS</li>
                <li>Windows</li>
                <li>iOS</li>
                <li>tvOS</li>
                <li>webOS</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td></td>
        <td>osFamily</td>
        <td>裝置的作業系統(OS)群組名稱。</td>
        <td>&amp;amp；檢查；</td>
        <td>
            值受到限制：
            <ul>
                <li>Android</li>
                <li>BSD</li>
                <li>Linux</li>
                <li>PlayStation作業系統</li>
                <li>Roku OS</li>
                <li>Symbian</li>
                <li>Tizen</li>
                <li>Windows</li>
                <li>iOS</li>
                <li>tvOS</li>
                <li>macOS</li>
                <li>webOS</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td></td>
        <td>osVendor</td>
        <td>裝置的作業系統(OS)供應商。</td>
        <td>&amp;amp；檢查；</td>
        <td>
            值受到限制：
            <ul>
                <li>Amazon</li>
                <li>Apple</li>
                <li>Google</li>
                <li>LG</li>
                <li>Microsoft</li>
                <li>Mozilla</li>
                <li>任天堂</li>
                <li>Nokia</li>
                <li>Roku</li>
                <li>Samsung</li>
                <li>Sony</li>
                <li>Tizen專案</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td><i>必填</i></td>
        <td>osVersion</td>
        <td>裝置的作業系統(OS)版本。</td>
        <td></td>
        <td>例如10.2、9.0.1等。</td>
    </tr>
    <tr>
        <td></td>
        <td>browserName</td>
        <td>瀏覽器的名稱。</td>
        <td>&amp;amp；檢查；</td>
        <td>
            值受到限制：
            <ul>
                <li>Android瀏覽器</li>
                <li>Chrome</li>
                <li>Edge</li>
                <li>Firefox</li>
                <li>Internet Explorer</li>
                <li>Opera</li>
                <li>Safari</li>
                <li>SeaMonke</li>
                <li>Symbian Browser</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td></td>
        <td>browserVendor</td>
        <td>瀏覽器的建置公司/組織。</td>
        <td>&amp;amp；檢查；</td>
        <td>
            值受到限制：
            <ul>
                <li>Amazon</li>
                <li>Apple</li>
                <li>Google</li>
                <li>Microsoft</li>
                <li>摩托羅拉</li>
                <li>Mozilla</li>
                <li>Netscape</li>
                <li>任天堂</li>
                <li>Nokia</li>
                <li>Samsung</li>
                <li>Sony Ericsson</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td></td>
        <td>browserVersion</td>
        <td>裝置的瀏覽器版本。</td>
        <td></td>
        <td>例如60.0.3112</td>
    </tr>
    <tr>
        <td></td>
        <td>userAgent</td>
        <td>裝置的使用者代理。</td>
        <td></td>
        <td>例如Mozilla/5.0 (Macintosh；Intel Mac OS X 10_12_3) AppleWebKit/602.4.8 （KHTML，如Gecko）版本/10.0.3 Safari/602.4.8</td>
    </tr>
    <tr>
        <td></td>
        <td>displaywidth</td>
        <td>裝置的實體熒幕寬度。</td>
        <td></td>
        <td></td>
    </tr>
    <tr>
        <td></td>
        <td>displayheight</td>
        <td>裝置的實體熒幕高度。</td>
        <td></td>
        <td></td>
    </tr>
    <tr>
        <td></td>
        <td>displayPpi</td>
        <td>裝置的實體熒幕畫素密度。</td>
        <td></td>
        <td>例如294</td>
    </tr>
    <tr>
        <td></td>
        <td>diagonalScreenSize</td>
        <td>裝置的實體熒幕對角線尺寸（英吋）。</td>
        <td></td>
        <td>例如5.5、10.1</td>
    </tr>
    <tr>
        <td></td>
        <td>connectionIp</td>
        <td>用於傳送HTTP要求的裝置IP。</td>
        <td></td>
        <td>例如8.8.4.4</td>
    </tr>
    <tr>
        <td></td>
        <td>connectionPort</td>
        <td>用於傳送HTTP要求的裝置連線埠。</td>
        <td></td>
        <td>例如53124</td>
    </tr>
    <tr>
        <td><i>必填</i></td>
        <td>connectionType</td>
        <td>網路連線型別。</td>
        <td></td>
        <td>例如WiFi、LAN、3G、4G、5G</td>
    </tr>
    <tr>
        <td></td>
        <td>connectionSecure</td>
        <td>網路連線安全性狀態。</td>
        <td>&amp;amp；檢查；</td>
        <td>
            值受到限制：
            <ul>
                <li>true — 在安全網路的情況下</li>
                <li>false — 在公開熱點的情況下</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td></td>
        <td>applicationId</td>
        <td>應用程式的唯一識別碼。</td>
        <td></td>
        <td>例如REF30</td>
    </tr>
</table>


## 範例 {#examples}

```JSON
// Device information
// {
//  "primaryHardwareType" : "MobilePhone",
//  "model":"SM-S901U",
//  "vendor":"samsung",
//  "version":"r0q",
//  "manufacturer":"samsung",
//  "osName":"Android",
//  "osVersion":"14"
// }
 
// BASE64-encoded
// ewogICJwcmltYXJ5SGFyZHdhcmVUeXBlIiA6ICJNb2JpbGVQaG9uZSIsCiAgIm1vZGVsIjoiU00tUzkwMVUiLAogICJ2ZW5kb3I
// iOiJzYW1zdW5nIiwKICAidmVyc2lvbiI6InIwcSIsCiAgIm1hbnVmYWN0dXJlciI6InNhbXN1bmciLAogICJvc05hbWUiOiJBbmRyb
// 2lkIiwKICAib3NWZXJzaW9uIjoiMTQiCn0=
 
X-Device-Info: ewogICJwcmltYXJ5SGFyZHdhcmVUeXBlIiA6ICJNb2JpbGVQaG9uZSIsCiAgIm1vZGVsIjoiU00tUzkwMVUiLAogICJ2ZW5kb3IiOiJzYW1zdW5nIiwKICAidmVyc2lvbiI6InIwcSIsCiAgIm1hbnVmYWN0dXJlciI6InNhbXN1bmciLAogICJvc05hbWUiOiJBbmRyb2lkIiwKICAib3NWZXJzaW9uIjoiMTQiCn0=
```

## 逐步指南 {#cookbooks}

>[!IMPORTANT]
> 
> 提供的程式碼片段和檔案資源僅供參考。
> 
> 程式碼片段並非詳盡無遺，且可能需要額外修改才能在專案中運作。
>
> 無論您實際實作為何，`X-Device-Info`標頭都必須包含如[指示](#directives)區段中所述格式化的值。

### 瀏覽器 {#browsers}

對於在瀏覽器中執行的使用者端應用程式，可以省略`X-Device-Info`標頭，因為瀏覽器會自動在`User-Agent`標頭中傳送一組最少的必要資訊。

您仍然可以使用`X-Device-Info`標頭來提供裝置、連線和應用程式的額外資訊，以防您的使用者端應用程式整合提供裝置識別機制的程式庫或服務。

### 行動裝置 {#mobile-devices}

#### iOS和iPadOS {#ios-ipados}

若要為執行`X-Device-Info`iOS或iPadOS[的裝置建置](https://developer.apple.com/documentation/ios-ipados-release-notes)標題，您可以參考下列檔案及下列程式碼片段：

* [UIDevice](https://developer.apple.com/documentation/uikit/uidevice#//apple_ref/occ/cl/UIDevice)的Apple開發人員檔案。
* [可存取性](https://developer.apple.com/library/archive/samplecode/Reachability/Introduction/Intro.html)的Apple開發人員檔案。
* [uname](https://man7.org/linux/man-pages/man2/uname.2.html)的Linux手動檔案。

```C
+ (NSString *)computeClientInformation {        
        struct utsname u;
        uname(&u);

        NSString *hardware = [NSString stringWithCString:u.machine encoding:NSUTF8StringEncoding];

        UIDevice *device = [UIDevice currentDevice];

        NSString *deviceType;

        switch (UI_USER_INTERFACE_IDIOM()) {
            case UIUserInterfaceIdiomPhone:
                deviceType = @"MobilePhone";
                break;
            case UIUserInterfaceIdiomPad:
                deviceType = @"Tablet";
                break;
            case UIUserInterfaceIdiomTV:
                deviceType = @"TV";
                break;
            default:
                deviceType = @"Unknown";
        }

        CGRect screenRect = [[UIScreen mainScreen] bounds];
        NSNumber *screenWidth = @((float) screenRect.size.width);
        NSNumber *screenHeight = @((float) screenRect.size.height);

        Reachability *reachability = [Reachability reachabilityForInternetConnection];
        [reachability startNotifier];

        NetworkStatus status = [reachability currentReachabilityStatus];

        NSString *connectionType;

        if (status == NotReachable) {
            connectionType = @"notConnected";
        } else if (status == ReachableViaWiFi) {
            connectionType = @"WiFi";
        } else if (status == ReachableViaWWAN) {
            connectionType = @"cellular";
        }

        NSMutableDictionary *clientInformation = [@{
                @"type": deviceType,
                @"model": device.model,
                @"vendor": @"Apple",
                @"manufacturer": @"Apple",
                @"version": [hardware stringByReplacingOccurrencesOfString:device.model withString:@""],
                @"osName": device.systemName,
                @"osVersion": device.systemVersion,
                @"displayWidth": screenWidth,
                @"displayHeight": screenHeight,
                @"connectionType": connectionType,
                @"applicationId": @"REF30" 
        } mutableCopy];

        NSError *error;
        NSData *jsonData = [NSJSONSerialization dataWithJSONObject:clientInformation options:NSJSONWritingPrettyPrinted error:&error];
        NSString *base64Encoded = [jsonData base64EncodedStringWithOptions:0];

        return base64Encoded;
}
```

裝置資訊的建構方式如下：

| 索引鍵 | Source | 值（範例） |
|---------------|------------------------|-----------------|
| 模型 | uname.machine | iPhone |
| 廠商 | 硬式編碼 | Apple |
| 製造商 | 硬式編碼 | Apple |
| 版本 | uname.machine | 8,1 |
| displaywidth | UIScreen.mainScreen | 320 |
| displayheight | UIScreen.mainScreen | 568 |
| osName | UIDevice.systemName | iOS |
| osVersion | UIDevice.systemVersion | 10.2 |

連線資訊的建構方式如下：

| 索引鍵 | Source | 值（範例） |
|------------------|------------------------------------------|-----------------|
| connectionType | [連線能力currentReachableStatus] |                 |
| connectionSecure |                                          |                 |


應用程式資訊的建構方式如下：

| 索引鍵 | Source | 值（範例） |
|---------------|-----------|-----------------|
| applicationId | 硬式編碼 | REF30 |

#### Android {#android}

若要為執行`X-Device-Info`Android[的裝置建置](https://developer.android.com/about/versions)標題，您可以參考下列檔案及下列程式碼片段：

* [Build](https://developer.android.com/reference/android/os/Build.html)類別的Android開發人員檔案。

```JAVA
private JSONObject computeClientInformation() {
     String LOGGING_TAG = "DefineClass.class";
  
     JSONObject clientInformation = new JSONObject();

     String connectionType;

     try {
          ConnectivityManager cm = (ConnectivityManager) getContext().getSystemService(CONNECTIVITY_SERVICE);
          NetworkInfo activeNetwork = cm.getActiveNetworkInfo();

          if (activeNetwork != null && activeNetwork.isConnectedOrConnecting()) {
              switch (activeNetwork.getType()) {
                    case ConnectivityManager.TYPE_WIFI: {
                        connectionType = "WIFI";
                        break;
                    }
                    case ConnectivityManager.TYPE_BLUETOOTH: {
                        connectionType = "BLUETOOTH";
                        break;
                    }
                    case ConnectivityManager.TYPE_MOBILE: {
                        connectionType = "MOBILE";
                        break;
                    }
                    case ConnectivityManager.TYPE_ETHERNET: {
                        connectionType = "ETHERNET";
                        break;
                    }
                    case ConnectivityManager.TYPE_VPN: {
                        connectionType = "VPN";
                        break;
                    }
                    case ConnectivityManager.TYPE_DUMMY: {
                        connectionType = "DUMMY";
                        break;
                    }
                    case ConnectivityManager.TYPE_MOBILE_DUN: {
                        connectionType = "MOBILE_DUN";
                        break;
                    }
                    case ConnectivityManager.TYPE_WIMAX: {
                        connectionType = "WIMAX";
                        break;
                    }
                    default:
                       connectionType = ConnectivityManager.EXTRA_OTHER_NETWORK_INFO;
              }
          } else {
                connectionType = ConnectivityManager.EXTRA_NO_CONNECTIVITY;
          }
     } catch (Exception e) {
          connectionType = "notAccessible";
     }

     try {
          clientInformation.put("model", Build.MODEL);
          clientInformation.put("vendor", Build.BRAND);
          clientInformation.put("manufacturer", Build.MANUFACTURER);
          clientInformation.put("version", Build.DEVICE);
          clientInformation.put("osName", "Android");
          clientInformation.put("osVersion", Build.VERSION.RELEASE);
          clientInformation.put("connectionType", connectionType);
          clientInformation.put("applicationId", "REF30");
     } catch (JSONException e) {
          Log.e(LOGGING_TAG, e.getMessage());
     }

     return Base64.encodeToString(clientInformation.toString().getBytes(), Base64.NO_WRAP);
}
```

裝置資訊的建構方式如下：

| 索引鍵 | Source | 值（範例） |
|---------------|-----------------------------|-----------------|
| 模型 | Build.MODEL | GT-I9505 |
| 廠商 | Build.BRAND | samsung |
| 製造商 | Build.MANUFACTURER | samsung |
| 版本 | Build.DEVICE | jflte |
| displaywidth | DisplayMetrics.widthPixels | 600 |
| displayheight | DisplayMetrics.heightPixels | 800 |
| osName | 硬式編碼 | Android |
| osVersion | Build.VERSION.RELEASE | 5.0.1 |

連線資訊的建構方式如下：

| 索引鍵 | Source | 值（範例） |
|------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------|
| connectionType | `<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>` `getSystemService(Context.CONNECTIVITY_SERVICE).getActiveNetworkInfo().getType()` | `"WIFI","BLUETOOTH","MOBILE","ETHERNET","VPN","DUMMY","MOBILE_DUN","WIMAX","notAccessible"` |
| connectionSecure |                                                                                                                                                               |                                                                                             |

應用程式資訊的建構方式如下：

| 索引鍵 | Source | 值（範例） |
|---------------|-----------|-----------------|
| applicationId | 硬式編碼 | REF30 |

### 電視連線裝置 {#tv-connected-devices}

#### tvOS {#tvos}

若要為執行`X-Device-Info`tvOS[的裝置建置](https://developer.apple.com/documentation/tvos-release-notes)標題，您可以參考下列檔案及下列程式碼片段：

* [UIDevice](https://developer.apple.com/documentation/uikit/uidevice#//apple_ref/occ/cl/UIDevice)的Apple開發人員檔案。
* [可存取性](https://developer.apple.com/library/archive/samplecode/Reachability/Introduction/Intro.html)的Apple開發人員檔案。
* [uname](https://man7.org/linux/man-pages/man2/uname.2.html)的Linux手動檔案。

```C
+ (NSString *)computeClientInformation {        
        struct utsname u;
        uname(&u);

        NSString *hardware = [NSString stringWithCString:u.machine encoding:NSUTF8StringEncoding];

        UIDevice *device = [UIDevice currentDevice];

        NSString *deviceType;

        switch (UI_USER_INTERFACE_IDIOM()) {
            case UIUserInterfaceIdiomPhone:
                deviceType = @"MobilePhone";
                break;
            case UIUserInterfaceIdiomPad:
                deviceType = @"Tablet";
                break;
            case UIUserInterfaceIdiomTV:
                deviceType = @"TV";
                break;
            default:
                deviceType = @"Unknown";
        }

        CGRect screenRect = [[UIScreen mainScreen] bounds];
        NSNumber *screenWidth = @((float) screenRect.size.width);
        NSNumber *screenHeight = @((float) screenRect.size.height);

        Reachability *reachability = [Reachability reachabilityForInternetConnection];
        [reachability startNotifier];

        NetworkStatus status = [reachability currentReachabilityStatus];

        NSString *connectionType;

        if (status == NotReachable) {
            connectionType = @"notConnected";
        } else if (status == ReachableViaWiFi) {
            connectionType = @"WiFi";
        } else if (status == ReachableViaWWAN) {
            connectionType = @"cellular";
        }

        NSMutableDictionary *clientInformation = [@{
                @"type": deviceType,
                @"model": device.model,
                @"vendor": @"Apple",
                @"manufacturer": @"Apple",
                @"version": [hardware stringByReplacingOccurrencesOfString:device.model withString:@""],
                @"osName": device.systemName,
                @"osVersion": device.systemVersion,
                @"displayWidth": screenWidth,
                @"displayHeight": screenHeight,
                @"connectionType": connectionType,
                @"applicationId": @"REF30" 
        } mutableCopy];

        NSError *error;
        NSData *jsonData = [NSJSONSerialization dataWithJSONObject:clientInformation options:NSJSONWritingPrettyPrinted error:&error];
        NSString *base64Encoded = [jsonData base64EncodedStringWithOptions:0];

        return base64Encoded;
}
```

裝置資訊的建構方式如下：

| 索引鍵 | Source | 值（範例） |
|---------------|------------------------|-----------------|
| 模型 | uname.machine | AppleTV |
| 廠商 | 硬式編碼 | Apple |
| 製造商 | 硬式編碼 | Apple |
| 版本 | uname.machine | 8,1 |
| displaywidth | UIScreen.mainScreen | 1920 |
| displayheight | UIScreen.mainScreen | 1080 |
| osName | UIDevice.systemName | tvOS |
| osVersion | UIDevice.systemVersion | 10.2 |

連線資訊的建構方式如下：

| 索引鍵 | Source | 值（範例） |
|------------------|------------------------------------------|-----------------|
| connectionType | [連線能力currentReachableStatus] |                 |
| connectionSecure |                                          |                 |

應用程式資訊的建構方式如下：

| 索引鍵 | Source | 值（範例） |
|---------------|-----------|-----------------|
| applicationId | 硬式編碼 | REF30 |

#### Fire OS {#fireos}

若要為執行`X-Device-Info`Fire OS[的裝置建置](https://developer.amazon.com/docs/fire-tv/fire-os-overview.html)標頭，您可以參考下列檔案：

* [Build](https://developer.android.com/reference/android/os/Build.html)類別的Android開發人員檔案。
* [識別Fire TV裝置](https://developer.amazon.com/docs/fire-tv/identify-amazon-fire-tv-devices.html)的Amazon開發人員檔案。

裝置資訊的建構方式如下：

| 索引鍵 | Source | 值（範例） |
|---------------|-----------------------------|-----------------|
| 模型 | Build.MODEL | AFTM |
| 廠商 | Build.BRAND | Amazon |
| 製造商 | Build.MANUFACTURER | Amazon |
| 版本 | Build.DEVICE | montoya |
| displaywidth | DisplayMetrics.widthPixels |                 |
| displayheight | DisplayMetrics.heightPixels |                 |
| osName | 硬式編碼 | Android |
| osVersion | Build.VERSION.RELEASE | 5.1.1 |

連線資訊的建構方式如下：

| 索引鍵 | Source | 值（範例） |
|------------------|--------|-----------------|
| connectionType |        |                 |
| connectionSecure |        |                 |

應用程式資訊的建構方式如下：

| 索引鍵 | Source | 值（範例） |
|---------------|-----------|-----------------|
| applicationId | 硬式編碼 | REF30 |

#### Roku OS {#rokuos}

若要為執行`X-Device-Info`Roku OS[的裝置建置](https://developer.roku.com/docs/developer-program/release-notes/roku-os-release-notes.md)標題，您可以參考下列檔案：

* [ifDeviceInfo](https://developer.roku.com/docs/references/brightscript/interfaces/ifdeviceinfo.md)的Roku開發人員檔案。

裝置資訊的建構方式如下：

| 索引鍵 | Source | 值（範例） |
|---------------|--------------------------------------------|-----------------|
| 模型 | 硬式編碼 | &quot;Roku&quot; |
| 廠商 | ifDeviceInfo.GetModelDetails().VendorName | &quot;Sharp&quot;、&quot;Roku&quot; |
| 製造商 | ifDeviceInfo.GetModelDetails().VendorName | &quot;Sharp&quot;、&quot;Roku&quot; |
| 版本 | ifDeviceInfo.GetModelDetails().ModelNumber | 「5303X」 |
| displaywidth | ifDeviceInfo.GetDisplaySize().w | 1920 |
| displayheight | ifDeviceInfo.GetDisplaySize().h | 1080 |
| osName | 硬式編碼 | &quot;Roku&quot; |
| osVersion | ifDeviceInfo.getVersion() |                 |

連線資訊的建構方式如下：

| 索引鍵 | Source | 值（範例） |
|-------------------|------------------------------------|---------------------------------------|
| connectionType | ifDeviceInfo.GetConnectionType() | 「WifiConnection」、「WiredConnection」 |
| connectionSecure | 硬式編碼 | 如果連線有線，則為true |

應用程式資訊的建構方式如下：

| 索引鍵 | Source | 值（範例） |
|---------------|-----------|-----------------|
| applicationId | 硬式編碼 | REF30 |

### 其他 {#others}

對於檔案中未涵蓋的裝置平台，使用者端資訊（裝置、連線和應用程式）應連結至任何可用的硬體和作業系統(OS)屬性，通常在裝置的硬體和作業系統手冊中指定。
