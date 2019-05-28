---
title: 停用個別重新導向的資料夾上的離線檔案
description: 如何停用離線檔案快取上使用資料夾重新導向來重新導向至網路共用的個別資料夾。
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 09/10/2018
ms.localizationpriority: medium
ms.openlocfilehash: b006742c9256c357d9aff3fb1b765dbed087383a
ms.sourcegitcommit: ed27ddbe316d543b7865bc10590b238290a2a1ad
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/09/2019
ms.locfileid: "65475874"
---
# <a name="disable-offline-files-on-individual-redirected-folders"></a>停用個別重新導向的資料夾上的離線檔案

>適用於：Windows 10，Windows 8、 Windows 8.1、 Windows Server 2019、 Windows Server 2016、 Windows Server 2012、windows Server 2012 R2 Windows （半年通道）

本主題描述如何停用離線檔案快取上使用資料夾重新導向來重新導向至網路共用的個別資料夾。 這讓您能夠指定要從本機快取中排除的資料夾，減少離線檔案快取大小和時間必須同步處理離線檔案。

>[!NOTE]
>本主題包含可讓您用來將部分所述的程序自動化的 Windows PowerShell Cmdlet 範例。 如需詳細資訊，請參閱 < [Windows PowerShell 基本概念](https://docs.microsoft.com/powershell/scripting/getting-started/fundamental/windows-powershell-basics?view=powershell-6)。

## <a name="prerequisites"></a>必要條件

若要停用離線檔案快取特定的資料夾重新導向，您的環境必須符合下列必要條件。

- 含有已加入網域的用戶端電腦的 Active Directory 網域服務 (AD DS) 網域。 沒有任何樹系或網域功能等級需求或結構描述的需求。
- 用戶端電腦執行 Windows 10，Windows 8.1，Windows 8、 Windows Server 2019、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012 或 Windows （半年通道）。
- 具有安裝群組原則管理的電腦。

## <a name="disabling-offline-files-on-individual-redirected-folders"></a>停用個別重新導向的資料夾上的離線檔案

若要停用離線檔案快取特定重新導向的資料夾，請使用群組原則來啟用**執行不會自動提供特定的資料夾重新導向離線**原則設定為適當的群組原則物件 (GPO)。 設定此原則設定可**已停用**或是**未設定**使得所有重新導向的資料夾可離線使用。

>[!NOTE]
>只有網域系統管理員、 企業系統管理員及群組原則建立者擁有者群組的成員可以建立 Gpo。

### <a name="to-disable-offline-files-on-specific-redirected-folders"></a>若要停用特定的重新導向資料夾上的離線檔案

1. 開啟**群組原則管理**。
2. 若要選擇性地建立新的 GPO，指定哪些使用者應該已在從可離線中排除的資料夾重新導向，以滑鼠右鍵按一下適當的網域或組織單位 (OU)，然後選取 **這個網域中建立 GPO 並連結這裡的 it**。
3. 在主控台樹狀目錄中，以滑鼠右鍵按一下您要設定資料夾重新導向設定，然後選取的 GPO**編輯**。 群組原則管理編輯器隨即出現。
4. 在主控台樹狀目錄中，在**使用者設定**，展開**原則**，展開**系統管理範本**，展開**系統**，及依序展開**資料夾重新導向**。
5. 以滑鼠右鍵按一下**不會自動進行特定的重新導向的資料夾可以使用離線**，然後選取**編輯**。 **執行不會自動提供特定的資料夾重新導向離線** 視窗隨即出現。
6. 選取 [已啟用]  。 在 **選項**窗格中選取的資料夾，不應提供離線選取適當的核取方塊。 選取 [確定]。 

### <a name="windows-powershell-equivalent-commands"></a>Windows PowerShell 對應的命令

下列 Windows PowerShell cmdlet 或指令程式執行相同的函式中的程序所述[停用離線檔案在個別的資料夾重新導向](#disabling-offline-files-on-individual-redirected-folders)。 在單一行中，輸入各個 Cmdlet (即使因為格式限制，它們可能會在這裡出現自動換行成數行)。

這個範例會建立名為新的 GPO*離線檔案設定*中*MyOu*中的組織單位*contoso.com*網域 (LDAP 辨別的名稱是"ou = MyOU，dc =contoso，dc = com")。 然後停用離線檔案的 [重新導向的影片] 資料夾。

```PowerShell
New-GPO -Name "Offline Files Settings" | New-Gplink -Target "ou=MyOu,dc=contoso,dc=com" -LinkEnabled Yes

Set-GPRegistryValue –Name "Offline Files Settings" –Key
"HKCU\Software\Policies\Microsoft\Windows\NetCache\{18989B1D-99B5-455B-841C-AB7C74E4DDFC}" -ValueName DisableFRAdminPinByFolder –Type DWORD –Value 1
```

請參閱下表中的登錄機碼名稱 （資料夾的 Guid），要用於每個重新導向資料夾的清單。

|重新導向的資料夾|登錄機碼名稱 （資料夾 GUID）|
|---|---|
|[Appdata\roaming]|{3EB685DB-65F9-4CF6-A03A-E3EF65729F3D}|
|桌面|{B4BFCC3A-DB2C-424C-B029-7FE99A87C641}|
|開始功能表|{625B53C3-AB48-4EC1-BA1F-A1EF4146FC19}|
|文件|{FDD39AD0-238F-46AF-ADB4-6C85480369C7}|
|圖片|{33E28130-4E1E-4676-835A-98395C3BC3BB}|
|音樂|{4BD8D571-6D19-48D3-BE97-422220080E43}|
|影片|{18989B1D-99B5-455B-841C-AB7C74E4DDFC}|
|我的最愛|{1777F761-68AD-4D8A-87BD-30B759FA33DD}|
|連絡人|{56784854-C6CB-462b-8169-88E350ACB882}|
|下載|{374DE290-123F-4565-9164-39C4925E467B}|
|連結|{BFB9D5E0-C6A9-404C-B2B2-AE6DB6AF4968}|
|搜尋|{7D1D3A04-DEBB-4115-95CF-2F29DA2920DA}|
|儲存的遊戲|{4C5C32FF-BB9D-43B0-B5B4-2D72E54EAAA4}|

## <a name="more-information"></a>詳細資訊

- [資料夾重新導向、 離線檔案和漫遊使用者設定檔的概觀](folder-redirection-rup-overview.md)
- [部署資料夾重新導向與離線檔案](deploy-folder-redirection.md)