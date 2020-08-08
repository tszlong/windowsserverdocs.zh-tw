---
ms.assetid: cea6011d-3753-4b95-aaa5-38d4e97d6e42
title: 在帳戶夥伴中準備用戶端電腦
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: b78c3d3b4c5562acfcc7b2657f6e49119fc491e2
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87967595"
---
# <a name="prepare-client-computers-in-the-account-partner"></a>在帳戶夥伴中準備用戶端電腦

若要讓帳戶夥伴組織中的系統管理員準備用戶端電腦以存取 Active Directory 同盟服務 AD FS 同盟應用程式，最簡單的方式 \( \) 就是使用群組原則。 群組原則提供便利的方式，讓您將同盟所需的特定憑證和設定，推入到將用來存取同盟應用程式的所有用戶端電腦。

如此一來，您的用戶端電腦就可以順暢地存取同盟應用程式，而不需要憑證提示或信任的網站相關提示，建議您先準備每部用戶端電腦，然後才在組織中廣泛部署 AD FS。 請考慮使用群組原則來自動進行：

-   在每部用戶端電腦上設定 Internet Explorer，以信任帳戶同盟伺服器。

    如需詳細資訊，請參閱 [Configure Client Computers to Trust the Account Federation Server](../../ad-fs/deployment/Configure-Client-Computers-to-Trust-the-Account-Federation-Server.md)。

-   安裝適當的帳戶同盟伺服器、資源同盟伺服器和 Web 服務器安全通訊端層 \( SSL \) 憑證， \( 或與每部 \) 用戶端電腦上受信任的根連結的對等憑證。

    如需詳細資訊，請參閱[使用群組原則將憑證散發到用戶端電腦](../../ad-fs/deployment/Distribute-Certificates-to-Client-Computers-by-Using-Group-Policy.md)。


## <a name="see-also"></a>另請參閱
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
