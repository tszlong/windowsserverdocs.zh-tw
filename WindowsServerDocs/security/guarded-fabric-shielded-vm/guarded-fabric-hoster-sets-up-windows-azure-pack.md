---
title: 受防護的 VM - 託管服務提供者會設定 Windows Azure Pack
description: 深入瞭解：受防護的 Vm-主機服務提供者設定 Windows Azure 套件
ms.topic: article
ms.assetid: d528c689-58b0-425c-9740-25e2553ed689
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 08/29/2018
ms.openlocfilehash: dec0272172cf3277000f0f1f592e4b7a204916bd
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97047376"
---
# <a name="shielded-vms---hosting-service-provider-sets-up-windows-azure-pack"></a>受防護的 VM - 託管服務提供者會設定 Windows Azure Pack

本主題說明主機服務提供者如何設定 Windows Azure 套件，讓租使用者可以使用它來部署受防護的 Vm。 Windows Azure 套件是一種 web 入口網站，可延伸 System Center Virtual Machine Manager 的功能，讓租使用者能夠透過簡單的 web 介面來部署和管理自己的 Vm。 Windows Azure 套件完全支援受防護的 Vm，讓您的租使用者更輕鬆地建立及管理其防護資料檔案。

若要瞭解本主題如何納入部署受防護 Vm 的整體程式，請參閱 [主機服務提供者設定步驟，以瞭解受防護主機和受防護的 vm](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)。

## <a name="setting-up-windows-azure-pack"></a>設定 Windows Azure 套件

您將完成下列工作，以在您的環境中設定 Windows Azure 套件：

1. 為您的主機網狀架構完成 System Center 2016-Virtual Machine Manager (VMM) 的設定。 這包括設定 VM 範本和 VM 雲端，其將透過 Windows Azure 套件公開：

    [案例 - 在 VMM 中部署受防護主機和受防護的虛擬機器](/system-center/vmm/deploy-guarded-host-fabric)

2. 安裝和設定 System Center 2016-Service Provider Foundation (SPF) 。 此軟體可讓 Windows Azure 套件與您的 VMM 伺服器通訊：

    [部署 Service Provider Foundation-SPF](https://technet.microsoft.com/system-center-docs/spf/deploy/deploy-spf)

3. 安裝 Windows Azure 套件，並將其設定為與 SPF 通訊：

    - [安裝](#install-windows-azure-pack) 本主題 Windows Azure 套件 () 
    - [設定](#configure-windows-azure-pack) 本主題 Windows Azure 套件 () 

4. 在 Windows Azure 套件中建立一或多個主控方案，以允許租使用者存取您的 VM 雲端：

    在本主題的 Windows Azure 套件 (中[建立方案](#create-a-plan-in-windows-azure-pack)) 

## <a name="install-windows-azure-pack"></a>安裝 Windows Azure 套件

在您想要裝載租使用者入口網站的電腦上，安裝並設定 Windows Azure 套件 (WAP) 。 這台電腦必須能夠連線到 SPF 伺服器，而且您的租使用者可連接到該伺服器。

1.  查看 [WAP 系統需求](/previous-versions/azure/windows-server-azure-pack/dn296442(v=technet.10)) ，並安裝 [先決條件軟體](/previous-versions/azure/windows-server-azure-pack/dn469335(v=technet.10))。

2.  下載並安裝 [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)。 如果電腦未連線到網際網路，請依照 [離線安裝指示](https://www.iis.net/learn/install/web-platform-installer/web-platform-installer-v4-command-line-webpicmdexe-rtw-release)進行。

3.  開啟 Web Platform Installer，然後在 [**產品**] 索引標籤底下尋找 **Windows Azure 套件： Portal 和 API Express** 。按一下 [**新增**]，然後在視窗底部進行 **安裝**。

4.  繼續安裝。 安裝完成之後，就會在您的網頁瀏覽器中開啟 configuration site (*HTTPs:// &lt; wapserver &gt; ： 30101/*) 。 在此網站上，提供 SQL server 的相關資訊，並完成 WAP 的設定。

如需設定 Windows Azure 套件的說明，請參閱 [安裝 Windows Azure 套件的快速部署](/previous-versions/azure/windows-server-azure-pack/dn296439(v=technet.10))。

> [!NOTE]
> 如果您已經在您的環境中執行 Windows Azure 套件，您可以使用現有的安裝。 不過，若要使用最新的受防護 VM 功能，您必須將安裝升級到至少更新彙總套件10。

### <a name="configure-windows-azure-pack"></a>設定 Windows Azure 套件

使用 Windows Azure 套件之前，您應該已經為您的基礎結構安裝並設定它。

1.  流覽至位於 *HTTPs:// &lt; wapserver &gt; ： 30091* 的 Windows Azure 套件系統管理員入口網站，然後使用您的系統管理員認證登入。

2.  在左窗格中，按一下 [ **VM** 雲端]。

3.  按一下 [ **註冊 System Center Service Provider Foundation**，將 Windows Azure 套件連接到 Service Provider Foundation 實例。 您將需要指定 Service Provider Foundation 的 URL，以及使用者名稱和密碼。

    ![註冊 System Center Service Provider Foundation](../media/Guarded-Fabric-Shielded-VM/guarded-host-azure-pack-01-register-spf.png)

4.  完成之後，您應該能夠看到 VM 雲端已在 VMM 環境中設定。 在繼續之前，請確定您至少有一個支援 WAP 可用受防護 Vm 的 VM 雲端。

### <a name="create-a-plan-in-windows-azure-pack"></a>在 Windows Azure 套件中建立方案

為了讓租使用者能夠在 WAP 中建立 Vm，您必須先建立租使用者可訂閱的主控方案。 方案會為您的租使用者定義允許的 VM 雲端、範本、網路和帳單實體。

1. 在入口網站的下方窗格中，按一下 [ **+ 新增** &gt; **方案** &gt; **建立方案**]。

2. 在嚮導的第一個步驟中，選擇方案的名稱。 這是您的租使用者在訂閱時將會看到的名稱。

3. 在第二個步驟中，選取 [ **虛擬機器** 雲端] 作為方案中要提供的其中一項服務。

4. 略過針對方案選取任何附加元件的步驟。

5. 按一下 **[確定]** (核取 [標記) ] 來建立方案。 雖然這會建立方案，但它尚未處於設定狀態。

   ![Windows Azure 套件中的計畫](../media/Guarded-Fabric-Shielded-VM/guarded-host-azure-pack-02-create-plan.png)

6. 若要開始設定方案，請按一下其名稱。

7. 在下一個頁面的 [ **方案服務**] 下，按一下 [ **虛擬機器** 雲端]。 這會開啟頁面，您可以在其中設定此方案的配額。

8. 在 [ **基本**] 底下，選取您要提供給租使用者的 VMM 管理伺服器和虛擬機器雲端。 可提供受防護 Vm 的雲端會顯示在其名稱旁邊 **) 支援 (防護** 。

9. 選取您要在此方案中套用的配額。  (例如，CPU 核心和 RAM 使用量的限制) 。 請務必選取 [ **允許受防護的虛擬機器** ] 核取方塊。

   ![Windows Azure 套件中的虛擬機器雲端設定](../media/Guarded-Fabric-Shielded-VM/guarded-host-azure-pack-03-virtual-machine-clouds.png)

10. 向下滾動至標題為 [ **範本**] 的區段，然後選取要提供給租使用者的一或多個範本。 您可以為租使用者提供受防護和無遮罩的範本，但必須提供受防護的範本，才能讓租使用者對 VM 及其秘密完整性的端對端保證進行端對端的保證。

11. 在 [ **網路** ] 區段中，為您的租使用者新增一或多個網路。

12. 設定方案的任何其他設定或配額之後，請按一下底部的 [ **儲存** ]。

13. 在畫面的左上方，按一下箭號，讓您回到 [ **計畫** ] 頁面。

14. 在畫面底部，將方案從 **私** 用變更為 **Public** ，讓租使用者可以訂閱者案。

    ![在 Windows Azure 套件中變更方案的存取權](../media/Guarded-Fabric-Shielded-VM/guarded-host-azure-pack-04-change-access.png)

    此時會設定 Windows Azure 套件，且租使用者將能夠訂閱您剛才建立的方案，並部署受防護的 Vm。 如需租使用者需要完成的其他步驟，請參閱租使用者的 [受防護 vm-使用 Windows Azure 套件部署受防護的 vm](guarded-fabric-shielded-vm-windows-azure-pack.md)。

## <a name="additional-references"></a>其他參考資料

- [適用於受防護主機和受防護 VM 的託管服務提供者設定步驟](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [受防護網狀架構與受防護的 VM](guarded-fabric-and-shielded-vms-top-node.md)
