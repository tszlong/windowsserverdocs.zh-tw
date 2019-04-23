---
title: 設定網域部署的群組原則
description: 了解如何設定 MultiPoint 服務中的 群組原則
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 13e5fa90-d330-4155-a6b8-78eb650cbbfa
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: f661fbdc40fd7dd2562d51756bc7642c8e9a4a82
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888039"
---
# <a name="configure-group-policies-for-a-domain-deployment"></a>設定網域部署的群組原則
若要確保您的網域部署 MultiPoint 服務運作正常，下列群組原則設定套用至 WMSshell 使用者帳戶，MultiPoint 服務系統上。  
  
> [!IMPORTANT]  
> 某些群組原則設定可以防止套用到 MultiPoint 服務的必要的組態設定。 請務必了解，並定義您的群組原則設定，讓它們在 MultiPoint 服務上正確運作。 比方說，可防止自動登入的群組原則設定可能會造成問題與 MultiPoint 服務的登入行為。  
  
## <a name="update-group-policies-for-the-wmsshell-user-account"></a>更新 WMSshell 使用者帳戶的群組原則 
WMSshell 使用者帳戶是系統帳戶的 MultiPoint 服務會使用登入到主控台 actuall 站台的建立位置。 此帳戶不會由 MultiPoint 管理員管理。
  
> [!NOTE]  
> 若要了解如何更新群組原則，請參閱[本機群組原則編輯器](https://technet.microsoft.com/library/dn265982.aspx)。  
  
**原則：** 使用者設定 > 系統管理範本] > [控制台 >**個人化**  
  
指定下列值：  
  
|設定|值|  
|-----------|----------|  
|啟用螢幕保護裝置|已停用|  
|螢幕保護裝置逾時|已停用<br /><br />秒數： xxx|  
|以密碼保護的螢幕保護裝置|已停用|  
  
**原則：** 電腦設定 > Windows 設定 > 安全性設定 > 本機原則 > 使用者權限指派 >**允許本機登入**  
  
|設定|值|  
|-----------|----------|  
|允許本機登入|請確定帳戶的清單包含 WMSshell 帳戶。<br /><br />**注意：** 根據預設，WMSshell 帳戶會是 「 使用者 」 群組的成員。 如果 「 使用者 」 群組是在清單中，而且 WMSshell 是 「 使用者 」 群組的成員，則您不需要將 WMSshell 帳戶新增至清單。|  
  
> [!IMPORTANT]  
> 當您設定的任何群組原則時，確定該原則不會干擾自動更新 」 和 「 錯誤 Windows 錯誤報告在 MultiPoint 伺服器上。 這些由設定**自動安裝更新**並**自動 Windows 錯誤報告**Windows MultiPoint Server 安裝，在 MultiPoint 中設定期間所選取的設定使用管理員**編輯伺服器設定**，或設定在 排程更新磁碟保護。  
  
## <a name="update-the-registry"></a>更新登錄  
MultiPoint 服務的網域部署，您應該更新下列登錄子機碼。  
  
> [!IMPORTANT]  
> 不正確地編輯登錄可能會對系統造成嚴重的損害。 變更登錄之前，您應該先備份電腦所有的重要資料。  
  
#### <a name="to-update-registry-subkeys-for-a-domain-deployment-of-multipoint-services"></a>若要更新網域部署的 MultiPoint 服務的登錄子機碼  
  
1.  開啟登錄編輯程式。 (在命令提示字元中，輸入**regedit.exe**，按 ENTER 鍵。)  
  
2.  在左窗格中，找出，然後選取下列登錄子機碼：  
  
    HKEY_USERS\<SIDofWMSshell>\Software\Policies\Microsoft\Windows\Control Panel\Desktop  
  
    其中 '<SIDofWMSshell>' WMSshell 帳戶的安全性識別碼 (SID)。 若要了解如何識別的 SID，請參閱[如何建立關聯的使用者名稱與安全性識別碼 (SID)](https://support.microsoft.com/kb/154599)。  
  
3.  在右側清單中，更新下列子機碼。  
  
    |子機碼|值名稱|[數值資料]|  
    |----------|--------------|--------------|  
    |ScreenSaveActive|REG_SZ|0 （零）|  
    |ScreenSaveTimeout|REG_SZ|120|  
    |ScreenSaverIsSecure|REG_SZ|0 （零）|  
  
    若要更新的登錄子機碼：  
  
    1.  在左窗格中選取的登錄機碼，以滑鼠右鍵按一下右窗格中的子機碼，然後按一下**修改**。  
  
    2.  在 [編輯字串] 對話方塊中，輸入中的新值**值的資料**，然後按一下**確定**。  
  
4.  完成更新登錄子機碼之後，重新啟動電腦以啟動變更。 