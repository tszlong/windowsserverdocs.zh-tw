---
title: 避免將虛擬機器設定為允許未篩選的 SCSI 命令
description: 此最佳做法分析程式規則之文字的線上版本。
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: dd4a3d78-a77f-451e-a383-d5cf45ea17cf
ms.date: 8/16/2016
ms.openlocfilehash: 95d7fa223371aa3dbc3b66efcfefed217eed5216
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/17/2020
ms.locfileid: "90747083"
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

在下列各節中，斜體指出出現在此問題的最佳做法分析程式工具中的 UI 文字。

## <a name="issue"></a>問題

*虛擬機器設定為允許未篩選的 SCSI 命令。*

## <a name="impact"></a>影響

*略過 SCSI 命令篩選會造成安全性風險。只有在與客體作業系統中執行的存放裝置應用程式相容時，才需要啟用此設定。下列虛擬機器設定為允許未篩選的 SCSI 命令：*

\<list of virtual machine names>

## <a name="resolution"></a>解決方案

*請洽詢您的存放裝置廠商，以判斷是否需要此設定。此外，如果管理作業系統或其他客體作業系統遭到入侵或出現不尋常的行為，請重新設定虛擬機器以封鎖命令。*



