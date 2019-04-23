---
ms.assetid: 6e711a96-9055-4508-b6d4-318d6aa95fd1
title: 使用身分識別委派的時機
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: af227d9e87ddb73f194dd46c8ce45fcdf12a34cf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872549"
---
# <a name="when-to-use-identity-delegation"></a>使用身分識別委派的時機

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012
  
## <a name="what-is-identity-delegation"></a>身分識別委派是什麼？  
身分識別委派是 Active Directory Federation Services 的功能\(AD FS\) ，可讓系統管理員\-指定帳戶，模擬使用者。 模擬使用者的帳戶稱為*委派*。 若有許多分散式應用程式，且必須針對原始要求的授權鏈結中的每個應用程式、資料庫或服務循序執行一系列存取控制檢查，則此委派功能相當重要。 許多真實\-世界的案例存在中 Web 應用程式 「 前端 」 必須擷取資料，從更安全 「 後端 」，例如連接到 Microsoft SQL Server 資料庫的 Web 服務。  
  
例如，現有的組件\-訂購網站可藉由程式設計強化，可讓夥伴組織檢視他們自己的購買記錄和帳戶狀態。 基於安全性理由，所有夥伴的財務資料都儲存在安全的資料庫上固定的結構化查詢語言\(SQL\)伺服器。 在此情況下，前端中的程式碼\-結束應用程式一無所知夥伴組織的財務資料。 因此，它必須擷取該資料，從另一部電腦裝載的網路上的其他地方\(在此情況下\)零件資料庫的 Web 服務\(後端\)。  
  
這項資料\-擷取程序成功，連續授權某些 「 手動\-搖晃 」 必須在進行 Web 應用程式和 Web 服務之間零件資料庫，如下圖所示。  
  
![身分識別委派](media/adfs2_identitydelegationconcept.gif)  
  
由於原始要求是對 Web 伺服器本身提出 (可能位於嘗試存取 Web 伺服器之使用者的組織以外的完全不同組織中)，因此連同要求一起傳送的安全性權杖不符合授權準則，以致於無法存取 Web 伺服器以外的任何其他電腦。 因此，可以滿足原始使用者要求的唯一方法是：將中繼的同盟伺服器放在資源夥伴組織中，協助重新發出具有適當存取權限的安全性權杖。  
  
## <a name="how-does-identity-delegation-work"></a>身分識別委派如何運作？  
多層式應用程式架構中的 Web 應用程式通常會呼叫 Web 服務來存取通用資料或功能。 這些 Web 服務必須知道原始使用者的身分識別，服務才能做出授權決策並進行稽核。 在此情況下，前端\-端 Web 應用程式到 Web 服務為委派表示的使用者。 AD FS 允許 Active Directory 帳戶做為另一個信賴憑證者的合作對象的使用者，從而完成此案例。 下圖顯示身分識別委派案例。  
  
![身分識別委派](media/adfs2_identitydelegationsteps.gif)  
  
1.  Frank 嘗試存取部分\-排序另一個組織中的 Web 應用程式的歷程記錄。 他的用戶端電腦要求並接收權杖，從最上層的 AD FS\-結束部分\-訂購 Web 應用程式。  
  
2.  用戶端電腦將要求傳送至 Web 應用程式，包括在步驟 1 所取得的權杖，以證明用戶端的身分識別。  
  
3.  Web 應用程式需要與 Web 服務溝通，才能完成其與用戶端的交易。 Web 應用程式會連絡 AD FS，以取得與 Web 服務互動的委派權杖。 委派權杖是安全性權杖，會發行給委派以做為使用者。 AD FS 會傳回包含針對 Web 服務的用戶端相關宣告的委派權杖。  
  
4.  Web 應用程式會使用從步驟 3 中的 AD FS，以存取 Web 服務，做為用戶端取得的權杖。 藉由檢查委派權杖，Web 服務可判斷 Web 應用程式是否做為用戶端。 Web 服務執行其授權原則、記錄要求，並將 Frank 原始要求的所需零件歷程記錄資料，提供給 Web 應用程式及 Frank。  
  
特定的委派，AD FS 可以限制的 Web 應用程式可能會要求委派權杖的 Web 服務。 用戶端電腦不需要 Active Directory 帳戶就能完成此作業。 最後，如先前所述，Web 服務可輕鬆地判斷做為使用者之委派的身分識別。 這可讓 Web 服務根據它們是直接連繫用戶端電腦，還是透過委派連繫，從而展現不同的行為。  
  
## <a name="configuring-ad-fs-for-identity-delegation"></a>設定身分識別委派的 AD FS  
您可以使用 AD FS 管理嵌入式管理單元\-中設定身分識別委派的 AD FS，每當您需要執行資料擷取程序。 您在設定之後，AD FS 可以產生新的安全性權杖會包含授權內容的後端\-才能提供受保護資料的存取權，可能需要結束服務。  
  
AD FS 不會限制哪些使用者可以模擬。 您設定 AD FS 身分識別委派之後，它會執行下列作業：  
  
-   它會判斷哪些伺服器可將授權委派給要求權杖來模擬使用者。  
  
-   它會建立並分開保留委派的用戶端帳戶，及做為委派的伺服器之身分識別內容。  
  
您可以藉由新增信賴憑證者的合作對象的委派授權規則信任 AD FS 管理嵌入式管理單元中設定身分識別委派\-中。 如需如何執行這項操作的詳細資訊，請參閱[檢查清單：建立信賴憑證者信任宣告規則](../../ad-fs/deployment/Checklist--Creating-Claim-Rules-for-a-Relying-Party-Trust.md)。  
  
## <a name="configuring-the-front-end-web-application-for-identity-delegation"></a>設定前端\-結束身分識別委派的 Web 應用程式  
開發人員有數個選項，它們可以用來適當地設計 Web 前端\-結束應用程式或服務，將委派要求重新導向到 AD FS 的電腦。 如需如何自訂 Web 應用程式來使用身分識別委派的詳細資訊，請參閱 [Windows Identity Foundation SDK](https://go.microsoft.com/fwlink/?LinkId=122266)。  
  
## <a name="see-also"></a>另請參閱
[Windows Server 2012 中 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
