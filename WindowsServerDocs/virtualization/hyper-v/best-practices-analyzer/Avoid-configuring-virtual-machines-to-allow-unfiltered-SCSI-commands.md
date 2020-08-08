---
title: 避免將虛擬機器設定為允許未篩選的 SCSI 命令
description: 此最佳做法分析程式規則的線上版本文字。
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: dd4a3d78-a77f-451e-a383-d5cf45ea17cf
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 33c211073c74da334abb8b483fa36974216a1185
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87948581"
---
# <a name="avoid-configuring-virtual-machines-to-allow-unfiltered-scsi-commands"></a>避免將虛擬機器設定為允許未篩選的 SCSI 命令

>適用於：Windows Server 2016



*如需最佳做法和掃描的詳細資訊，請參閱*[最佳做法分析](https://go.microsoft.com/fwlink/?LinkId=122786)程式。

|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|警告|
|**類別**|作業|

在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。

## <a name="issue"></a>問題

*虛擬機器已設定為允許未篩選的 SCSI 命令。*

## <a name="impact"></a>影響

*略過 SCSI 命令篩選會造成安全性風險。只有在與客體作業系統中執行的存放裝置應用程式相容時，才需要啟用此設定。下列虛擬機器已設定為允許未篩選的 SCSI 命令：*

\<list of virtual machine names>

## <a name="resolution"></a>解決方法

*請洽詢您的存放裝置廠商，以判斷是否需要此設定。此外，如果管理作業系統或其他客體作業系統遭到入侵，或呈現不尋常的行為，請重新設定虛擬機器以封鎖命令。*



