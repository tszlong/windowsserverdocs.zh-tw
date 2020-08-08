---
title: 建議以憑證為基礎的驗證進行複寫
description: 此最佳做法分析程式規則的線上版本文字。
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: d931cc57-414f-4bdf-9ebd-08fd5e22b19d
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: ec5415d0bdad10c4b0f4f0560141dd290eca8e36
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87954495"
---
# <a name="certificate-based-authentication-is-recommended-for-replication"></a>建議以憑證為基礎的驗證進行複寫

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。

|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|警告|
|**類別**|組態|

在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。

## <a name="issue"></a>**問題**
*為複寫選取的一或多部虛擬機器已設定 Kerberos 驗證。*

## <a name="impact"></a>**影響**
*從主伺服器傳送到複寫伺服器的複寫網路流量未加密。這會影響下列虛擬機器：*

\<list of virtual machines>

## <a name="resolution"></a>**解決方案**
*如果正在使用另一個方法來執行加密，您可以忽略此情況。否則，請修改虛擬機器設定，以選擇以憑證為基礎的驗證。*



