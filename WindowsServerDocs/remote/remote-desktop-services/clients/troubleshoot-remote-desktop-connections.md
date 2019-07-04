---
title: 針對遠端桌面連線進行疑難排解
description: 針對依徵兆排列的程序進行疑難排解
ms.custom: na
ms.reviewer: rklemen; josh.bender
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: troubleshooting
ms.assetid: ''
author: RobVyK
manager: ''
ms.author: kaushika; rklemen; josh.bender; v-tea
ms.date: 02/22/2019
ms.localizationpriority: medium
ms.openlocfilehash: c6ce719ffa24cfc6704348c17548fe5cf33d9271
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2019
ms.locfileid: "67284141"
---
# <a name="troubleshooting-remote-desktop-connections"></a>針對遠端桌面連線進行疑難排解
如需最常見幾個遠端桌面服務 (RDS) 問題的簡短說明，請參閱[關於遠端桌面用戶端的常見問題集](https://review.docs.microsoft.com/en-us/windows-server/remote/remote-desktop-services/clients/remote-desktop-client-faq)。 本文會說明數個更進階的方法以針對連線問題進行疑難排解。 無論您是針對簡單的設定進行疑難排解，例如一部實體電腦連線到另一部實體電腦，或是針對更複雜的設定進行疑難排解，許多這些程序皆適用。 部分程序可處理只在更複雜的多使用者案例中發生的問題。 有關遠端桌面元件及其如何共同運作的詳細資訊，請參閱[遠端桌面服務架構](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/desktop-hosting-logical-architecture)。

> [!NOTE]  
> 許多本文所述的程序需要您存取多部電腦，其中有些可能要從遠端存取。 有關遠端系統管理工具及其設定方式的詳細資訊，請參閱[適用於 Windows 作業系統的遠端伺服器管理工具 (RSAT)](https://support.microsoft.com/en-us/help/2693643/remote-server-administration-tools-rsat-for-windows-operating-systems)。

在下列清單中，找出您 (或使用者) 遭遇到的徵兆類型。

- [遠端桌面用戶端無法連線到遠端桌面，但沒有任何特定徵兆或訊息 (一般疑難排解步驟)](#no-specific-symptoms-or-messages-general-troubleshooting-steps)
- [遠端桌面用戶端無法連線到遠端桌面，且收到「類別未登錄」訊息](#client-cannot-connect-class-not-registered)
- [遠端桌面用戶端無法連線到遠端桌面，且收到「沒有可用授權」或「安全性錯誤」訊息](#client-cannot-connect-no-licenses-available)
- [使用者收到「拒絕存取」訊息，或必須提供認證兩次](#user-cannot-authenticate-or-must-authenticate-twice)
- [在連線時收到「遠端桌面服務目前忙碌中」訊息](#on-connecting-user-receives-remote-desktop-service-is-currently-busy-message)
- [遠端桌面用戶端連線中斷，且無法重新連線至同一工作階段](#rd-client-disconnects-and-cannot-reconnect-to-the-same-session)
- [使用者透過無線網路連線到遠端膝上型電腦，接著膝上型電腦網路連線中斷](#remote-laptop-disconnects-from-wireless-network)。
- [使用者遇到效能不佳的問題或遠端應用程式問題](#user-experiences-poor-performance-or-application-problems)

> [!NOTE]  
> 如需目前的 RDP 中斷連線程式碼清單，請參閱 [ExtendedDisconnectReasonCode 列舉](https://docs.microsoft.com/windows/desktop/TermServ/extendeddisconnectreasoncode)。 

## <a name="no-specific-symptoms-or-messages-general-troubleshooting-steps"></a>沒有特定徵兆或訊息 (一般疑難排解步驟)

當遠端桌面用戶端無法連線到遠端桌面，但未提供可協助找出原因的訊息或其他徵兆，請使用下列步驟。 若要解決許多這類問題最常見的原因，請使用下列方法：

- [檢查 RDP 通訊協定的狀態](#check-the-status-of-the-rdp-protocol)
- [檢查 RDP 服務的狀態](#check-the-status-of-the-rdp-services)
- [檢查 RDP 接聽程式是否正常運作](#check-that-the-rdp-listener-is-functioning)
- [檢查 RDP 接聽程式連接埠](#check-the-rdp-listener-port)

### <a name="check-the-status-of-the-rdp-protocol"></a>檢查 RDP 通訊協定的狀態

#### <a name="check-the-status-of-the-rdp-protocol-on-a-local-computer"></a>檢查本機電腦上 RDP 通訊協定的狀態

若要檢查並變更本機電腦上 RDP 通訊協定的狀態，請參閱[如何啟用遠端桌面](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/clients/remote-desktop-allow-access#how-to-enable-remote-desktop)。

> [!NOTE]  
> 若無法使用遠端桌面選項，請參閱[檢查群組原則物件是否封鎖 RDP](#check-whether-a-group-policy-object-gpo-is-blocking-rdp-on-a-local-computer)。

#### <a name="check-the-status-of-the-rdp-protocol-on-a-remote-computer"></a>檢查遠端電腦上 RDP 通訊協定的狀態

> [!IMPORTANT]  
> 請仔細依循本節中的步驟。 如果您未正確修改登錄，可能會發生嚴重問題。 在修改之前，[備份登錄以供還原](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22)，以免發生問題。

若要檢查並變更遠端電腦上 RDP 通訊協定的狀態，請使用網路登錄連線：

1. 選取 [開始]  ，選取 [執行]  ，然後輸入 **regedt32**。
2. 在登錄編輯程式中，選取 [檔案]  ，然後選取 [連線網路登錄]  。
3. 在 [選取電腦]  對話方塊中，輸入遠端電腦的名稱，選取 [檢查名稱]  ，然後選取 [確定]  。
4. 巡覽至 **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server**。  
   ![登錄編輯程式，顯示 fDenyTSConnections 項目](../media/troubleshoot-remote-desktop-connections/RegEntry_fDenyTSConnections.png)
   - 如果 **fDenyTSConnections** 金鑰值為 **0**，則會啟用 RDP
   - 如果 **fDenyTSConnections** 金鑰值為 **1**，則會停用 RDP
5. 若要啟用 RDP，請將 **fDenyTSConnections** 的值從 **1** 變更為 **0**。

#### <a name="check-whether-a-group-policy-object-gpo-is-blocking-rdp-on-a-local-computer"></a>檢查群組原則物件 (GPO) 是否會封鎖本機電腦上的 RDP

如果您無法開啟使用者介面中的 RDP，或如果 **fDenyTSConnections** 的值在您變更後還原成 **1**，則 GPO 可能覆寫電腦層級設定。

若要檢查本機電腦上的群組原則設定，請以系統管理員身分開啟命令提示字元視窗，並輸入下列命令：
```
gpresult /H c:\gpresult.html
```
此命令完成後，開啟 gpresult.html。 在**電腦設定\\系統管理範本\\Windows 元件\\遠端桌面服務\\遠端桌面工作階段主機\\連線**中，尋找**允許使用者使用遠端桌面服務從遠端連線**原則。

- 如果此原則的設定為**已啟用**，則群組原則不會封鎖 RDP 連線。
- 如果此原則的設定為**已停用**，請檢查**優勢 GPO**。 這是封鎖 RDP 連線的 GPO。
  ![Gpresult.html 的範例區段，在其中網域層級 GPO **封鎖 RDP** 會停用 RDP。](../media/troubleshoot-remote-desktop-connections/GPResult_RDSH_Connections_GP.png)
   
  ![Gpresult.html 的範例區段，在其中 **本機群組原則** 會停用 RDP。](../media/troubleshoot-remote-desktop-connections/GPResult_RDSH_Connections_LGP.png)

#### <a name="check-whether-a-gpo-is-blocking-rdp-on-a-remote-computer"></a>檢查 GPO 是否會封鎖遠端電腦上的 RDP

若要檢查遠端電腦上的群組原則設定，命令與用於本機電腦幾乎一樣：
```
gpresult /S <computer name> /H c:\gpresult-<computer name>.html
```
此命令產生的檔案 (**gpresult-\<computer name\>.html**) 與本機電腦版本 (**gpresult.html**) 會使用相同的資訊格式。

#### <a name="modifying-a-blocking-gpo"></a>修改封鎖 GPO

您可以在群組原則物件編輯器 (GPE) 和群組原則管理主控台 (GPM) 中修改這些設定。 如需如何使用群組原則的詳細資訊，請參閱[進階群組原則管理](https://docs.microsoft.com/microsoft-desktop-optimization-pack/agpm/)。

若要修改封鎖原則，請使用下列其中一種方法：

- 在 GPE 中，存取適當的 GPO 層級 (例如本機或網域)，並巡覽至**電腦設定\\系統管理範本\\Windows 元件\\遠端桌面服務\\遠端桌面工作階段主機\\連線**\\**允許使用者使用遠端桌面服務從遠端連線**。  
   1. 將原則設為**已啟用**或**未設定**。
   2. 在受影響的電腦上，以系統管理員身分開啟命令提示字元視窗，並執行 **gpupdate /force** 命令。
- 在 GPM 中巡覽到 OU，其中的封鎖原則已套用至受影響的電腦，從 OU 中刪除該原則。

### <a name="check-the-status-of-the-rdp-services"></a>檢查 RDP 服務的狀態

在本機 (用戶端) 電腦和遠端 (目標) 電腦上，應執行下列服務：

- 遠端桌面服務 (TermService)
- 遠端桌面服務使用者模式連接埠重新導向器 (UmRdpService)

您可以使用 [服務] MMC 嵌入式管理單元，在本機或遠端管理服務。 您也可以在本機或遠端使用 PowerShell (如果遠端電腦設定為可接受遠端的 PowerShell 命令)。

![[服務] MMC 嵌入式管理單元中的遠端桌面服務。 請勿修改預設的服務設定。](../media/troubleshoot-remote-desktop-connections/RDSServiceStatus.png)

在其中一台電腦上，如果其中一個或兩個服務皆未執行，請將其啟動。

> [!NOTE]  
> 如果您啟動「遠端桌面服務」服務，按一下 [是]  以自動重新啟動「遠端桌面服務使用者模式連接埠重新導向器」服務。

### <a name="check-that-the-rdp-listener-is-functioning"></a>檢查 RDP 接聽程式是否正常運作

> [!IMPORTANT]  
> 請仔細依循本節中的步驟。 如果您未正確修改登錄，可能會發生嚴重問題。 在修改之前，[備份登錄以供還原](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22)，以免發生問題。

#### <a name="check-the-status-of-the-rdp-listener"></a>檢查 RDP 接聽程式的狀態

針對此程序，使用具有系統管理權限的 PowerShell 執行個體。 針對本機電腦，您也可以使用具有系統管理權限的命令提示字元。 不過，此程序使用 PowerShell，因為相同的命令在本機和遠端皆可運作。

1. 開啟 PowerShell 視窗。 若要連線到遠端電腦，請輸入 **Enter-PSSession -ComputerName \<電腦名稱\>** 。
2. 輸入 **qwinsta**。 
    ![qwinsta 命令會列出電腦連接埠上接聽的處理程序。](../media/troubleshoot-remote-desktop-connections/WPS_qwinsta.png)
3. 如果清單包含 **rdp-tcp** 且狀態為**接聽**，則 RDP 接聽程式運作正常。 繼續[檢查 RDP 接聽程式連接埠](#check-the-rdp-listener-port)。 否則，請繼續執行步驟 4。
4. 從工作電腦匯出 RDP 接聽程式設定。
    1. 登入與受影響電腦具有相同作業系統版本的電腦，並存取該電腦的登錄 (例如，藉由使用登錄編輯程式)。
    2. 巡覽至下列登錄項目：  
        **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server\\WinStations\\RDP-Tcp**
    3. 將項目匯出為 .reg 檔案。 例如，在登錄編輯程式中，以滑鼠右鍵按一下項目，選取 [匯出]  ，然後輸入匯出設定的檔案名稱。
    4. 將匯出的 .reg 檔案複製到受影響的電腦。
5. 若要匯入 RDP 接聽程式設定，請在受影響的電腦上開啟具有系統管理權限的 PowerShell 視窗 (或開啟 PowerShell 視窗並從遠端連線到受影響的電腦)。
   1. 若要備份現有的登錄項目，請輸入下列命令：  
   
      ```powershell  
      cmd /c 'reg export "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-tcp" C:\Rdp-tcp-backup.reg'   
      ```  
   

   2. 若要移除現有的登錄項目，請輸入下列命令：  
   
      ```powershell  
      Remove-Item -path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-tcp' -Recurse -Force  
      ```
   
   3. 若要匯入新的登錄項目，並重新啟動服務，請輸入下列命令：  
   
      ```powershell  
      cmd /c 'regedit /s c:\<filename>.reg'  
      Restart-Service TermService -Force  
      ```   
      \<filename\> 是匯出的 .reg 檔案名稱。
6. 再次嘗試遠端桌面連線以測試設定。 如果您仍然無法連線，請重新啟動受影響的電腦。
7. 如果您仍然無法連線，請[檢查 RDP 自我簽署憑證的狀態](#check-the-status-of-the-rdp-self-signed-certificate)。

#### <a name="check-the-status-of-the-rdp-self-signed-certificate"></a>檢查 RDP 自我簽署憑證的狀態

1. 如果您仍然無法連線，請開啟 [憑證] MMC 嵌入式管理單元。 當系統提示您選取要管理的憑證存放區，請選取**電腦帳戶**，然後選取受影響的電腦。
2. 在 [遠端桌面]  下方的 [憑證]  資料夾中，刪除 RDP 自我簽署憑證。 
    ![MMC 憑證嵌入式管理單元中的遠端桌面憑證。](../media/troubleshoot-remote-desktop-connections/MMCCert_Delete.png)
3. 在受影響的電腦上，重新啟動「遠端桌面服務」服務。
4. 重新整理 [憑證] 嵌入式管理單元。
5. 如果 RDP 自我簽署憑證尚未重建，請[檢查 MachineKeys 資料夾的權限](#check-the-permissions-of-the-machinekeys-folder)。

#### <a name="check-the-permissions-of-the-machinekeys-folder"></a>檢查 MachineKeys 資料夾的權限

1. 在受影響的電腦上開啟 Explorer，然後巡覽至 **C:\\ProgramData\\Microsoft\\Crypto\\RSA\\** 。
2. 以滑鼠右鍵按一下 **MachineKeys**，選取 [屬性]  ，選取 [安全性]  ，然後選取 [進階]  。
3. 確認已設定下列權限：
      - 內建\\系統管理員：完全控制
      - 所有人：讀取、寫入

### <a name="check-the-rdp-listener-port"></a>檢查 RDP 接聽程式連接埠

在本機 (用戶端) 電腦和遠端 (目標) 電腦上，RDP 接聽程式應在連接埠 3389 上進行接聽。 其他應用程式不應使用此連接埠。

> [!IMPORTANT]  
> 請仔細依循本節中的步驟。 如果您未正確修改登錄，可能會發生嚴重問題。 在修改之前，[備份登錄以供還原](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22)，以免發生問題。

若要檢查或變更 RDP 連接埠，請使用登錄編輯程式：

1. 選取 [開始]，選取 [執行]  ，然後輸入 **regedt32**。
      - 若要連線至遠端電腦，選取 [檔案]  ，然後選取 [連線網路登錄]  。
      - 在 [選取電腦]  對話方塊中，輸入遠端電腦的名稱，選取 [檢查名稱]  ，然後選取 [確定]  。
2. 開啟登錄並巡覽至 **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server\\WinStations\\\<listener\>** 。 
    ![RDP 通訊協定的 PortNumber 子機碼。](../media/troubleshoot-remote-desktop-connections/RegEntry_PortNumber.png)
3. 如果 **PortNumber** 的值不是 **3389**，將其變更為 **3389**。 
   > [!IMPORTANT]  
    > 您可以使用其他連接埠來操作遠端桌面服務。 不過，我們不建議您這麼做。 針對這類設定進行疑難排解已超出本文範圍。
4. 變更連接埠號碼之後，重新啟動「遠端桌面服務」服務。

#### <a name="check-that-another-application-is-not-trying-to-use-the-same-port"></a>確認其他應用程式並未嘗試使用同一連接埠

針對此程序，使用具有系統管理權限的 PowerShell 執行個體。 針對本機電腦，您也可以使用具有系統管理權限的命令提示字元。 不過，此程序使用 PowerShell，因為相同的命令在本機和遠端皆可運作。

1. 開啟 PowerShell 視窗。 若要連線到遠端電腦，請輸入 **Enter-PSSession -ComputerName \<電腦名稱\>** 。
2. 輸入下列命令：  
   
     ```powershell  
    cmd /c 'netstat -ano | find "3389"'  
    ```
  
    ![Netstat 命令會產生連接埠和接聽服務的清單。](../media/troubleshoot-remote-desktop-connections/WPS_netstat.png)
3. 尋找具有 **Listening** 狀態之 TCP 連接埠 3389 (或指派的 RDP 連接埠) 的項目。 
    > [!NOTE]  
   > 使用該連接埠之處理程序或服務的 PID (處理程序識別碼) 會顯示在 [PID] 欄位底下。
4. 若要判斷哪個應用程式正在使用連接埠 3389 (或指派的 RDP 連接埠)，請輸入下列命令：  
   
     ```powershell  
    cmd /c 'tasklist /svc | find "<pid listening on 3389>"'  
    ```  
  
    ![Tasklist 命令會回報特定處理程序的詳細資料。](../media/troubleshoot-remote-desktop-connections/WPS_tasklist.png)
5. 尋找與此連接埠相關聯之 PID 號碼的項目 (根據 **netstat** 輸出)。 與該 PID 相關聯的服務或處理程序會顯示在右側。
6. 如果應用程式或遠端桌面服務 (TermServ.exe) 以外的服務正在使用該連接埠，您可以使用下列方法之一來解決衝突：
      - 設定其他應用程式或服務使用不同的連接埠 (建議)。
      - 解除安裝其他應用程式或服務。
      - 設定 RDP 使用不同的連接埠，然後重新啟動「遠端桌面服務」服務 (不建議)。

#### <a name="check-whether-a-firewall-is-blocking-the-rdp-port"></a>檢查防火牆是否封鎖 RDP 連接埠

使用 **psping** 工具測試是否可藉由使用連接埠 3389 來觸達受影響的電腦。

1. 在受影響電腦以外的電腦上，從 <https://live.sysinternals.com/psping.exe> 下載 **psping**。
2. 以系統管理員身分開啟命令提示字元視窗，變更至您安裝 **psping** 所在的目錄，然後輸入下列命令：  
   
   ```  
   psping -accepteula <computer IP>:3389  
   ```
   
3. 檢查 **psping** 命令的輸出是否有如下結果：  
      - **連線到\<電腦 IP\>** ：遠端電腦可連線。
      - **(0% 遺失)** ：所有連線嘗試皆成功。
      - **遠端電腦拒絕網路連線**：遠端電腦無法連線。
      - **(100% 遺失)** ：所有連線嘗試皆失敗。
1. 在多部電腦上執行 **psping** 來測試能否連線到受影響的電腦。
1. 請注意受影響的電腦是否封鎖來自所有其他電腦、部分其他電腦，或者僅一部其他電腦的連線。
1. 建議的後續步驟：
      - 請連絡網路系統管理員，確認網路允許 RDP 流量傳送到受影響的電腦。
      - 調查來源電腦與受影響電腦之間的任何防火牆設定 (包括受影響電腦上的 Windows 防火牆)，以判斷是否有防火牆封鎖 RDP 連接埠。

## <a name="client-cannot-connect-class-not-registered"></a>用戶端無法連線，「類別未登錄」

當您嘗試使用執行 Windows 10 版本 1709 或更新版本的用戶端來連線到遠端電腦，用戶端可能無法連線，且遠端桌面工作階段主機伺服器會回報包含「類別未登錄 (0x80040154)」錯誤碼的訊息。

如果嘗試連線的使用者具有強制使用者設定檔，就會發生此問題。 若要解決此問題，請安裝 [2018 年 7 月 24 日—KB4338817 (作業系統組建 16299.579)](https://support.microsoft.com/en-us/help/4338817/windows-10-update-kb4338817) Windows 10 更新。

## <a name="client-cannot-connect-no-licenses-available"></a>用戶端無法連線，沒有可用授權

這種情況會發生在包含 RDSH 伺服器和遠端桌面授權伺服器的部署。

找出使用者看到的是哪種行為：

- 工作階段已中斷連線，因為沒有可用授權或沒有可用授權伺服器
- 由於安全性錯誤而拒絕存取

以網域系統管理員身分登入 RD 工作階段主機，並開啟 RD 授權診斷程式。 尋找如下的訊息：

  - 遠端桌面工作階段主機伺服器的寬限期已過期，但 RD 工作階段主機伺服器尚未透過任何授權伺服器進行設定。 RD 工作階段主機伺服器的連線將遭到拒絕，除非授權伺服器針對 RD 工作階段主機伺服器進行設定。
  - 授權伺服器\<電腦名稱\>無法使用。 這可能是網路連線問題所造成，在授權伺服器上「遠端桌面授權」服務已停止，或 RD 授權已不再安裝於電腦上。

這些問題通常與下列使用者訊息相關聯：

  - 遠端工作階段已中斷連線，因為此電腦沒有可用的遠端桌面用戶端存取使用權。
  - 遠端工作階段已中斷連線，因為沒有可用的遠端桌面授權伺服器可提供授權。

在此情況下，[設定 RD 授權服務](#configure-the-rd-licensing-service)。

如果 RD 授權診斷程式列出其他問題，例如「RDP 通訊協定元件 X.224 在通訊協定串流中偵測到錯誤，並已中斷連線用戶端」，則可能存在影響授權憑證的問題。 這類問題通常與如下的使用者訊息相關聯：

由於安全性錯誤，用戶端無法連線到終端機伺服器。 在確定您已登入網路後，再次嘗試連線到伺服器。

在此情況下，[重新整理 X509 憑證登錄機碼](#refresh-the-x509-certificate-registry-keys)。

### <a name="configure-the-rd-licensing-service"></a>設定 RD 授權服務

下列程序會使用伺服器管理員進行設定變更。 有關如何設定及使用伺服器管理員的資訊，請參閱[伺服器管理員](https://docs.microsoft.com/windows-server/administration/server-manager/server-manager)。

1. 開啟 [伺服器管理員]，然後巡覽至**遠端桌面服務**。
2. 在 [部署概觀]  中，選取 [工作]  ，然後選取 [編輯部署內容]  。
3. 選取 [RD 授權]  ，然後為您的部署選取適當的授權模式 (**每一裝置**或**每一使用者**)。
4. 輸入 RD 授權伺服器的完整網域名稱 (FQDN)，然後選取 [新增]  。
5. 如果您有多部 RD 授權伺服器，請為每部伺服器重複步驟 4。 
    ![伺服器管理員中的 RD 授權伺服器設定選項。](../media/troubleshoot-remote-desktop-connections/RDLicensing_Configure.png)

### <a name="refresh-the-x509-certificate-registry-keys"></a>重新整理 X509 憑證登錄機碼

> [!IMPORTANT]  
> 請仔細依循本節中的步驟。 如果您未正確修改登錄，可能會發生嚴重問題。 在修改之前，[備份登錄以供還原](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22)，以免發生問題。

若要解決此問題，請備份並移除 X509 憑證登錄機碼，重新啟動電腦，然後重新啟動 RD 授權伺服器。 若要這樣做，請執行下列步驟。

> [!NOTE]  
> 在每個 RDSH 伺服器上執行下列程序。

1. 開啟登錄編輯程式，並巡覽至 **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server\\RCM**。
2. 在 [登錄] 功能表上，選取 [匯出登錄檔案]  。
3. 在 [檔案名稱]  方塊中鍵入**匯出- 憑證**，然後選取 [儲存]  。
4. 以滑鼠右鍵按一下以下每個值，選取 [刪除]  ，然後選取 [是]  以確認刪除：  
      - **憑證**
      - **X509 憑證**
      - **X509 憑證識別碼**
      - **X509 憑證 2**
5. 結束登錄編輯程式，然後重新啟動 RDSH 伺服器。

## <a name="user-cannot-authenticate-or-must-authenticate-twice"></a>使用者無法驗證，或必須驗證兩次

有幾個問題可能會造成影響使用者驗證的問題。 本節會說明下列案例：

  - [使用者存取遭拒且出現「限制的登入類型」訊息](#access-denied-restricted-type-of-logon)
  - [使用者或應用程式存取遭拒且出現事件 16965，「SAM 資料庫的遠端呼叫已遭到拒絕」](#access-denied-a-remote-call-to-the-sam-database-has-been-denied)
  - [使用者無法使用智慧卡登入](#a-user-cannot-sign-in-by-using-a-smart-card)
  - [如果遠端電腦已鎖定，使用者必須輸入密碼兩次](#if-the-remote-pc-is-locked-a-user-needs-to-enter-a-password-twice)
  - [使用者無法登入且收到「驗證錯誤」和「CredSSP 加密預示修復」訊息](#user-cannot-sign-in-and-receives-authentication-error-and-credssp-encryption-oracle-remediation-messages)
  - [更新用戶端電腦之後，部分使用者需要登入兩次](#after-you-update-client-computers-some-users-need-to-sign-in-twice)。
  - [使用者在某部署上存取遭拒，該部署使用具多個 RD 連線代理人的 RemoteGuard](#users-are-denied-access-on-a-deployment-that-uses-remote-credential-guard-with-multiple-rd-connection-brokers)

### <a name="access-denied-restricted-type-of-logon"></a>拒絕存取，限制的登入類型

在此情況下，嘗試連線到 Windows 10 或 Windows Server 2016 電腦的 Windows 10 使用者存取遭拒，並出現下列訊息：

> 遠端桌面連線：  
> 系統管理員已限制您可以使用的登入類型 (網路或互動式)。 如需協助，請連絡系統管理員或技術支援。

當網路層級驗證 (NLA) 為 RDP 連線所需，且使用者不是**遠端桌面使用者**群組的成員，就會發生這個問題。 如果**遠端桌面使用者**群組未指派給**從網路存取此電腦**使用者權限，也會發生這個問題。

若要解決此問題，請依循下列步驟之一：

  - [修改使用者的群組成員資格或使用者權限指派](#modify-the-users-group-membership-or-user-rights-assignment)。
  - 關閉 NLA (不建議)。
  - 使用 Windows 10 以外的遠端桌面用戶端。 例如，Windows 7 用戶端並沒有此問題。

#### <a name="modify-the-users-group-membership-or-user-rights-assignment"></a>修改使用者的群組成員資格或使用者權限指派

如果此問題影響的是單一使用者，則此問題最簡單的解決方案是將使用者新增至**遠端桌面使用者**群組。

如果使用者已經是該群組的成員 (或如果多個群組成員有相同問題)，請檢查遠端 Windows 10 或 Windows Server 2016 電腦上的使用者權限設定。

1. 開啟群組原則物件編輯器 (GPE)，並連線到遠端電腦的本機原則。
2. 巡覽至**電腦設定\\Windows 設定\\安全性設定\\本機原則\\使用者權限指派**，以滑鼠右鍵按一下 [從網路存取此電腦]  ，然後選取 [內容]  。
3. 針對**遠端桌面使用者** (或父群組) 檢查使用者和群組的清單。
4. 如果清單不包含**遠端桌面使用者** (或父群組，例如**所有人**)，您必須將其新增至清單 (如果您在部署中有超過一部或兩部電腦，請使用群組原則物件)。  
    例如，**從網路存取此電腦**的預設成員資格包含**所有人**。 如果您的部署使用群組原則物件來移除**所有人**，您可能需要更新群組原則物件來新增**遠端桌面使用者**，以便還原存取。

### <a name="access-denied-a-remote-call-to-the-sam-database-has-been-denied"></a>拒絕存取，SAM 資料庫的遠端呼叫已遭到拒絕

如果您的網域控制站執行 Windows Server 2016 或更新版本，且使用者嘗試使用自訂連線應用程式來連線，則很有可能發生此行為。 特別是，在 Active Directory 中存取使用者設定檔資訊的應用程式將被拒絕存取。

此行為起因於 Windows 的變更。 在 Windows Server 2012 R2 及較舊版本中，當使用者登入遠端桌面，遠端連線管理員 (RCM) 會連絡網域控制站 (DC) 來查詢 Active Directory Domain Services (AD DS) 中使用者物件上遠端桌面的特定設定。 這項資訊會顯示在 [Active Directory 使用者和電腦] MMC 嵌入式管理單元中使用者物件屬性的 [遠端桌面服務設定檔] 索引標籤。

從 Windows Server 2016 開始，RCM 不會再查詢 AD DS 中的使用者物件。 如果因為您正在使用遠端桌面服務屬性，而需要 RCM 查詢 AD DS，則您必須手動啟用先前的 RCM 行為。

> [!IMPORTANT]  
> 請仔細依循本節中的步驟。 如果您未正確修改登錄，可能會發生嚴重問題。 在修改之前，[備份登錄以供還原](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22)，以免發生問題。

若要啟用 RD 工作階段主機伺服器上的舊版 RCM 行為，請設定下列登錄項目，然後重新啟動**遠端桌面服務**服務：  
  - **HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Policies\\Microsoft\\Windows NT\\Terminal Services**
  - **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server\\WinStations\\\<Winstation name\>\\**  
      - 名稱：**fQueryUserConfigFromDC**
      - 輸入：**Reg\_DWORD**
      - 值：**1** (十進位)

若要在 RD 工作階段主機伺服器以外的伺服器上啟用舊版 RCM 行為，請設定這些登錄項目以及下列額外登錄項目 (然後重新啟動服務)：
  - **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server**

有關此行為的詳細資訊，請參閱 KB 3200967 [在 Windows Server 中變更遠端連線管理員](https://support.microsoft.com/en-us/help/3200967/changes-to-remote-connection-manager-in-windows-server)。

### <a name="a-user-cannot-sign-in-by-using-a-smart-card"></a>使用者無法使用智慧卡登入

本文會說明使用者無法使用智慧卡登入遠端桌面的三種常見情況：  
  - [使用者無法登入使用唯讀網域控制站 (RODC) 的分公司](#cannot-sign-in-with-a-smart-card-in-a-branch-office-with-a-rodc)
  - [使用者無法登入最近更新的 Windows Server 2008 SP2 電腦](#cannot-sign-in-with-a-smart-card-to-a-windows-server-2008-sp2-computer)
  - [使用者無法保持登入最近更新的 Windows 電腦，且「遠端桌面服務」服務沒有回應 (停止回應)](#cannot-stay-signed-in-with-a-smart-card-and-remote-desktop-services-service-hangs)

#### <a name="cannot-sign-in-with-a-smart-card-in-a-branch-office-with-a-rodc"></a>無法使用智慧卡登入具有 RODC 的分公司

此問題會發生在包含 RDSH 伺服器的部署中，該伺服器位於使用 RODC 的分公司網站。 RDSH 伺服器裝載在根網域中。 在分公司網站的使用者屬於子網域，且使用智慧卡進行驗證。 RODC 已設定快取使用者密碼 (RODC 屬於**允許的 RODC 密碼複寫群組**)。 當使用者嘗試登入 RDSH 伺服器上的工作階段，使用者會收到訊息如「登入嘗試無效。 因為使用者名稱或驗證資訊不正確。」

這是根 DC 與 RODC 管理使用者認證加密方式的已知問題。 根 DC 會使用加密金鑰來加密認證，而 RODC 會向用戶端提供解密金鑰。 不過，兩個金鑰不相符。

若要解決此問題，請考慮變更您的 DC 拓撲，您可以關閉 RODC 上的密碼快取，或將可寫入的 DC 部署到分公司網站。 另一個方法是將 RDSH 伺服器移至與使用者相同的子網域。 其他替代方法則是讓使用者不需透過智慧卡登入。 這些解決方案都可能需要效能或安全性層級上的妥協。

#### <a name="cannot-sign-in-with-a-smart-card-to-a-windows-server-2008-sp2-computer"></a>無法使用智慧卡登入 Windows Server 2008 SP2 電腦

當使用者登入透過 KB4093227 (2018.4B) 更新的 Windows Server 2008 SP2 電腦，就會發生此問題。 當使用者嘗試使用智慧卡登入，使用者存取遭拒且收到訊息如「找不到有效憑證。 請確認已正確插入智慧卡，且卡片緊密吻合插槽。」 在此同時，Windows Server 電腦會記錄應用程式事件「從插入的智慧卡擷取數位憑證時發生錯誤。 簽章無效。」

若要解決此問題，請使用 KB 4093227 的 2018.06 B 重新發行版本更新 Windows Server 電腦 ([在 Windows Server 2008 中 Windows 遠端桌面通訊協定 (RDP) 阻斷服務弱點的安全性更新說明：2018 年 4 月 10 日](https://support.microsoft.com/en-us/help/4093227/security-update-for-vulnerabilities-in-windows-server-2008))。

#### <a name="cannot-stay-signed-in-with-a-smart-card-and-remote-desktop-services-service-hangs"></a>無法使用智慧卡保持登入，且「遠端桌面服務」服務停止回應

當使用者登入透過 KB 4056446 更新的 Windows 或 Windows Server 電腦，就會發生此問題。 一開始，使用者或許可以使用智慧卡登入系統，但接著會收到「SCARD\_E\_NO\_SERVICE」錯誤訊息。 遠端電腦可能沒有回應。

若要解決此問題，請重新啟動遠端電腦。

若要解決此問題，請使用適當的修正來更新遠端電腦：

  - Windows Server 2008 SP2：KB 4090928，[Windows 流失 lsm.exe 處理程序中的控點，且智慧卡應用程式可能會顯示「SCARD\_E\_NO\_SERVICE」錯誤](https://support.microsoft.com/en-us/help/4090928/scard-e-no-service-errors-when-windows-leaks-handles-in-the-lsm-exe)
  - Windows Server 2012 R2：KB 4103724，[2018 年 5 月 17 日—KB4103724 (每月彙總套件預覽)](https://support.microsoft.com/en-us/help/4103724/windows-81-update-kb4103724)
  - Windows Server 2016 與 Windows 10 版本 1607：KB 4103720，[2018 年 5 月 17 日—KB4103720 (作業系統組建 14393.2273)](https://support.microsoft.com/en-us/help/4103720/windows-10-update-kb4103720)

### <a name="if-the-remote-pc-is-locked-a-user-needs-to-enter-a-password-twice"></a>如果遠端電腦已鎖定，使用者必須輸入密碼兩次

當使用者在 RDP 連線不需要 NLA 的部署中嘗試連線到執行 Windows 10 版本 1709 的遠端桌面，就可能會發生此問題。 在這些條件下，如果遠端桌面已鎖定，使用者連線時必須輸入認證兩次。

若要解決此問題，請使用 KB 4343893，[2018 年 8 月 30 日—KB4343893 (作業系統組建 16299.637)](https://support.microsoft.com/en-us/help/4343893/windows-10-update-kb4343893) 來更新 Windows 10 版本 1709 電腦。

### <a name="user-cannot-sign-in-and-receives-authentication-error-and-credssp-encryption-oracle-remediation-messages"></a>使用者無法登入且收到「驗證錯誤」和「CredSSP 加密預示修復」訊息

使用者使用任一版本的 Windows (從 Windows Vista SP2 和更新版本或 Windows Server 2008 SP2 和更新版本) 嘗試登入時，使用者存取遭拒且收到如下訊息：

  - 發生驗證錯誤。 不支援要求的功能。
  - 這可能是因為 CredSSP 加密預示修復

「CredSSP 加密預示修復」是在 2018 年 3 月、4 月和 5 月發行的一組安全性更新。 CredSSP 是處理其他應用程式驗證要求的驗證提供者。 2018 年 3 月 13 日「3B」和後續更新處理了以下惡意探索問題，攻擊者可以轉送使用者認證以在目標系統上執行程式碼。

初始更新已為新的群組原則物件**加密預示修復**新增支援，其具有下列可能的設定：

  - **易受攻擊**。 使用 CredSSP 的用戶端應用程式可能回復為不安全的版本，但這種行為會使遠端桌面暴露於攻擊風險中。 使用 CredSSP 的服務會接受尚未更新的用戶端。
  - **風險緩解**。 使用 CredSSP 的用戶端應用程式無法回復為不安全的版本，但使用 CredSSP 的服務會接受尚未更新的用戶端。
  - **強制更新的用戶端**。 使用 CredSSP 的用戶端應用程式無法回復為不安全的版本，且使用 CredSSP 的服務將不接受未修補的用戶端。 
    注意：這項設定不應進行部署，直到所有遠端主機都支援最新版本為止。

2018 年 5 月 8 日的更新將**加密預示修復**預設設定從**易受攻擊**變更為**風險緩解**。 套用此項變更之後，已更新的遠端桌面用戶端無法連線到未更新的伺服器 (或已更新但尚未重新啟動的伺服器)。 如需更新效果和其所封鎖的通訊類型詳細資料，請參閱 KB 4093492，[CVE-2018-0886 的 CredSSP 更新](https://support.microsoft.com/en-us/help/4093492/credssp-updates-for-cve-2018-0886-march-13-2018)。

若要解決此問題，請確定所有系統都已完整更新並重新啟動。 如需更新的完整清單和弱點的詳細資訊，請參閱 [CVE-2018-0886 | CredSSP 遠端程式碼執行弱點](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-0886)。

若要在更新完成之前處理此問題，請查看 KB 4093492 以取得允許的連線類型。 如果沒有可行的替代方案，您可以考慮下列方法之一：

- 針對受影響的用戶端電腦，將**加密預示修復**原則設回**易受攻擊**。
- 在**電腦設定\\系統管理範本\\Windows 元件\\遠端桌面服務\\遠端桌面工作階段主機\\安全性**群組原則資料夾中修改下列原則：  
  - **需要對遠端 (RDP) 連線使用特定的安全層**：設為 [已啟用]  並選取 [RDP]  。
  - **需要使用網路層級驗證對遠端連線進行使用者驗證**：設為 [已停用]  。
    > [!IMPORTANT]  
    > 這些修改會降低部署的安全性。 如需使用，這些修改應只是暫時性的。

如需使用群組原則的詳細資訊，請參閱[修改封鎖 GPO](#modifying-a-blocking-gpo)。

### <a name="after-you-update-client-computers-some-users-need-to-sign-in-twice"></a>更新用戶端電腦之後，部分使用者需要登入兩次

部分 Windows 7 或 Windows 10 版本 1709 的使用者登入遠端桌面工作階段後，會立即看到另一個登入畫面。 如果用戶端電腦已收到下列更新，就會發生此問題：

  - Windows 7：KB 4103718，[2018 年 5 月 8 日—KB4103718 (每月彙總套件)](https://support.microsoft.com/en-us/help/4103718/windows-7-update-kb4103718)
  - Windows 10 1709：KB 4103727，[2018 年 5 月 8 日—KB4103727 (作業系統組建 16299.431)](https://support.microsoft.com/en-us/help/4103727/windows-10-update-kb4103727)

若要解決此問題，請確定使用者要連線到的電腦 (以及 RDSH 或 RDVI 伺服器) 已在 2018 年 6 月完整更新。 這包括下列更新︰

  - Windows Server 2016：KB 4284880，[2018 年 6 月 12 日—KB4284880 (作業系統組建 14393.2312)](https://support.microsoft.com/en-us/help/4284880/windows-10-update-kb4284880)
  - Windows Server 2012 R2：KB 4284815，[2018 年 6 月 12 日—KB4284815 (每月彙總套件)](https://support.microsoft.com/en-us/help/4284815/windows-81-update-kb4284815)
  - Windows Server 2012：KB 4284855，[2018 年 6 月 12 日—KB4284855 (每月彙總套件)](https://support.microsoft.com/en-us/help/4284855/windows-server-2012-update-kb4284855)
  - Windows Server 2008 R2：KB 4284826，[2018 年 6 月 12 日—KB4284826 (每月彙總套件)](https://support.microsoft.com/en-us/help/4284826/windows-7-update-kb4284826)
  - Windows Server 2008 SP2：KB4056564，[在 Windows Server 2008、Windows Embedded POSReady 2009 和 Windows Embedded Standard 2009 中 CredSSP 遠端程式碼執行弱點的安全性更新說明：2018 年 3 月 13 日](https://support.microsoft.com/en-us/help/4056564/security-update-for-vulnerabilities-in-windows-server-2008)

### <a name="users-are-denied-access-on-a-deployment-that-uses-remote-credential-guard-with-multiple-rd-connection-brokers"></a>使用者在某部署上存取遭拒，該部署使用具多個 RD 連線代理人的遠端認證防護

如果系統正在使用 Windows Defender 遠端認證防護，此問題會發生在使用兩個或多個遠端桌面連線代理人的高可用性部署中。 使用者無法登入遠端桌面。

因為遠端認證防護使用 Kerberos 進行驗證，並限制 NTLM，因而發生此問題。 不過，在具負載平衡的高可用性設定中，RD 連線代理人無法支援 Kerberos 作業。

如果您需使用具負載平衡 RD 連線代理人的高可用性設定，您可以藉由停用遠端認證防護處理此問題。 有關如何管理 Windows Defender 遠端認證防護的詳細資訊，請參閱[使用 Windows Defender 遠端認證防護來保護遠端桌面認證](https://docs.microsoft.com/windows/security/identity-protection/remote-credential-guard#enable-windows-defender-remote-credential-guard)。

## <a name="on-connecting-user-receives-remote-desktop-service-is-currently-busy-message"></a>在連線時使用者收到「遠端桌面服務目前忙碌中」訊息

若要判斷對此問題的適當回應，請使用下列步驟：

1. 「遠端桌面服務」服務是否沒有回應 (例如，遠端桌面用戶端似乎在歡迎畫面「停止回應」)。  
      - 如果服務變得沒有回應，請參閱 [RDSH 伺服器記憶體問題](#rdsh-server-memory-issue)。
      - 如果用戶端似乎與服務正常互動，請繼續下一個步驟。
2. 如果一或多個使用者中斷連線其遠端桌面工作階段，使用者可以重新連線嗎？  
   - 如果不論有多少使用者中斷工作階段連線，服務仍持續拒絕連線，請參閱 [RD 接聽程式問題](#rd-listener-issue)。
   - 如果在一些使用者中斷工作階段連線之後，服務開始再次接受連線，請[檢查連線限制原則](#check-the-connection-limit-policy)。

### <a name="rdsh-server-memory-issue"></a>RDSH 伺服器記憶體問題

在部分 Windows Server 2012 R2 RDSH 伺服器上發現記憶體流失。 經過一段時間，這些伺服器開始拒絕遠端桌面連線和本機主控台登入，並出現如下訊息：

> 無法完成您嘗試執行的工作，因為遠端桌面服務目前忙碌中。 請等候一段時間後再試一次。 其他使用者應仍能登入。

嘗試連線的遠端桌面用戶端也變得沒有回應。

若要解決此問題，請重新啟動 RDSH 伺服器。

若要解決此問題，請將 KB 4093114 [2018 年 4 月 10 日—KB4093114 (每月彙總套件)](file:///C:/Users/v-jesits/AppData/Local/Microsoft/Windows/INetCache/Content.Outlook/FUB8OO45/April%2010,%202018—KB4093114%20(Monthly%20Rollup)) 套用至 RDSH 伺服器。

### <a name="rd-listener-issue"></a>RD 接聽程式問題

在直接從 Windows Server 2008 R2 升級到 Windows Server 2012 R2 或 Windows Server 2016 的某些 RDSH 伺服器上發現問題。 當遠端桌面用戶端連線到 RDSH 伺服器，RDSH 伺服器會為使用者工作階段建立 RD 接聽程式。 受影響的伺服器會計算 RD 接聽程式的數量，其隨著使用者連線而增加，但永遠不會減少。

若要解決此問題，您可以使用下列方法：

  - 重新啟動 RDSH 伺服器以重設 RD 接聽程式計數
  - 修改連線限制原則，將其設定為非常大的值。 如需管理連線限制原則的詳細資訊，請參閱[檢查連線限制原則](#check-the-connection-limit-policy)。

若要解決此問題，請將下列更新套用到 RDSH 伺服器：

  - Windows Server 2012 R2：KB 4343891，[2018 年 8 月 30 日—KB4343891 (每月彙總套件預覽)](https://support.microsoft.com/en-us/help/4343891/windows-81-update-kb4343891)
  - Windows Server 2016：KB 4343884，[2018 年 8 月 30 日—KB4343884 (作業系統組建 14393.2457)](https://support.microsoft.com/en-us/help/4343884/windows-10-update-kb4343884)

### <a name="check-the-connection-limit-policy"></a>檢查連線限制原則

您可以在個別電腦層級設定同時遠端桌面連線的數量限制，或藉由設定群組原則物件 (GPO)。 預設未設定限制。

若要檢查目前的設定，並找出 RDSH 伺服器上任何現有的 GPO，請以系統管理員身分開啟命令提示字元視窗，並輸入下列命令：
  
```
gpresult /H c:\gpresult.html
```
   
此命令完成後，開啟 gpresult.html，在**電腦設定\\系統管理範本\\Windows 元件\\遠端桌面服務\\遠端桌面工作階段主機\\連線**中，尋找**連線限制數目**原則。

  - 如果此原則的設定為**已停用**，則群組原則不會限制 RDP 連線。
  - 如果此原則的設定為**已啟用**，則請檢查**優勢 GPO**。 如果您需要移除或變更連線限制，請編輯此 GPO。

若要強制執行原則變更，請在受影響的電腦上開啟命令提示字元視窗，並輸入下列命令：
  
```
gpupdate /force
```
  
## <a name="rd-client-disconnects-and-cannot-reconnect-to-the-same-session"></a>RD 用戶端連線中斷，且無法重新連線至同一工作階段

遠端桌面用戶端失去遠端桌面連線後，用戶端無法立即重新連線。 使用者會收到錯誤訊息，如下所示：

  - 由於安全性錯誤，用戶端無法連線到終端機伺服器。 在確定您已登入網路後，再次嘗試連線到伺服器。
  - 遠端桌面中斷連線。 由於安全性錯誤，用戶端無法連線到遠端電腦。 請確認您已登入網路，並再次嘗試連線。

稍後，遠端桌面用戶端會重新連線到遠端桌面。 不過，RDSH 伺服器會將用戶端連線到新的工作階段，而非原本的工作階段。 檢查 RDSH 伺服器會顯示，原本的工作階段並未進入中斷連線的狀態，而是保持使用中。

若要處理此問題，您可以在**電腦設定\\系統管理範本\\Windows 元件\\遠端桌面服務\\遠端桌面工作階段主機\\連線**群組原則資料夾中，啟用**設定持續連線連線間隔**原則。 如果您啟用此原則，您必須輸入持續連線間隔。 持續連線間隔可決定伺服器檢查工作階段狀態的頻率 (以分鐘為單位)。

此問題可能起因於驗證和組態設定的設定錯誤。 您可以在伺服器層級或使用 GPO 來進行這些設定。 群組原則設定可在**電腦設定\\系統管理範本\\Windows 元件\\遠端桌面服務\\遠端桌面工作階段主機\\安全性**群組原則資料夾中使用。

1. 若要解決此問題，必須重新啟動遠端桌面連線代理人服務。
2. 在 [連線]  底下，以滑鼠右鍵按一下連線名稱，然後按一下 [內容]  。
3. 在連線的 [內容]  對話方塊中，[一般]  索引標籤上的 [安全性]  圖層中，選取安全性方法。
4. 在 [加密層級]  中，按一下您要的層級。 您可以選取 [低]  、[用戶端相容]  、[高]  或 [FIPS 相容]  。

> [!NOTE]  
>  - 當用戶端與 RD 工作階段主機伺服器之間的通訊需要最高層級的加密時，請使用 FIPS 相容加密。
>  - 您在群組原則中設定的任何加密層級設定都會覆寫使用遠端桌面服務設定工具所設的設定。 此外，如果您啟用[系統密碼編譯：使用 FIPS 相容演算法以供加密、雜湊和簽署](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc780081\(v=ws.10\))原則，此設定會覆寫**設定用戶端連線加密層級**原則。 系統密碼編譯原則位於**電腦設定\\Windows 設定\\安全性設定\\本機原則\\安全性選項**資料夾中。
>  - 當您變更加密層級時，新的加密層級會在下次使用者登入時生效。 如果您需要在單一伺服器上設定多重加密層級，請安裝多張網路介面卡並分別設定每張介面卡。
>  - 若要確認憑證具有對應的私密金鑰，在遠端桌面服務設定中，以滑鼠右鍵按一下要檢視憑證的連線，選取 [一般]  ，選取 [編輯]  ，選取要檢視的憑證，然後選取 [檢視憑證]  。 在 [一般]  索引標籤底部，應出現陳述式「您有此憑證相對應的私密金鑰」。 您也可以使用 [憑證] 嵌入式管理單元來檢視這項資訊。
>  - FIPS 相容加密 (**系統密碼編譯：使用 FIPS 相容演算法以供加密、雜湊和簽署**原則或在遠端桌面伺服器設定中的 **FIPS 相容**設定) 使用 Microsoft 密碼編譯模組，透過聯邦資訊處理標準 (FIPS) 140-1 加密演算法，可加密及解密從用戶端傳送到伺服器以及從伺服器傳送到用戶端的資料。 如需詳細資訊，請參閱 [FIPS 140 驗證](https://docs.microsoft.com/windows/security/threat-protection/fips-140-validation)。
>  - **高**設定可使用強式 128 位元加密，將用戶端傳送到伺服器以及伺服器傳送到用戶端的資料加密。
>  - **用戶端相容**設定會以用戶端所支援的最大機碼強度加密用戶端和伺服器之間傳送的資料。
>  - **低**設定可使用 56 位元加密，將用戶端傳送到伺服器的資料加密。

## <a name="remote-laptop-disconnects-from-wireless-network"></a>遠端膝上型電腦從無線網路中斷連線

遠端桌面用戶端使用 802.1x 無線網路連線到膝上型電腦時，可能會發生此問題。 偶爾膝上型電腦會從無線網路中斷連線，且不會自動重新連線。

這是個已知問題，當無線網路連線的網路驗證設定為**使用者驗證**，就會發生這個問題。

若要處理此問題，請將網路驗證設定設為**使用者或電腦驗證**或者**電腦驗證**。

 > [!NOTE]  
> 若要在單一電腦上變更網路驗證設定，您需要使用 [網路和共用中心] 控制台，透過新的設定建立新的無線連線。

如需如何使用 GPO 設定無線網路設定的完整說明，請參閱[設定無線網路 (IEEE 802.11) 原則](https://docs.microsoft.com/windows-server/networking/core-network-guide/cncg/wireless/e-wireless-access-deployment#bkmk_policies)。

## <a name="user-experiences-poor-performance-or-application-problems"></a>使用者遇到效能不佳的問題或應用程式問題

本節說明在使用遠端桌面功能時，使用者可能會遇到的幾個常見問題：

  - [連線到新虛擬機器 Microsoft Azure 的使用者會間歇性遇到問題](#intermittent-problems-with-new-microsoft-azure-virtual-machines)。
  - [連線到 Windows 10 版本 1709 遠端桌面的使用者播放影片可能會有問題](#video-playback-issues-on-windows-10-version-1709)。
  - [連線到 Windows 10 遠端桌面且具有唯讀使用者設定檔的使用者無法共用桌面。](#desktop-sharing-issues-on-windows-10)
  - [NLA 停用時，若使用在舊版 Windows 10 上執行的用戶端來連線到 Windows 10 遠端桌面，使用者會遇到效能不佳的問題](#performance-issues-when-mixing-versions-of-windows-10-if-nla-is-disabled)
  - [當使用者在遠端桌面工作階段中執行多個應用程式，並頻繁地中斷連線和重新連線，使用者可能會在重新連線時遇到黑色畫面。](#black-screen-issue)

### <a name="intermittent-problems-with-new-microsoft-azure-virtual-machines"></a>新的 Microsoft Azure 虛擬機器發生間歇性問題

此問題會影響最近佈建的虛擬機器。 使用者連線至虛擬機器後，遠端桌面工作階段可能不會正確載入所有使用者的設定。

若要處理此問題，請從虛擬機器中斷連線，等候至少 20 分鐘，然後再次連線。

若要解決此問題，請視需要將下列更新套用到虛擬機器：

  - Windows 10 與 Windows Server 2016：KB 4343884，[2018 年 8 月 30 日—KB4343884 (作業系統組建 14393.2457)](https://support.microsoft.com/en-us/help/4343884/windows-10-update-kb4343884)
  - Windows Server 2012 R2：KB 4343891，[2018 年 8 月 30 日—KB4343891 (每月彙總套件預覽)](https://support.microsoft.com/en-us/help/4343891/windows-81-update-kb4343891)

### <a name="video-playback-issues-on-windows-10-version-1709"></a>Windows 10 版本 1709 上的影片播放問題

使用者連線到執行 Windows 10 版本 1709 的遠端電腦時，就會發生此問題。 當這些使用者使用 VMR9 (影片混合轉譯器 9) 轉碼器播放影片，播放程式僅會顯示黑色視窗。

這是 Windows 10 版本 1709 中的已知問題。 在 Windows 10 版本 1703 中不會發生該問題。

### <a name="desktop-sharing-issues-on-windows-10"></a>Windows 10 上的桌面共用問題

當使用者具有唯讀使用者設定檔 (及關聯的登錄區)，例如在 Kiosk 的情況下，就會發生此問題。 當這類使用者連線到執行 Windows 10 版本 1803 的遠端電腦，使用者無法共用桌面。

若要修正此問題，請套用 Windows 10 更新 4340917，[2018 年 7 月 24 日—KB4340917 (作業系統組建 17134.191)](https://support.microsoft.com/en-us/help/4340917/windows-10-update-kb4340917)。

### <a name="performance-issues-when-mixing-versions-of-windows-10-if-nla-is-disabled"></a>如果已停用 NLA，混合 Windows 10 版本時會發生效能問題

當 NLA 已停用，且執行 Windows 10 的遠端桌面用戶端電腦連線到執行不同版本 Windows 10 的遠端桌面時，就會發生此問題。 遠端桌面用戶端的使用者若使用執行 Windows 10 版本 1709 或較舊版本的電腦，連線到執行 Windows 10 版本 1803 或更新版本的遠端桌面時，會遇到效能不佳的問題。

這是因為若 NLA 已停用，舊版用戶端電腦連線到 Windows 10 版本 1803 或更新版本時，會使用速度較慢的通訊協定。

若要解決此問題，請套用 KB 4340917，[2018 年 7 月 24 日—KB4340917 (作業系統組建 17134.191)](https://support.microsoft.com/en-us/help/4340917/windows-10-update-kb4340917)。

### <a name="black-screen-issue"></a>黑色畫面問題

此問題發生在 Windows 8.0、Windows 8.1、Windows 10 RTM 和 Windows Server 2012 R2 中。 使用者在遠端桌面中啟動多個應用程式，然後從工作階段中斷連線。 使用者會定期重新連線到遠端桌面與應用程式互動，並再次中斷連線。 某些時候，當使用者重新連線，遠端桌面工作階段只會顯示黑色畫面。 使用者必須從遠端電腦的主控台 (或 RDSH 伺服器主控台) 結束遠端桌面工作階段。 此動作會停止應用程式。

若要解決此問題，請視需要套用下列更新：

  - Windows 8 與 Windows Server 2012：KB4103719，[2018 年 5 月 17 日—KB4103719 (每月彙總套件預覽)](https://support.microsoft.com/en-us/help/4103719/windows-server-2012-update-kb4103719)
  - Windows 8.1 與 Windows Server 2012 R2：KB4103724，[2018 年 5 月 17 日—KB4103724 (每月彙總套件預覽)](https://support.microsoft.com/en-us/help/4103724/windows-81-update-kb4103724) 和 KB 4284863，[2018 年 6 月 21 日—KB4284863 (每月彙總套件預覽)](https://support.microsoft.com/en-us/help/4284863/windows-81-update-kb4284863)
  - Windows 10：已修正於 KB4284860，[2018 年 6 月 12 日—KB4284860 (作業系統組建 10240.17889)](https://support.microsoft.com/en-us/help/4284860/windows-10-update-kb4284860)
