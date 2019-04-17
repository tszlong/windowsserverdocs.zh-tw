---
title: 停用離線個別重新導向的資料夾上的檔案
description: 如何停用個別的資料夾，使用資料夾重新導向重新導向到網路共用上快取的離線檔案。
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 09/10/2018
ms.localizationpriority: medium
ms.openlocfilehash: adc93906cb7ff958fc1db7b00abdc557623e764e
ms.sourcegitcommit: 9ed4c9fe04ebf3ef488170503c9a354c992b6fde
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/03/2018
ms.locfileid: "4339306"
---
# 停用離線個別重新導向的資料夾上的檔案

>適用於： Windows 10，Windows 8、 Windows 8.1、 Windows Server 2012、 Windows Server 2012 R2，Windows Server 2016

本主題說明如何停用個別的資料夾，使用資料夾重新導向重新導向到網路共用上快取的離線檔案。 這可提供讓您指定要排除在本機快取的資料夾，減少離線檔案快取大小及時間才能同步處理離線檔案。

>[!NOTE]
>本主題包含您可用來自動化某些所述的程序的範例 Windows PowerShell cmdlet。 如需詳細資訊，請參閱[Windows PowerShell 的基本知識](https://docs.microsoft.com/powershell/scripting/getting-started/fundamental/windows-powershell-basics?view=powershell-6)。

## 必要條件

若要停用特定資料夾重新導向的快取，離線檔案，您的環境必須符合下列先決條件。

- Active Directory 網域服務 (AD DS) 網域，與已加入網域的用戶端電腦。 沒有樹系或網域功能等級需求或結構描述的需求。
- 執行 Windows 10、 Windows 8.1、 Windows 8、 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012 的用戶端電腦。
- 使用群組原則管理已安裝在電腦。

## 停用離線個別重新導向的資料夾上的檔案

若要停用特定資料夾重新導向的快取，離線檔案，使用群組原則來啟用**離線不會自動進行特定重新導向的資料夾提供**這項原則設定 「 適當群組原則物件 (GPO)。 設定**已停用**或**未設定**這個原則設定可讓所有重新導向的資料夾使用離線。

>[!NOTE]
>網域系統管理員、 企業系統管理員，以及群組原則建立者擁有者群組的成員可以建立 Gpo。

### 若要停用特定的離線檔案重新導向的資料夾

1. 開啟**群組原則管理**。
2. 選擇性地建立新的 GPO，指定哪些使用者應該已重新導向的資料夾排除正在離線使用，以滑鼠右鍵按一下適當的網域或組織單位 (OU) 然後選取**建立這個網域中的 GPO 並連結到此處**.
3. 在主控台樹狀目錄中，以滑鼠右鍵按一下您要設定資料夾重新導向設定，然後選取 [**編輯**的 GPO。 群組原則管理編輯器會出現。
4. 在主控台樹狀目錄中，在**使用者設定**] 下展開**原則**、 展開**\ [系統管理範本**]，展開 [**系統**] 並展開**資料夾重新導向**。
5. 以滑鼠右鍵按一下 [**離線不會自動進行特定的重新導向的資料夾可用**，然後選取 [**編輯**。 **離線不會自動進行特定的重新導向的資料夾可用**的視窗隨即顯示。
6. 選取 **\[已啟用\]**。 在 [**選項**] 窗格中選取的資料夾，應該不可供離線瀏覽透過選取適當的核取方塊。 選取 **\[確定\]**。

### 對等的 Windows PowerShell 指令

下列 Windows PowerShell cmdlet 執行相同的功能做為[停用離線上的檔案個別重新導向的資料夾](#disabling-offline-files-on-individual-redirected-folders)中所述的程序。 輸入一行，每個 cmdlet，即使它們可能會顯示文字換行，透過以下幾行因為格式限制式。

這個範例會建立新的 GPO 命名*離線檔案設定*為在*contoso.com*網域中的*MyOu*組織單位 (LDAP 辨別的名稱是 「 ou = MyOU，dc = contoso，dc = com 」)。 然後停用離線影片重新導向資料夾的檔案。

```PowerShell
New-GPO -Name "Offline Files Settings" | New-Gplink -Target "ou=MyOu,dc=contoso,dc=com" -LinkEnabled Yes

Set-GPRegistryValue –Name "Offline Files Settings" –Key
"HKCU\Software\Policies\Microsoft\Windows\NetCache\{18989B1D-99B5-455B-841C-AB7C74E4DDFC}" -ValueName DisableFRAdminPinByFolder –Type DWORD –Value 1
```

請參閱下表如登錄機碼名稱 （資料夾的 Guid），以用於每個重新導向的資料夾的清單。

|重新導向的資料夾|登錄機碼名稱 （資料夾 GUID）|
|---|---|
|AppData(Roaming)|{3EB685DB-65F9-4CF6-A03A-E3EF65729F3D}|
|桌面|{B4BFCC3A-DB2C-424C-B029-7FE99A87C641}|
|[開始] 功能表|{625B53C3-AB48-4EC1-BA1F-A1EF4146FC19}|
|文件|{FDD39AD0-238F-46AF-ADB4-6C85480369C7}|
|圖片|{33E28130-4E1E-4676-835A-98395C3BC3BB}|
|音樂|{4BD8D571-6D19-48D3-BE97-422220080E43}|
|影片|{18989B1D-99B5-455B-841C-AB7C74E4DDFC}|
|我的最愛|{1777F761-68AD-4D8A-87BD-30B759FA33DD}|
|連絡人|{56784854-C6CB-462b-8169-88E350ACB882}|
|下載|{374DE290-123F-4565-9164-39C4925E467B}|
|連結|{BFB9D5E0-C6A9-404C-B2B2-AE6DB6AF4968}|
|搜尋|{7D1D3A04-DEBB-4115-95CF-2F29DA2920DA}|
|所有的遊戲|{4C5C32FF-BB9D-43B0-B5B4-2D72E54EAAA4}|

## 更多資訊

- [資料夾重新導向、 離線檔案及漫遊使用者設定檔的概觀](folder-redirection-rup-overview.md)
- [部署資料夾重新導向與離線檔案](deploy-folder-redirection.md)