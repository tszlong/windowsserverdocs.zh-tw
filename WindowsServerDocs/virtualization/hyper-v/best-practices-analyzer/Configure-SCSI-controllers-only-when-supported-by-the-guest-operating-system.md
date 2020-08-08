---
title: 只有在客體作業系統支援時，才設定 SCSI 控制器
description: 此最佳做法分析程式規則的線上版本文字。
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: 861f194f-467e-4b07-a1c5-55b35f6327c4
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 456035731372aa7cd152efea329faee32d1cde7c
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87960661"
---
# <a name="configure-scsi-controllers-only-when-supported-by-the-guest-operating-system"></a>只有在客體作業系統支援時，才設定 SCSI 控制器

>適用於：Windows Server 2016



|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|警告|
|**類別**|組態|

在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。

## <a name="issue"></a>問題

*虛擬機器設定了無法使用的 SCSI 控制器，因為客體作業系統不支援 SCSI 控制器。*

## <a name="impact"></a>影響

*虛擬機器無法使用連接到 SCSI 控制器的儲存體。這會影響下列虛擬機器：*

\<list of virtual machines>

## <a name="resolution"></a>解決方法

*關閉虛擬機器，並使用 Hyper-v 管理員從虛擬機器移除 SCSI 控制器。然後，重新開機虛擬機器。*



