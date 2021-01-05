---
title: 只有當客體作業系統支援時，才設定 SCSI 控制器
description: 瞭解當虛擬機器以無法使用的 SCSI 控制器設定的虛擬機器，因為客體作業系統不支援 SCSI 控制器。
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 861f194f-467e-4b07-a1c5-55b35f6327c4
ms.date: 8/16/2016
ms.openlocfilehash: 3661eb1010dd163d33cb518a16174ed4133b0d98
ms.sourcegitcommit: 42581433c0bb62e291d412ee9e13869b42e69a4b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/01/2021
ms.locfileid: "97846095"
---
# <a name="configure-scsi-controllers-only-when-supported-by-the-guest-operating-system"></a>只有當客體作業系統支援時，才設定 SCSI 控制器

>適用於：Windows Server 2016



|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|警告|
|**類別**|組態|

在下列各節中，斜體指出出現在此問題的最佳做法分析程式工具中的 UI 文字。

## <a name="issue"></a>問題

*虛擬機器設定為具有無法使用的 SCSI 控制器，因為客體作業系統不支援 SCSI 控制器。*

## <a name="impact"></a>影響

*虛擬機器無法使用連接至 SCSI 控制器的儲存體。這會影響下列虛擬機器：*

\<list of virtual machines>

## <a name="resolution"></a>解決方案

*關閉虛擬機器，並使用 Hyper-v 管理員從虛擬機器移除 SCSI 控制器。然後，重新開機虛擬機器。*



