---
title: iOS/tvOS儲存完整性檢查機制
description: iOS/tvOS完整性檢查機制
exl-id: 5d7cdc46-3e51-4e14-9e30-d7f48bc87506
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '323'
ht-degree: 0%

---

# iOS/tvOS完整性檢查機制 {#iostvos-sdk-storage-integrity-checks}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

## 簡介 {#Intro}

從iOS/tvOS AccessEnabler SDK 3.8.3版開始，AccessEnabler初始化時提供執行儲存整合檢查的選項。

為了使用此機制，API已擴充為AccessEnabler類別的額外初始化方法。

```
- (nonnull id) initWithStorageCheck:(IntegrityCheckType)performIntegrityCheck softwareStatement:(nonnull NSString *)softwareStatement;
```


## 完整性檢查 {#Checks}

當懷疑AccessEnabler儲存體損毀（例如在讀取/寫入儲存作業期間發生競爭狀況）時，儲存體完整性檢查很有用。

下列檢查可於AccessEnabler初始化時執行：
- 儲存可操作性：檢查讀取和寫入作業是否成功
- 已儲存值的完整性：檢查所有值是否有效且使用預期的格式

>[!IMPORTANT]
> 
>如果其中一項檢查失敗，儲存空間中的所有值都會被清除並且使用者會登出，這可能會導致不良的使用者體驗。 只有在認為必要時才使用儲存完整性檢查。


## 預設行為 {#Default}

使用預設初始化方法初始化AccessEnabler時，儲存體完整性檢查預設為關閉：

```
///  SWIFT
let accessEnabler: AccessEnabler = AccessEnaler(softwareStatement)

///  Objective C
AccessEnabler *accessEnabler = [[AccessEnabler alloc] init:softwareStatement];
```

若要明確指定在AccessEnabler初始化時要執行哪些儲存體完整性檢查，請使用下列初始化方法：

```
///  SWIFT
let accessEnabler: AccessEnabler = AccessEnabler(storageCheck: IntegrityCheckType.INTEGRITY_CHECK_ALL, softwareStatement: softwareStatement)

///  Objective C
AccessEnabler *accessEnabler = [[AccessEnabler alloc] initWithStorageCheck:INTEGRITY_CHECK_ALL softwareStatement:softwareStatement];
```


## IntegrityCheckType {#Switcher}

IntegrityCheckType列舉會公開給使用者端應用程式，並具有下列值：

| 值 | 已執行的檢查 | 儲存空間已清除 | 說明 | 建議的使用案例 |
|-----------------------|-----------------------------------------------------|-----------------|------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------|
| INTEGRITY_CHECK_NONE | 無 | 從不 | 儲存初始化時不會執行完整性檢查 | 當SDK流程如期運作時 |
| INTEGRITY_CHECK_ALL | 儲存操作性<br/>儲存值的有效性 | 檢查時失敗 | 所有可用的完整性檢查都會在儲存初始化時執行 | 懷疑SDK儲存損毀時。 <br/>如果任何完整性檢查失敗，使用者將會登出 |
| INTEGRITY_CHECK_CLEAR | 無 | 一直 | 儲存體初始化時會清除儲存體 | 當SDK流程無法如預期完成時 |
