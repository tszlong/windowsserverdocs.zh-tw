---
ms.assetid: 1b21b0a9-1fe6-4fd1-8a25-92e578d774ed
title: 部署同盟伺服器 Proxy
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: b914141a0445febd3961b688aadc2f444b2eee7b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816259"
---
# <a name="deploying-federation-server-proxies"></a>部署同盟伺服器 Proxy

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

若要部署在 Active Directory Federation Services 中的同盟伺服器 proxy \(AD FS\)，完成每一項工作中[檢查清單：設定同盟伺服器 Proxy](Checklist--Setting-Up-a-Federation-Server-Proxy.md)。  
  
> [!NOTE]  
> 當您使用這份檢查清單時，我們建議您先閱讀規劃指南中的同盟伺服器 proxy 的參考[Windows Server 2012 中 AD FS 設計指南](https://technet.microsoft.com/library/dd807036.aspx)開始設定伺服器的程序之前。 下列檢查清單提供進一步了解設計和部署程序的同盟伺服器 proxy。  
  
## <a name="about-federation-server-proxies"></a>有關同盟伺服器 proxy  
同盟伺服器 proxy 是執行 Windows Server® 2012年和 AD FS 軟體以扮演 proxy 角色已手動設定的電腦。 您可以在組織中使用同盟伺服器 Proxy 來提供網際網路用戶端與公司網路防火牆後方同盟伺服器之間的媒介服務。  
  
> [!NOTE]  
> 雖然同盟伺服器和同盟伺服器 proxy 角色無法安裝在同一部電腦上，同盟伺服器可以執行同盟伺服器 proxy 函式。 如需詳細資訊，請參閱＜ [When to Create a Federation Server](https://technet.microsoft.com/library/dd807101.aspx)＞。  
  
Windows Server® 2012年的電腦上安裝 AD FS 軟體並設定它以 proxy 角色所做的動作可讓該電腦的同盟伺服器 proxy。  
  

