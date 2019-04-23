---
ms.assetid: cea6011d-3753-4b95-aaa5-38d4e97d6e42
title: 在帳戶夥伴中準備用戶端電腦
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 0c5bdcb0a80b15a1905109229ddd20ee642a8dd7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868519"
---
# <a name="prepare-client-computers-in-the-account-partner"></a>在帳戶夥伴中準備用戶端電腦

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

最簡單的方式，讓系統管理員帳戶中合作夥伴組織準備用戶端電腦存取 Active Directory Federation Services \(AD FS\)同盟應用程式是使用群組原則。 群組原則提供便利的方式，讓您將同盟所需的特定憑證和設定，推入到將用來存取同盟應用程式的所有用戶端電腦。  
  
使您的用戶端電腦可以順暢地存取同盟應用程式，而不需要憑證提示或受信任的網站相關的提示，建議您先準備每部用戶端電腦，再將 AD FS 部署大致上您的組織。 請考慮使用群組原則來自動進行：  
  
-   設定 Internet Explorer 信任的帳戶同盟伺服器，每個用戶端電腦上。  
  
    如需詳細資訊，請參閱 [Configure Client Computers to Trust the Account Federation Server](../../ad-fs/deployment/Configure-Client-Computers-to-Trust-the-Account-Federation-Server.md)。  
  
-   安裝適當的帳戶同盟伺服器、 資源同盟伺服器和網頁伺服器安全通訊端層\(SSL\)憑證\(或對等項目憑證鏈結至信任的根\)上每個用戶端電腦。  
  
    如需詳細資訊，請參閱 <<c0> [ 發佈的憑證，透過使用群組原則的用戶端電腦](../../ad-fs/deployment/Distribute-Certificates-to-Client-Computers-by-Using-Group-Policy.md)。  
  

## <a name="see-also"></a>另請參閱
[Windows Server 2012 中 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
