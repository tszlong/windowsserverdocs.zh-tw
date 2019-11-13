---
title: 在儲存虛擬機器檔案的檔案共用上，至少使用 SMB 通訊協定版本3.0 設定為持續可用性
description: 此最佳做法分析程式規則的線上版本文字。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: a1fa5cf9-8a48-4f63-bb57-d81e63e77b30
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 3a6cbb6052e2e50b7fd78792c5e01885d7672932
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393335"
---
# <a name="use-at-least-smb-protocol-version-30-configured-for-continuous-availability-on-file-shares-that-store-files-for-virtual-machines"></a>在儲存虛擬機器檔案的檔案共用上，至少使用 SMB 通訊協定版本3.0 設定為持續可用性

>適用於︰Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**低於**|警告|  
|**類別**|設定|  
  
在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。  
  
## <a name="issue"></a>**問題**  
*虛擬機器檔案或虛擬硬碟檔案儲存在網路檔案共用上，但未使用 SMB 通訊協定3.0 版的持續可用性功能進行設定。*  
  
## <a name="impact"></a>**產生**  
*Microsoft 不建議此設定，因為它可能會影響使用伺服器之虛擬機器的可用性。這會影響下列虛擬機器：*  
  
\<的虛擬機器清單 >  
  
## <a name="resolution"></a>**解決方法**  
執行下列其中一項：  
  
-   將檔案移至設定為持續可用性的 SMB 3.0 檔案共用。  
  
-   重新設定目前的檔案共用，以提供持續的可用性。  
  


