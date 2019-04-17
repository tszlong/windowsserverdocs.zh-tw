---
ms.assetid: 3a840b63-78b7-4e62-af7b-497026bfdb93
title: "逐步解說指南-管理條件存取控制的風險"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 11d2d567f9264dca53a3426263a172649d7d7c11
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="walkthrough-guide-manage-risk-with-conditional-access-control"></a>逐步解說指南： 管理條件存取控制的風險

>適用於： Windows Server 2012 R2


## <a name="about-this-guide"></a>有關本指南
本節提供管理因素 （使用者資料） 提供的其中一種風險透過條件存取控制機制在 Windows Server 2012 R2 的 Active Directory 同盟 Services (AD FS) 中的指示。 如需條件存取控制與授權機制 AD FS 在 Windows Server 2012 R2 的詳細資訊，請查看[使用風險管理條件存取控制與](../../ad-fs/operations/Manage-Risk-with-Conditional-Access-Control.md)。

本節下列各節所組成：

-   [步驟 1： 實驗室設定](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md#BKMK_1)

-   [步驟 2： 驗證預設 AD FS 存取控制機制](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md#BKMK_2)

-   [步驟 3： 設定的使用者資料的條件存取控制原則](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md#BKMK_3)

-   [步驟 4： 確認條件存取控制機制](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md#BKMK_4)

## <a name="BKMK_1"></a>步驟 1： 實驗室設定
完成本節，您必須環境，包含下列元件：

-   測試使用者和群組帳號，升級到 Windows Server 2012 R2 或 Windows Server 2012 R2 上執行 Active Directory domain 其結構描述 Windows Server 2008、 Windows Server 2008 R2 或 Windows Server 2012 上執行 Active Directory domain

-   Windows Server 2012 R2 上執行的聯盟伺服器

-   網頁伺服器裝載範例應用程式

-   Client 電腦，您可以存取的範例應用程式

> [!WARNING]
> 我們建議 （兩者皆 production 或測試環境），不做聯盟伺服器和您的網頁伺服器使用相同的電腦。

在此環境中聯盟伺服器問題，讓使用者可以存取的範例應用程式所需的宣告。 Web 伺服器裝載的範例應用程式，將信任的使用者提供宣告聯盟伺服器的問題。

如何設定此環境中的指示，請查看[在 Windows Server 2012 R2 AD FS 設定實驗室](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)。

## <a name="BKMK_2"></a>步驟 2： 驗證預設 AD FS 存取控制機制
在此步驟，您將會驗證預設 AD FS 存取控制機制，使用者會重新導向至 AD FS 登入頁面，提供有效的憑證，並會授與應用程式存取。 您可以使用**劉小龍 Hatley** AD account 和**claimapp**範例應用程式中設定[設定在 Windows Server 2012 R2 AD FS 實驗室](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)。

#### <a name="to-verify-the-default-ad-fs-access-control-mechanism"></a>若要確認預設 AD FS 存取控制機制

1.  您 client 在電腦上，開放瀏覽器視窗，並瀏覽到您的範例應用程式： **https://webserv1.contoso.com/claimapp**。

    這個動作會自動重新導向至 amc 要求聯盟伺服器，並提示您使用的使用者名稱和密碼登入。

2.  輸入認證**劉小龍 Hatley**您在建立廣告 account[設定實驗室 AD FS 在 Windows Server 2012 R2 的](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)。

    您將會授與應用程式的存取。

## <a name="BKMK_3"></a>步驟 3： 設定的使用者資料的條件存取控制原則
在此步驟將會設定根據群組成員資格資料使用者存取控制原則。 亦即，您將會設定**發行授權規則**代表您的範例應用程式-信賴廠商信任聯盟伺服器上**claimapp**。 此規則的邏輯，**劉小龍 Hatley** AD 使用者發行宣告因為他屬於存取此應用程式所需的**財經**群組。 您新增**劉小龍 Hatley**帳號至**財經**群組中[設定在 Windows Server 2012 R2 AD FS 實驗室](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)。

您可以完成使用其中一個 AD FS 管理主控台這項工作或透過 Windows PowerShell。

#### <a name="to-configure-conditional-access-control-policy-based-on-user-data-via-the-ad-fs-management-console"></a>設定使用者資料 AD FS 管理主控台透過為基礎的條件存取控制原則

1.  在 AD FS 管理主控台中，瀏覽至**信任關係**，然後**做為基礎的派對信任**。

2.  選取信賴廠商信任，表示您的範例應用程式 (**claimapp**)，然後再在**動作**窗格或以滑鼠右鍵按一下這個信賴廠商信任，選取**編輯理賠要求規則**。

3.  在**適用於 claimapp 編輯理賠要求規則**視窗中，選取**發行授權規則**索引標籤，然後按一下 [**新增規則**。

4.  在**新增發行授權取得規則精靈**，在**頁面選取 [規則範本**，請選取**允許] 或 [拒絕使用者根據傳入取得**取得規則範本，然後按一下**下一步**。

5.  在**設定規則**頁面上，執行下列所有，然後按**完成]**:

    1.  輸入宣告規則名稱，例如**TestRule**。

    2.  選取 [**群組 SID**為**傳入取得輸入**。

    3.  按一下**瀏覽]**，輸入**財經**AD 名稱測試群組，並將其的解析**傳入取得值**欄位。

    4.  選取 [**拒絕這個傳入理賠要求的使用者存取的**選項。

6.  在**編輯理賠要求規則 claimapp**視窗中，請務必 delete**允許所有使用者存取**建立這個信賴廠商信任已建立預設規則。

#### <a name="to-configure-conditional-access-control-policy-based-on-user-data-via-windows-powershell"></a>設定的使用者資料是透過 Windows PowerShell 條件存取控制原則

1.  在您聯盟的伺服器，開放 Windows PowerShell 命令視窗中，執行下列命令：


    `$rp = Get-AdfsRelyingPartyTrust -Name claimapp`


2.  在同一個 Windows PowerShell 命令視窗中，執行下列命令：


    `$GroupAuthzRule = '@RuleTemplate = "Authorization" @RuleName = "Foo" c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "^(?i)<group_SID>$"] =>issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "DenyUsersWithClaim");'
    Set-AdfsRelyingPartyTrust -TargetRelyingParty $rp -IssuanceAuthorizationRules $GroupAuthzRule`

> [!NOTE]
> 確定要取代的 AD SID 值 < group_SID >**財經**群組。

## <a name="BKMK_4"></a>步驟 4： 確認條件存取控制機制
您將會在此步驟來驗證您在上一個步驟中設定的條件存取控制原則。 您可以使用下列程序，以確認**劉小龍 Hatley** AD 使用者可以存取您的範例應用程式，因為他屬於**財經**群組並不屬於 AD 使用者**財經**群組無法存取的範例應用程式。

1.  您 client 在電腦上，開放瀏覽器視窗，並瀏覽到您的範例應用程式： **https://webserv1.contoso.com/claimapp**

    這個動作會自動重新導向至 amc 要求聯盟伺服器，並提示您使用的使用者名稱和密碼登入。

2.  輸入認證**劉小龍 Hatley**您在建立廣告 account[設定實驗室 AD FS 在 Windows Server 2012 R2 的](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)。

    您將會授與應用程式的存取。

3.  不屬於的另一個 AD 使用者的認證中輸入**財經**群組。 (如需如何建立廣告帳號，請查看[https://technet.microsoft.com/library/cc7833232.aspx](https://technet.microsoft.com/library/cc783323%28v=ws.10%29.aspx)。

    此時，您設定一個步驟中存取控制原則，因為拒絕存取 」 的訊息會顯示不屬於此 AD 使用者**財經**群組。 預設的訊息文字是**您未授權存取此網站。按一下此處以登出並重新登入或連絡您的系統管理員權限。** 不過，這文字可完全自訂。 如需了解如何自訂體驗登入資訊，請查看[[自訂頁面 AD FS 登入](https://technet.microsoft.com/library/dn280950.aspx)。

## <a name="see-also"></a>也了
[管理風險條件存取控制與](../../ad-fs/operations/Manage-Risk-with-Conditional-Access-Control.md)
[設定實驗室 AD FS 在 Windows Server 2012 R2](../deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)



