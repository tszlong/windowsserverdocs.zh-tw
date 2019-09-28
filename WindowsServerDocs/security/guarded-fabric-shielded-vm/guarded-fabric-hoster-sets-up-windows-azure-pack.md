---
title: 受防護的 VM - 託管服務提供者會設定 Windows Azure Pack
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: d528c689-58b0-425c-9740-25e2553ed689
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: d388da2b7416543c307bd931636902b4a7543e1e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403658"
---
# <a name="shielded-vms---hosting-service-provider-sets-up-windows-azure-pack"></a>受防護的 VM - 託管服務提供者會設定 Windows Azure Pack

本主題說明主機服務提供者可以如何設定 Windows Azure 套件，讓租使用者可以使用它來部署受防護的 Vm。 Windows Azure 套件是一種 web 入口網站，可延伸 System Center Virtual Machine Manager 的功能，讓租使用者透過簡單的 web 介面部署和管理自己的 Vm。 Windows Azure 套件完全支援受防護的 Vm，讓您的租使用者更輕鬆地建立及管理其防護資料檔案。

若要瞭解本主題如何適用于部署受防護 Vm 的整體程式，請參閱[主機服務提供者設定步驟中的受防護主機和受防護的 vm](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)。

## <a name="setting-up-windows-azure-pack"></a>設定 Windows Azure 套件

您將完成下列工作，在您的環境中設定 Windows Azure 套件：

1. 針對您的主機網狀架構，完成 System Center 2016-Virtual Machine Manager （VMM）的設定。 這包括設定 VM 範本和 VM 雲端，這會透過 Windows Azure 套件公開：

    [案例-在 VMM 中部署受保護的主機和受防護的虛擬機器](https://technet.microsoft.com/system-center-docs/vmm/scenario/guarded-overview)

2. 安裝和設定 System Center 2016-Service Provider Foundation （SPF）。 此軟體可讓 Windows Azure 套件與您的 VMM 伺服器進行通訊：

    [部署 Service Provider Foundation-SPF](https://technet.microsoft.com/system-center-docs/spf/deploy/deploy-spf)

3. 安裝 Windows Azure 套件，並將它設定為與 SPF 進行通訊：

    - [安裝 Windows Azure 套件](#install-windows-azure-pack)（在本主題中）
    - [設定 Windows Azure 套件](#configure-windows-azure-pack)（在本主題中）

4. 在 Windows Azure 套件中建立一或多個主機方案，以允許租使用者存取您的 VM 雲端：

    [在 Windows Azure 套件中建立方案](#create-a-plan-in-windows-azure-pack)（在本主題中）

## <a name="install-windows-azure-pack"></a>安裝 Windows Azure 套件

在您要裝載租使用者入口網站的電腦上安裝並設定 Windows Azure 套件（WAP）。 這部電腦必須能夠連線到 SPF 伺服器，並可供您的租使用者連接。

1.  查看[WAP 系統需求](https://technet.microsoft.com/library/dn296442.aspx)，並安裝[先決條件軟體](https://technet.microsoft.com/library/dn469335.aspx)。

2.  下載並安裝[Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)。 如果電腦未連線到網際網路，請遵循[離線安裝指示](http://www.iis.net/learn/install/web-platform-installer/web-platform-installer-v4-command-line-webpicmdexe-rtw-release)。

3.  開啟 Web Platform Installer 並尋找 @no__t 0Windows Azure Pack：入口網站和 API Express @ no__t-0 下的 [**產品**] 索引標籤。按一下 [**新增**]，然後在視窗底部進行 [**安裝**]。

4.  繼續安裝。 安裝完成之後，就會在您的網頁瀏覽器中開啟設定網站（*HTTPs://&lt;wapserver @ no__t-2： 30101/* ）。 在此網站上，提供 SQL server 的相關資訊，並完成 WAP 的設定。

如需設定 Windows Azure 套件的說明，請參閱[安裝 Windows Azure 套件的快速部署](https://technet.microsoft.com/dn296439.aspx)。

> [!NOTE]
> 如果您已在環境中執行 Windows Azure 套件，您可以使用現有的安裝。 不過，若要使用最新的受防護 VM 功能，您必須將安裝升級至至少更新彙總套件10。

### <a name="configure-windows-azure-pack"></a>設定 Windows Azure 套件

使用 Windows Azure 套件之前，您應該已經安裝並設定您的基礎結構。

1.  流覽至位於*HTTPs://&lt;wapserver @ no__t-2： 30091*的 Windows Azure 套件系統管理員入口網站，然後使用您的系統管理員認證登入。

2.  在左窗格中，按一下 [ **VM**雲端]。

3.  按一下 [**註冊 System Center Service Provider Foundation**]，將 Windows Azure 套件連接到 Service Provider Foundation 實例。 您將需要指定 Service Provider Foundation 的 URL，以及使用者名稱和密碼。

    ![註冊 System Center Service Provider Foundation](../media/Guarded-Fabric-Shielded-VM/guarded-host-azure-pack-01-register-spf.png)

4.  完成之後，您應該就可以在 VMM 環境中看到 VM 雲端的設定。 在繼續之前，請確定您至少有一個 VM 雲端支援可供 WAP 使用的受防護 Vm。

### <a name="create-a-plan-in-windows-azure-pack"></a>在 Windows Azure 套件中建立方案

為了讓租使用者能夠在 WAP 中建立 Vm，您必須先建立可供租使用者訂閱的主控方案。 方案會為您的租使用者定義允許的 VM 雲端、範本、網路和計費實體。

1. 在入口網站的下方窗格中，按一下 [ **+ 新增**] [&gt;**方案**] [&gt;**建立方案**]。

2. 在嚮導的第一個步驟中，為您的方案選擇 [名稱]。 這是您的租使用者在訂閱時將會看到的名稱。

3. 在第二個步驟中，選取 [**虛擬機器**雲端] 做為方案中提供的其中一項服務。

4. 略過選取方案的任何附加元件的步驟。

5. 按一下 **[確定]** （核取記號）以建立方案。 雖然這會建立方案，但它尚未處於已設定的狀態。

   ![Windows Azure 套件中的計畫](../media/Guarded-Fabric-Shielded-VM/guarded-host-azure-pack-02-create-plan.png)

6. 若要開始設定方案，請按一下其名稱。

7. 在下一個頁面的 [**方案服務**] 底下，按一下 [**虛擬機器**雲端]。 這會開啟頁面，您可以在其中設定此方案的配額。

8. 在 [**基本**] 底下，選取您想要提供給租使用者的 VMM 管理伺服器和虛擬機器雲端。 可以提供受防護 Vm 的雲端會顯示其名稱旁邊的 **（支援防護）** 。

9. 選取您想要在此方案中套用的配額。 （例如，CPU 核心和 RAM 使用量的限制）。 請務必選取 [**允許受防護的虛擬機器**] 核取方塊。

   ![Windows Azure 套件中的虛擬機器雲端設定](../media/Guarded-Fabric-Shielded-VM/guarded-host-azure-pack-03-virtual-machine-clouds.png)
    
10. 向下流覽至標題為 [**範本**] 的區段，然後選取一或多個要提供給您租使用者的範本。 您可以為租使用者提供受防護和非遮罩的範本，但必須提供受防護的範本，以提供租使用者關於 VM 完整性及其秘密的端對端保證。

11. 在 [**網路**] 區段中，為您的租使用者新增一或多個網路。

12. 設定方案的任何其他設定或配額之後，請按一下底部的 [**儲存**]。

13. 在畫面左上方，按一下箭號，將您帶回 [**方案**] 頁面。

14. 在畫面底部，將方案從 [**私**用] 變更為 [**公開**]，讓租使用者可以訂閱者案。

    ![變更 Windows Azure 套件中方案的存取權](../media/Guarded-Fabric-Shielded-VM/guarded-host-azure-pack-04-change-access.png)

    此時，已設定 Windows Azure 套件，而且租使用者將能夠訂閱您剛建立的方案，並部署受防護的 Vm。 如需租使用者需要完成的其他步驟，請參閱適用于租使用者的[受防護 vm-使用 Windows Azure 套件部署受防護的 vm](guarded-fabric-shielded-vm-windows-azure-pack.md)。

## <a name="see-also"></a>另請參閱

- [適用于受防護主機和受防護 Vm 的主機服務提供者設定步驟](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [受防護網狀架構與受防護的 VM](guarded-fabric-and-shielded-vms-top-node.md)
