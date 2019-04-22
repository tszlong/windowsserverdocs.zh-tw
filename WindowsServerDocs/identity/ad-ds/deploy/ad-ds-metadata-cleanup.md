---
title: 清除 AD DS 伺服器中繼資料
description: 使用內建工具來清除已移除的網域控制站的中繼資料
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 11/14/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: fbb6e720c9289c608d71d3c36695ba623a9df5f6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818079"
---
# <a name="clean-up-active-directory-domain-controller-server-metadata"></a>清除 Active Directory 網域控制站伺服器中繼資料

適用於：Windows Server

中繼資料清理之後強制移除 Active Directory 網域服務 (AD DS) 的必要程序。 您的網域控制站強制移除網域中網域控制站上執行中繼資料清除作業。 中繼資料清除作業會移除識別複寫系統的網域控制站的 AD DS 中的資料。 中繼資料清除作業也會移除檔案複寫服務 (FRS) 和分散式檔案系統 (DFS) 複寫連線，並嘗試轉移或拿取任何操作主機 （也稱為彈性單一主機操作或 FSMO） 角色，已停用的網域控制站會保留。

有三個選項可以清理伺服器中繼資料：

- 使用 GUI 工具來清理伺服器中繼資料
- 清理伺服器中繼資料，使用命令列
- 使用指令碼清理伺服器中繼資料

> [!NOTE]
> 如果您使用任何一種方法來執行中繼資料清除作業時，您會收到 「 拒絕存取 」 錯誤，，請確定電腦物件和網域控制站的 NTDS 設定物件未保護免於遭到意外刪除。 若要電腦物件或 NTDS 設定物件，請確認此按一下滑鼠右鍵，按一下**屬性**，按一下**物件**，並清除**保護物件以防止被意外刪除**核取方塊。 在 Active Directory 使用者和電腦**物件**之物件的索引標籤會出現，如果您按一下**檢視**，然後按一下 **進階功能**。

## <a name="clean-up-server-metadata-using-gui-tools"></a>清理伺服器中繼資料使用 GUI 工具

當您使用遠端伺服器管理工具 (RSAT) 或 Active Directory 使用者和電腦 主控台 (Dsa.msc) 隨附於 Windows Server 網域控制站組織單位 (OU)，從刪除的網域控制站電腦帳戶清理伺服器中繼資料會自動執行。 Windows Server 2008 之前，您必須執行個別的中繼資料清理程序。

您也可以使用 Active Directory 站台及服務 主控台 (Dssite.msc) 若要刪除的網域控制站電腦帳戶，也會自動完成中繼資料清除作業。 不過，Active Directory 站台及服務中繼資料會自動移除只有當您第一次刪除以下 Dssite.msc 中的電腦帳戶的 NTDS 設定物件。

只要您使用 Windows Server 2008 或更新版本的 RSAT 版本，Dsa.msc 或 Dssite.msc，您可以清除會自動為執行舊版 Windows 作業系統的網域控制站的中繼資料。

中的成員資格**Domain Admins**，或同等權限，才能完成這些程序的最小值。

## <a name="clean-up-server-metadata-using-activedirectory-users-and-computers"></a>清理伺服器中繼資料使用 Active Directory 使用者和電腦

1. 開啟 [Active Directory 使用者和電腦] 。
2. 如果您已識別出複寫協力電腦，以準備進行此程序，而且如果您未連接到複寫協力電腦移除的網域控制站中繼資料您清理，以滑鼠右鍵按一下**Active Directory 使用者和電腦**節點，然後再按一下**變更網域控制站**。 按一下您想要移除中繼資料，然後按一下 網域控制站的名稱**確定**。
3. 展開 網域控制站強制移除，然後按一下 網域**網域控制站**。
4. 在詳細資料窗格中，以滑鼠右鍵按一下您想要清除，然後按一下其中繼資料的網域控制站的電腦物件**刪除**。
5. 在  **Active Directory 網域服務**對話方塊方塊中，確認您想要刪除的網域控制站名稱會顯示，並按一下 **是**以確認刪除電腦物件。
6. 在 **刪除的網域控制站**對話方塊中，選取**此網域控制站已永久離線，而且可以不再被降級使用 Active Directory 網域服務安裝精靈 (DCPROMO)**，然後按一下**刪除**。
7. 如果網域控制站是通用類別目錄伺服器，在**刪除的網域控制站** 對話方塊中，按一下**是**若要繼續刪除作業。
8. 如果網域控制站目前保留一或多個操作主機角色，請按一下**確定**移動或多個角色，會顯示網域控制站。 您無法變更此網域控制站。 如果您想要將角色移到不同的網域控制站，您必須將角色移之後完成的伺服器中繼資料清理程序。

## <a name="clean-up-server-metadata-using-activedirectory-sites-and-services"></a>清除 使用 Active Directory 站台和服務的伺服器中繼資料

1. 開啟 [Active Directory 站台及服務]。
2. 如果您已識別出複寫協力電腦，以準備進行此程序，而且如果您未連接到複寫協力電腦移除的網域控制站中繼資料您清理，以滑鼠右鍵按一下**Active Directory 站台及服務**，然後按一下**變更網域控制站**。 按一下您想要移除中繼資料，然後按一下 網域控制站的名稱**確定**。
3. 依序展開 已強制移除網域控制站的站台**伺服器**，展開 網域控制站的名稱、 NTDS 設定物件，以滑鼠右鍵按一下，然後按一下**刪除**。
4. 在 [ **Active Directory 站台及服務**] 對話方塊中，按一下**是**確認 NTDS 設定刪除。
5. 在 **刪除的網域控制站**對話方塊中，選取**此網域控制站已永久離線，而且可以不再被降級使用 Active Directory 網域服務安裝精靈 (DCPROMO)**，然後按一下**刪除**。
6. 如果網域控制站是通用類別目錄伺服器，在**刪除的網域控制站** 對話方塊中，按一下**是**若要繼續刪除作業。
7. 如果網域控制站目前保留一或多個操作主機角色，請按一下**確定**移動或多個角色，會顯示網域控制站。
8. 以滑鼠右鍵按一下 強制移除網域控制站，然後按一下 刪除。
9. 在 [ **Active Directory 網域服務**] 對話方塊中，按一下**是**確認網域控制站的刪除動作。

## <a name="clean-up-server-metadata-using-the-command-line"></a>清理伺服器中繼資料，使用命令列

或者，您可以清除使用 Ntdsutil.exe，會自動安裝在所有網域控制站和 Active Directory 輕量型目錄服務 (AD LDS) 安裝的伺服器的命令列工具的中繼資料。 Ntdsutil.exe 也是在已安裝 RSAT 的電腦上使用。

## <a name="to-clean-up-server-metadata-by-using-ntdsutil"></a>若要使用 Ntdsutil 清理伺服器中繼資料

1. 系統管理員身分開啟命令提示字元：在上**開始**] 功能表中，以滑鼠右鍵按一下**命令提示字元**，然後按一下 [**系統管理員身分執行**。 如果**使用者帳戶控制** 對話方塊出現時，請提供認證的企業系統管理員，如有需要，然後按一下**繼續**。
2. 在命令提示字元中輸入下列命令，然後按 ENTER：

   `ntdsutil`

3. 在 `ntdsutil:` 提示中輸入下列命令，然後按 ENTER 鍵：

   `metadata cleanup`

4. 在 `metadata cleanup:` 提示中輸入下列命令，然後按 ENTER 鍵：

   `remove selected server <ServerName>`

5. 在 **伺服器移除組態對話方塊**檢閱的資訊和警告，然後按一下 **是**若要移除的伺服器物件和中繼資料。

   此時，Ntdsutil 會確認已成功移除網域控制站。 如果您收到錯誤訊息，指出找不到物件時，網域控制站可能已移除稍早。

6. 在`metadata cleanup:`並`ntdsutil:`提示字元中，輸入`quit`，然後按 ENTER 鍵。

7. 若要確認網域控制站移除：

   開啟 Active Directory 使用者和電腦。 在移除的網域控制站的網域中，按一下**網域控制站**。 在 [詳細資料] 窗格中，應該不會顯示網域控制站移除的物件。

   開啟 [Active Directory 站台及服務]。 瀏覽至**伺服器**容器，並確認您所移除的網域控制站的伺服器物件不包含 NTDS 設定物件。 如果沒有任何子物件會出現下方的伺服器物件，您可以刪除伺服器物件。 如果子系物件出現時，請勿刪除伺服器物件，因為另一個應用程式正在使用的物件。

## <a name="see-also"></a>另請參閱

* [降級網域控制站](Demoting-Domain-Controllers-and-Domains--Level-200-.md)
* [Ntdsutil 命令參考](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753343(v=ws.10))
