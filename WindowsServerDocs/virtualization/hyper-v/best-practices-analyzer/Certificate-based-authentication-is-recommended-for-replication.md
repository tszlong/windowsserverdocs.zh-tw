---
title: 建議針對複寫使用以憑證為基礎的驗證
description: 瞭解當選取要進行複寫的一或多部虛擬機器設定為 Kerberos 驗證時，該怎麼辦。
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: d931cc57-414f-4bdf-9ebd-08fd5e22b19d
ms.date: 8/16/2016
ms.openlocfilehash: 83d4b90670678660da0610d800eaf1fbe87a863e
ms.sourcegitcommit: 48d45b2adf44afb0207214be9c57fe589360d177
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/31/2020
ms.locfileid: "97833553"
---
# <a name="certificate-based-authentication-is-recommended-for-replication"></a>建議針對複寫使用以憑證為基礎的驗證

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。

|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|警告|
|**類別**|組態|

在下列各節中，斜體指出出現在此問題的最佳做法分析程式工具中的 UI 文字。

## <a name="issue"></a>**問題**
*針對複寫選取的一或多部虛擬機器，會設定為 Kerberos 驗證。*

## <a name="impact"></a>**影響**
*從主伺服器到複寫伺服器的複寫網路流量未加密。這會影響下列虛擬機器：*

\<list of virtual machines>

## <a name="resolution"></a>**解決方法**
*如果另一種方法是用來執行加密，您可以忽略這項功能。否則，請修改虛擬機器設定，以選擇以憑證為基礎的驗證。*



