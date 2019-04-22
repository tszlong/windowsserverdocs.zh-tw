---
title: 避免將一個儲存體路徑對應至多個資源集區
description: 此最佳做法分析程式規則之文字的線上版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 24992453-762b-4892-9a50-55d237b9b7f2
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 7c012836309f722e55c28b2ddbe3d54de641b4af
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823959"
---
# <a name="avoid-mapping-one-storage-path-to-multiple-resource-pools"></a>避免將一個儲存體路徑對應至多個資源集區

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**Severity**|警告|  
|**分類**|操作|  
  
在下列章節中，斜體表示會出現在此問題的最佳做法分析程式工具的 UI 文字。
  
## <a name="issue"></a>**問題**  
*儲存體檔案路徑會對應至多個資源集區。*  
  
## <a name="impact"></a>**影響**  
*針對指定的儲存體集區類型，下列的父系和子系集區會共用相同的儲存體路徑：*  
  
\<集區的清單 >  
  
## <a name="resolution"></a>**解決方法**  
*使用 Windows PowerShell 來重新設定存放裝置資源集區，好讓多個集區不會使用相同的儲存體路徑。*  
  


