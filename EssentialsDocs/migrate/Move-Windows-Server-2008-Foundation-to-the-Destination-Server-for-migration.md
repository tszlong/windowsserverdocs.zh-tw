---
title: 將 Windows Server 2008 Foundation 的設定和資料移至目的地伺服器以進行 Windows Server Essentials 移轉
description: 說明如何使用 Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: 3ff7d040-ebd1-421c-80db-765deacedd4c
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 99c7eb77c0bd768cb8e74d476bd2ffcf6b468a53
ms.sourcegitcommit: d99bc78524f1ca287b3e8fc06dba3c915a6e7a24
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/27/2020
ms.locfileid: "87180546"
---
# <a name="move-windows-server-2008-foundation-settings-and-data-to-the-destination-server-for-windows-server-essentials-migration"></a>將 Windows Server 2008 Foundation 的設定和資料移至目的地伺服器以進行 Windows Server Essentials 移轉

>適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials

如下所示將設定和資料移動到目的地伺服器：

1. [將資料複製到目的地伺服器 (選擇性)](#copy-data-to-the-destination-server)

2. [將 Active Directory 使用者帳戶匯入 Windows Server Essentials 儀表板（選擇性）](#import-active-directory-user-accounts-to-the-windows-server-essentials-dashboard)

3. [將 DHCP 伺服器角色從來源伺服器移動到路由器](#move-the-dhcp-server-role-from-the-source-server-to-the-router)

4. [設定網路](#configure-the-network)

5. [將允許的電腦對應到使用者帳戶](#map-permitted-computers-to-user-accounts)

## <a name="copy-data-to-the-destination-server"></a>將資料複製到目的地伺服器
 在您將資料從來源伺服器複製到目的地伺服器之前，請執行下列工作：

- 檢閱來源伺服器上共用資料夾的清單，包括每個資料夾的權限。 建立或自訂目的地伺服器上的資料夾，以符合您從來源伺服器移轉的資料夾結構。

- 檢閱每個資料夾的大小，並確定目的地伺服器具有足夠的儲存空間。

- 針對所有使用者將來源伺服器上的共用資料夾變成唯讀，因此在您將檔案複製到目的伺服器時，磁碟機上不會發生寫入動作。

#### <a name="to-copy-data-from-the-source-server-to-the-destination-server"></a>從來源伺服器將資料複製到目的地伺服器

1.  以網域系統管理員身分登入目的地伺服器，然後開啟命令視窗。

2.  在命令提示字元中輸入下列命令，然後按 ENTER：

    `robocopy \\<SourceServerName> \<SharedSourceFolderName> \\<DestinationServerName> \<SharedDestinationFolderName> /E /B /COPY:DATSOU /LOG:C:\Copyresults.txt`

     其中：
     - \<SourceServerName\> 是來源伺服器的名稱
     - \<SharedSourceFolderName\> 是來源伺服器上共用資料夾的名稱
     - \<DestinationServerName\>這是目的地伺服器的名稱，
     - \<SharedDestinationFolderName\>這是要將資料複製到其中的目的地伺服器上的共用資料夾。

3.  在每一個從來源伺服器移轉的共用資料夾重複上述步驟。

## <a name="import-active-directory-user-accounts-to-the-windows-server-essentials-dashboard"></a>將 Active Directory 使用者帳戶匯入 Windows Server Essentials 儀表板
 根據預設，在來源伺服器上建立的所有使用者帳戶都會自動遷移至 Windows Server Essentials 中的儀表板。 不過，如果所有內容都不符合移轉需求，Active Directory 使用者帳戶的自動移轉將會失敗。 您可以使用以下 Windows PowerShell Cmdlet 匯入 Active Directory 使用者。

#### <a name="to-import-an-active-directory-user-account-to-the-windows-server-essentials-dashboard"></a>將 Active Directory 使用者帳戶匯入 Windows Server Essentials 儀表板

1.  以網域系統管理員身分登入目的地伺服器。

2.  以系統管理員身分開啟 Windows PowerShell。

3.  執行下列的 Cmdlet，其中 `[AD username]` 為您想要匯入的 Active Directory 使用者帳戶名稱：

     `Import-WssUser  SamAccountName [AD username]`

## <a name="move-the-dhcp-server-role-from-the-source-server-to-the-router"></a>將 DHCP 伺服器角色從來源伺服器移動到路由器
 如果您的來源伺服器正在執行 DHCP 角色，請執行下列步驟，將 DHCP 角色移至路由器。

#### <a name="to-move-the-dhcp-role-from-the-source-server-to-the-router"></a>將 DHCP 角色從來源伺服器移動到路由器

1.  如下所示關閉來源伺服器上的 DHCP 服務：

    1.  在來源伺服器上，依序按一下 [開始]****、[系統管理工具]****，然後按一下 [服務]****。

    2.  在目前執行中服務的清單中，以滑鼠右鍵按一下 [DHCP 伺服器]****，然後按一下 [屬性]****。

    3.  於 [啟動類型]**** 選取 [停用]****。

    4.  停止服務。

2.  開啟路由器上的 DHCP 角色。

    1.  依照路由器的文件指示，開啟路由器上的 DHCP 角色。

    2.  若要確保來源伺服器所發出的 IP 位址維持不變，請依照路由器文件中的指示，將路由器上的 DHCP 範圍設定為與來源伺服器上的 DHCP 範圍相同。

        > [!IMPORTANT]
        >  如果您沒有為目的地伺服器的路由器設定靜態 IP 或 DHCP 保留區，且 DHCP 範圍與來源伺服器的 DHCP 範圍不相同，路由器可能為目的地伺服器發出新的 IP 位址。 如果發生這種情況，請重設路由器的連接埠轉送規則，轉送至目的地伺服器的新 IP 位址。

## <a name="configure-the-network"></a>設定網路
 在您將 DHCP 角色移動到路由器之後，請在目的地伺服器上設定網路設定。

#### <a name="to-configure-the-network"></a>若要設定網路

1. 在目的地伺服器上，開啟儀表板。

2. 在 [儀表板] 的 [首頁]**** 頁面上，按一下 [設定]****，按一下 [設定隨處存取]****，然後選擇 [按一下以設定隨處存取]**** 選項。

3. 完成精靈中的指示，來設定您的路由器及網域名稱。

   如果您的路由器不支援 UPnP 架構，或已停用 UPnP 架構，路由器名稱旁邊可能會出現一個黃色警告圖示。 請確定下列連接埠已開啟，且會導向到目的地伺服器的 IP 位址：

-   連接埠 80：HTTP 網路流量

-   連接埠 443：HTTP 網路流量

## <a name="map-permitted-computers-to-user-accounts"></a>將允許的電腦對應到使用者帳戶
 在 Windows Server Essentials 中，必須明確地將使用者指派給電腦，才會顯示在遠端 Web 存取。 從 Windows Server 2008 Foundation 移轉的每個使用者帳戶都必須對應至一或多部電腦。

#### <a name="to-map-user-accounts-to-computers"></a>將使用者帳戶對應至電腦

1.  開啟 [Windows Server Essentials 儀表板]。

2.  在瀏覽列中，按一下 [使用者]****。

3.  在使用者帳戶清單中，以滑鼠右鍵按一下使用者帳戶，然後按一下 [檢視帳戶內容]****。

4.  按一下 [隨處存取]**** 索引標籤，然後按一下 [允許遠端 Web 存取，以及存取 Web 服務應用程式]****。

5.  選取 [共用資料夾]****，選取 [電腦]****、[首頁連結]****，然後按一下 [套用]****。

6.  按一下 [電腦存取]**** 索引標籤，然後再按一下您想要允許存取的電腦名稱。

7.  對每個使用者帳戶重複步驟 3、4、5 和 6。

> [!NOTE]
> 您不需要變更用戶端電腦的設定。 它會自動設定。
>
> 完成移轉之後，如果您在目的伺服器上建立第一個新的使用者帳戶時遇到問題，請移除您新增的使用者帳戶，然後再重新建立一次。
