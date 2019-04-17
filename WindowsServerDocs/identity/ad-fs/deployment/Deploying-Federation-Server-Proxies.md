---
ms.assetid: 1b21b0a9-1fe6-4fd1-8a25-92e578d774ed
title: "部署聯盟的 Proxy 伺服器"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: b914141a0445febd3961b688aadc2f444b2eee7b
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="deploying-federation-server-proxies"></a>部署聯盟的 Proxy 伺服器

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

若要部署聯盟伺服器 proxy 在 Active Directory 同盟服務 \(AD FS\)，完成的工作中每個[檢查清單︰ 設定好聯盟伺服器 Proxy](Checklist--Setting-Up-a-Federation-Server-Proxy.md)。  
  
> [!NOTE]  
> 當您使用此檢查清單時，我們建議您第一次看到聯盟伺服器 proxy 計劃中的指導方針參考[在 Windows Server 2012 中 AD FS 程式設計指南](https://technet.microsoft.com/library/dd807036.aspx)設定伺服器的程序在您開始之前。 下列檢查清單可提供深入瞭解聯盟設計和部署程序的 proxy 伺服器。  
  
## <a name="about-federation-server-proxies"></a>關於聯盟的 proxy 伺服器  
聯盟伺服器 proxy 是執行 Windows Server® 2012 年和 AD FS 軟體的電腦已在 [proxy 角色做手動設定。 您可以使用您在組織中聯盟伺服器 proxy 提供網際網路 client 與您的企業網路有防火牆聯盟伺服器中間服務。  
  
> [!NOTE]  
> 雖然聯盟伺服器及聯盟伺服器 proxy 角色無法安裝所在的電腦上，聯盟伺服器可以執行聯盟伺服器 proxy 功能。 如需詳細資訊，請查看[當建立聯盟伺服器](https://technet.microsoft.com/library/dd807101.aspx)。  
  
Windows Server® 2012 年的電腦上安裝軟體 AD FS 並將它設為 proxy 角色中進行的動作可該電腦聯盟 proxy 伺服器。  
  

