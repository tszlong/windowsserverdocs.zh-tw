---
title: 針對儲存虛擬機器檔案的檔案共用，至少使用 SMB 通訊協定3.0 版。
description: 瞭解當虛擬機器檔案或虛擬硬碟檔案儲存在不支援 SMB 通訊協定3.0 版的檔案共用時，該怎麼辦。
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 4bb832b8-f1aa-4c1f-a0f2-324dd53553ea
ms.date: 8/16/2016
ms.openlocfilehash: 783ba7b2e8b5dc83eb575ee395d85e58701c0b89
ms.sourcegitcommit: 42581433c0bb62e291d412ee9e13869b42e69a4b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/01/2021
ms.locfileid: "97846217"
---
# <a name="use-at-least-smb-protocol-version-30-for-file-shares-that-store-files-for-virtual-machines"></a>針對儲存虛擬機器檔案的檔案共用，至少使用 SMB 通訊協定3.0 版。

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。

|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|錯誤|
|**類別**|組態|

在下列各節中，斜體指出出現在此問題的最佳做法分析程式工具中的 UI 文字。

## <a name="issue"></a>**問題**
*虛擬機器檔案或虛擬硬碟檔案會儲存在不支援至少 SMB 通訊協定3.0 版的檔案共用上。*

## <a name="impact"></a>**影響**
*Microsoft 不支援此設定。這會影響下列虛擬機器：*

\<list of virtual machines>

## <a name="resolution"></a>**解決方法**
*將檔案移至至少使用 SMB 通訊協定3.0 版的檔案共用。*



