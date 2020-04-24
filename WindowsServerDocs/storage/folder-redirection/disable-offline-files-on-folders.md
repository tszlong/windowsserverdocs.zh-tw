---
title: 停用個別重新導向資料夾上的離線檔案
description: 如何使用資料夾重新導向，在重新導向至網路共用的個別資料夾上停用離線檔案快取。
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 09/10/2018
ms.localizationpriority: medium
ms.openlocfilehash: c2614c0180b32a0215454f2d725d6a962986ef1f
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/23/2020
ms.locfileid: "71394398"
---
# <a name="disable-offline-files-on-individual-redirected-folders"></a>停用個別重新導向資料夾上的離線檔案

>適用於：Windows 10、Windows 8、Windows 8.1、Windows Server 2019、Windows Server 2016、Windows Server 2012、Windows Server 2012 R2 和 Windows (半年通道)

本主題說明如何使用資料夾重新導向，在重新導向至網路共用的個別資料夾上停用離線檔案快取。 這可讓您指定要從本機快取排除的資料夾，減少同步處理離線檔案所需的離線檔案快取大小和時間。

>[!NOTE]
>本主題包含可讓您用來將部分所述的程序自動化的 Windows PowerShell Cmdlet 範例。 如需詳細資訊，請參閱 [Windows PowerShell 基本概念](https://docs.microsoft.com/powershell/scripting/getting-started/fundamental/windows-powershell-basics?view=powershell-6)。

## <a name="prerequisites"></a>必要條件

若要停用特定重新導向資料夾的離線檔案快取，您的環境必須符合下列必要條件。

- 用戶端電腦已加入其中的 Active Directory 網域服務 (AD DS) 網域。 沒有樹系或網域功能層級需求或結構描述需求。
- 執行 Windows 10、Windows 8.1、Windows 8、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012 或 Windows (半年通道) 的用戶端電腦。
- 已安裝群組原則管理的電腦。

## <a name="disabling-offline-files-on-individual-redirected-folders"></a>停用個別重新導向資料夾上的離線檔案

若要停用特定重新導向資料夾的離線檔案快取，請使用群組原則啟用適用於適當群組原則物件 (GPO) 的 [不要將特定重新導向資料夾自動變成離線可用]  原則設定。 將此原則設定設為**停用**或**未設定**會使所有重新導向的資料夾離線可用。

>[!NOTE]
>只有網域系統管理員、企業系統管理員及群組原則建立者擁有者群組的成員，才能建立 GPO。

### <a name="to-disable-offline-files-on-specific-redirected-folders"></a>要停用特定重新導向資料夾上的離線檔案

1. 開啟 [群組原則管理]  。
2. 若要選擇性地建立新的 GPO，以指定哪些使用者應該將重新導向資料夾排除以便在離線時無法使用，請在適當的網域或組織單位 (OU) 上按一下滑鼠右鍵，然後選取 [在這個網域中建立 GPO 並連結]  。
3. 在主控台樹狀目錄中，以滑鼠右鍵按一下您要設定離線資料夾重新導向設定的 GPO，然後選取 [編輯]  。 [群組原則管理編輯器] 隨即出現。
4. 在主控台樹狀目錄的 [使用者組態]  下方，依序展開 [原則]  、[系統管理範本]  、[系統]  和 [資料夾重新導向]  。
5. 以滑鼠右鍵按一下 [不要將特定重新導向資料夾自動變成離線可用]  ，然後選取 [編輯]  。 [不要將特定重新導向資料夾自動變成離線可用]  視窗隨即開啟。
6. 選取 [已啟用]  。 在 [選項]  窗格中，選取適當的核取方塊，以選取不應離線使用的資料夾。 選取 [確定]  。

### <a name="windows-powershell-equivalent-commands"></a>Windows PowerShell 對應的命令

下列 Windows PowerShell Cmdlet 或多個 Cmdlet 會執行與[停用個別重新導向資料夾離線檔案](#disabling-offline-files-on-individual-redirected-folders)中所述的程序相同的功能。 在單一行中，輸入各個 Cmdlet (即使因為格式限制，它們可能會在這裡出現自動換行成數行)。

這個範例會在 contoso.com  網域的 MyOu  組織單位中，建立名為「離線檔案設定」  的新 GPO (LDAP 辨別名稱為 "ou=MyOU,dc=contoso,dc=com")。 然後，它會停用影片重新導向資料夾的離線檔案。

```PowerShell
New-GPO -Name "Offline Files Settings" | New-Gplink -Target "ou=MyOu,dc=contoso,dc=com" -LinkEnabled Yes

Set-GPRegistryValue –Name "Offline Files Settings" –Key
"HKCU\Software\Policies\Microsoft\Windows\NetCache\{18989B1D-99B5-455B-841C-AB7C74E4DDFC}" -ValueName DisableFRAdminPinByFolder –Type DWORD –Value 1
```

請參閱以下表格，了解每個重新導向資料夾使用的登錄機碼名稱清單 (資料夾 GUID)。

|重新導向資料夾|登錄機碼名稱 (資料夾 GUID)|
|---|---|
|AppData(Roaming)|{3EB685DB-65F9-4CF6-A03A-E3EF65729F3D}|
|[桌上型電腦]|{B4BFCC3A-DB2C-424C-B029-7FE99A87C641}|
|開始功能表|{625B53C3-AB48-4EC1-BA1F-A1EF4146FC19}|
|Documents|{FDD39AD0-238F-46AF-ADB4-6C85480369C7}|
|圖片|{33E28130-4E1E-4676-835A-98395C3BC3BB}|
|音樂|{4BD8D571-6D19-48D3-BE97-422220080E43}|
|視訊|{18989B1D-99B5-455B-841C-AB7C74E4DDFC}|
|我的最愛|{1777F761-68AD-4D8A-87BD-30B759FA33DD}|
|連絡人|{56784854-C6CB-462b-8169-88E350ACB882}|
|下載|{374DE290-123F-4565-9164-39C4925E467B}|
|連結|{BFB9D5E0-C6A9-404C-B2B2-AE6DB6AF4968}|
|搜尋|{7D1D3A04-DEBB-4115-95CF-2F29DA2920DA}|
|儲存的遊戲|{4C5C32FF-BB9D-43B0-B5B4-2D72E54EAAA4}|

## <a name="more-information"></a>其他資訊

- [資料夾重新導向、離線檔案及漫遊使用者設定檔概觀](folder-redirection-rup-overview.md)
- [使用離線檔案部署資料夾重新導向](deploy-folder-redirection.md)