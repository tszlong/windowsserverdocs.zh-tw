---
title: 在儲存虛擬機器檔案的檔案共用上，至少使用 SMB 通訊協定3.0 版設定持續可用性
description: 此最佳做法分析程式規則之文字的線上版本。
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: a1fa5cf9-8a48-4f63-bb57-d81e63e77b30
ms.date: 8/16/2016
ms.openlocfilehash: 9e913ac96075d7ad15d4e50872e52aa3c863ac5a
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/17/2020
ms.locfileid: "90746793"
---
# <a name="use-at-least-smb-protocol-version-30-configured-for-continuous-availability-on-file-shares-that-store-files-for-virtual-machines"></a>在儲存虛擬機器檔案的檔案共用上，至少使用 SMB 通訊協定3.0 版設定持續可用性

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。

|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|警告|
|**類別**|設定|

在下列各節中，斜體指出出現在此問題的最佳做法分析程式工具中的 UI 文字。

## <a name="issue"></a>**問題**
*虛擬機器檔案或虛擬硬碟檔案會儲存在未以 SMB 通訊協定版本3.0 的持續可用性功能設定的網路檔案共用上。*

## <a name="impact"></a>**影響**
*Microsoft 不建議此設定，因為它可能會影響使用伺服器的虛擬機器可用性。這會影響下列虛擬機器：*

\<list of virtual machines>

## <a name="resolution"></a>**解決方法**
執行下列其中一個動作：

-   將檔案移至設定為持續可用性的 SMB 3.0 檔案共用。

-   重新設定目前的檔案共用，以提供持續的可用性。



