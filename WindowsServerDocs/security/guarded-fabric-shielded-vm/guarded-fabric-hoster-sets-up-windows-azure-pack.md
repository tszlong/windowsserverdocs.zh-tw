---
title: 受防護的 VM - 託管服務提供者會設定 Windows Azure Pack
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: d528c689-58b0-425c-9740-25e2553ed689
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 9ca9f41770c977b6e7c4900b090471dbfe11a450
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855719"
---
# <a name="shielded-vms---hosting-service-provider-sets-up-windows-azure-pack"></a>受防護的 VM - 託管服務提供者會設定 Windows Azure Pack

本主題描述如何裝載的服務提供者可以設定 Windows Azure Pack 讓租用戶可以使用它來部署受防護的 Vm。 Windows Azure Pack 是入口網站擴充功能的 System Center Virtual Machine Manager，讓部署及管理自己的 Vm，透過簡單的 web 介面的租用戶。 Windows Azure Pack 完全支援受防護的 Vm，且更輕鬆針對您的租用戶建立及管理其虛擬機器防護資料檔案。

若要了解本主題如何放入部署受防護的 Vm 的整體程序，請參閱[裝載服務提供者設定步驟，針對受防護主機和受防護的 Vm](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)。

## <a name="setting-up-windows-azure-pack"></a>設定 Windows Azure Pack

您將完成下列工作以在您的環境中設定 Windows Azure Pack:

1. 完成 System Center 2016-Virtual Machine Manager (VMM) 控管網狀架構設定。 這包括設定 VM 範本，以及 VM 雲端，將會公開透過 Windows Azure Pack:

    [案例-部署受防護的主機和受防護的虛擬機器在 VMM 中](https://technet.microsoft.com/system-center-docs/vmm/scenario/guarded-overview)

2. 安裝和設定 System Center 2016-Service Provider Foundation (SPF)。 此軟體可讓您的 VMM 伺服器進行通訊的 Windows Azure Pack:

    [部署 Service Provider Foundation-SPF](https://technet.microsoft.com/system-center-docs/spf/deploy/deploy-spf)

3. 安裝 Windows Azure Pack，並將它設定為使用 SPF 通訊：

    - [安裝 Windows Azure Pack](#install-windows-azure-pack) （在本主題中）
    - [設定 Windows Azure Pack](#configure-windows-azure-pack) （在本主題中）

4. 在 Windows Azure Pack，以允許存取您的 VM 雲端的租用戶中建立一個以上的主控方案：

    [在 Windows Azure Pack 中建立計劃](#create-a-plan-in-windows-azure-pack)（在本主題中）

## <a name="install-windows-azure-pack"></a>安裝 Windows Azure 套件

安裝和設定您想要用來裝載您的租用戶入口網站的電腦上的 Windows Azure Pack (WAP)。 此電腦必須能夠連線到 SPF 伺服器，而且可由您租用戶。

1.  檢閱[WAP 系統需求](https://technet.microsoft.com/library/dn296442.aspx)並安裝[先決條件軟體](https://technet.microsoft.com/library/dn469335.aspx)。

2.  下載並安裝[Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)。 如果電腦未連線到網際網路，請遵循[離線安裝指示](http://www.iis.net/learn/install/web-platform-installer/web-platform-installer-v4-command-line-webpicmdexe-rtw-release)。

3.  開啟 Web Platform Installer 並尋找**Windows Azure Pack:入口網站和 API Express**底下**產品**] 索引標籤。按一下 [**新增**，然後**安裝**視窗的底部。

4.  繼續安裝。 安裝完成時，設定站台之後 (*https://&lt;wapserver&gt;: 30101 /*) 會在您的 web 瀏覽器中開啟。 在本網站提供您的 SQL server 的相關資訊並完成設定 WAP。

如需協助 Windows Azure Pack 安裝的設定，請參閱 <<c0> [ 安裝 Windows Azure Pack 的快速部署](https://technet.microsoft.com/dn296439.aspx)。

> [!NOTE]
> 如果您已經在環境中執行 Windows Azure Pack，您可以使用您現有的安裝。 若要使用最新的受防護 VM 的功能，不過，您必須至少升級到您安裝更新彙總套件 10。

### <a name="configure-windows-azure-pack"></a>設定 Windows Azure 套件

使用 Windows Azure Pack 之前，您應該已安裝並設定您的基礎結構。

1.  瀏覽 Windows Azure Pack 系統管理員入口網站，網址*https://&lt;wapserver&gt;: 30091*，然後登入使用您的系統管理員認證。

2.  在左窗格中，按一下**VM 雲端**。

3.  連接到 Service Provider Foundation 的執行個體的 Windows Azure Pack，依序按一下**註冊 System Center Service Provider Foundation**。 您必須指定 Service Provider Foundation 的 URL，以及使用者名稱和密碼。

    ![註冊 System Center Service Provider Foundation](../media/Guarded-Fabric-Shielded-VM/guarded-host-azure-pack-01-register-spf.png)

4.  完成後，您應該能夠看到您的 VMM 環境中設定的 VM 雲端。 請確定您有至少一個 VM 雲端，支援受防護的 Vm 能夠 WAP 再繼續進行。

### <a name="create-a-plan-in-windows-azure-pack"></a>在 Windows Azure Pack 建立方案

若要讓租用戶可以在 WAP 中建立 Vm，您必須先建立租用戶可以訂閱的主控方案。 計劃定義您的租用戶允許的 VM 雲端、 範本、 網路和計費的實體。

1.  在入口網站的下方窗格中，按一下 **+ 新增** &gt; **規劃** &gt; **建立計劃**。

2.  在精靈的第一個步驟中，選擇您的計劃的名稱。 這是訂閱時，會看到您的租用戶的名稱。

3.  在第二個步驟中，選取**虛擬機器雲端**做為其中一個計劃中提供的服務。

4.  略過選取任何計劃的附加元件的相關步驟。

5.  按一下 **確定**（核取記號），以建立方案。 雖然這會建立方案時，它不在設定的狀態。

    ![在 Windows Azure 中的方案套件](../media/Guarded-Fabric-Shielded-VM/guarded-host-azure-pack-02-create-plan.png)

6.  若要開始設定此計劃，請按一下其名稱。

7.  在下一步 頁面上，底下**規劃服務**，按一下**虛擬機器雲端**。 這會開啟您可以在其中設定此計劃的配額頁面。

8.  底下**基本**，選取您想要提供給租用戶的虛擬機器雲端與 VMM 管理伺服器。 受防護的 Vm 可以提供的雲端會顯示 **（防護支援）** 其名稱旁邊。

9.  選取您想要套用這個計劃中的配額。 （例如，限制 CPU 核心和 RAM 使用量）。 請務必保留**允許虛擬機器可受防護的**選取核取方塊。

    ![在 Windows Azure Pack 中的虛擬機器雲端設定](../media/Guarded-Fabric-Shielded-VM/guarded-host-azure-pack-03-virtual-machine-clouds.png)
    
10.  向下一節捲動**範本**，然後選取一或多個範本，以提供給您的租用戶。 您可同時受防護，並提供給租用戶的 VM 和其密碼完整性相關的端對端保證，則必須提供給租用戶，受防護的範本，但受防護的範本。

11.  在 **網路**區段中，將一或多個網路加入您的租用戶。

12.  設定後設定或配額計劃，請按一下**儲存**底部。

13.  在畫面的左上方，按一下箭頭，即可帶您回到**計劃**頁面。

14.  在畫面底部，變更計劃從正在**私用**要**公用**，讓租用戶可以訂閱方案。

    ![變更 Windows Azure Pack 中的計劃的存取權](../media/Guarded-Fabric-Shielded-VM/guarded-host-azure-pack-04-change-access.png)

    此時，設定 Windows Azure Pack 和租用戶能夠訂閱您剛剛建立的計劃和部署受防護的 Vm。 如需租用戶需要完成的額外步驟，請參閱[受防護的 Vm，可供租用戶-使用 Windows Azure Pack 部署受防護的 VM](guarded-fabric-shielded-vm-windows-azure-pack.md)。

## <a name="see-also"></a>另請參閱

- [裝載服務提供者設定步驟，針對受防護主機和受防護的 Vm](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [受防護網狀架構與受防護的 Vm](guarded-fabric-and-shielded-vms-top-node.md)
