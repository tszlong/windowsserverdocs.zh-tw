---
title: 避免在已設定為 Active Directory 網域控制站的電腦上安裝 RemoteFX
description: 此最佳做法分析程式規則之文字的線上版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: da58694e-91f6-45d8-a599-18966db165f4
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 9e1c642e3f36b5fe25f34bb417a83b8510adcc02
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832559"
---
# <a name="avoid-installing-remotefx-on-a-computer-that-is-configured-as-an-active-directory-domain-controller"></a>避免在已設定為 Active Directory 網域控制站的電腦上安裝 RemoteFX

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**Severity**|錯誤|  
|**分類**|組態|  
  
在下列章節中，斜體表示會出現在此問題的最佳做法分析程式工具的 UI 文字。  
  
## <a name="issue"></a>**問題**  
*RemoteFX 被安裝在網域控制站。*  
  
## <a name="impact"></a>**影響**  
*設定 remotefx 的虛擬電腦不能在這些電腦上。*  
  
## <a name="resolution"></a>**解決方法**  
*決定是否要讓此伺服器設定使用 RemoteFX 的 HYPER-V 或 Active Directory 網域控制站，然後將伺服器設定為必要。*  
  


