---
title: 設定即時移轉，而不容錯移轉叢集的主機
description: 說明如何設定 在非叢集環境中的即時移轉
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b5e3c405-cb76-4ff2-8042-c2284448c435
author: KBDAzure
ms.author: kathydav
ms.date: 9/30/2016
ms.openlocfilehash: 49e36d7fae5eec07772fd6f82c5a5d69838351d8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821989"
---
# <a name="set-up-hosts-for-live-migration-without-failover-clustering"></a>設定即時移轉，而不容錯移轉叢集的主機

>適用於：Windows Server 2016、 Microsoft HYPER-V Server 2016 中，Windows Server 2019，Microsoft HYPER-V Server 2019

這篇文章會示範如何設定讓您可以進行即時移轉，其間未叢集化的主機。 如果您未設定即時移轉時已安裝 HYPER-V，或如果您想要變更設定，請使用下列指示。 若要設定叢集主機，使用 容錯移轉叢集工具。  
  
## <a name="requirements-for-setting-up-live-migration"></a>設定即時移轉的需求  
  
若要設定即時移轉的非叢集主機，您將需要：   
  
-  具有使用者帳戶執行各種步驟的權限。 在本機 HYPER-V Administrators 群組或在來源和目的地電腦上的 Administrators 群組的成員資格會符合此需求，除非您設定限制的委派。 網域系統管理員群組的成員資格，才能設定限制的委派。  
  
- 在 Windows Server 2016 或 Windows Server 2012 R2 的來源和目的地伺服器上安裝 HYPER-V 角色。 您可以執行即時移轉虛擬機器至少為執行 Windows Server 2016 和 Windows Server 2012 R2 的主機之間第 5 版。 <br>版本升級的指示，請參閱[更新虛擬機器版本，在 Windows 10 或 Windows Server 2016 的 HYPER-V 中](Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md)。 如需安裝指示，請參閱[Windows Server 上安裝 HYPER-V 角色](../get-started/Install-the-Hyper-V-role-on-Windows-Server.md)。  
  
- 來源和目的地電腦，屬於相同的 Active Directory 網域，或是屬於彼此信任的網域。    
- 除非來源或目的地伺服器上安裝工具，在執行 Windows Server 2016 或 Windows 10 的電腦上安裝 HYPER-V 管理工具會從伺服器中執行工具。  
  
## <a name="consider-options-for-authentication-and-networking"></a>請考慮進行驗證和網路功能選項  
  
請考慮您要設定下列的方式：  
  
-  **驗證**:會使用哪些通訊協定，來驗證來源和目的地伺服器之間的即時移轉流量？ 選擇會決定您是否要登入來源伺服器再開始即時移轉：   
   - Kerberos 可讓您不必登入伺服器，但需要設定限制的委派。 如需相關指示，請參閱下方內容。  
   - CredSSP 可讓您避免設定限制的委派，但需要您登入來源伺服器。 透過本機主控台工作階段，遠端桌面工作階段或遠端的 Windows PowerShell 工作階段，您可以執行這項操作。  
  
      CredSPP 要求可能不明顯的情況下進行登入。 比方說，如果登入 testserver01 將虛擬機器移到 TestServer02，，然後想要將虛擬機器移回 TestServer01，您必須在您嘗試將虛擬機器移回 TestServer01 再登入 TestServer02。 如果您不這樣做，驗證嘗試失敗，發生錯誤，並會顯示下列訊息：  
    
      「 無法位於移轉來源的虛擬機器移轉作業。  
      無法建立與主機的連線*電腦名稱*:無認證可用於安全性套件 0x8009030E。 」
  
-   **效能**:若要設定效能選項的意義會讓它？ 這些選項可以減少網路與 CPU 使用率，以及進行更快速的即時移轉。 請考慮您的需求和您的基礎結構，並測試不同的設定，幫助您決定。 步驟 2 的結尾將描述這些選項。  
  
-  **網路喜好設定**:您願意讓即時移轉流量通過任何可用的網路，或將流量隔離到特定網路？ 最佳的安全性做法，建議您將流量隔離到受信任的私用網路，因為在網路上傳送即時移轉時不會加密。 透過實際隔離的網路或另一種受信任的網路技術 (例如 Vlan) 可以達成網路隔離。  
  
## <a name="BKMK_Step1"></a>步驟 1:設定限制的委派 （選擇性）  
如果您已決定使用 Kerberos 來驗證即時移轉流量，設定限制的委派使用的帳戶是 Domain Administrators 群組的成員。  
  
### <a name="use-the-users-and-computers-snap-in-to-configure-constrained-delegation"></a>使用 [使用者和電腦] 嵌入式管理單元來設定限制的委派  
  
1.  開啟 [Active Directory 使用者和電腦] 嵌入式管理單元。 (如果未選取，從 [伺服器管理員] 中，選取伺服器，請按一下**工具** >> **Active Directory 使用者和電腦**)。  
  
2.  從瀏覽窗格**Active Directory 使用者和電腦**、 選取網域，然後按兩下**電腦**資料夾。  
  
3.  從**電腦**資料夾，以滑鼠右鍵按一下來源伺服器的電腦帳戶，然後再按一下**屬性**。  
  
4.  從**屬性**，按一下**委派** 索引標籤。  
  
5.  在 [委派] 索引標籤上選取**信任這台電腦，但只針對指定服務的委派**，然後選取**使用任何驗證通訊協定**。  
  
6.  按一下 **\[新增\]**。  
  
7.  從**新增服務**，按一下**使用者或電腦**。  
  
8.  從**選取使用者或電腦**，輸入目的地伺服器的名稱。 按一下 **檢查名稱**來確認，然後按一下**確定**。  
  
9. 從**新增服務**，在可用服務清單中，執行下列動作，然後按一下**確定**:  
  
    -   若要移動虛擬機器存放裝置，請選取 [cifs] 。 如果您想要移動虛擬機器，以及儲存體也如同您想要移動只有虛擬機器的儲存體，這是必要的。 如果伺服器設定為在 Hyper-V 使用 SMB 存放裝置，這個選項應該已經選取。  
  
    -   若要移動虛擬機器，請選取 [Microsoft 虛擬系統移轉服務]。  
  
10. 在 [內容] 對話方塊的 [委派]  索引標籤上，確認您在上一個步驟中選取的服務已經列示為目的電腦可以顯示委派認證的服務。 按一下 [確定] 。  
  
11. 從 [Computers]  資料夾，選取目的地伺服器的電腦帳戶，然後重複這個程序。 在 [選取使用者或電腦] 對話方塊中，確定指定了來源伺服器的名稱。  
  
下列兩個動作發生之後，組態變更才會生效：  
  
  -  所做的變更會複寫到執行 HYPER-V 的伺服器登入的網域控制站。  
  -  網域控制站發出新的 Kerberos 票證。  
  
## <a name="BKMK_Step2"></a>步驟 2:設定即時移轉的來源和目的地電腦  
這個步驟包括選擇進行驗證和網路功能選項。 最佳安全性做法，我們建議您選取特定的網路，以用於即時移轉流量，如上面所討論。 此步驟也示範如何選擇效能選項。   
  
### <a name="use-hyper-v-manager-to-set-up-the-source-and-destination-computers-for-live-migration"></a>若要設定即時移轉的來源和目的地電腦使用 HYPER-V 管理員  
  
1.  開啟 \[Hyper-V 管理員\]。 (從 [伺服器管理員] 中，按一下**工具** >>**HYPER-V 管理員**。)  
  
2.  在 [導覽] 窗格中，選取其中一個伺服器。 (如果未列出，以滑鼠右鍵按一下**HYPER-V 管理員**，按一下**連接到伺服器**，輸入伺服器名稱，然後按一下**確定**。 若要新增更多伺服器重複。）  
  
3.  在 **動作**窗格中，按一下**HYPER-V 設定** >>**即時移轉**。  
  
4.  在 [即時移轉]  窗格中，核取 [啟用連入與連出即時移轉] 。  
  
5.  底下**同時即時移轉**，指定不同的數字，如果您不想要使用預設值是 2。  
  
6.  在 [連入即時移轉] 下，如果您想要使用特定網路連線來接受即時移轉流量，請按一下 [新增]  以便輸入 IP 位址資訊。 否則，請按一下 [使用任何可用的網路來進行即時移轉] 。 按一下 [確定] 。  
  
7.  若要選擇 Kerberos 和 效能選項，依序展開**即時移轉**，然後選取**進階功能**。  
  
    - 如果您已設定限制的委派，底下**驗證通訊協定**，選取**Kerberos**。  
    - 底下**效能選項**，檢閱詳細資料，然後選擇不同的選項，它是否適合您的環境。   
  
8. 按一下 [確定] 。  
  
9. 在 HYPER-V 管理員中選取另一部伺服器，並重複步驟。  
  
### <a name="use-windows-powershell-to-set-up-the-source-and-destination-computers-for-live-migration"></a>使用 Windows PowerShell 來設定即時移轉的來源和目的地電腦  
  
三個 cmdlet 可供非叢集主機上設定即時移轉：[Enable-vmmigration](https://technet.microsoft.com/library/hh848544.aspx)， [Set-vmmigrationnetwork](https://technet.microsoft.com/library/hh848467.aspx)，以及[Set-vmhost](https://technet.microsoft.com/library/hh848524.aspx)。 這個範例會使用這三個，且會執行下列作業：   
  - 設定本機主機上的即時移轉  
  - 只在特定的網路上允許連入移轉流量  
  - 選擇 Kerberos 的驗證通訊協定   
  
每一列代表一個個別的命令。  
  
```  
PS C:\> Enable-VMMigration  
  
PS C:\> Set-VMMigrationNetwork 192.168.10.1  
  
PS C:\> Set-VMHost -VirtualMachineMigrationAuthenticationType Kerberos  
  
```  
Set-vmhost 也可讓您選擇的效能選項 （以及許多其他的主機設定）。 例如，若要選擇 SMB，但保留設為預設值的 CredSSP 驗證通訊協定，請輸入：  
  
```  
PS C:\> Set-VMHost -VirtualMachineMigrationPerformanceOption SMBTransport  
  
```  
  
下表描述的效能選項的運作方式。  
  
|選項|描述|  
|----------|---------------|  
    |TCP/IP|複製到目的地伺服器的虛擬機器的記憶體，透過 TCP/IP 連線。|  
    |壓縮|透過 TCP/IP 連線複製到目的地伺服器之前壓縮虛擬機器的記憶體內容。 **注意：** 這是**預設**設定。|  
    |SMB|複製到目的地伺服器的虛擬機器的記憶體，透過 SMB 3.0 連線。<br /><br />-使用 SMB 直接傳輸是當來源和目的地伺服器上的網路介面卡已啟用遠端直接記憶體存取 (RDMA) 功能。<br />-SMB 多重通道會自動偵測，並識別出適當的 SMB 多重通道組態時，會使用多個連線。<br /><br />如需詳細資訊，請參閱[使用 SMB 直接傳輸改善檔案伺服器的效能](https://technet.microsoft.com/library/jj134210(WS.11).aspx)。|  
      
 ## <a name="next-steps"></a>後續步驟
 
 設定主機之後，您即可執行即時移轉。 如需相關指示，請參閱 <<c0> [ 使用而不需要容錯移轉叢集的即時移轉來移動虛擬機器](../manage/Use-live-migration-without-Failover-Clustering-to-move-a-virtual-machine.md)。 
    


