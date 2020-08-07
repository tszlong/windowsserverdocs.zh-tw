---
title: 設定網域部署的群組原則
description: 瞭解如何在 MultiPoint 服務中設定群組原則
ms.date: 07/22/2016
ms.topic: article
ms.assetid: 13e5fa90-d330-4155-a6b8-78eb650cbbfa
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 67dfc93793710e9d2f06a66205644fa15db44141
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87937321"
---
# <a name="configure-group-policies-for-a-domain-deployment"></a>設定網域部署的群組原則
若要確保 MultiPoint 服務的網域部署正常運作，請將下列群組原則設定套用至 MultiPoint 服務系統上的 WMSshell 使用者帳戶。

> [!IMPORTANT]
> 某些群組原則設定可能會導致無法將所需的設定套用至 MultiPoint 服務。 請確定您瞭解並定義群組原則設定，使其在 MultiPoint 服務上正確運作。 例如，防止自動登入的群組原則設定可能會出現 MultiPoint 服務登入行為的問題。

## <a name="update-group-policies-for-the-wmsshell-user-account"></a>更新 WMSshell 使用者帳戶的群組原則
WMSshell 使用者帳戶是 MultiPoint 服務用來登入主控台的系統帳戶，其中會建立實際的工作站。 此帳戶不應該由 MultiPoint 管理員管理。

> [!NOTE]
> 若要瞭解如何更新群組原則，請參閱[本機群組原則編輯器](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn265982(v=ws.11))。

**原則：** 使用者設定 > 系統管理範本 > 控制台 >**個人**化

指派下列值：

|設定|值|
|-----------|----------|
|啟用螢幕保護裝置|已停用|
|螢幕保護裝置超時|已停用<p>秒數： xxx|
|密碼保護螢幕保護裝置|已停用|

**原則：** 電腦設定 >Windows 設定 >安全性設定 >本機原則 >使用者權限指派 >**允許本機登**入

|設定|值|
|-----------|----------|
|允許本機登入|確定帳戶清單包含 WMSshell 帳戶。<p>**注意：** 根據預設，WMSshell 帳戶是 Users 群組的成員。 如果使用者群組在清單中，而 WMSshell 是 Users 群組的成員，您就不需要將 WMSshell 帳戶新增至清單。|

> [!IMPORTANT]
> 當您設定任何群組原則時，請確定原則不會干擾 MultiPoint 伺服器上的自動更新和錯誤 Windows 錯誤報告。 這些設定是由在 Windows MultiPoint Server 安裝期間選取的 [**自動安裝更新**] 和 [**自動 Windows 錯誤報告**設定，在 [multipoint 管理員] 中使用 [**編輯服務器設定**] 或設定于 [磁片保護] 的 [已排程的更新] 中進行。

## <a name="update-the-registry"></a>更新登錄
針對 MultiPoint 服務的網域部署，您應該更新下列登錄子機碼。

> [!IMPORTANT]
> 不正確地編輯登錄可能會對系統造成嚴重的損害。 變更登錄之前，您應該先備份電腦所有的重要資料。

#### <a name="to-update-registry-subkeys-for-a-domain-deployment-of-multipoint-services"></a>更新 MultiPoint 服務網域部署的登錄子機碼

1.  開啟 [登錄編輯程式]。  (命令提示字元中，輸入**regedit.exe**，然後按 enter 鍵。 ) 

2.  在左窗格中，找出並選取下列登錄子機碼：

    HKEY_USERS \<SIDofWMSshell> \Software\Policies\Microsoft\Windows\Control Panel\Desktop

    其中 ' <SIDofWMSshell> ' 是 WMSshell 帳戶 (SID) 的安全識別碼。 若要瞭解如何識別 SID，請參閱[如何將使用者名稱與安全識別碼建立關聯 (SID) ](https://support.microsoft.com/kb/154599)。

3.  在右邊的清單中，更新下列子機碼。

    |子機碼|值名稱|值資料|
    |----------|--------------|--------------|
    |ScreenSaveActive|REG_SZ|0 (零)|
    |ScreenSaveTimeout|REG_SZ|120|
    |ScreenSaverIsSecure|REG_SZ|0 (零)|

    若要更新登錄子機碼：

    1.  在左窗格中選取登錄機碼後，以滑鼠右鍵按一下右窗格中的子機碼，然後按一下 [**修改**]。

    2.  在 [編輯字串] 對話方塊的 [**數值資料**] 中輸入新值，然後按一下 **[確定]**。

4.  在您完成更新登錄子機碼之後，請重新開機電腦以啟用變更。
