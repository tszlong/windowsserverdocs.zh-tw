---
title: 清理 AD DS 的伺服器中繼資料
description: 使用內建工具來清除已移除網域控制站的中繼資料
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 11/14/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 622ff33437a3aef14a185c9a4157dba68db0a2ee
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80824782"
---
# <a name="clean-up-active-directory-domain-controller-server-metadata"></a>清理 Active Directory 網域控制器伺服器中繼資料

適用於︰Windows Server

在強制移除 Active Directory Domain Services （AD DS）之後，中繼資料清除是必要的程式。 您會在您強制移除的網域控制站網域中的網域控制站上執行中繼資料清除。 中繼資料清除會從識別網域控制站至複寫系統的 AD DS 中移除資料。 中繼資料清除也會移除檔案複寫服務（FRS）和分散式檔案系統（DFS）複寫連線，並嘗試傳輸或收回已淘汰的網域控制站所保有的任何操作主機（也稱為彈性單一主機操作或 FSMO）角色。

有三個選項可清除伺服器中繼資料：

- 使用 GUI 工具清理伺服器中繼資料
- 使用命令列清除伺服器中繼資料
- 使用腳本清除伺服器中繼資料

> [!NOTE]
> 如果您在使用其中任何一種方法來執行中繼資料清除時收到「拒絕存取」錯誤，請確定網域控制站的電腦物件和 NTDS 設定物件未受到保護，不會遭到意外刪除。 若要確認此操作，請以滑鼠右鍵按一下 [電腦] 物件或 [NTDS 設定] 物件 **，按一下 [** 內容]，按一下 [**物件**]，然後清除 [保護物件不被**意外刪除**] 核取方塊。 在 Active Directory 使用者和電腦 中，如果您按一下  **View** ，然後按一下  **Advanced Features**，就會出現物件的 **物件** 索引標籤

## <a name="clean-up-server-metadata-using-gui-tools"></a>使用 GUI 工具清理伺服器中繼資料

當您使用 Windows Server 隨附的 [遠端伺服器管理工具（RSAT）] 或 [Active Directory 使用者和電腦] 主控台（Dsa.msc），從網域控制站的組織單位（OU）中刪除網域控制站電腦帳戶時，會自動執行伺服器中繼資料的清除。 在 Windows Server 2008 之前，您必須執行個別的中繼資料清除程式。

您也可以使用 Active Directory Sites 和服務主控台（Dssite.msc）來刪除網域控制站的電腦帳戶，這也會自動完成中繼資料清除。 不過，只有當您第一次刪除 Dssite.msc 中電腦帳戶底下的 NTDS 設定物件時，Active Directory 網站和服務才會自動移除中繼資料。

只要您使用的是 Windows Server 2008 或更新版的 Dsa.msc 或 Dssite.msc，就可以針對執行舊版 Windows 作業系統的網域控制站自動清除中繼資料。

若要完成這些程式，至少需要**Domain Admins**的成員資格或同等許可權。

## <a name="clean-up-server-metadata-using-activedirectory-users-and-computers"></a>使用 Active Directory 使用者和電腦來清除伺服器中繼資料

1. 開啟 [Active Directory 使用者和電腦]。
2. 如果您已找出複寫協力電腦以準備進行此程式，而且如果您未連線到已移除之網域控制站的複寫協力電腦（您正在清除其中繼資料），請在 [ **Active Directory 使用者和電腦**] 節點上按一下滑鼠右鍵，然後按一下 [**變更網域控制站**]。 按一下您要移除中繼資料之網域控制站的名稱，然後按一下 **[確定]** 。
3. 展開已強制移除之網域控制站的網域，然後按一下 [**網域控制站**]。
4. 在詳細資料窗格中，以滑鼠右鍵按一下您要清除其中繼資料之網域控制站的電腦物件，然後按一下 [**刪除**]。
5. 在 [ **Active Directory Domain Services** ] 對話方塊中，確認您想要刪除的網域控制站名稱已顯示，然後按一下 **[是]** 確認電腦物件刪除。
6. 在 [**刪除網域控制站**] 對話方塊中，選取 [**此網域控制站已永久離線，而且無法再使用 Active Directory Domain Services 安裝精靈（DCPROMO）降級**]，然後按一下 [**刪除**]。
7. 如果網域控制站是通用類別目錄伺服器，請在 [**刪除網域控制站**] 對話方塊中，按一下 [**是]** 繼續刪除作業。
8. 如果網域控制站目前擁有一或多個操作主機角色，請按一下 **[確定]** ，將角色或角色移到顯示的網域控制站。 您無法變更此網域控制站。 如果您想要將角色移至不同的網域控制站，您必須在完成伺服器中繼資料清除程式後移動角色。

## <a name="clean-up-server-metadata-using-activedirectory-sites-and-services"></a>使用 Active Directory 的網站和服務清理伺服器中繼資料

1. 開啟 [Active Directory 站台及服務]。
2. 如果您已找出複寫協力電腦以準備進行此程式，而且如果您未連線到已移除之網域控制站的複寫協力電腦，請以滑鼠右鍵按一下 [ **Active Directory 的網站和服務**]，然後按一下 [**變更網域控制站**]。 按一下您要移除中繼資料之網域控制站的名稱，然後按一下 **[確定]** 。
3. 展開已強制移除之網域控制站的網站，展開 [**伺服器**]，展開網域控制站的名稱，在 [NTDS 設定] 物件上按一下滑鼠右鍵，然後按一下 [**刪除**]。
4. 在 [ **Active Directory 網站和服務**] 對話方塊中，按一下 [**是]** 確認刪除 NTDS 設定。
5. 在 [**刪除網域控制站**] 對話方塊中，選取 [**此網域控制站已永久離線，而且無法再使用 Active Directory Domain Services 安裝精靈（DCPROMO）降級**]，然後按一下 [**刪除**]。
6. 如果網域控制站是通用類別目錄伺服器，請在 [**刪除網域控制站**] 對話方塊中，按一下 [**是]** 繼續刪除作業。
7. 如果網域控制站目前擁有一或多個操作主機角色，請按一下 **[確定]** ，將角色或角色移到顯示的網域控制站。
8. 以滑鼠右鍵按一下已強制移除的網域控制站，然後按一下 [刪除]。
9. 在 [ **Active Directory Domain Services** ] 對話方塊中，按一下 [**是]** 確認刪除網域控制站。

## <a name="clean-up-server-metadata-using-the-command-line"></a>使用命令列清除伺服器中繼資料

或者，您可以使用 Ntdsutil.exe 來清除中繼資料，此命令列工具會自動安裝在已安裝 Active Directory 輕量型目錄服務（AD LDS）的所有網域控制站和伺服器上。 Ntdsutil.exe 也可以在已安裝 RSAT 的電腦上使用。

## <a name="to-clean-up-server-metadata-by-using-ntdsutil"></a>使用 Ntdsutil 清理伺服器中繼資料

1. 以系統管理員身分開啟命令提示字元：在 [**開始**] 功能表上，以滑鼠右鍵按一下 [**命令提示**字元]，然後按一下 [以**系統管理員身分執行**]。 如果出現 [**使用者帳戶控制**] 對話方塊，請視需要提供企業系統管理員的認證，然後按一下 [**繼續**]。
2. 在命令提示字元，輸入下列命令，然後按 ENTER：

   `ntdsutil`

3. 在 `ntdsutil:` 提示中輸入下列命令，然後按 ENTER 鍵：

   `metadata cleanup`

4. 在 `metadata cleanup:` 提示中輸入下列命令，然後按 ENTER 鍵：

   `remove selected server <ServerName>`

5. 在 [**伺服器移除設定] 對話方塊**中，檢查資訊和警告，然後按一下 **[是]** 以移除伺服器物件和中繼資料。

   此時，Ntdsutil 會確認已成功移除網域控制站。 如果您收到錯誤訊息，指出找不到該物件，可能是先前已移除網域控制站。

6. 在 `metadata cleanup:` 並 `ntdsutil:` 提示中，輸入 `quit`，然後按 ENTER。

7. 若要確認移除網域控制站：

   開啟 Active Directory 使用者和電腦。 在已移除網域控制站的網域中，按一下 [**網域控制站**]。 在詳細資料窗格中，您所移除之網域控制站的物件不應該出現。

   開啟 Active Directory 網站和服務。 流覽至 [**伺服器**] 容器，並確認您所移除之網域控制站的伺服器物件未包含 [NTDS 設定] 物件。 如果伺服器物件底下沒有出現任何子物件，您可以刪除伺服器物件。 如果出現子物件，請不要刪除伺服器物件，因為另一個應用程式正在使用該物件。

## <a name="see-also"></a>另請參閱

* [降級網域控制站](Demoting-Domain-Controllers-and-Domains--Level-200-.md)
* [Ntdsutil 命令參考](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753343(v=ws.10))
