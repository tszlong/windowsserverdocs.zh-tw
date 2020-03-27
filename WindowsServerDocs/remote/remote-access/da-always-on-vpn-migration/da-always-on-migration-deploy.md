---
title: 從 DirectAccess 遷移至 Always On VPN
description: 從 DirectAccess 遷移至 Always On VPN 需要特定的程式來遷移用戶端，這有助於減少因執行遷移步驟而造成的競爭情形不按照順序。
manager: dougkim
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: eeca4cf7-90f0-485d-843c-76c5885c54b0
ms.author: lizross
author: eross-msft
ms.date: 06/07/2018
ms.openlocfilehash: 9f78edf0e48dc914b09a5e6f2d054e0fafba62e3
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80309307"
---
# <a name="migrate-to-always-on-vpn-and-decommission-directaccess"></a>移轉至 Always On VPN 並解除委任 DirectAccess

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows 10

&#171;[**上一步：** 規劃 DIRECTACCESS 以 Always On VPN 遷移](da-always-on-migration-planning.md)<br>

從 DirectAccess 遷移至 Always On VPN 需要特定的程式來遷移用戶端，這有助於減少因執行遷移步驟而造成的競爭情形不按照順序。 大致來說，遷移套裝程式含下列四個主要步驟：

1.  **部署並存 VPN 基礎結構。** 在決定您的遷移階段和您想要包含在部署中的功能之後，您將會與現有的 DirectAccess 基礎結構並存部署 VPN 基礎結構。  

2.  **將憑證和設定部署到用戶端。**  一旦 VPN 基礎結構準備就緒，您就可以建立所需的憑證，並將其發佈至用戶端。 當用戶端收到憑證時，您會部署 VPN_Profile. ps1 設定腳本。 或者，您可以使用 Intune 來設定 VPN 用戶端。 使用 Microsoft Endpoint Configuration Manager 或 Microsoft Intune 監視是否已成功部署 VPN 設定。

3.  [!INCLUDE [remove-da-security-group-shortdesc-include](../includes/remove-da-security-group-shortdesc-include.md)]

4.  [!INCLUDE [decommission-da-shortdesc-include](../includes/decommission-da-shortdesc-include.md)]

開始從 DirectAccess 到 Always On VPN 的遷移程式之前，請確定您已規劃進行遷移。  如果您尚未規劃您的遷移，請參閱[規劃 DirectAccess 以 ALWAYS ON VPN 遷移](da-always-on-migration-planning.md)。

>[!TIP] 
>本節不是 Always On VPN 的逐步部署指南，而是要補充[適用于 Windows Server 和 windows 10 的 ALWAYS ON Vpn 部署](../vpn/always-on-vpn/deploy/always-on-vpn-deploy.md)，並提供遷移特定的部署指導方針。

## <a name="deploy-a-side-by-side-vpn-infrastructure"></a>部署並存 VPN 基礎結構

您會並存部署 VPN 基礎結構與現有 DirectAccess 基礎結構。  如需逐步詳細資料，請參閱[適用于 Windows Server 和 windows 10 的 ALWAYS ON VPN 部署](../vpn/always-on-vpn/deploy/always-on-vpn-deploy.md)，以安裝和設定 Always On vpn 基礎結構。 

並存部署包含下列高層級工作：

1.  建立 VPN 使用者、VPN 伺服器和 NPS 伺服器群組。

2.  建立併發布必要的憑證範本。

3.  註冊伺服器憑證。

4.  安裝和設定 Always On VPN 的遠端存取服務。

5.  安裝和設定 NPS。

6.  設定 Always On VPN 的 DNS 和防火牆規則。

下圖提供整個 DirectAccess 對 Always On VPN 遷移中基礎結構變更的視覺參考。

![](../../media/DA-to-AlwaysOnVPN/6b64f322f945f837f22a32bf87a228f8.png)

## <a name="deploy-certificates-and-vpn-configuration-script-to-the-clients"></a>將憑證和 VPN 設定腳本部署至用戶端

雖然[部署 ALWAYS ON vpn](../vpn/always-on-vpn/deploy/always-on-vpn-deploy-deployment.md)一節中大量的 VPN 用戶端設定，但需要執行兩個額外步驟，才能順利完成從 DirectAccess 遷移至 Always On VPN。 

您必須確定在頒發證書_之後_， **VPN_Profile. PS1** ，讓 VPN 用戶端不會嘗試在沒有該憑證的情況下進行連線。 若要這麼做，您可以執行腳本，只將已在憑證中註冊的使用者新增至您的 VPN 部署就緒群組，以用來部署 Always On VPN 設定。

>[!NOTE] 
>Microsoft 建議您先測試此程式，再于任何使用者遷移環上執行。

1.  **建立併發布 VPN 憑證，並啟用自動註冊群組原則物件（GPO）。** 針對傳統的憑證型 Windows 10 VPN 部署，會將憑證發行給裝置或使用者，讓它可以驗證連線。 建立併發布新的驗證憑證以進行自動註冊時，您必須使用設定為 VPN 使用者群組的自動註冊設定來建立和部署 GPO。 如需設定憑證和自動註冊的步驟，請參閱[設定伺服器基礎結構](../vpn/always-on-vpn/deploy/vpn-deploy-server-infrastructure.md)。

2.  **將使用者新增至 VPN 使用者群組。** 新增您遷移至 VPN 使用者群組的任何使用者。 這些使用者在您遷移之後會保留在該安全性群組中，以便日後可以接收任何憑證更新。 繼續將使用者新增至此群組，直到您已將每位使用者從 DirectAccess 移至 Always On VPN 為止。 

3.  **識別已接收 VPN 驗證憑證的使用者。** 您要從 DirectAccess 進行遷移，因此您必須新增方法來識別用戶端何時收到所需的憑證，並準備好接收 VPN 設定資訊。 執行**GetUsersWithCert**腳本，將目前發出 nonrevoked 憑證的使用者，從指定的範本名稱新增至指定的 AD DS 安全性群組。 例如，在執行**GetUsersWithCert**腳本之後，任何使用者從 Vpn 驗證憑證範本發出的有效憑證都會新增至 Vpn 部署就緒群組。

    >[!NOTE] 
    >如果您沒有方法可以識別用戶端何時收到所需的憑證，您可以在將憑證發行給使用者之前，先部署 VPN 設定，使 VPN 連線失敗。 若要避免這種情況，請在憑證授權單位單位上執行**GetUsersWithCert**腳本，或依照排程將已收到憑證的使用者同步至 VPN 部署就緒群組。 接著，您將使用該安全性群組，將 Microsoft Endpoint Configuration Manager 或 Intune 中的 VPN 設定部署設為目標，以確保受管理用戶端在收到憑證之前不會收到 VPN 設定。
    
    ### <a name="getuserswithcertps1"></a>GetUsersWithCert. ps1
    
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

4. 部署 Always On VPN 設定。 當發出 VPN 驗證憑證，而且您執行**GetUsersWithCert**腳本時，使用者會新增至 Vpn 部署就緒安全性群組。


| 如果您使用 。  | 結果 |
| ---- | ---- |
| 組態管理員 | 根據該安全性群組的成員資格來建立使用者集合。<br><br>![](../../media/DA-to-AlwaysOnVPN/b38723b3ffcfacd697b83dd41a177f66.png)！|
| Intune | 只有在同步處理之後，才直接以安全性群組為目標。 |
|
    
每次執行**GetUsersWithCert**設定腳本時，您也必須執行 AD DS 探索規則，以更新 Configuration Manager 中的安全性群組成員資格。 此外，請確定部署集合的成員資格更新經常發生（與腳本和探索規則一致）。

如需使用 Configuration Manager 或 Intune 將 Always On VPN 部署至 Windows 用戶端的詳細資訊，請參閱[適用于 Windows Server 和 windows 10 的 ALWAYS ON VPN 部署](../vpn/always-on-vpn/deploy/always-on-vpn-deploy.md)。 不過，請務必納入這些遷移特有的工作。

>[!NOTE] 
>整合這些特定的工作，是簡單 Always On VPN 部署與從 DirectAccess 遷移至 Always On VPN 之間的重要差異。 請務必適當地定義集合，以將目標設為安全性群組，而不是使用部署指南中的方法。


## <a name="remove-devices-from-the-directaccess-security-group"></a>從 DirectAccess 安全性群組移除裝置

當使用者收到驗證憑證和**VPN_Profile. ps1**設定腳本時，您會在 Configuration Manager 或 Intune 中看到對應的成功 VPN 設定腳本部署。 在每個部署之後，從 DirectAccess 安全性群組中移除該使用者的裝置，讓您可以在稍後移除 DirectAccess。 Intune 和 Configuration Manager 都包含使用者裝置指派資訊，可協助您判斷每個使用者的裝置。

>[!NOTE] 
>如果您是透過組織單位（Ou）而不是電腦群組來套用 DirectAccess Gpo，請將使用者的電腦物件移出 OU。

## <a name="decommission-the-directaccess-infrastructure"></a>解除委任 DirectAccess 基礎結構

當您完成將所有 DirectAccess 用戶端遷移至 Always On VPN 時，您可以解除委任 DirectAccess 基礎結構，並從群組原則移除 DirectAccess 設定。 Microsoft 建議執行下列步驟，以正常地從您的環境移除 DirectAccess：

1.  [!INCLUDE [remove-config-settings-shortdesc-include](../includes/remove-config-settings-shortdesc-include.md)]

    ![](../../media/DA-to-AlwaysOnVPN/dbdc3d80e8dc1b8665f7b15d7d2ee1f6.png)

2.  [!INCLUDE [remove-da-security-group-shortdesc-include](../includes/remove-da-security-group-shortdesc-include.md)]

3.  **清除 DNS。** 請務必從內部 DNS 伺服器和與 DirectAccess 相關的公用 DNS 伺服器中移除任何記錄，例如 DA.contoso.com、DAGateway.contoso.com。

4.  [!INCLUDE [decommission-da-shortdesc-include](../includes/decommission-da-shortdesc-include.md)]

5.  **移除 Active Directory 憑證服務中的任何 DirectAccess 憑證。** 如果您使用電腦憑證進行 DirectAccess 的執行，請從憑證授權單位單位主控台的 [憑證範本] 資料夾中移除已發佈的範本。

## <a name="next-step"></a>後續步驟
您已完成從 DirectAccess 遷移至 Always On VPN。 

---