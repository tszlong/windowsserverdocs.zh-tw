---
title: 從 DirectAccess，讓它一律開啟 VPN 移轉
description: 從 DirectAccess 移轉至一律開啟 」 VPN 需要特定的程序，來移轉用戶端，可協助最小化所引發的執行移轉步驟順序的競爭情形。
manager: dougkim
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: eeca4cf7-90f0-485d-843c-76c5885c54b0
ms.author: pashort
author: shortpatti
ms.date: 06/07/2018
ms.openlocfilehash: 94852b33936ece0b329614f428e845f2c7a2ac6a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854579"
---
# <a name="migrate-to-always-on-vpn-and-decommission-directaccess"></a>移轉至 Always On VPN 並解除委任 DirectAccess

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows 10

&#171;[**前一個：** 規劃 DirectAccess，讓它一律開啟 」 VPN 移轉](da-always-on-migration-planning.md)<br>

從 DirectAccess 移轉至一律開啟 」 VPN 需要特定的程序，來移轉用戶端，可協助最小化所引發的執行移轉步驟順序的競爭情形。 概括而言，移轉程序包含下列四個主要步驟：

1.  **部署的並排顯示 VPN 基礎結構。** 您決定移轉階段，以及您想要在您的部署中包含的功能之後，您將部署與現有的 DirectAccess 基礎結構的 VPN 基礎結構。  

2.  **部署至用戶端的憑證及設定。**  將 VPN 基礎結構準備就緒後，您會建立，並將所需的憑證發佈到用戶端。 當用戶端已收到的憑證時，您會部署 VPN_Profile.ps1 組態指令碼。 或者，您可以使用 Intune 設定 VPN 用戶端。 使用 Microsoft System Center Configuration Manager 或 Microsoft Intune，以監視成功的 VPN 設定部署。

3.  [!INCLUDE [remove-da-security-group-shortdesc-include](../includes/remove-da-security-group-shortdesc-include.md)]

4.  [!INCLUDE [decommission-da-shortdesc-include](../includes/decommission-da-shortdesc-include.md)]

開始之前移轉程序從 DirectAccess 一律開啟 」 VPN，請務必在規劃移轉。  如果您有未規劃您的移轉，請參閱[計劃一律開啟 」 VPN 移轉至 DirectAccess](da-always-on-migration-planning.md)。

>[!TIP] 
>本節不是一律開啟 」 VPN 的逐步部署指南，但而要補充[Always On VPN 部署的 Windows Server 和 Windows 10](../vpn/always-on-vpn/deploy/always-on-vpn-deploy.md) ，並提供移轉特定專案的部署指引。

## <a name="deploy-a-side-by-side-vpn-infrastructure"></a>部署的並排顯示 VPN 基礎結構

您部署與現有的 DirectAccess 基礎結構的 VPN 基礎結構。  如需逐步的詳細資訊，請參閱[Always On VPN 部署的 Windows Server 和 Windows 10](../vpn/always-on-vpn/deploy/always-on-vpn-deploy.md)安裝和設定一律開啟 」 VPN 基礎結構。 

並排顯示部署包含下列高階工作：

1.  建立 VPN 使用者、 VPN 伺服器和 NPS 伺服器群組。

2.  建立並發行所需的憑證範本。

3.  註冊伺服器憑證。

4.  安裝並設定一律開啟 」 VPN 遠端存取服務。

5.  安裝和設定 NPS。

6.  一律開啟 」 VPN 設定 DNS 和防火牆規則。

下圖提供視覺上的參考在 DirectAccess-至-Alwayson VPN 移轉整個基礎結構的變更。

![](../../media/DA-to-AlwaysOnVPN/6b64f322f945f837f22a32bf87a228f8.png)

## <a name="deploy-certificates-and-vpn-configuration-script-to-the-clients"></a>部署至用戶端的憑證和 VPN 組態指令碼

雖然 VPN 用戶端組態中的大量[部署一律開啟 」 VPN](../vpn/always-on-vpn/deploy/always-on-vpn-deploy-deployment.md)區段中，兩個額外步驟才能順利完成從 DirectAccess 移轉至一律開啟 」 VPN。 

您必須確定**VPN_Profile.ps1**出現_之後_，讓 VPN 用戶端不會嘗試連接，而它已發出的憑證。 若要這麼做，您可以執行憑證中的已註冊的使用者加入您 VPN 部署就緒的群組，您用來部署一律開啟 」 VPN 組態指令碼。

>[!NOTE] 
>Microsoft 建議您測試此程序，然後再執行任何您的使用者移轉環形。

1.  **建立和發佈 VPN 憑證，並啟用自動註冊的群組原則物件 (GPO)。** 針對傳統、 以憑證為基礎的 Windows 10 VPN 部署，憑證被發給裝置或使用者，讓它可以驗證連線。 當建立新的驗證憑證，並發佈的自動註冊時，您必須建立並部署與設定為 「 VPN 使用者 」 群組的自動註冊設定的 GPO。 如需設定憑證和自動註冊的步驟，請參閱[設定的伺服器基礎結構](../vpn/always-on-vpn/deploy/vpn-deploy-server-infrastructure.md)。

2.  **將使用者新增至 VPN 使用者群組。** 新增任何您移轉至 「 VPN 使用者 」 群組的使用者。 之後您移轉它們，讓他們可以在未來收到任何憑證更新，這些使用者會停留在該安全性群組。 繼續將使用者新增至此群組，直到您已經將移動的每位使用者從 DirectAccess 一律開啟 」 VPN。 

3.  **識別已收到的 VPN 驗證憑證的使用者。** 因此您必須加入一個方法來識別用戶端已收到所需的憑證，並已準備好接收 VPN 組態資訊時，您從 DirectAccess 之後，進行移轉。 執行**GetUsersWithCert.ps1**指令碼以加入目前發行 nonrevoked 來自指定的範本名稱，以指定的 AD DS 安全性群組的使用者。 例如，執行後**GetUsersWithCert.ps1**指令碼中，任何使用者發出的有效憑證從 VPN 驗證憑證範本新增至 VPN 部署就緒的群組。

    >[!NOTE] 
    >如果您沒有可用的方法，來識別用戶端已收到所需的憑證時，您可以在憑證發行給使用者時，造成 VPN 連線失敗之前部署的 VPN 組態。 若要避免這種情況下，執行**GetUsersWithCert.ps1**指令碼在憑證授權單位，或依排程同步處理已收到的憑證給 VPN 部署就緒的群組的使用者。 您接著將使用該安全性群組為目標的 System Center Configuration Manager 或 Intune，以確保受管理的用戶端不會不接收 VPN 組態之前收到的憑證中的 VPN 設定部署。
    
    ### <a name="getuserswithcertps1"></a>GetUsersWithCert.ps1
    
    ```powershell
    Import-module ActiveDirectory
    Import-Module AdcsAdministration
    
    $TemplateName = 'VPNUserAuthentication'##Certificate Template Name (not the friendly name)
    $GroupName = 'VPN Deployment Ready' ##Group you will add the users to
    $CSServerName = 'localhost\corp-dc-ca' ##CA Server Information
    
    $users= @()
    $TemplateID = (get-CATemplate | Where-Object {$_.Name -like $TemplateName} | Select-Object oid).oid
    $View = New-Object -ComObject CertificateAuthority.View
    $NULL = $View.OpenConnection($CSServerName)
    $View.SetResultColumnCount(3)
    $i1 = $View.GetColumnIndex($false, "User Principal Name")
    $i2 = $View.GetColumnIndex($false, "Certificate Template")
    $i3 = $View.GetColumnIndex($false, "Revocation Date")
    $i1, $i2, $i3 | %{$View.SetResultColumn($_) }
    $Row= $View.OpenView()
    
    while ($Row.Next() -ne -1){
    $Cert = New-Object PsObject
    $Col = $Row.EnumCertViewColumn()
    [void]$Col.Next()
    do {
    $Cert | Add-Member -MemberType NoteProperty $($Col.GetDisplayName()) -Value $($Col.GetValue(1)) -Force
          }
    until ($Col.Next() -eq -1)
    $col = ''
    
    if($cert."Certificate Template" -eq $TemplateID -and $cert."Revocation Date" -eq $NULL){
       $users= $users+= $cert."User Principal Name"
    $temp = $cert."User Principal Name"
    $user = get-aduser -Filter {UserPrincipalName -eq $temp} –Property UserPrincipalName
    Add-ADGroupMember $GroupName $user
       }
      }
    ```

4. 部署 一律開啟 」 VPN 組態。 為 VPN 驗證憑證發行，而且您執行**GetUsersWithCert.ps1**指令碼中，使用者會新增至 VPN 部署就緒的安全性群組。


| 如果您使用...  | 則... |
| ---- | ---- |
| System Center Configuration Manager | 建立以該安全性群組的成員資格使用者集合。<br><br>![](../../media/DA-to-AlwaysOnVPN/b38723b3ffcfacd697b83dd41a177f66.png)\!|
| Intune | 只要在一旦同步處理之後，直接目標的安全性群組。 |
|
    
每次執行**GetUsersWithCert.ps1**組態指令碼，您必須執行 AD DS 探索規則可用來更新安全性群組成員資格在 System Center Configuration Manager。 此外，請確定該部署集合的成員資格更新頻率夠 （指令碼，並探索規則與對齊）。

如需有關使用 System Center Configuration Manager 或 Intune 部署至 Windows 用戶端的 一律開啟 」 VPN 的詳細資訊，請參閱[Always On VPN 部署的 Windows Server 和 Windows 10](../vpn/always-on-vpn/deploy/always-on-vpn-deploy.md)。 請務必，不過，將這些移轉的特定工作。

>[!NOTE] 
>納入這些移轉特定的工作是簡單的一律開啟 」 VPN 部署和從 DirectAccess 移轉至一律開啟 」 VPN 之間的差別。 請務必正確地定義為目標的安全性群組，而不是使用部署指南中的方法集合。


## <a name="remove-devices-from-the-directaccess-security-group"></a>DirectAccess 安全性群組中移除裝置

當使用者收到的驗證憑證並**VPN_Profile.ps1**組態指令碼，您會看到對應成功 VPN 組態指令碼部署 System Center Configuration Manager 或 Intune 中的。 在每次部署中，如此您就可以移除 DirectAccess，從 DirectAccess 安全性群組移除該使用者的裝置。 Intune 和 System Center Configuration Manager 包含可協助您判斷每個使用者裝置的使用者裝置指派資訊。

>[!NOTE] 
>如果您要將 DirectAccess Gpo 套用到組織單位 (Ou)，而不電腦群組，將移出 OU 的使用者的電腦物件。

## <a name="decommission-the-directaccess-infrastructure"></a>解除委任 DirectAccess 基礎結構

當您完成將所有 DirectAccess 用戶端都移轉至一律開啟 」 VPN 時，您可以解除委任 DirectAccess 基礎結構，並從群組原則中移除的 DirectAccess 設定。 Microsoft 建議執行下列步驟以從環境中依正常程序移除 DirectAccess:

1.  [!INCLUDE [remove-config-settings-shortdesc-include](../includes/remove-config-settings-shortdesc-include.md)]

    ![](../../media/DA-to-AlwaysOnVPN/dbdc3d80e8dc1b8665f7b15d7d2ee1f6.png)

2.  [!INCLUDE [remove-da-security-group-shortdesc-include](../includes/remove-da-security-group-shortdesc-include.md)]

3.  **清除 DNS。** 請務必移除您的內部 DNS 伺服器中的任何記錄和公用 DNS 伺服器與 DirectAccess，比方說，DA.contoso.com，DAGateway.contoso.com。

4.  [!INCLUDE [decommission-da-shortdesc-include](../includes/decommission-da-shortdesc-include.md)]

5.  **移除 Active Directory 憑證服務中的 DirectAccess 的任何憑證。** 如果您使用電腦憑證 DirectAccess 實作時，請從在憑證授權單位主控台的 [憑證範本] 資料夾中移除已發佈的範本。

## <a name="next-step"></a>後續步驟
您完成從 DirectAccess 移轉至一律開啟 」 VPN。 

---