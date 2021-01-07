---
title: 疑難排解一般問題
description: 瞭解如何針對與遠端存取相關的一般問題進行疑難排解。
manager: brianlic
ms.topic: article
ms.assetid: 354ae5e3-bae1-44f9-afd7-7eaba70f2346
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: d0866ca88f109d6304fe7da4e424506c22defc52
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97950524"
---
# <a name="troubleshooting-general-issues"></a>疑難排解一般問題

>適用於：Windows Server (半年度管道)、Windows Server 2016

本主題包含與遠端存取相關之一般問題的疑難排解資訊。

## <a name="gpo-retrieval-error"></a>GPO 抓取錯誤
**收到的錯誤**。 無法取出 DirectAccess 伺服器 GPO 設定。 確定您有 GPO 的編輯許可權。

遠端存取管理主控台在收到此錯誤之後沒有回應。

**原因**

DirectAccess 無法存取部署中其中一個進入點的 GPO，因此設定無法載入。

**解決方案**

請確定部署中的每個進入點在其網域控制站上都有對應的 GPO，並確認登入的使用者具有遠端存取部署中所有設定之 Gpo 的讀取和寫入權限。

因應措施是使用 configuration Cmdlet，而不是使用遠端存取管理主控台;例如，使用 `Get-RemoteAccess` 和 `Get-DAEntryPoint` 。

> [!NOTE]
> 無法使用目前進入點的伺服器 GPO 時，不會發生此情況。

您可以使用指令 `Get-DAEntryPointDC` 程式來列出所有儲存伺服器 gpo 的網域控制站，以及 `Get-DAMultiSite` 如何在 `Get-RemoteAccess` 部署中取出伺服器 gpo 的完整清單。 例如：

```
$ServerGpos = Get-DAEntryPointDC | ForEach-Object {
   @{
       GpoName = (Get-RemoteAccess -EntryPoint $_.EntryPointName).ServerGpoName;
        DC = $_.DomainControllerName }
}
$ServerGpos | ForEach-Object { $GpoName = $_['GpoName'] ; $DC = $_['DC'] ; Write-Host "Server GPO '$GpoName' on DC '$DC'" }
```

## <a name="windows-7-to-windows-8-or-10-client-upgrade"></a>Windows 7 至 Windows 8 或10用戶端升級
**徵兆**。 在 Windows 7 用戶端升級至多網站部署 Windows 10 或 Windows 8 之後，[網路] 清單中不會顯示 DirectAccess 連接。

**原因**

多網站部署中的 Windows 7 Gpo 不包含 Windows 8 Network Connectivity Assistant 設定。

 Windows 7 用戶端應該使用 DirectAccess 連線助理來監視其 DirectAccess 線上狀態，此狀態需要在 Windows 7 用戶端 Gpo 中進行個別的手動設定。 當 Windows 7 用戶端升級為 Windows 10 或 Windows 8 時，如果仍套用 Windows 7 用戶端 GPO，則網路連線助理將無法運作。

**解決方案**

如果已在 Windows 7 Gpo 中設定 DirectAccess 連線助理設定，則您可以使用下列 PowerShell Cmdlet 修改 Windows 7 Gpo，以在升級用戶端電腦之前解決此問題：

```
Set-GPRegistryValue -Name <Windows7GpoName> -Domain <DomainName> -Key "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\NetworkConnectivityAssistant" -ValueName "TemporaryValue" -Type Dword -Value 1
Remove-GPRegistryValue -Name <Windows7GpoName> -Domain <DomainName> -Key "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\NetworkConnectivityAssistant"
```

如果用戶端已升級或未設定 DCA，請將用戶端電腦移至 Windows 10 或 Windows 8 安全性群組。

## <a name="general-cmdlet-errors"></a>一般 Cmdlet 錯誤

-   **問題 1**

    **收到的錯誤**。 <server_name 或 entry_point_name> 無法達到網域控制站 <domain_controller>。

    **原因**

    為了保持多站台部署中的設定一致，確認每一個 GPO 都是由單一網域控制站所管理非常重要。 當管理進入點伺服器 GPO 的網域控制站無法使用時，就無法讀取或修改遠端存取設定。

    **解決方案**

    請依照2.4 中所述的「變更管理伺服器 Gpo 的網域控制站」程式進行操作 [。設定 Gpo](assetId:///b1960686-a81e-4f48-83f1-cc4ea484df43#ConfigGPOs)。

-   **問題 2**

    **收到的錯誤**。 無法連線到網域 <domain_name> 的主域控制站。

    **原因**

    為了保持多站台部署中的設定一致，確認每一個 GPO 都是由單一網域控制站所管理非常重要。 用戶端 GPO 是在網域主控站上管理。 如果網域主控站無法使用，便無法讀取或修改遠端存取組態設定。

    **解決方案**

    依照2.4 中所述的「傳送 PDC 模擬器角色」程式進行操作 [。設定 Gpo](assetId:///b1960686-a81e-4f48-83f1-cc4ea484df43#ConfigGPOs)。



