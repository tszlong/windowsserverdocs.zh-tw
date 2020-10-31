---
title: 清除 AD DS server 中繼資料
description: 使用內建工具清除已移除網域控制站的中繼資料
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 11/14/2018
ms.topic: article
ms.openlocfilehash: e8ff94e418da7e306440bc836745d90e9672c2c9
ms.sourcegitcommit: b115e5edc545571b6ff4f42082cc3ed965815ea4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93069090"
---
# <a name="clean-up-active-directory-domain-controller-server-metadata"></a>清除 Active Directory 網域控制器伺服器中繼資料

適用於：Windows Server

Active Directory Domain Services (AD DS) 強制移除之後，中繼資料清除是必要的程式。 您可以在強制移除的網域控制站網域中的網域控制站上執行元資料清理。 中繼資料清除會將識別網域控制站的 AD DS 中的資料移除至複寫系統。 中繼資料清除也會移除檔案複寫服務 (FRS) 和分散式檔案系統 (DFS) 複寫連線，並嘗試傳輸或轉送任何操作主機 (也稱為彈性單一主機操作，或已淘汰網域控制站保存的 FSMO) 角色。

清除伺服器中繼資料有三個選項：

- 使用 GUI 工具清除伺服器中繼資料
- 使用命令列清除伺服器中繼資料
- 使用腳本清除伺服器中繼資料

> [!NOTE]
> 如果您在使用這些方法中的任何一種來執行中繼資料清除時收到「拒絕存取」錯誤，請確定網域控制站的電腦物件和 NTDS 設定物件不會受到保護，以防止意外刪除。 若要確認以滑鼠右鍵按一下 [電腦] 物件或 [NTDS 設定] 物件，請按一下 [內容]，按一下 [ **物件** **]，然後** 清除 [保護物件不被 **意外刪除** ] 核取方塊。 在 [ **Active Directory 消費者和電腦] 中** ，如果您按一下 [ **View** ]，然後按一下 [ **Advanced Features （Advanced Features** ）]，就會顯示物件的

## <a name="clean-up-server-metadata-using-gui-tools"></a>使用 GUI 工具清除伺服器中繼資料

當您使用遠端伺服器管理工具 (RSAT) 或 Active Directory 消費者和電腦主控台 (Windows Server 隨附的) Dsa.msc (，從網域控制站組織單位) OU 刪除網域控制站電腦帳戶時，會自動執行伺服器中繼資料的清除。 在 Windows Server 2008 之前，您必須執行個別的中繼資料清除程式。

您也可以使用 Active Directory 網站和服務] 主控台 (Dssite.msc) 刪除網域控制站的電腦帳戶，這也會自動完成中繼資料清除。 不過，Active Directory 網站和服務只會在您第一次刪除 Dssite.msc 中電腦帳戶下的 NTDS 設定物件時，自動移除中繼資料。

只要您使用的是 Windows Server 2008 或更新版本的 Dsa.msc 或 Dssite.msc，就可以針對執行舊版 Windows 作業系統的網域控制站自動清除中繼資料。

若要完成這些程式，至少需要 **Domain Admins** 的成員資格或同等許可權。

## <a name="clean-up-server-metadata-using-active-directory-users-and-computers"></a>使用 Active Directory 消費者和電腦清除伺服器中繼資料

1. 開啟 [Active Directory 使用者及電腦]。
2. 如果您已找出複寫協力電腦準備進行此程式，且未連線到已移除之網域控制站的複寫協力電腦，且該控制器的中繼資料正在清除，請以滑鼠右鍵按一下 [ **Active Directory 消費者和電腦** 節點]，然後按一下 [ **變更網域控制站** ]。 按一下您要從中移除中繼資料之網域控制站的名稱，然後按一下 **[確定]** 。
3. 展開被強制移除的網域控制站網域，然後按一下 [ **網域控制站** ]。
4. 在詳細資料窗格中，以滑鼠右鍵按一下您要清除其中繼資料之網域控制站的電腦物件，然後按一下 [ **刪除** ]。
5. 在 [ **Active Directory Domain Services** ] 對話方塊中，確認已顯示您想要刪除之網域控制站的名稱，然後按一下 **[是]** 以確認電腦物件刪除。
6. 在 [ **刪除網域控制站** ] 對話方塊中，選取 [ **此網域控制站已永久離線，而且無法再使用 Active Directory Domain Services 安裝精靈 (DCPROMO) 降級** ]，然後按一下 [ **刪除** ]。
7. 如果網域控制站是通用類別目錄伺服器，請在 [ **刪除網域控制站** ] 對話方塊中，按一下 [ **是]** 繼續進行刪除。
8. 如果網域控制站目前保留一或多個操作主機角色，請按一下 **[確定]** ，將角色或角色移至所顯示的網域控制站。 您無法變更此網域控制站。 如果您想要將角色移至不同的網域控制站，您必須在完成伺服器中繼資料清除程式之後移動角色。

## <a name="clean-up-server-metadata-using-active-directory-sites-and-services"></a>使用 Active Directory 網站和服務清除伺服器中繼資料

1. 開啟 [Active Directory 站台及服務]。
2. 如果您已識別要為此程式做好準備的複寫協力電腦，而且如果您未連接到已移除的網域控制站的複寫協力電腦上，請以滑鼠右鍵按一下 [ **Active Directory 的網站和服務** ]，然後按一下 [ **變更網域控制站** ]。 按一下您要從中移除中繼資料之網域控制站的名稱，然後按一下 **[確定]** 。
3. 展開已強制移除之網域控制站的網站，展開 [ **伺服器** ]，展開網域控制站的名稱，以滑鼠右鍵按一下 [NTDS 設定] 物件，然後按一下 [ **刪除** ]。
4. 在 [ **Active Directory 網站和服務** ] 對話方塊中，按一下 [ **是]** 以確認刪除 NTDS 設定。
5. 在 [ **刪除網域控制站** ] 對話方塊中，選取 [ **此網域控制站已永久離線，而且無法再使用 Active Directory Domain Services 安裝精靈 (DCPROMO) 降級** ]，然後按一下 [ **刪除** ]。
6. 如果網域控制站是通用類別目錄伺服器，請在 [ **刪除網域控制站** ] 對話方塊中，按一下 [ **是]** 繼續進行刪除。
7. 如果網域控制站目前保留一或多個操作主機角色，請按一下 **[確定]** ，將角色或角色移至所顯示的網域控制站。
8. 在強制移除的網域控制站上按一下滑鼠右鍵，然後按一下 [刪除]。
9. 在 [ **Active Directory Domain Services** ] 對話方塊中，按一下 [ **是]** 以確認刪除網域控制站。

## <a name="clean-up-server-metadata-using-the-command-line"></a>使用命令列清除伺服器中繼資料

或者，您可以使用 Ntdsutil.exe 來清除中繼資料，此工具會自動安裝在已安裝 Active Directory 輕量型目錄服務 (AD LDS) 的所有網域控制站和伺服器上。 您也可以在已安裝 RSAT 的電腦上使用 Ntdsutil.exe。

## <a name="to-clean-up-server-metadata-by-using-ntdsutil"></a>使用 Ntdsutil 清除伺服器中繼資料

1. 以系統管理員身分開啟命令提示字元：在 [ **開始** ] 功能表上，以滑鼠右鍵按一下 [ **命令提示** 字元]，然後按一下 [以 **系統管理員身分執行** ]。 如果出現 [ **使用者帳戶控制** ] 對話方塊，請視需要提供企業系統管理員的認證，然後按一下 [ **繼續** ]。
2. 在命令提示字元中輸入下列命令，然後按 ENTER：

   `ntdsutil`

3. 在 `ntdsutil:` 提示中輸入下列命令，然後按 ENTER 鍵：

   `metadata cleanup`

4. 在 `metadata cleanup:` 提示中輸入下列命令，然後按 ENTER 鍵：

   `remove selected server <ServerName>`

5. 在 [ **伺服器移除設定] 對話方塊** 中，檢查資訊和警告，然後按一下 [ **是]** ，移除伺服器物件和中繼資料。

   此時，Ntdsutil 確認已成功移除網域控制站。 如果您收到錯誤訊息，指出找不到該物件，則可能是先前已移除網域控制站。

6. 在 `metadata cleanup:` 和 `ntdsutil:` 提示字元中，輸入 `quit` ，然後按 enter。

7. 確認移除網域控制站：

   開啟 [Active Directory 使用者和電腦]。 在已移除網域控制站的網域中，按一下 [ **網域控制站** ]。 在詳細資料窗格中，不應該出現您所移除之網域控制站的物件。

   開啟 [Active Directory 站台及服務]。 流覽至 [ **伺服器** ] 容器，並確認您所移除之網域控制站的伺服器物件未包含 NTDS 設定物件。 如果伺服器物件底下沒有出現任何子物件，您可以刪除伺服器物件。 如果出現子物件，請勿刪除伺服器物件，因為另一個應用程式正在使用物件。

## <a name="see-also"></a>另請參閱

* [降級網域控制站](Demoting-Domain-Controllers-and-Domains--Level-200-.md)
* [Ntdsutil 命令參考](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc753343(v=ws.10))
