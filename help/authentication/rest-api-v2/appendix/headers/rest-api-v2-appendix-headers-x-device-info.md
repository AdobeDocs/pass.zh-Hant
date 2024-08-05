---
title: 頁首 — X-Device-Info
description: REST API V2 — 標題 — X-Device-Info
source-git-commit: c3aa2a24b242669ce0818b95ec34de2adec8001b
workflow-type: tm+mt
source-wordcount: '402'
ht-degree: 0%

---


# 頁首 — X-Device-Info {#header-x-device-info}

## 概觀 {#overview}

<b>X-Device-Info</b>要求標頭包含與實際串流裝置相關的使用者端資訊（裝置、連線和應用程式）。

## 語法 {#syntax}

<table>
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

<table>
    <tr>
        <th style="background-color: #EFF2F7; width: 15%;">索引鍵</th>
        <th style="background-color: #EFF2F7;">說明</th>    
        <th style="background-color: #EFF2F7; width: 15%;">是否存在</th>
        <th style="background-color: #EFF2F7;">可能的值</th>
    </tr>
    <tr>
        <td>primaryHardwaretype</td>
        <td>裝置的主要硬體型別。</td>
        <td></td>
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
        <td>模型</td>
        <td>裝置的型號名稱。</td>
        <td><i>必填</i></td>
        <td>例如iPhone、SM-G930V、AppleTV等。</td>
    </tr>
    <tr>
        <td>版本</td>
        <td>裝置的版本。</td>
        <td></td>
        <td>例如2.0.1等。</td>
    </tr>
    <tr>
        <td>製造商</td>
        <td>裝置的製造公司/組織。</td>
        <td></td>
        <td>例如三星、LG、ZTE、華為、摩托羅拉、Apple等。</td>
    </tr>
    <tr>
        <td>廠商</td>
        <td>裝置的銷售公司/組織。</td>
        <td></td>
        <td>例如Apple、Samsung、LG、Google等。</td>
    </tr>
    <tr>
        <td>osName</td>
        <td>裝置的作業系統(OS)名稱。</td>
        <td><i>必填</i></td>
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
        <td>osFamily</td>
        <td>裝置的作業系統(OS)群組名稱。</td>
        <td></td>
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
        <td>osVendor</td>
        <td>裝置的作業系統(OS)供應商。</td>
        <td></td>
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
        <td>osVersion</td>
        <td>裝置的作業系統(OS)版本。</td>
        <td></td>
        <td>例如10.2、9.0.1等。</td>
    </tr>
    <tr>
        <td>browserName</td>
        <td>瀏覽器的名稱。</td>
        <td></td>
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
        <td>browserVendor</td>
        <td>瀏覽器的建置公司/組織。</td>
        <td></td>
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
        <td>browserVersion</td>
        <td>裝置的瀏覽器版本。</td>
        <td></td>
        <td>例如60.0.3112</td>
    </tr>
    <tr>
        <td>userAgent</td>
        <td>裝置的使用者代理。</td>
        <td></td>
        <td>例如Mozilla/5.0 (Macintosh；Intel Mac OS X 10_12_3) AppleWebKit/602.4.8 （KHTML，如Gecko）版本/10.0.3 Safari/602.4.8</td>
    </tr>
    <tr>
        <td>displaywidth</td>
        <td>裝置的實體熒幕寬度。</td>
        <td></td>
        <td></td>
    </tr>
    <tr>
        <td>displayheight</td>
        <td>裝置的實體熒幕高度。</td>
        <td></td>
        <td></td>
    </tr>
    <tr>
        <td>displayPpi</td>
        <td>裝置的實體熒幕畫素密度。</td>
        <td></td>
        <td>例如294</td>
    </tr>
    <tr>
        <td>diagonalScreenSize</td>
        <td>裝置的實體熒幕對角線尺寸（英吋）。</td>
        <td></td>
        <td>例如5.5、10.1</td>
    </tr>
    <tr>
        <td>connectionIp</td>
        <td>用於傳送HTTP要求的裝置IP。</td>
        <td></td>
        <td>例如8.8.4.4</td>
    </tr>
    <tr>
        <td>connectionPort</td>
        <td>用於傳送HTTP要求的裝置連線埠。</td>
        <td></td>
        <td>例如53124</td>
    </tr>
    <tr>
        <td>connectionType</td>
        <td>網路連線型別。</td>
        <td></td>
        <td>例如WiFi、LAN、3G、4G、5G</td>
    </tr>
    <tr>
        <td>connectionSecure</td>
        <td>網路連線安全性狀態。</td>
        <td></td>
        <td>
            值受到限制：
            <ul>
                <li>true — 在安全網路的情況下</li>
                <li>false — 在公開熱點的情況下</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>applicationId</td>
        <td>應用程式的唯一識別碼。</td>
        <td></td>
        <td>例如CNN</td>
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
