---
title: 建議執行 Hyper-v 的伺服器使用網域成員資格
description: 瞭解當伺服器是工作組的成員時該怎麼辦。
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 2f4578e5-0848-46b4-a50b-7dbd480b80bf
ms.date: 8/16/2016
ms.openlocfilehash: e6e17da7f23eed04800a4c839ce52791f6221f9c
ms.sourcegitcommit: 42581433c0bb62e291d412ee9e13869b42e69a4b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/01/2021
ms.locfileid: "97845977"
---
# <a name="domain-membership-is-recommended-for-servers-running-hyper-v"></a>建議執行 Hyper-v 的伺服器使用網域成員資格

>適用於：Windows Server 2016



*如需最佳做法和掃描的詳細資訊，請參閱*[最佳做法分析](https://go.microsoft.com/fwlink/?LinkId=122786)程式。

|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|警告|
|**類別**|組態|

在下列各節中，斜體指出出現在此問題的最佳做法分析程式工具中的 UI 文字。

## <a name="issue"></a>問題

*此伺服器為工作組的成員。*

## <a name="impact"></a>影響

*此伺服器沒有集中管理。*

將此電腦加入網域可透過原則進行集中式管理，以進行身分識別、安全性和審核。

## <a name="resolution"></a>解決方案

*如果您有可用的網域環境，請將此伺服器加入該網域。*

> [!IMPORTANT]
> 建議您複習這部電腦上的虛擬機器中執行的工作負載，以判斷將這部電腦加入網域是否有安全性含意。 如果有任何虛擬機器是虛擬網域控制站，請參閱 [虛擬網域控制站 (的規劃考慮](https://go.microsoft.com/fwlink/?LinkId=190192) https://go.microsoft.com/fwlink/?LinkId=190192) 。

將電腦加入網域需要電腦和網域的許可權：
- 在電腦上，您必須是 Administrators 群組成員的使用者帳戶。 請使用這種類型的帳戶登入，或在系統提示時提供帳戶的使用者名稱和密碼。
- 在網域上，您需要有權將電腦加入網域的使用者帳戶。 系統會提示您輸入使用者名稱和密碼。

如需相關指示，請參閱將 [電腦加入網域](https://go.microsoft.com/fwlink/?LinkId=190193) (https://go.microsoft.com/fwlink/?LinkId=190193) 。



