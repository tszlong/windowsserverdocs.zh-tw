---
title: 至少使用 SMB 通訊協定設定為儲存虛擬機器檔案的檔案共用上的持續可用性的 3.0 版
description: 此最佳做法分析程式規則之文字的線上版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: a1fa5cf9-8a48-4f63-bb57-d81e63e77b30
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 67f41293433bd8d14096688fbaa23eb43334c738
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877869"
---
# <a name="use-at-least-smb-protocol-version-30-configured-for-continuous-availability-on-file-shares-that-store-files-for-virtual-machines"></a>至少使用 SMB 通訊協定設定為儲存虛擬機器檔案的檔案共用上的持續可用性的 3.0 版

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**Severity**|警告|  
|**分類**|組態|  
  
在下列章節中，斜體表示會出現在此問題的最佳做法分析程式工具的 UI 文字。  
  
## <a name="issue"></a>**問題**  
*虛擬機器的檔案或虛擬硬碟檔案會儲存在未設定使用 SMB 通訊協定 3.0 版的持續可用性功能的網路檔案共用中。*  
  
## <a name="impact"></a>**影響**  
*Microsoft 不建議此設定，因為它可能會影響使用伺服器的虛擬機器的可用性。這會影響下列虛擬機器：*  
  
\<虛擬機器清單 >  
  
## <a name="resolution"></a>**解決方法**  
執行下列其中一項：  
  
-   將檔案移至已針對連續可用性的 SMB 3.0 檔案共用。  
  
-   重新設定目前的檔案共用，以提供持續可用性。  
  


