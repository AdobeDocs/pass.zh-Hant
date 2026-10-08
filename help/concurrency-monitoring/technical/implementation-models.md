---
title: 實作模型
description: 實作模型
exl-id: 3bcb63ba-9b4a-4df4-8d24-e520b8830a10
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '63'
ht-degree: 0%
---
# 實作模型 {#imp-models}

## 伺服器端原則 {#ss-policies}

此模型將使用CM作為原則決定點，從而將存取決定委派給服務。

由於使用者端不應就套用的原則做出任何假設，因此實施作業需要在心率回應播放期間定期檢查工作階段初始化決策。
