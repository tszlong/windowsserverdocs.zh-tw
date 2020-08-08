---
title: 設定主機以進行即時移轉而不需要容錯移轉叢集
description: 提供在非叢集環境中設定即時移轉的指示
manager: dongill
ms.topic: article
ms.assetid: b5e3c405-cb76-4ff2-8042-c2284448c435
author: kbdazure
ms.author: kathydav
ms.date: 9/30/2016
ms.openlocfilehash: 7bcd4e625f340ba7358a8ce9bdd860581c390e96
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87948017"
---
# <a name="set-up-hosts-for-live-migration-without-failover-clustering"></a>設定主機以進行即時移轉而不需要容錯移轉叢集

>適用於：Windows Server 2016、Microsoft Hyper-V Server 2016、Windows Server 2019、Microsoft Hyper-V Server 2019

本文說明如何設定未叢集化的主機，讓您可以在它們之間進行即時移轉。 如果您未在安裝 Hyper-v 時設定即時移轉，或想要變更設定，請使用這些指示。 若要設定叢集主機，請使用容錯移轉叢集的工具。

## <a name="requirements-for-setting-up-live-migration"></a>設定即時移轉的需求

若要設定非叢集主機以進行即時移轉，您需要：

-  具有執行各種步驟之許可權的使用者帳戶。 除非您要設定限制委派，否則來源和目的電腦上的本機 Hyper-v Administrators 群組或 Administrators 群組的成員資格會符合此需求。 若要設定限制委派，必須要有 Domain Administrators 群組的成員資格。

- Windows Server 2016 或 Windows Server 2012 R2 中的 Hyper-v 角色已安裝在來源和目的地伺服器上。 如果虛擬機器至少為第5版，您可以在執行 Windows Server 2016 和 Windows Server 2012 R2 的主機之間進行即時移轉。 <br>如需版本升級指示，請參閱在[windows 10 或 Windows Server 2016 上的 hyper-v 中升級虛擬機器版本](Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md)。 如需安裝指示，請參閱[在 Windows Server 上安裝 hyper-v 角色](../get-started/Install-the-Hyper-V-role-on-Windows-Server.md)。

- 屬於相同 Active Directory 網域，或屬於彼此信任之網域的來源和目的地電腦。
- Hyper-v 管理工具安裝在執行 Windows Server 2016 或 Windows 10 的電腦上，除非來源或目的地伺服器上已安裝這些工具，而且您將從伺服器執行這些工具。

## <a name="consider-options-for-authentication-and-networking"></a>考慮驗證和網路功能的選項

請考慮您想要如何設定下列各項：

-  **驗證**：會使用哪一個通訊協定來驗證來源與目的地伺服器之間的即時移轉流量？ 選擇會決定您是否必須先登入來源伺服器，才能開始進行即時移轉：
   - Kerberos 可讓您避免需要登入伺服器，但必須設定限制委派。 如需相關指示，請參閱下文。
   - CredSSP 可讓您避免設定限制委派，但需要您登入來源伺服器。 您可以透過本機主控台會話、遠端桌面會話或遠端 Windows PowerShell 會話來執行此動作。

      CredSPP 需要在可能不明顯的情況下進行登入。 例如，如果您登入 TestServer01 以將虛擬機器移至 Testserver02,，然後想要將虛擬機器移回 TestServer01，則在嘗試將虛擬機器移回 TestServer01 之前，您必須先登入 Testserver02,。 如果您未這麼做，驗證嘗試會失敗，發生錯誤，並顯示下列訊息：

      「在遷移來源上的虛擬機器遷移作業失敗。
      無法建立與主*電腦名稱稱*的連線：安全性套件0x8009030E 中沒有可用的認證。」

-   **效能**：設定效能選項是否合理？ 這些選項可減少網路和 CPU 使用量，以及讓即時移轉更快。 請考慮您的需求和基礎結構，並測試不同的設定，以協助您做出決定。 步驟2的結尾會描述這些選項。

-  **網路喜好**設定：您是否允許透過任何可用的網路進行即時移轉流量，或將流量隔離到特定網路？ 最佳的安全性做法，建議您將流量隔離到受信任的私用網路，因為在網路上傳送即時移轉時不會加密。 透過實際隔離的網路或另一種受信任的網路技術 (例如 Vlan) 可以達成網路隔離。

## <a name="step-1-configure-constrained-delegation-optional"></a><a name="BKMK_Step1"></a>步驟1：設定限制委派 (選擇性) 
如果您已決定使用 Kerberos 來驗證即時移轉流量，請使用網域系統管理員群組成員的帳戶來設定限制委派。

### <a name="use-the-users-and-computers-snap-in-to-configure-constrained-delegation"></a>使用 [使用者和電腦] 嵌入式管理單元來設定限制委派

1.  開啟 [Active Directory 使用者和電腦] 嵌入式管理單元。 從伺服器管理員 (，選取未選取的伺服器，然後按一下 [**工具**]  >>  **Active Directory [使用者和電腦**] [) ]。

2.  從**Active Directory 使用者和電腦**] 的流覽窗格中，選取網域，然後按兩下 [**電腦**] 資料夾。

3.  在 [**電腦**] 資料夾中，以滑鼠**按右鍵來源**伺服器的電腦帳戶，然後按一下 [內容]。

4.  在 [**屬性**] 中，按一下 [**委派**] 索引標籤。

5.  在 [委派] 索引標籤上，選取 [**信任這台電腦，但只委派指定的服務**]，然後選取 [**使用任何驗證通訊協定**]。

6.  按一下 [新增] 。

7.  在 [**新增服務**] 中，按一下 [**使用者或電腦**]。

8.  在 [**選取使用者或電腦**] 中，輸入目的地伺服器的名稱。 按一下 [**檢查名稱**] 進行驗證，然後按一下 **[確定]**。

9. 從 [**新增服務**]，在 [可用的服務] 清單中執行下列動作，然後按一下 **[確定]**：

    -   若要移動虛擬機器存放裝置，請選取 [cifs]****。 如果您想要將存放裝置與虛擬機器一起移動，以及只想要移動虛擬機器的存放裝置，就必須這麼做。 如果伺服器設定為在 Hyper-V 使用 SMB 存放裝置，這個選項應該已經選取。

    -   若要移動虛擬機器，請選取 [Microsoft 虛擬系統移轉服務]****。

10. 在 [內容] 對話方塊的 [委派]**** 索引標籤上，確認您在上一個步驟中選取的服務已經列示為目的電腦可以顯示委派認證的服務。 按一下 [確定]  。

11. 從 [Computers]**** 資料夾，選取目的地伺服器的電腦帳戶，然後重複這個程序。 在 [選取使用者或電腦]**** 對話方塊中，確定指定了來源伺服器的名稱。

設定變更會在下列兩種情況發生之後生效：

  -  這些變更會複寫到執行 Hyper-v 的伺服器登入的網域控制站。
  -  網域控制站會發出新的 Kerberos 票證。

## <a name="step-2-set-up-the-source-and-destination-computers-for-live-migration"></a><a name="BKMK_Step2"></a>步驟2：設定即時移轉的來源和目的電腦
此步驟包括選擇驗證和網路的選項。 基於安全性最佳做法，我們建議您選取要用於即時移轉流量的特定網路，如上面所述。 此步驟也會說明如何選擇 [效能] 選項。

### <a name="use-hyper-v-manager-to-set-up-the-source-and-destination-computers-for-live-migration"></a>使用 Hyper-v 管理員設定來源和目的地電腦以進行即時移轉

1.  開啟 Hyper-V 管理員。 從伺服器管理員 (，按一下 [**工具**] [  >> **hyper-v 管理員**]。 ) 

2.  在流覽窗格中，選取其中一部伺服器。  (如果未列出，請以滑鼠右鍵按一下 [ **Hyper-v 管理員**]，按一下 **[連線到伺服器]**，輸入伺服器名稱，然後按一下 **[確定]**。 重複以新增更多伺服器。 ) 

3.  在 [**動作**] 窗格中，按一下 [ **hyper-v 設定**] [  >> **即時移轉**]。

4.  在 [即時移轉]**** 窗格中，核取 [啟用連入與連出即時移轉]****。

5.  如果您不想使用預設值2，請在 [**同時即時移轉**] 底下，指定不同的數位。

6.  在 [連入即時移轉]**** 下，如果您想要使用特定網路連線來接受即時移轉流量，請按一下 [新增]**** 以便輸入 IP 位址資訊。 否則，請按一下 [使用任何可用的網路來進行即時移轉]****。 按一下 [確定]  。

7.  若要選擇 Kerberos 和效能選項，請展開 [**即時移轉**]，然後選取 [ **Advanced Features**]。

    - 如果您已設定限制委派，請在 [**驗證通訊協定**] 底下選取 [ **Kerberos**]。
    - 在 [**效能選項**] 底下，檢查詳細資料，如果適用于您的環境，請選擇不同的選項。

8. 按一下 [確定]  。

9. 在 [Hyper-v 管理員] 中選取其他伺服器，並重複執行步驟。

### <a name="use-windows-powershell-to-set-up-the-source-and-destination-computers-for-live-migration"></a>使用 Windows PowerShell 設定來源和目的電腦以進行即時移轉

有三個 Cmdlet 可用於設定非叢集主機上的即時移轉： [Enable-enable-vmmigration](https://technet.microsoft.com/library/hh848544.aspx)、 [get-vmmigrationnetwork](https://technet.microsoft.com/library/hh848467.aspx)和[VMHost](https://technet.microsoft.com/library/hh848524.aspx)。 這個範例會使用這三個，並執行下列動作：
  - 設定本機主機上的即時移轉
  - 只允許特定網路上的傳入遷移流量
  - 選擇 Kerberos 做為驗證通訊協定

每一列代表一個個別的命令。

```PowerShell
PS C:\> Enable-VMMigration

PS C:\> Set-VMMigrationNetwork 192.168.10.1

PS C:\> Set-VMHost -VirtualMachineMigrationAuthenticationType Kerberos
```

VMHost 也可讓您選擇 (和其他許多主機設定) 的效能選項。 例如，若要選擇 SMB，但讓驗證通訊協定保持設定為預設的 CredSSP，請輸入：

```PowerShell
PS C:\> Set-VMHost -VirtualMachineMigrationPerformanceOption SMB
```

下表描述效能選項的作用。

|選項|描述|
|----------|---------------|
    |TCP/IP|透過 TCP/IP 連接，將虛擬機器的記憶體複製到目的地伺服器。|
    |壓縮|會先壓縮虛擬機器的記憶體內容，再透過 TCP/IP 連接將它複製到目的地伺服器。 **注意：** 這是**預設**設定。|
    |SMB|透過 SMB 3.0 連線，將虛擬機器的記憶體複製到目的地伺服器。<p>-當來源和目的地伺服器上的網路介面卡已啟用 (RDMA) 功能的遠端直接記憶體存取時，會使用 SMB 直接傳輸。<br />-當識別出適當的 SMB 多重通道設定時，SMB 多重通道會自動偵測並使用多個連接。<p>如需詳細資訊，請參閱[使用 SMB 直接傳輸改善檔案伺服器的效能](https://technet.microsoft.com/library/jj134210(WS.11).aspx)。|

 ## <a name="next-steps"></a>後續步驟

 設定主機之後，您就可以開始進行即時移轉。 如需相關指示，請參閱[使用沒有容錯移轉叢集的即時移轉來移動虛擬機器](../manage/Use-live-migration-without-Failover-Clustering-to-move-a-virtual-machine.md)。
