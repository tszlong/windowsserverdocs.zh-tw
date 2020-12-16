---
description: '深入瞭解：安裝新的 Windows Server 2012 Active Directory 樹系 (層級 200) '
ms.assetid: b3d6fb87-c4d4-451c-b3de-a53d2402d295
title: 安裝新的 Windows Server 2012 Active Directory 樹系 (等級 200)
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 96e9ee67b60c6c2e2125e8c518a70bdde7a9a863
ms.sourcegitcommit: 6fbe337587050300e90340f9aa3e899ff5ce1028
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/16/2020
ms.locfileid: "97599791"
---
# <a name="install-a-new-windows-server-2012-active-directory-forest-level-200"></a>安裝新的 Windows Server 2012 Active Directory 樹系 (等級 200)

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主題簡單說明新的 Windows Server 2012 Active Directory 網域服務網域控制站升級功能。 在 Windows Server 2012 中，AD DS 以伺服器管理員和以 Windows PowerShell 為基礎的部署系統取代了 Dcpromo 工具。

-   [Active Directory 網域服務簡化的系統管理](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md#BKMK_SimplifiedAdmin)

-   [技術總覽](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md#BKMK_TechOverview)

-   [使用 [伺服器管理員] 部署樹系](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md#BKMK_SMForest)

-   [使用 Windows PowerShell 部署樹系](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md#BKMK_PSForest)

## <a name="active-directory-domain-services-simplified-administration"></a><a name="BKMK_SimplifiedAdmin"></a>Active Directory 網域服務簡化的系統管理
Windows Server 2012 引進新一代的 Active Directory 網域服務簡化的系統管理，而且是自 Windows 2000 Server 以來最基本的網域重新構想。 AD DS 簡化的系統管理參考 Active Directory 十二年的經驗，為架構設計人員和系統管理員提供更耐久、更有彈性、更直覺的系統管理經驗。 這意味著以現有技術建立新版本，以及擴充 Windows Server 2008 R2 中所發行元件的功能。

### <a name="what-is-ad-ds-simplified-administration"></a>什麼是 AD DS 簡化的系統管理？
AD DS 簡化的系統管理是網域部署的重新構思。 其中的一些功能包括：

-   AD DS 角色部署現在是新伺服器管理員架構的一部分，並允許遠端安裝。

-   AD DS 部署和設定引擎現在是 Windows PowerShell，即使是使用圖形化設定也一樣。

-   升級現在包含先決條件檢查，可驗證樹系和網域對新網域控制站的整備度，以減少升級失敗的機會。

-   Windows Server 2012 樹系功能等級不會實作新功能，而且只有新的 Kerberos 功能子集需要網域功能等級，因此系統管理員便不需經常使用同質性的網域控制站環境。

### <a name="purpose-and-benefits"></a>用途與優點
這些變更看起來可能更複雜，而非更簡單。 但是在重新設計的 AD DS 部署程序方面，有機會將許多步驟和最佳做法合併為更少、更簡單的動作。 例如，這表示新的複本網域控制站的圖形化設定現在是八個對話方塊，而不是先前的 12 個。 建立新的 Active Directory 樹系需要只含 *一個* 引數 (也就是網域名稱) 的 *單一* Windows PowerShell 命令。

為什麼在 Windows Server 2012 中會如此強調 Windows PowerShell 呢？ 隨著分散式運算的發展，Windows PowerShell 可以讓單一引擎從圖形化介面和和命令列介面設定和維護。 它可允許以等同 API 授予開發人員的最高級 IT 專家成員資格，對任一元件執行功能齊全的指令碼處理。 隨著雲端運算的普及，Windows PowerShell 也終於能夠從遠端管理伺服器，讓沒有圖形化介面的電腦和有監視器及滑鼠的電腦具有相同的管理功能。

資深的 AD DS 系統管理員應該會發現與其先前的知識有高度相關性。 初學的系統管理員將發現學習難度不高。

## <a name="technical-overview"></a><a name="BKMK_TechOverview"></a>技術總覽

### <a name="what-you-should-know-before-you-begin"></a>開始之前的需知
本主題假設您已熟悉舊版 Active Directory 網域服務，因此不會提供關於它們的用途與功能的基本詳細資訊。 如需 AD DS 的詳細資訊，請參閱下面連結的 TechNet 入口網站頁面：

-   [Windows Server 2008 R2 的 Active Directory 網域服務](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd378801(v=ws.10))

-   [適用於 Windows Server 2008 的 Active Directory 網域服務](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd378891(v=ws.10))

-   [Windows Server 技術參考資料](/previous-versions/windows/it-pro/windows-server-2003/cc739127(v=ws.10))

### <a name="functional-descriptions"></a>功能描述

#### <a name="ad-ds-role-installation"></a>AD DS 角色安裝
![顯示 [新增角色及功能] wizard 中 [伺服器角色] 頁面的螢幕擷取畫面。](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_SelectServerRoles.gif)

Active Directory 網域服務安裝使用伺服器管理員與 Windows PowerShell，就像 Windows Server 2012 中的其他所有伺服器角色與功能一樣。 Dcpromo.exe 程式不再提供 GUI 設定選項。

您可以在本機和遠端安裝中使用伺服器管理員中的圖形化精靈或 Windows PowerShell 的 ServerManager 模組。 透過執行多個精靈的執行個體或 Cmdlet 和以不同的伺服器為目標，您可以從單一主控台同時將 AD DS 部署到多個網域控制站。 雖然這些新功能不相容於 Windows Server 2008 R2 或舊版作業系統，您也依然可以使用 Windows Server 2008 R2 中引進的 Dism.exe 應用程式，透過傳統的命令列來安裝本機角色。

![顯示 Windows PowerShell 終端機視窗的螢幕擷取畫面。](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSAddWindowsFeature.png)

#### <a name="ad-ds-role-configuration"></a>AD DS 角色設定
![顯示 [Active Directory Domain Services 設定] 頁面中 [部署設定] 頁面的螢幕擷取畫面。](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_DeploymentConfiguration_Forest.gif)

Active Directory Domain Services 設定「先前稱為 DCPROMO」現在是角色安裝的獨立作業。 安裝 AD DS 角色後，系統管理員要使用伺服器管理員中獨立的精靈或使用 ADDSDeployment Windows PowerShell 模組，將伺服器設定為網域控制站。

AD DS 角色設定有十二年的實際經驗做基礎，現在可依據最新的 Microsoft 最佳做法來設定網域控制站。 例如，網域名稱系統和通用類別目錄預設會安裝在每個網域控制站。

伺服器管理員 AD DS configuration wizard 會將許多個別的對話合併為較少的提示，而且不會再隱藏「advanced」模式中的設定。 安裝期間，整個升級程序是在一個擴充的對話方塊中進行。 精靈和 ADDSDeployment Windows PowerShell 模組會顯示值得注意的變更與安全性考量，以及進一步資訊的連結。

Dcpromo.exe 保留在 Windows Server 2012 中只是為了執行命令列的自動安裝，但不會再執行圖形化安裝精靈。 強烈建議您不要繼續使用 Dcpromo.exe 進行自動安裝，而是改用 ADDSDeployment 模組來替代，因為這個現已被取代的可執行檔不會包含在下一版的 Windows 中。

這些新功能不相容於 Windows Server 2008 R2 或舊版作業系統。

![在安裝期間顯示 Windows PowerShell 終端機視窗的螢幕擷取畫面。](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSInstallADDSForest.png)

> [!IMPORTANT]
> Dcpromo.exe 不再包含圖形化精靈，因而不會再安裝角色或功能二進位檔。 嘗試透過 Explorer Shell 執行 Dcpromo.exe 會傳回：
>
> 「Active Directory Domain Services 安裝精靈在伺服器管理員重新置放。 如需詳細資訊，請參閱 <https://go.microsoft.com/fwlink/?LinkId=220921> 。
>
> 與在舊版的作業系統中一樣，嘗試執行 Dcpromo.exe /unattend 仍會安裝二進位檔，但會發出警告：
>
> 「Dcpromo 自動作業已由 Windows PowerShell 的 ADDSDeployment 模組取代。 如需詳細資訊，請參閱 <https://go.microsoft.com/fwlink/?LinkId=220924> 。
>
> dcpromo.exe 在 Windows Server 2012 中是不建議使用的功能 ，因此它將不會包含在未來的 Windows 版本中，在此作業系統中也不會有進一步的增強。 系統管理員應停止繼續使用它，如果他們想透過命令列建立網域控制站，應改用支援的 Windows PowerShell 模組。

#### <a name="prerequisite-checking"></a>先決條件檢查
在繼續升級網域控制站之前，網域控制站設定也會實作評估樹系和網域的先決條件檢查階段。 這包括 FSMO 角色可用性、使用者權限、延伸結構描述相容性及其他需求。 這個新設計可減少網域控制站開始升級後因嚴重的設定錯誤而中途停止的問題。 這可減少樹系中有孤立的網域控制站中繼資料或伺服器誤認其為網域控制站的機會。

## <a name="deploying-a-forest-with-server-manager"></a><a name="BKMK_SMForest"></a>使用 [伺服器管理員] 部署樹系
本節說明如何使用圖形化的 Windows Server 2012 電腦上的伺服器管理員，在樹系根網域中安裝第一個網域控制站。

### <a name="server-manager-ad-ds-role-installation-process"></a>伺服器管理員 AD DS 角色安裝程序
下圖說明 Active Directory 網域服務角色安裝程序，從您執行 ServerManager.exe 開始，到網域控制站升級前結束。

![說明 Active Directory Domain Services 角色安裝程式的圖表，從執行 ServerManager.exe 開始，並在網域控制站升級前結束。](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/adds_servermanagerdeployment.png)

#### <a name="server-pool-and-add-roles"></a>伺服器集區與新增角色
任何可從執行伺服器管理員的電腦存取的 Windows Server 2012 電腦都有資格加入集區。 加入集區後，您要在伺服器管理員中選取要遠端安裝 AD DS 的伺服器，或執行任何其他設定選項。

如果要新增伺服器，請選擇下列其中一項：

-   按一下儀表板歡迎使用磚上的 [新增其他要管理的伺服器]

-   按一下 [管理] 功能表，然後選取 [新增伺服器]

-   以滑鼠右鍵按一下 [所有伺服器]，選擇 [新增伺服器]

[新增伺服器] 對話方塊隨即開啟：

![顯示 [新增伺服器] 對話方塊中 [Active Directory] 索引標籤的螢幕擷取畫面。](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_AddServers.png)

其中提供三種方式，供您將伺服器新增至集區以使用或分組：

-   Active Directory 搜尋 (使用 LDAP，電腦必須屬於網域，允許作業系統篩選且支援萬用字元)

-   DNS 搜尋 (透過 ARP 或 NetBIOS 廣播或 WINS 查詢，使用 DNS 別名或 IP 位址，不允許作業系統篩選或支援萬用字元)

-   匯入 (使用以 CR/LF 區隔的伺服器文字檔清單)

按一下 [立即尋找] 即可傳回該電腦加入的 Active Directory 網域中的伺服器清單，請從伺服器清單中按一下一或多個伺服器名稱。 按一下向右箭號，將伺服器新增到 [已選取] 清單。 使用 [新增伺服器] 對話方塊將選取的伺服器新增至儀表板角色群組。 或按一下 [管理]，然後按一下 [建立伺服器群組]，或按一下儀表板上 [歡迎使用伺服器管理員] 磚上的 [建立伺服器群組] 來建立自訂的伺服器群組。

> [!NOTE]
> 新增伺服器程序不會驗證伺服器是上線或可存取狀態。 不過，任何無法連線的伺服器在下次重新整理時將於伺服器管理員的 [管理性] 檢視中加上旗標。

您可以在加入集區的任一部 Windows Server 2012 電腦上遠端安裝角色，如下所示：

![顯示如何在已新增至集區的任何 Windows Server 2012 電腦上遠端安裝角色的螢幕擷取畫面。](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/tADDS_SMI_TR_AddRolesFeatures.png)

您無法完整管理作業系統比 Windows Server 2012 舊的伺服器。 [新增角色及功能] 選取項目執行的是 ServerManager Windows PowerShell 模組 **Install-WindowsFeature**。

![顯示 [將 AD DS 加入至另一個伺服器] 功能表選項的螢幕擷取畫面。](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_AddADDSToAnotherServer.png)

您也可以使用現有的網域控制站上的 [伺服器管理員儀表板] 來選取已預先選取好角色的遠端伺服器 AD DS 安裝，方法是在 AD DS 儀表板磚上按一下滑鼠右鍵，然後選取 [將 AD DS 新增至另一部伺服器]。 這樣會叫用 **Install-WindowsFeature AD-Domain-Services**。

執行伺服器管理員的電腦會自行加入集區。 如果要在此安裝 AD DS 角色，只要按一下 [管理] 功能表，再按一下 [新增角色及功能]。

![顯示如何存取 [新增角色及功能] 功能表選項的螢幕擷取畫面。](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ManageAddRoles.png)

#### <a name="installation-type"></a>安裝類型
![螢幕擷取畫面，顯示 [新增角色及功能] Wizard 中的 [安裝類型] 頁面。](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_SelectInstallationType.png)

[安裝類型] 對話方塊提供不支援 Active Directory 網域服務的選項：[遠端桌面服務案例型安裝]。 該選項只允許在多伺服器分散式工作負載中的遠端桌面服務。 如果您選取該選項，AD DS 則無法安裝。

安裝 AD DS 時，請始終保留預設的選取項目：[角色型或功能型安裝] 。

#### <a name="server-selection"></a>伺服器選項
![螢幕擷取畫面，顯示 [移除角色及功能] Wizard 中的 [伺服器選擇] 頁面。](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_SelectDestinationServer.png)

[伺服器選取項目] 對話方塊可讓您從先前已加入集區的伺服器中選擇其中之一 (只要其可供存取)。 執行伺服器管理員的本機伺服器會自動成為可存取狀態。

此外，您可以選取離線 Hyper-V VHD 檔案與 Windows Server 2012 作業系統，伺服器管理員會透過元件服務直接新增角色。 這可讓您在進一步設定虛擬伺服器之前，先使用必要的元件佈建虛擬伺服器。

#### <a name="server-roles-and-features"></a>伺服器角色與功能
![顯示 [新增角色及功能] Wizard 中 [伺服器角色] 頁面的螢幕擷取畫面。](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_SelectServerRoles.png)

如果您想升級網域控制站，請選取 [Active Directory 網域服務] 角色。 所有 Active Directory 管理功能和必要服務都會自動安裝 (即使它們明顯是另一個角色的一部分或在 [伺服器管理員] 介面中不是已選取狀態也一樣)。

伺服器管理員也會提供一個資訊對話方塊，其中顯示此角色以隱含方式安裝的管理功能；這等同於 **-IncludeManagementTools** 引數。

![顯示此角色會隱含安裝哪些管理功能的螢幕擷取畫面;這相當於-IncludeManagementTools 引數。](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_AddFeaturesDialog.gif)

![顯示 [新增角色及功能] Wizard 中 [功能] 頁面的螢幕擷取畫面。](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_SelectFeatures.png)

您可以視需要在此新增其他 [功能]。

#### <a name="active-directory-domain-services"></a>Active Directory 網域服務
![顯示 [移除角色及功能] Wizard 中 AD DS 頁面的螢幕擷取畫面。](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ADDSIntro.png)

[Active Directory 網域服務] 對話方塊提供有限的需求及最佳做法資訊。 它主要是做為確認您選擇的 AD DS 角色」。如果此畫面未出現，表示您未選取 AD DS。

#### <a name="confirmation"></a>確認
![顯示 [新增角色及功能] Wizard 中 [確認] 頁面的螢幕擷取畫面。](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_Confirmation.png)

在角色安裝開始之前，[確認] 對話方塊是最後的檢查點。 它提供在角色安裝後視需要重新啟動電腦的選項，但 AD DS 安裝不需要重新開機。

透過按一下 [安裝]，您可以確認您已經準備好開始安裝角色。 開始安裝角色後即無法取消。

#### <a name="results"></a>結果
![顯示 [新增角色及功能] Wizard 中 [結果] 頁面的螢幕擷取畫面。](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_Results.png)

[結果] 對話方塊顯示目前的安裝進度和目前的安裝狀態。 無論伺服器管理員是否關閉，都會繼續安裝角色。

驗證安裝結果仍是最佳做法。 如果您在安裝完成之前關閉 [結果] 對話方塊，您可以使用伺服器管理員通知旗標來檢查結果。 針對任何已安裝 AD DS 角色但未進一步設定為網域控制站的伺服器，伺服器管理員也會顯示警告訊息。

**工作通知**

![顯示工作通知的螢幕擷取畫面。](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_TaskNotofications.png)

**AD DS 詳細資料**

![顯示 AD DS 詳細資料之位置的螢幕擷取畫面。](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ADDSDetails.png)

**工作詳細資料**

![顯示要在何處查看工作詳細資料的螢幕擷取畫面。](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_TaskDetails.png)

#### <a name="promote-to-domain-controller"></a>升級成網域控制站
![顯示 [將這部伺服器升級為網域控制站] 連結的螢幕擷取畫面。](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_Promote.png)

在 AD DS 角色安裝結束時，您可以繼續使用 [將此伺服器升級為網域控制站] 連結來設定。 要使伺服器成為網域控制站，這是必要的動作，但不需要立即執行設定精靈。 例如，您可能只想先使用 AD DS 二進位檔佈建伺服器，再將它們送到另一個分公司進行後續設定。 在運送前新增 AD DS 角色，到了目的地便能節省時間。 您也必須遵守勿使網域控制站離線數天或數週的最佳做法。 最後，這可讓您在升級網域控制站之前更新元件，至少讓您少一次後續重新開機的動作。

稍後選取此連結會叫用 ADDSDeployment Cmdlet：**install-addsforest**、**install-addsdomain** 或 **install-addsdomaincontroller**。

### <a name="uninstallingdisabling"></a>解除安裝/停用
無論您是否已將伺服器升級為網域控制站，移除 AD DS 角色的方法和任何其他的角色一樣。 不過，移除 AD DS 角色需要在完成時重新啟動電腦。

Active Directory 網域服務角色移除不同於安裝，它需要先將網域控制站降級才能完成。 這是為了避免網域控制站在未適當清除樹系中的中繼資料的情況下便解除安裝其角色二進位檔。 如需詳細資訊，請參閱 [降級網域控制站和網域 &#40;層級 200&#41;](../../ad-ds/deploy/Demoting-Domain-Controllers-and-Domains--Level-200-.md)。

> [!WARNING]
> 不支援在升級為網域控制站之後使用 Dism.exe 或 Windows PowerShell DISM 模組移除 AD DS 角色，這樣做將會導致伺服器無法正常開機。
>
> 有別於伺服器管理員或 Windows PowerShell 的 AD DS 部署模組，DISM 是原生的服務系統，不會知道固有的 AD DS 或其設定。 除非伺服器不再是網域控制站，否則請勿使用 Dism.exe 或 Windows PowerShell DISM 模組來解除安裝 AD DS 角色。

### <a name="create-an-ad-ds-forest-root-domain-with-server-manager"></a>使用伺服器管理員建立 AD DS 樹系根網域
下圖說明 Active Directory 網域服務設定程序，在案例中，您先前已安裝過 AD DS 角色，並使用伺服器管理員啟動 [Active Directory 網域服務設定精靈]。

![說明 Active Directory Domain Services 設定程式的圖表，在您先前已安裝 AD DS 角色並使用伺服器管理員啟動 Active Directory Domain Services 設定向導的情況下。 ](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/adds_forestdeploy2.png)

#### <a name="deployment-configuration"></a>部署組態
![顯示部署設定的螢幕擷取畫面。](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_AddNewForest.png)

[伺服器管理員] 會從 [部署設定] 頁面開始升級每個網域控制站。 這個頁面及後續頁面的剩餘選項及必要欄位會隨著您選取的部署操作而變更。

如果要建立新的 Active Directory 樹系，請按一下 [新增樹系]。 您必須提供有效的根網域名稱。名稱不可以是單一標籤 (例如，名稱必須是 *contoso.com* 或類似格式，不能只有 *contoso*)，而且必須使用允許的 DNS 網域命名需求。

如需有效網域名稱的詳細資訊，請參閱知識庫文章 [Active Directory 中的電腦、網域、網站及 OU 的命名慣例](https://support.microsoft.com/kb/909264)。

> [!WARNING]
> 請勿使用與外部 DNS 名稱相同的名稱來建立新的 Active Directory 樹系。 例如，如果您的網際網路 DNS URL 是 http://contoso.com ，您必須為您的內部樹系選擇不同的名稱，以避免未來的相容性問題。 這個名稱必須是唯一而且不像是會用在網路流量的名稱。 例如：corp.contoso.com。

新樹系的網域系統管理員帳戶不需要新的認證。 網域控制站升級程序會使用用來建立樹系根的第一個網域控制站內建的 Administrator 帳戶的認證。 沒有任何方法 (依預設) 可停用或鎖定內建的 Administrator 帳戶，如果其他系統管理網域帳戶無法使用，它可能是樹系唯一的進入點。 在部署新樹系之前，請務必知道密碼。

**DomainName** 需要有效的完整網域 DNS 名稱，而且是必須的。

#### <a name="domain-controller-options"></a>網域控制站選項
![螢幕擷取畫面，顯示 Active Directory Domain Services 設定向導中的網域控制站選項。](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_DCOptions_Forest.gif)

[網域控制站選項] 可讓您設定新的樹系根網域的 [樹系功能等級] 和 [網域功能等級]。 根據預設，這些設定是新樹系根域中的 Windows Server 2012。 Windows Server 2012 樹系功能等級不提供 Windows Server 2008 R2 樹系功能等級的任何新功能。 只有在執行新的 Kerberos 設定「永遠提供宣告」和「未受防護驗證要求失敗」時，才需要 Windows Server 2012 網域功能等級。 Windows Server 2012 中功能等級的主要用途是將網域的參與限制為符合最低允許作業系統需求的網域控制站。 換句話說，您可以指定 Windows Server 2012 網域功能等級，只有執行 Windows Server 2012 的網域控制站可以裝載網域。  Windows Server 2012 在 NetLogon 的 **DSGetDcName** 函式中，會將稱為 **DS_WIN8_REQUIRED** 的新網域控制站旗標，以獨佔方式找出 Windows Server 2012 網域控制站。 就何種作業系統可在網域控制站上執行而言，這可讓您彈性擁有更同質或異質的樹系。

如需網域控制站定位的詳細資訊，請檢閱 [目錄服務功能](/windows/win32/ad/directory-service-functions)。

唯一一個可設定的網域控制站功能為 DNS 伺服器選項。 Microsoft 建議分散式環境中的所有網域控制站都提供 DNS 服務以獲得高可用性，這就是為什麼在任何模式或網域中安裝網域控制站時，預設會選取此選項。 建立新的樹系根網域時，通用類別目錄和唯讀的網域控制站選項都無法使用；第一個網域控制站必須是 GC，不可以是唯讀的網域控制站 (RODC)。

指定的 [目錄服務還原模式密碼] 必須遵守套用至伺服器的密碼原則，預設不需為強式密碼；只需是非空白密碼。 務必選擇複雜的強式密碼，或者最好是使用複雜密碼。

#### <a name="dns-options-and-dns-delegation-credentials"></a>DNS 選項與 DNS 委派認證
![螢幕擷取畫面，顯示 Active Directory Domain Services 設定向導中的 DNS 選項。](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestDNSOptions.png)

[DNS 選項] 頁面可讓您設定 DNS 委派，並提供替代的 DNS 系統管理認證。

安裝新的 Active Directory 樹系根網域時，若在 [網域控制站選項] 頁面上選取 [DNS 伺服器]，則您無法在 Active Directory 網域服務設定精靈中設定 DNS 選項或委派 。 在現有的 DNS 伺服器基礎結構中建立新的樹系根 DNS 區域時，會提供 [建立 DNS 委派] 選項。 此選項可讓您提供替代的 DNS 系統管理認證，擁有更新 DNS 區域的權限。

如需是否需要建立 DNS 委派的詳細資訊，請參閱[了解區域委派](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771640(v=ws.11))。

#### <a name="additional-options"></a>其他選項
![顯示 Active Directory Domain Services 設定向導中 [其他選項] 頁面的螢幕擷取畫面。](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestAdditionalOptions.png)

[其他選項] 頁面會顯示網域的 NetBIOS 名稱，並可讓您覆寫它。 根據預設，NetBIOS 網域名稱符合 [部署設定] 頁面所提供之完整網域名稱最左邊的標籤。 例如，如果您提供的完整的網域名稱是 corp.contoso.com，則預設的 NetBIOS 網域名稱是 CORP。

如果名稱為 15 個字元或更短，且不與其他 NetBIOS 名稱衝突，則名稱不變。 如果名稱與其他 NetBIOS 名稱衝突，系統會在名稱後方附加數字。 如果名稱超過 15 個字元，精靈會提供縮短且具唯一性的建議。 在任一狀況下，精靈都會先透過 WINS 查閱與 NetBIOS 廣播來驗證名稱未使用。

如需有效網域名稱的詳細資訊，請參閱知識庫文章 [Active Directory 中的電腦、網域、網站及 OU 的命名慣例](https://support.microsoft.com/kb/909264)。

#### <a name="paths"></a>路徑
![顯示 Active Directory Domain Services 設定向導中 [路徑] 頁面的螢幕擷取畫面。](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestPaths.png)

[路徑] 頁面能讓您覆寫 AD DS 資料庫、資料庫交易記錄以及 SYSVOL 共用的預設資料夾位置。 預設位置一律是 %systemroot%(如 C:\Windows) 的子目錄。

#### <a name="review-options-and-view-script"></a>檢閱選項和檢視指令碼
![顯示 Active Directory Domain Services 設定向導中 [審核選項] 頁面的螢幕擷取畫面。](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestReviewOptions.png)

[檢閱選項] 頁面可讓您在開始安裝之前先驗證設定，並確保它們符合您的需求。 使用 [伺服器管理員] 時，這不是能停止安裝的最後機會。 這只是可讓您在繼續設定前確認設定的選項。

[伺服器管理員] 中的 [檢閱選項] 頁面也提供選用的 [檢視指令碼] 按鈕，用來建立一個包含目前的 ADDSDeployment 設定的 Unicode 文字檔，以便做為一個 Windows PowerShell 指令碼。 這樣可以讓您將 [伺服器管理員] 的圖形介面當作 Windows PowerShell 部署工作室一樣操作。 使用 [Active Directory 網域服務設定精靈] 來設定選項、匯出設定，然後取消精靈。 這個程序會建立一個有效且合乎語義的正確範例，以備日後修改或直接使用。 例如：

```powershell
#
# Windows PowerShell Script for AD DS Deployment
#

Import-Module ADDSDeployment
Install-ADDSForest `
-CreateDNSDelegation `
-DatabasePath "C:\Windows\NTDS" `
-DomainMode "Win2012" `
-DomainName "corp.contoso.com" `
-DomainNetBIOSName "CORP" `
-ForestMode "Win2012" `
-InstallDNS:$true `
-LogPath "C:\Windows\NTDS" `
-NoRebootOnCompletion:$false `
-SYSVOLPath "C:\Windows\SYSVOL"
-Force:$true

```

> [!NOTE]
> [伺服器管理員] 通常會在升級時填入所有引數的值，並不會依賴預設值 (因為它們在未來的 Windows 版本或 Service Pack 中可能會變更)。 **-safemodeadministratorpassword** 引數是個例外 (刻意在指令碼中省略)。 若要強制確認提示，以互動方式執行 Cmdlet 時請省略該值。

#### <a name="prerequisites-check"></a>先決條件檢查
![顯示 Active Directory Domain Services 設定向導中必要條件檢查頁面的螢幕擷取畫面。](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestPrereqCheck.png)

[先決條件檢查] 是 AD DS 網域設定中的新功能。 這個新階段會驗證伺服器設定是否能夠支援新的 AD DS 樹系。

安裝新的樹系根網域時，[伺服器管理員] 的 [Active Directory 網域服務設定精靈] 會叫用一系列的模組化測試。 這些測試會提醒您建議的修復選項。 您可以視需要執行多次測試。 必須通過所有先決條件測試，網域控制站程序才能繼續。

[先決條件檢查] 也會提供諸如影響舊版作業系統之安全性變更的相關資訊。

如需特定先決條件檢查的詳細資訊，請參閱[先決條件檢查](../../ad-ds/manage/AD-DS-Simplified-Administration.md#BKMK_PrereuisiteChecking)。

#### <a name="installation"></a>安裝
![顯示 Active Directory Domain Services 設定向導中 [安裝] 頁面的螢幕擷取畫面。](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestInstallation.png)

當 [安裝] 頁面顯示時，網域控制站設定程序就開始執行，而且無法暫停或取消。 詳細的作業會顯示此頁面上，而且會寫入到記錄檔：

-   %systemroot%\debug\dcpromo.log

-   %systemroot%\debug\dcpromoui.log

> [!NOTE]
> 您可以從相同的伺服器管理員主控台中同時執行多個角色安裝與 AD DS 設定精靈。

#### <a name="results"></a>結果
![顯示 [結果] 頁面的螢幕擷取畫面，您可以在其中查看升級是否成功或失敗。](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestSignOff.png)

[結果] 頁面會顯示升級成功或失敗，以及任何重要的系統管理資訊。 網域控制站會在 10 秒後自動重新開機。

## <a name="deploying-a-forest-with-windows-powershell"></a><a name="BKMK_PSForest"></a>使用 Windows PowerShell 部署樹系
本節說明如何在核心 Windows Server 2012 電腦上使用 Windows PowerShell，在樹系根網域中安裝第一個網域控制站。

### <a name="windows-powershell-ad-ds-role-installation-process"></a>Windows PowerShell AD DS 角色安裝程序
透過執行幾個簡單的 ServerManager 部署 Cmdlet 到您的部署程序，您就能進一步了解 AD DS 簡化的系統管理的願景。

下圖說明 Active Directory 網域服務角色安裝程序，從您執行 **PowerShell.exe** 開始，到網域控制站升級前結束。

![說明 Active Directory Domain Services 角色安裝程式的圖表，從您執行 PowerShell.exe 開始，並在網域控制站升級前結束。](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/adds_servermanagerdeployment_powershell.png)

| ServerManager Cmdlet | 引數 (**粗體** 的引數是必要的。 *斜體* 的引數可以使用 Windows PowerShell 或 [AD DS 設定精靈] 來指定。) |
|--|--|
| Install-WindowsFeature/Add-WindowsFeature | ***-Name** _<p>_-Restart *<p>* -IncludeAllSubFeature *<p>* -IncludeManagementTools *<p> -Source <p>*-ComputerName *<p> -Credential <p> -LogPath <p>*-Vhd *<p>* -ConfigurationFilePath * |

> [!NOTE]
> **-IncludeManagementTools** 雖然不是必要引數，但在安裝 AD DS 角色二進位檔時，強烈建議您使用此引數。

ServerManager 模組公開 Windows PowerShell 的新 DISM 模組中角色安裝、狀態及移除的部分。 這個分層簡化了大部分的工作，並降低直接使用強大 (但濫用時有風險) 的 DISM 模組的需求。

使用 **Get-Command** 可匯出 ServerManager 中的別名和 Cmdlet。

```powershell
Get-Command -module ServerManager
```

例如：

![終端機視窗的螢幕擷取畫面，顯示要在哪裡找到 Install-WindowsFeature Cmdlet。](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSGetCommand.png)

如果要新增 Active Directory 網域服務角色，只需執行 **Install-WindowsFeature** 並使用 AD DS 角色名稱做為引數。 和伺服器管理員一樣，AD DS 角色隱含的所有必要服務都會自動安裝。

```powershell
Install-WindowsFeature -name AD-Domain-Services
```

如果您也想安裝 AD DS 管理工具 (強烈建議安裝)，請提供 **-IncludeManagementTools** 引數：

```powershell
Install-WindowsFeature -name AD-Domain-Services -IncludeManagementTools
```

例如：

![終端機視窗的螢幕擷取畫面，其中顯示要提供-IncludeManagementTools 引數的位置。](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSInstallWinFeature.png)

如果要列出所有功能與角色和其安裝狀態，請使用不含引數的 **Get-WindowsFeature**。 指定 **-ComputerName** 引數以從遠端伺服器取得安裝狀態。

```powershell
Get-WindowsFeature
```

因為 **Get-WindowsFeature** 沒有篩選機制，您必須使用 **Where-Object** 與管線以尋找特定的功能。 管線是用於在多個 Cmdlet 之間傳送資料的通道，而 Where-Object Cmdlet 則做為篩選條件。 內建的 **$_** 變數做為目前通過管線且可能包含任何屬性的物件。

```powershell
Get-WindowsFeature | where-object <options>
```

例如，如果要尋找 **顯示名稱** 屬性中包含 "Active Dir" 的所有功能，請使用：

```powershell
Get-WindowsFeature | where displayname -like "*active dir*"
```

以下是進一步的範例：

![安裝新樹系](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSGetWindowsFeature.png)

如需更多 Windows PowerShell 作業搭配管線與 Where-Object 的詳細資訊，請參閱 [Windows PowerShell 中的管線處理與管線](/previous-versions/windows/it-pro/windows-powershell-1.0/ee176927(v=technet.10))。

也請注意，Windows PowerShell 3.0 大幅簡化此管線作業所需的命令列引數。 Windows PowerShell 2.0 會需要：

```powershell
Get-WindowsFeature | where {$_.displayname - like "*active dir*"}
```

使用 Windows PowerShell 管線，您可以建立可讀取的結果。 例如：

```powershell
Install-WindowsFeature | Format-List
Install-WindowsFeature | select-object | Format-List

```

![終端機視窗的螢幕擷取畫面，顯示如何建立可讀取的結果。](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSInstallADDS.png)

請注意使用 **Select-Object** Cmdlet 搭配 **-expandproperty** 引數會如何傳回感興趣的資料：

![終端機視窗的螢幕擷取畫面，顯示如何搭配-expandproperty 引數使用 Select-Object Cmdlet 會傳回感興趣的資料。](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSInstallADDSWithTools.png)

> [!NOTE]
> **Select-Object -expandproperty** 引數會稍微降低整體安裝效能。

### <a name="create-an-ad-ds-forest-root-domain-with-windows-powershell"></a><a name="BKMK_PS"></a>使用 Windows PowerShell 建立 AD DS 樹系根網域
如果要使用 ADDSDeployment 模組安裝新的 Active Directory 樹系，請使用下列 Cmdlet：

```powershell
Install-addsforest
```

**Install-AddsForest** Cmdlet 只有兩個階段 (先決條件檢查與安裝)。 下列兩個圖形顯示使用 **-domainname** 的基本必要引數的安裝階段。

| ADDSDeployment Cmdlet | 引數 (**粗體** 的引數是必要的。 *斜體* 的引數可以使用 Windows PowerShell 或 [AD DS 設定精靈] 來指定。) |
|--|--|
| install-addsforest | -Confirm<p>*-CreateDNSDelegation*<p>*-DatabasePath*<p>*-DomainMode*<p>***-DomainName** _<p>_*_-DomainNetBIOSName_* _<p>_-DNSDelegationCredential *<p>* -ForestMode *<p> -Force <p>*-InstallDNS *<p>* -LogPath *<p> -NoDnsOnNetwork <p> -NoRebootOnCompletion <p>*-SafeModeAdministratorPassword *<p> -SkipAutoConfigureDNS <p> -SkipPreChecks <p>*-SYSVOLPath *<p>* -Whatif * |

> [!NOTE]
> 如果您想變更依據 DNS 網域名稱首碼自動產生的 15 個字元名稱，或是這個名稱超過 15 個字元，必須使用 **-DomainNetBIOSName** 引數。

對等的伺服器管理員部署設定 ADDSDeployment Cmdlet 與引數為：

```powershell
Install-ADDSForest
-DomainName <string>
```

對等的伺服器管理員網域控制站選項 ADDSDeployment Cmdlet 引數為：

```powershell
-ForestMode <{Win2003 | Win2008 | Win2008R2 | Win2012 | Default}>
-DomainMode <{Win2003 | Win2008 | Win2008R2 | Win2012 | Default}>
-InstallDNS <{$false | $true}>
-SafeModeAdministratorPassword <secure string>
```

如未指定，**Install-ADDSForest** 引數會使用與伺服器管理員相同的預設值。

**SafeModeAdministratorPassword** 引數的操作方式比較特殊：

-   如果 *沒有指定* 為引數，Cmdlet 會提示您輸入並確認不顯示字元的密碼。 這是以互動方式執行 Cmdlet 時的慣用用法。

    例如，建立名稱為 corp.contoso.com 的新樹系，並提示輸入和確認不顯示字元的密碼：

    ```powershell
    Install-ADDSForest "DomainName corp.contoso.com
    ```

-   如果 *使用值* 指定，則這個值必須是安全字串。 這不是以互動方式執行 Cmdlet 時的慣用用法。

例如，您可以使用 **Read-Host** Cmdlet 手動提示輸入密碼，提示使用者輸入安全字串：

```powershell
-safemodeadministratorpassword (read-host -prompt "Password:" -assecurestring)
```

> [!WARNING]
> 因為前一個選項不會確認密碼，所以請務必小心使用：密碼是看不到的。

您也可以提供轉換的純文字變數當做安全字串，不過我們不鼓勵這種做法。

```powershell
-safemodeadministratorpassword (convertto-securestring "Password1" -asplaintext -force)
```

最後，您可以將模糊化密碼儲存到檔案中以在稍後重複使用，而不顯示純文字密碼。 例如：

```powershell
$file = "c:\pw.txt"
$pw = read-host -prompt "Password:" -assecurestring
$pw | ConvertFrom-SecureString | Set-Content $file

-safemodeadministratorpassword (Get-Content $File | ConvertTo-SecureString)

```

> [!WARNING]
> 不建議提供或儲存純文字或模糊化的密碼。 在指令碼中執行這個命令或監視您的任何人都會知道這個網域的 DSRM 密碼。 任何能夠存取該檔案的人都可以回復模糊化的密碼。 一旦具備該知識，他們可以登入在 DSRM 啟動的網域控制站，最終模擬網域控制站本身，提高其權限到 Active Directory 樹系中的最高層。 有一組建議的額外步驟使用 **System.Security.Cryptography** 將文字檔資料加密，不過這不在本文的討論範圍內。 最佳做法是完全避免儲存密碼。

ADDSDeployment Cmdlet 提供略過自動設定 DNS 用戶端設定、轉寄站及根目錄提示的額外選項。 使用伺服器管理員時，無法略過此設定選項。 如果您是在設定網域控制站之前安裝 DNS 伺服器角色，才需要這個引數：

```powershell
-SkipAutoConfigureDNS
```

**DomainNetBIOSName** 也是特殊作業：

-   如果 **DomainNetBIOSName** 引數未與 NetBIOS 網域名稱同時指定，且 **DomainName** 引數中的單一標籤首碼網域名稱是 15 個字元或更少，則升級會以自動產生的名稱繼續。

-   如果 **DomainNetBIOSName** 引數未與 NetBIOS 網域名稱同時指定，且 **DomainName** 引數中的單一標籤首碼網域名稱是 16 個字元或更少，則升級會失敗。

-   如果 **DomainNetBIOSName** 引數與一個 15 個字元或更少的 NetBIOS 網域名稱同時指定，則升級會使用該指定名稱繼續。

-   如果 **DomainNetBIOSName** 引數指定的是 16 個字元或更多的 NetBIOS 網域名稱，升級就會失敗。

對等的伺服器管理員其他選項 ADDSDeployment Cmdlet 引數為：

```powershell
-domainnetbiosname <string>
```

對等的伺服器管理員 **Paths** ADDSDeployment Cmdlet 引數為：

```powershell
-databasepath <string>
-logpath <string>
-sysvolpath <string>

```

使用選擇性的 **Whatif** 引數搭配 **Install-ADDSForest** Cmdlet 來檢閱設定資訊。 這可讓您看到明確和隱含的 Cmdlet 引數值。

例如：

![終端機視窗的螢幕擷取畫面，顯示如何使用選用的 Whatif 引數搭配 Install-ADDSForest Cmdlet 來檢查設定資訊。](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSPaths.png)

使用 [伺服器管理員] 時無法略過 [先決條件檢查]，但您可以在使用 AD DS 部署 Cmdlet 時使用下列引數略過該程序：

```powershell
-skipprechecks
```

> [!WARNING]
> Microsoft 建議您不要略過先決條件檢查，因為這樣可能會導致網域控制站升級不完整或 AD DS 樹系損毀。

請注意 **Install-ADDSForest** 如何提醒您升級會自動將伺服器重新開機 (和伺服器管理員一樣)：

![終端機視窗的螢幕擷取畫面，其中顯示 Install-ADDSForest 提醒您升級會自動重新開機伺服器。](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSReboot.png)

![顯示重新開機流程進度的終端機視窗螢幕擷取畫面。](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSInstallProgress.png)

若要自動接受重新開機的提示，請使用 **-force** 或 **-confirm:$false** 引數搭配任一 ADDSDeployment Windows PowerShell Cmdlet。 若要避免伺服器在升級結束時自動重新開機，請使用 **-norebootoncompletion** 引數。

> [!WARNING]
> 建議您不要覆寫重新開機設定。 網域控制站必須重新開機才能正常運作。

## <a name="see-also"></a>另請參閱
[Active Directory Domain Services (TechNet 入口網站) ](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc770946(v=ws.10)) 
[適用于 Windows Server 2008 R2](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd378801(v=ws.10)) 
 的 Active Directory Domain Services[適用于 Windows Server 2008](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd378891(v=ws.10)) 
 的 Active Directory Domain ServicesWindows server [2003) ](/previous-versions/windows/it-pro/windows-server-2003/cc739127(v=ws.10)) 
 (Windows server 技術參考[Active Directory 管理中心：消費者入門 (Windows Server 2008 R2) ](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd560651(v=ws.10)) 
[Active Directory Windows PowerShell (Windows Server 2008 R2) 管理](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd378937(v=ws.10)) 
[詢問目錄服務小組 (官方 Microsoft 商用技術支援的 Blog) ](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd378937(v=ws.10))
