---
description: 深入瞭解：何時使用身分識別委派
ms.assetid: 6e711a96-9055-4508-b6d4-318d6aa95fd1
title: 使用身分識別委派的時機
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: a2d3987e4051ad4248381e9a425b2d728755eb55
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97044966"
---
# <a name="when-to-use-identity-delegation"></a>使用身分識別委派的時機

## <a name="what-is-identity-delegation"></a>身分識別委派是什麼？
身分識別委派是 Active Directory 同盟服務 \( AD FS 的功能 \) ，可讓系統管理員 \- 指定的帳號模擬使用者。 模擬使用者的帳戶稱為 *委派*。 若有許多分散式應用程式，且必須針對原始要求的授權鏈結中的每個應用程式、資料庫或服務循序執行一系列存取控制檢查，則此委派功能相當重要。 許多真實 \- 案例都有 Web 應用程式「前端」必須從更安全的「後端」取出資料，例如連接到 Microsoft SQL Server 資料庫的 web 服務。

例如，您可以透過程式設計方式增強現有的零件 \- 訂購網站，讓夥伴組織能夠查看自己的購買歷程記錄和帳戶狀態。 基於安全性理由，所有的夥伴財務資料都會儲存在專用結構化查詢語言 (SQL) SQL server 的安全資料庫中 \( \) 。 在此情況下，前端應用程式中的程式碼不會 \- 知道夥伴組織的財務資料。 因此，它必須從裝載于此案例的網路上其他位置的另一部電腦抓取該資料，以 \( \) 供元件資料庫後端的 Web 服務之用 \( \) 。

為了讓此資料抓取程式 \- 成功， \- 必須在 web 應用程式與元件資料庫的 web 服務之間進行某些授權「手搖動」，如下圖所示。

![身分識別委派](media/adfs2_identitydelegationconcept.gif)

由於原始要求是對 Web 伺服器本身提出 (可能位於嘗試存取 Web 伺服器之使用者的組織以外的完全不同組織中)，因此連同要求一起傳送的安全性權杖不符合授權準則，以致於無法存取 Web 伺服器以外的任何其他電腦。 因此，可以滿足原始使用者要求的唯一方法是：將中繼的同盟伺服器放在資源夥伴組織中，協助重新發出具有適當存取權限的安全性權杖。

## <a name="how-does-identity-delegation-work"></a>身分識別委派如何運作？
多層式應用程式架構中的 Web 應用程式通常會呼叫 Web 服務來存取通用資料或功能。 這些 Web 服務必須知道原始使用者的身分識別，服務才能做出授權決策並進行稽核。 在此情況下，前端 \- web 應用程式會將使用者表示為 Web 服務的委派。 AD FS 允許 Active Directory 帳戶做為其他信賴憑證者的使用者，藉此協助此案例。 下圖顯示身分識別委派案例。

![身分識別委派](media/adfs2_identitydelegationsteps.gif)

1.  Frank 嘗試 \- 從另一個組織的 Web 應用程式存取部分訂購歷程記錄。 他的用戶端電腦會要求並接收來自 AD FS 的權杖，以供前端 \- 部分 \- 訂購 Web 應用程式。

2.  用戶端電腦會將要求傳送至 Web 應用程式，包括在步驟1中取得的權杖，以證明用戶端的身分識別。

3.  Web 應用程式需要與 Web 服務溝通，才能完成其與用戶端的交易。 Web 應用程式會聯絡 AD FS 以取得委派權杖，以與 Web 服務互動。 委派權杖是安全性權杖，會發行給委派以做為使用者。 AD FS 會傳回包含用戶端宣告的委派權杖（以 Web 服務為目標）。

4.  Web 應用程式會使用從步驟 3 AD FS 取得的權杖來存取作為用戶端的 Web 服務。 藉由檢查委派權杖，Web 服務可判斷 Web 應用程式是否做為用戶端。 Web 服務執行其授權原則、記錄要求，並將 Frank 原始要求的所需零件歷程記錄資料，提供給 Web 應用程式及 Frank。

針對特定的委派，AD FS 可以限制 Web 應用程式可能會要求委派權杖的 Web 服務。 用戶端電腦不需要 Active Directory 帳戶就能完成此作業。 最後，如先前所述，Web 服務可輕鬆地判斷做為使用者之委派的身分識別。 這可讓 Web 服務根據它們是直接連繫用戶端電腦，還是透過委派連繫，從而展現不同的行為。

## <a name="configuring-ad-fs-for-identity-delegation"></a>設定身分識別委派的 AD FS
您可以使用 AD FS 管理嵌入式管理單元 \- ，在需要協助資料抓取程式時，為身分識別委派設定 AD FS。 設定好之後，AD FS 可以產生新的安全性權杖，其中將包含後 \- 端服務可能需要的授權內容，才能提供受保護資料的存取權。

AD FS 不會限制可模擬的使用者。 設定身分識別委派 AD FS 之後，它會執行下列動作：

-   它會判斷哪些伺服器可將授權委派給要求權杖來模擬使用者。

-   它會建立並分開保留委派的用戶端帳戶，及做為委派的伺服器之身分識別內容。

您可以在 AD FS 管理] 嵌入式管理單元中，將委派授權規則新增至信賴憑證者信任，以設定身分識別委派 \- 。 如需做法詳細資訊，請參閱＜ [Checklist: Creating Claim Rules for a Relying Party Trust](../../ad-fs/deployment/Checklist--Creating-Claim-Rules-for-a-Relying-Party-Trust.md)＞。

## <a name="configuring-the-front-end-web-application-for-identity-delegation"></a>設定用於身分 \- 識別委派的前端 Web 應用程式
開發人員有幾個選項可用來適當地程式設計 Web 前端 \- 應用程式或服務，以將委派要求重新導向 AD FS 電腦。 如需如何自訂 Web 應用程式來使用身分識別委派的詳細資訊，請參閱 [Windows Identity Foundation SDK](https://go.microsoft.com/fwlink/?LinkId=122266)。

## <a name="see-also"></a>另請參閱
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
