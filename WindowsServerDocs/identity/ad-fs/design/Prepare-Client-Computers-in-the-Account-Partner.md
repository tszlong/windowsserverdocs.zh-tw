---
ms.assetid: cea6011d-3753-4b95-aaa5-38d4e97d6e42
title: "準備 Client Account 合作夥伴在的電腦"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 0c5bdcb0a80b15a1905109229ddd20ee642a8dd7
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="prepare-client-computers-in-the-account-partner"></a>準備 Client Account 合作夥伴在的電腦

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

Client 電腦準備存取 Active Directory 同盟服務 \(AD FS\) 聯盟應用程式的系統管理員的身分 account 合作夥伴組織中的最簡單方式是使用群組原則。 群組原則提供方便您發送特定的憑證，並設定所需的所有用於存取聯盟應用程式的 client 電腦聯盟。  
  
使 client 電腦可以順暢地進行存取聯盟應用程式，不需要憑證提示或受信任的網站相關的提示，建議部署之前 AD FS 針對您在組織中，首先準備每個 client 的電腦。 請考慮使用群組原則來自動：  
  
-   Internet Explorer 每個 client 在電腦上設定信任 account 聯盟伺服器。  
  
    如需詳細資訊，請查看[設定 Client 電腦信任 Account 聯盟伺服器](../../ad-fs/deployment/Configure-Client-Computers-to-Trust-the-Account-Federation-Server.md)。  
  
-   安裝適當 account 聯盟伺服器、資源聯盟伺服器，與 Web 伺服器安全通訊端層 \(SSL\) 憑證 \（或相當於憑證鏈結該受信任的 root\）每個 client 電腦上。  
  
    如需詳細資訊，請查看[Client 電腦使用群組原則來散發憑證](../../ad-fs/deployment/Distribute-Certificates-to-Client-Computers-by-Using-Group-Policy.md)。  
  

## <a name="see-also"></a>也了
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
