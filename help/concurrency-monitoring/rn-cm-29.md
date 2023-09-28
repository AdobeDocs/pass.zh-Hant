---
title: Adobe並行監控2.9發行說明
description: Adobe並行監控2.9發行說明
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '153'
ht-degree: 0%

---


# Concurrency Monitoring 2.9發行說明 {#rn-cm29}

本頁說明此版本的新功能、變更和已知問題。

## 發行日期 {#release-date}

03/14/2019


## 版本總覽 {#release-overview}

* 自此版本開始，我們引進了新的報告以瞭解並行使用情況：並行層級的長條圖，顯示：

* 在每個詳細程度間隔內，達到每個並行層級的使用者人數（即有多少使用者曾經有2個並行資料流、3個並行資料流等）
* 每個並行層級的總持續時間（以分鐘為單位，平均值可以只用這個值除以上述計數來計算）
* 使用者達到每個並行層級的總次數，用來評估指定規則的影響，包括受影響的使用者和彙總的使用者體驗。詳細資訊請參閱 [使用情況報表](/help/concurrency-monitoring/cm-usage-reports.md) 頁面。

我們也改善了SQL插入保護，並新增了數個錯誤修正。

## 已知問題 {#known-issues}

無。
