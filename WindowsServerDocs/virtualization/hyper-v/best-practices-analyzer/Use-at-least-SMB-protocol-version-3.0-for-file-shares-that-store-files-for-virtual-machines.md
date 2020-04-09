---
title: 針對儲存虛擬機器檔案的檔案共用，請至少使用 SMB 通訊協定版本3.0。
description: 此最佳做法分析程式規則的線上版本文字。
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 4bb832b8-f1aa-4c1f-a0f2-324dd53553ea
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 64c78c4725c84cbd02276cb2c0eadffae42d7060
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854191"
---
# <a name="use-at-least-smb-protocol-version-30-for-file-shares-that-store-files-for-virtual-machines"></a>針對儲存虛擬機器檔案的檔案共用，請至少使用 SMB 通訊協定版本3.0。

>適用於︰Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**低於**|錯誤|  
|**類別**|組態|  
  
在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。  
  
## <a name="issue"></a>**問題**  
*虛擬機器檔案或虛擬硬碟檔案儲存在不支援 SMB 通訊協定3.0 版的檔案共用上。*  
  
## <a name="impact"></a>**產生**  
*Microsoft 不支援這種設定。這會影響下列虛擬機器：*  
  
\<的虛擬機器清單 >  
  
## <a name="resolution"></a>**解決方法**  
*將檔案移至至少使用 SMB 通訊協定3.0 版的檔案共用。*  
  


