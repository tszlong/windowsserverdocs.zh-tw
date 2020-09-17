---
title: 只有當客體作業系統支援時，才設定 SCSI 控制器
description: 此最佳做法分析程式規則之文字的線上版本。
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 861f194f-467e-4b07-a1c5-55b35f6327c4
ms.date: 8/16/2016
ms.openlocfilehash: cadf4c0d0ce8cdd50d66a61f428b3e547dd457dc
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/17/2020
ms.locfileid: "90746993"
---
# <a name="configure-scsi-controllers-only-when-supported-by-the-guest-operating-system"></a>只有當客體作業系統支援時，才設定 SCSI 控制器

>適用於：Windows Server 2016



|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|警告|
|**類別**|設定|

在下列各節中，斜體指出出現在此問題的最佳做法分析程式工具中的 UI 文字。

## <a name="issue"></a>問題

*虛擬機器設定為具有無法使用的 SCSI 控制器，因為客體作業系統不支援 SCSI 控制器。*

## <a name="impact"></a>影響

*虛擬機器無法使用連接至 SCSI 控制器的儲存體。這會影響下列虛擬機器：*

\<list of virtual machines>

## <a name="resolution"></a>解決方案

*關閉虛擬機器，並使用 Hyper-v 管理員從虛擬機器移除 SCSI 控制器。然後，重新開機虛擬機器。*



