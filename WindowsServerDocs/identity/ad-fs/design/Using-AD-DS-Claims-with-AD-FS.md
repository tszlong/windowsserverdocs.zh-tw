---
description: 深入瞭解：搭配使用 AD DS 宣告與 AD FS
ms.assetid: 460792e4-9f1d-4e7b-b6b2-53e057f839df
title: 搭配使用 AD DS 宣告與 AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 3220d171bfb54a249da1e444d48b2462019650b8
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97049956"
---
# <a name="using-ad-ds-claims-with-ad-fs"></a>搭配使用 AD DS 宣告與 AD FS


您可以使用 Active Directory Domain Services \( AD DS \) \- 發出的使用者和裝置宣告以及 Active Directory 同盟服務 \( AD FS，為同盟應用程式啟用更豐富的存取控制 \) 。

## <a name="about-dynamic-access-control"></a>關於動態存取控制
在 Windows Server &reg; 2012 中，動態存取控制功能可讓組織根據使用者宣告，授與檔案的存取權，這些宣告是由 \( 使用者帳戶屬性 \) 和裝置宣告（由 \( \) Active Directory Domain Services AD DS 發出的電腦帳戶屬性 \( \) 所來源）所來源。 AD DS 發出的宣告會透過 Kerberos 驗證通訊協定整合至 Windows 整合式驗證。

如需動態存取控制的詳細資訊，請參閱 [動態存取控制內容藍圖](../../solution-guides/Dynamic-Access-Control--Scenario-Overview.md#BKMK_APP)。

### <a name="whats-new-in-ad-fs"></a>AD FS 的新功能
作為動態存取控制案例的擴充功能，Windows Server 2012 中的 AD FS 現在可：

-   除了 AD DS 內的使用者帳戶屬性之外，還可以存取電腦帳戶屬性。 在舊版 AD FS 中，同盟服務無法從 AD DS 存取電腦帳戶屬性。

-   取用位於 Kerberos 驗證票證的 AD DS 發行的使用者或裝置宣告。 在舊版 AD FS 中，宣告引擎可以從 Kerberos 讀取使用者和群組安全性識別碼 sid， \( \) 但無法讀取 kerberos 票證中包含的任何宣告資訊。

-   將 AD DS 發出的使用者或裝置宣告轉換成 SAML 權杖，以供信賴應用程式用來執行更豐富的存取控制。

## <a name="benefits-of-using-ad-ds-claims-with-ad-fs"></a>使用 AD DS 宣告搭配 AD FS 的優點
這些 AD DS 發出的宣告可以插入至 Kerberos 驗證票證，並與 AD FS 搭配使用，以提供下列優點：

-   如果組織需要更豐富的存取控制原則，可以 \- 使用以指定使用者或電腦帳戶 AD DS 中儲存的屬性值為基礎的 AD DS 發行的宣告，啟用對應用程式和資源的宣告式存取。 這可協助系統管理員減少與建立和管理相關的額外負荷：

    -   AD DS 安全性群組，這些安全性群組將用來控制可透過 Windows 整合式驗證存取的應用程式和資源的存取權。

    -   此樹系信任會用來控制對企業 \- 對 \- 企業 \( B2B \) \/ 網際網路可存取應用程式和資源的存取。

-   組織現在可以根據 AD DS 中儲存的特定電腦帳戶屬性值，防止未經授權存取用戶端電腦上的網路資源，例如， \( 電腦的 DNS 名稱是否 \) 符合資源的存取控制原則， \( 例如，使用宣告 ACLd 的檔案伺服器或信賴憑證者 \) 原則，例如 \( 宣告 \- 感知 Web 應用程式 \) 。 這可協助系統管理員為下列資源或應用程式設定更細微的存取控制原則：

    -   只能透過 Windows 整合式驗證來存取。

    -   可透過 AD FS 驗證機制存取網際網路。 AD FS 可以用來將 AD DS 發出的裝置宣告轉換成 AD FS 宣告，這些宣告可封裝至可供網際網路存取的資源或信賴憑證者應用程式使用的 SAML 權杖。

## <a name="differences-between-ad-ds-and-ad-fs-issued-claims"></a>AD DS 與 AD FS 發出的宣告之間的差異
若要瞭解從 AD DS 和 AD FS 發出的宣告，請務必瞭解兩個區分因素。 這些差異包括：

-   AD DS 只能發出以 Kerberos 票證封裝的宣告，而不是 SAML 權杖。 如需 AD DS 如何發出宣告的詳細資訊，請參閱 [動態存取控制內容藍圖](../../solution-guides/Dynamic-Access-Control--Scenario-Overview.md#BKMK_APP)。

-   AD FS 只能發出封裝在 SAML 權杖中的宣告，而不是 Kerberos 票證。 如需 AD FS 如何發出宣告的詳細資訊，請參閱 [宣告引擎的角色](../../ad-fs/technical-reference/The-Role-of-the-Claims-Engine.md)。

## <a name="how-ad-ds-issued-claims-work-with-ad-fs"></a>已發出的 AD DS 宣告如何與 AD FS 搭配
AD DS 發出的宣告可與 AD FS 搭配使用，以直接從使用者的驗證內容存取使用者和裝置宣告，而不是對 Active Directory 進行個別的 LDAP 呼叫。 下圖和對應的步驟會討論此程式如何更詳細地運作，以啟用 \- 動態存取控制案例的宣告型存取控制。

![使用宣告](media/UsingADDSClaimswithADFS.gif)

1.  AD DS 系統管理員會使用 Active Directory 管理中心主控台或 PowerShell Cmdlet，在 AD DS 架構中啟用特定的宣告類型物件。

2.  AD FS 系統管理員使用 AD FS 管理主控台來建立和設定宣告提供者，以及透過傳遞或轉換宣告規則來設定信賴憑證者信任 \- 。

3.  Windows 用戶端會嘗試存取網路。 在 Kerberos 驗證過程中，用戶端會向網域控制站呈現其使用者和電腦票證授與票證 TGT，但尚未 \- \( \) 包含任何宣告。 接著，網域控制站會查看已啟用之宣告類型的 AD DS，並在傳回的 Kerberos 票證中包含任何產生的宣告。

4.  當使用者 \/ 用戶端嘗試存取 ACLd 為需要宣告的檔案資源時，它們可以存取資源，因為從 Kerberos 呈現的複合識別碼具有這些宣告。

5.  當相同的用戶端嘗試存取設定為 AD FS authentication 的網站或 Web 應用程式時，系統會將使用者重新導向至為 Windows 整合式驗證設定的 AD FS 同盟伺服器。 用戶端會使用 Kerberos 將要求傳送至網域控制站。 網域控制站會發出 Kerberos 票證，其中包含用戶端可以呈現給同盟伺服器之要求的宣告。

6.  根據系統管理員先前設定的宣告提供者和信賴憑證者信任的宣告規則設定方式，AD FS 讀取 Kerberos 票證的宣告，並將其包含在針對用戶端發出的 SAML 權杖中。

7.  用戶端會收到包含正確宣告的 SAML 權杖，然後再重新導向至網站。

如需有關如何建立 AD DS 發出的宣告以使用 AD FS 所需的宣告規則的詳細資訊，請參閱 [建立轉換傳入](../../ad-fs/operations/Create-a-Rule-to-Transform-an-Incoming-Claim.md)宣告的規則。

## <a name="see-also"></a>另請參閱
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
