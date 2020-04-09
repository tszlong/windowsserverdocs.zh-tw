---
title: 避免在設定為 Active Directory 網域控制站的電腦上安裝 RemoteFX
description: 此最佳做法分析程式規則的線上版本文字。
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: da58694e-91f6-45d8-a599-18966db165f4
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 597dd26d0a317ca026cd94ab29138ce679d7c3b9
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857761"
---
# <a name="avoid-installing-remotefx-on-a-computer-that-is-configured-as-an-active-directory-domain-controller"></a>避免在設定為 Active Directory 網域控制站的電腦上安裝 RemoteFX

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
*RemoteFX 會安裝在網域控制站上。*  
  
## <a name="impact"></a>**產生**  
*針對 RemoteFX 設定的虛擬電腦無法在這些電腦上使用。*  
  
## <a name="resolution"></a>**解決方法**  
*決定您是否要使用適用于 Hyper-v 的 RemoteFX 或 Active Directory 網網域控制站來設定此伺服器，然後視需要重新設定伺服器。*  
  


