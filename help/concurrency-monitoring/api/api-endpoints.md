---
title: api端點
description: 並行監視API的完整清單
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
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
