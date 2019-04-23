---
title: 疑難排解一般問題
description: 本主題是本指南的一部分部署多部遠端存取伺服器在 Windows Server 2016 中的多站台部署中。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 354ae5e3-bae1-44f9-afd7-7eaba70f2346
ms.author: pashort
author: shortpatti
ms.openlocfilehash: fe1fdc4fb5aff2e34555b08d3b2c4347e643085e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831079"
---
# <a name="troubleshooting-general-issues"></a>疑難排解一般問題

>適用於：Windows Server （半年通道），Windows Server 2016

本主題包含與遠端存取相關的一般問題的疑難排解資訊。  
  
## <a name="gpo-retrieval-error"></a>GPO 抓取錯誤  
**收到的錯誤**。 無法擷取 DirectAccess 伺服器 GPO 設定。 請確定您具有 gpo 的編輯權限。  
  
收到此錯誤之後，[遠端存取管理] 主控台沒有回應。  
  
**原因**  
  
DirectAccess 無法存取其中一個部署中的進入點的 GPO，並因此無法載入組態。  
  
**解決方法**  
  
請確定在部署中的每個進入點以在其網域控制站上具有對應的 GPO，並確認登入的使用者具有讀取和寫入設定遠端存取部署中的所有 Gpo 的權限。  
  
因應措施，而不是使用 遠端存取管理 主控台中，使用 設定 cmdlet例如，使用`Get-RemoteAccess`和`Get-DAEntryPoint`。  
  
> [!NOTE]  
> 伺服器 GPO 的目前項目點無法使用時，就不會發生這種情況。  
  
您可以使用`Get-DAEntryPointDC`cmdlet 來列出儲存伺服器 Gpo 的所有網域控制站並`Get-DAMultiSite`搭配`Get-RemoteAccess`擷取在部署的伺服器 Gpo 的完整都清單。 例如:   
  
```  
$ServerGpos = Get-DAEntryPointDC | ForEach-Object {   
   @{   
       GpoName = (Get-RemoteAccess -EntryPoint $_.EntryPointName).ServerGpoName;   
        DC = $_.DomainControllerName }   
}  
$ServerGpos | ForEach-Object { $GpoName = $_['GpoName'] ; $DC = $_['DC'] ; Write-Host "Server GPO '$GpoName' on DC '$DC'" }  
```  
  
## <a name="windows-7-to-windows-8-or-10-client-upgrade"></a>Windows 8 的 Windows 7 或 10 的用戶端升級  
**徵兆**。 在多站台部署中升級為 Windows 10 或 Windows 8 的 Windows 7 用戶端之後，DirectAccess 連線不在網路清單中看到。  
  
**原因**  
  
Windows 7 Gpo，在多站台部署中不包含 Windows 8 網路連線助理的設定。  
  
 Windows 7 用戶端應該使用 DirectAccess 連線助理 （jlca） 來監視他們需要個別的手動組態，在 Windows 7 用戶端 Gpo 中的 DirectAccess 連線狀態。 當 Windows 7 用戶端會升級為 Windows 10 或 Windows 8 時，網路連線助理將無法運作如果仍套用 Windows 7 用戶端 GPO。  
  
**解決方法**  
  
如果在 Windows 7 的 Gpo 中設定 DirectAccess 連線助理的設定，您可以解決這個問題，然後再升級用戶端電腦，藉由修改 Windows 7 Gpo，使用下列 PowerShell cmdlet:  
  
```  
Set-GPRegistryValue -Name <Windows7GpoName> -Domain <DomainName> -Key "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\NetworkConnectivityAssistant" -ValueName "TemporaryValue" -Type Dword -Value 1  
Remove-GPRegistryValue -Name <Windows7GpoName> -Domain <DomainName> -Key "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\NetworkConnectivityAssistant"  
```  
  
如果已升級的用戶端，或未設定 DCA，將用戶端電腦移到 Windows 10 或 Windows 8 的 「 安全性 」 群組。  
  
## <a name="general-cmdlet-errors"></a>一般指令程式錯誤  
  
-   **問題 1**  
  
    **收到的錯誤**。 無法連線網域控制站 < g >，< v e _ 或進入 >。  
  
    **原因**  
  
    為了保持多站台部署中的設定一致，確認每一個 GPO 都是由單一網域控制站所管理非常重要。 管理 進入點之伺服器 GPO 的網域控制站無法使用時，無法讀取或修改遠端存取組態設定。  
  
    **解決方法**  
  
    請依照 「 若要變更管理伺服器 Gpo 的網域控制站 」 所述的程序[2.4。設定 Gpo](assetId:///b1960686-a81e-4f48-83f1-cc4ea484df43#ConfigGPOs)。  
  
-   **問題 2**  
  
    **收到的錯誤**。 無法連線網域 < 網域名稱 > 的主要網域控制站。  
  
    **原因**  
  
    為了保持多站台部署中的設定一致，確認每一個 GPO 都是由單一網域控制站所管理非常重要。 用戶端 GPO 是在網域主控站上管理。 如果網域主控站無法使用，便無法讀取或修改遠端存取組態設定。  
  
    **解決方法**  
  
    請遵循 「 轉移 PDC 模擬器角色 」 中所描述的程序[2.4。設定 Gpo](assetId:///b1960686-a81e-4f48-83f1-cc4ea484df43#ConfigGPOs)。  
  


