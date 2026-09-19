---
title: api端點
description: 並行監視API的完整清單
exl-id: e8a9dfd2-cd16-4971-b9bc-9646987dd3ce
source-git-commit: 39384d753e7808fa433f30d8dafabd531dbf3acf
workflow-type: tm+mt
source-wordcount: '54'
ht-degree: 3%
---
# api端點

## 核心工作階段管理

| 端點 | 方法 | 說明 |
|---------------------------------------|--------|---------------------------------------|
| `/sessions/{idp}/{subject}` | POST | 建立新的串流工作階段 |
| `/sessions/{idp}/{subject}/{session}` | POST | 傳送心率以保持工作階段作用中 |
| `/sessions/{idp}/{subject}/{session}` | DELETE | 終止工作階段 |
| `/runningStreams/{idp}/{subject}` | GET | 取得主題的所有作用中工作階段 |

## 中繼資料管理

| 端點 | 方法 | 說明 |
|-------------|--------|----------------------------------------------|
| `/metadata` | GET | 取得應用程式所需的中繼資料欄位 |
