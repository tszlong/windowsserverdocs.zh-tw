---
title: 針對遠端桌面連線進行疑難排解
description: 依徵狀排列的疑難排解程序
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
ms.openlocfilehash: 4bbdd17f5e6e2b161e0dda0e172ea862a9107841
ms.sourcegitcommit: 564158d760f902ced7f18e6d63a9daafa2a92bd4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2019
ms.locfileid: "64988330"
---
# <a name="troubleshooting-remote-desktop-connections"></a>針對遠端桌面連線進行疑難排解
如需幾個最常見的遠端桌面服務 (RDS) 問題的簡短說明，請參閱[遠端桌面用戶端的相關的常見問題集](https://review.docs.microsoft.com/en-us/windows-server/remote/remote-desktop-services/clients/remote-desktop-client-faq)。 本文會說明數個更進階的方法，若要疑難排解連線問題。 許多這些程序適用於是否針對簡單的組態，例如一部實體電腦，連線到另一部實體電腦或更複雜的組態進行疑難排解。 某些程序因應只在更複雜的多使用者案例中發生的問題。 如需有關遠端桌面的元件以及它們如何一起運作的詳細資訊，請參閱 <<c0> [ 遠端桌面服務架構](https://docs.microsoft.com/en-us/windows-server/remote/remote-desktop-services/desktop-hosting-logical-architecture)。

> [!NOTE]  
> 有許多這篇文章所述的程序需要您存取多部電腦，其中有些可能要從遠端存取。 如需有關遠端系統管理工具和設定方式的詳細資訊，請參閱[遠端伺服器管理工具 (RSAT) 的 Windows 作業系統](https://support.microsoft.com/en-us/help/2693643/remote-server-administration-tools-rsat-for-windows-operating-systems)。

在下列清單中，找出您 （或您的使用者） 遭遇到的徵狀的類型。

- [遠端桌面用戶端無法連線到遠端桌面，但沒有任何特定徵狀或訊息 （一般疑難排解步驟）](#no-specific-symptoms-or-messages-general-troubleshooting-steps)
- [遠端桌面用戶端無法連線到遠端桌面，並接收 「 類別未登錄 」 訊息](#client-cannot-connect-class-not-registered)
- [遠端桌面用戶端無法連線到遠端桌面，並收到 「 沒有可用的授權 」 或 「 安全性錯誤 」 訊息](#client-cannot-connect-no-licenses-available)
- [使用者會收到 「 拒絕存取 」 訊息，或必須提供認證兩次](#user-cannot-authenticate-or-must-authenticate-twice)
- [在連接時，收到 「 遠端桌面服務目前忙碌中 」 訊息](#on-connecting-user-receives-remote-desktop-service-is-currently-busy-message)
- [遠端桌面用戶端中斷連線，並無法重新連接至相同的工作階段](#rd-client-disconnects-and-cannot-reconnect-to-the-same-session)
- [使用者透過無線網路，連接到遠端的膝上型電腦和膝上型電腦與網路中斷然後](#remote-laptop-disconnects-from-wireless-network)。
- [使用者體驗不佳的效能或遠端應用程式的問題](#user-experiences-poor-performance-or-application-problems)

> [!NOTE]  
> 如需目前的 RDP 中斷連接程式碼清單，請參閱 < [ExtendedDisconnectReasonCode 列舉](https://docs.microsoft.com/en-us/windows/desktop/TermServ/extendeddisconnectreasoncode)。 

## <a name="no-specific-symptoms-or-messages-general-troubleshooting-steps"></a>沒有特定徵狀或訊息 （一般疑難排解步驟）

當遠端桌面用戶端無法連線到遠端桌面，但不會提供訊息或其他症狀，幫助找出原因，請使用下列步驟。 若要解決許多常見的原因，這種問題，請使用下列方法：

- [檢查 RDP 通訊協定的狀態](#check-the-status-of-the-rdp-protocol)
- [檢查 RDP 服務的狀態](#check-the-status-of-the-rdp-services)
- [確認 RDP 接聽程式正常運作](#check-that-the-rdp-listener-is-functioning)
- [核取 RDP 接聽程式連接埠](#check-the-rdp-listener-port)

### <a name="check-the-status-of-the-rdp-protocol"></a>檢查 RDP 通訊協定的狀態

#### <a name="check-the-status-of-the-rdp-protocol-on-a-local-computer"></a>檢查本機電腦上的 RDP 通訊協定的狀態

若要檢查並變更 RDP 通訊協定，在本機電腦上的狀態，請參閱[如何啟用遠端桌面](https://docs.microsoft.com/en-us/windows-server/remote/remote-desktop-services/clients/remote-desktop-allow-access#how-to-enable-remote-desktop)。

> [!NOTE]  
> 無法使用遠端桌面選項時，請參閱[檢查群組原則物件是否會封鎖 RDP](#check-whether-a-group-policy-object-gpo-is-blocking-rdp-on-a-local-computer)。

#### <a name="check-the-status-of-the-rdp-protocol-on-a-remote-computer"></a>檢查遠端電腦上的 RDP 通訊協定的狀態

> [!IMPORTANT]  
> 請仔細依照本節中的步驟。 如果您未正確修改登錄，可能會發生嚴重問題。 您可以修改它，才能[備份進行還原登錄](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22)在發生問題。

若要檢查並變更 RDP 通訊協定在遠端電腦上的狀態，使用網路登錄連線：

1. 選取 **開始**，選取**執行**，然後輸入**regedt32**。
2. 在 [登錄編輯器] 中，選取**檔案**，然後選取**連線網路登錄**。
3. 中**選取的電腦**對話方塊方塊中，輸入遠端電腦的名稱，選取**檢查名稱**，然後選取**確定**。
4. 瀏覽至**HKEY\_本機\_機器\\SYSTEM\\CurrentControlSet\\控制\\終端機伺服器**。  
   ![登錄編輯程式，顯示 fDenyTSConnections 項目](..\media\troubleshoot-remote-desktop-connections\RegEntry_fDenyTSConnections.png)
   - 如果值**fDenyTSConnections**金鑰**0**，就會啟用 RDP
   - 如果值**fDenyTSConnections**金鑰**1**，就會停用 RDP
5. 若要啟用 RDP，請將變更的值**fDenyTSConnections**從**1**來**0**。

#### <a name="check-whether-a-group-policy-object-gpo-is-blocking-rdp-on-a-local-computer"></a>檢查群組原則物件 (GPO) 是否會封鎖 RDP 在本機電腦上

如果您無法開啟使用者介面中的 RDP 或 windows 7 **fDenyTSConnections**還原成**1**變更它之後，GPO 可能會覆寫的電腦層級設定。

若要檢查本機電腦上的群組原則設定，請開啟命令提示字元視窗中的，身為管理員，並輸入下列命令：
```
gpresult /H c:\gpresult.html
```
此命令完成後，開啟 gpresult.html。 在 **電腦組態\\系統管理範本\\Windows 元件\\遠端桌面服務\\遠端桌面工作階段主機\\連線**，尋找**允許使用者使用遠端桌面服務從遠端連線**原則。

- 如果此原則設定為**已啟用**，群組原則不會封鎖 RDP 連線。
- 如果此原則設定為**已停用**，檢查**優勢 GPO**。 這是封鎖 RDP 連線的 GPO。
![Gpresult.html，在其中的範例區段網域層級 GPO * * 區塊 RDP * * 停用 RDP。](..\media\troubleshoot-remote-desktop-connections\GPResult_RDSH_Connections_GP.png)
   
  ![Gpresult.html，在其中的範例區段 * * 本機群組原則 停用 RDP。](..\media\troubleshoot-remote-desktop-connections\GPResult_RDSH_Connections_LGP.png)

#### <a name="check-whether-a-gpo-is-blocking-rdp-on-a-remote-computer"></a>檢查 GPO 是否會封鎖 RDP 的遠端電腦上

若要檢查遠端電腦上的群組原則設定，這個命令會為本機電腦幾乎一樣：
```
gpresult /S <computer name> /H c:\gpresult-<computer name>.html
```
此命令會產生的檔案 (**gpresult-\<電腦名稱\>.html**) 做為本機電腦版本會使用相同的資訊格式 (**gpresult.html**) 使用。

#### <a name="modifying-a-blocking-gpo"></a>修改封鎖的 GPO

您可以修改這些設定中的群組原則物件編輯器 (GPE) 和群組原則管理主控台 （毛利率）。 如需如何使用群組原則的詳細資訊，請參閱[Advanced Group Policy Management](https://docs.microsoft.com/en-us/microsoft-desktop-optimization-pack/agpm/)。

若要修改的封鎖原則，請使用下列方法之一：

- 在 GPE，存取 （例如本機或網域），適當的 GPO 層級，並瀏覽至**電腦組態\\系統管理範本\\Windows 元件\\Remote Desktop Services\\遠端桌面工作階段主機\\連線**\\**允許使用者使用遠端桌面服務從遠端連線**。  
   1. 若要設定原則**已啟用**或是**未設定**。
   2. 在受影響的電腦上開啟命令提示字元視窗身為管理員，並執行**gpupdate /force**命令。
- 在毛利率，瀏覽到 OU 中封鎖的原則套用至受影響的電腦，並刪除該原則從 OU。

### <a name="check-the-status-of-the-rdp-services"></a>檢查 RDP 服務的狀態

在本機 （用戶端） 的電腦和遠端 （目標） 的電腦，應該執行下列服務：

- 遠端桌面服務 (TermService)
- 遠端桌面服務 UserMode Port Redirector (UmRdpService)

您可以使用 [服務] MMC 嵌入式管理單元，在本機或遠端管理的服務。 您也可以使用 PowerShell，在本機或遠端 （如果遠端電腦設定為接受遠端的 PowerShell 命令）。

![在 [服務] MMC 嵌入式管理單元的遠端桌面服務。 請勿修改預設的服務設定。](..\media\troubleshoot-remote-desktop-connections\RDSServiceStatus.png)

其中一台電腦，如果一或兩個服務未執行，啟動它們。

> [!NOTE]  
> 如果您啟動遠端桌面服務的服務，按一下**是**自動重新啟動遠端桌面服務使用者模式連接埠重新導向程式服務。

### <a name="check-that-the-rdp-listener-is-functioning"></a>確認 RDP 接聽程式正常運作

> [!IMPORTANT]  
> 請仔細依照本節中的步驟。 如果您未正確修改登錄，可能會發生嚴重問題。 您可以修改它，才能[備份進行還原登錄](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22)在發生問題。

#### <a name="check-the-status-of-the-rdp-listener"></a>檢查 RDP 接聽程式的狀態

此程序中，使用具有系統管理權限的 PowerShell 執行個體。 本機電腦，您也可以使用具有系統管理權限的命令提示字元。 不過，此程序使用 PowerShell，因為相同的命令在本機和遠端上同時工作。

1. 開啟 PowerShell 視窗。 若要連線到遠端電腦，請輸入**Enter-pssession-ComputerName\<電腦名稱\>** 。
2. 請輸入**qwinsta**。 
    ![Qwinsta 命令會列出電腦的連接埠上接聽的處理程序。](..\media\troubleshoot-remote-desktop-connections\WPS_qwinsta.png)
3. 如果清單包含**rdp tcp**狀態**接聽**，RDP 接聽程式是否運作。 請繼續進行[檢查 RDP 接聽程式連接埠](#check-the-rdp-listener-port)。 否則，請繼續在步驟 4。
4. 從工作電腦匯出 RDP 接聽程式組態。
    1. 登入為受影響的電腦，具有相同的作業系統版本的電腦，並存取該電腦的登錄 （例如，藉由使用登錄編輯程式）。
    2. 瀏覽至下列登錄項目：  
        **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server\\WinStations\\RDP-Tcp**
    3. 將匯出之項目的.reg 檔案。 例如，在 登錄編輯程式，以滑鼠右鍵按一下項目，請選取**匯出**，然後輸入檔案名稱的匯出的設定。
    4. 將匯出的.reg 檔案複製到 受影響的電腦。
5. 若要匯入的 RDP 接聽程式組態，請開啟 PowerShell 視窗，在受影響的電腦上具有系統管理權限 （或開啟 PowerShell 視窗並從遠端連線到受影響的電腦）。
   1. 若要備份現有的登錄項目，請輸入下列命令：  
   
      ```powershell  
      cmd /c 'reg export "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-tcp" C:\Rdp-tcp-backup.reg'   
      ```  
   

   2. 若要移除現有的登錄項目，請輸入下列命令：  
   
      ```powershell  
      Remove-Item -path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-tcp' -Recurse -Force  
      ```
   
   3. 若要匯入新的登錄項目，並再重新啟動服務，請輸入下列命令：  
   
      ```powershell  
      cmd /c 'regedit /s c:\<filename>.reg'  
      Restart-Service TermService -Force  
      ```   
      何處\<filename\>是匯出的.reg 檔案的名稱。
6. 藉由嘗試遠端桌面連線一次測試設定。 如果您仍然無法連線，請重新啟動受影響的電腦。
7. 如果您仍然無法連線，[檢查 RDP 的自我簽署憑證的狀態](#check-the-status-of-the-rdp-self-signed-certificate)。

#### <a name="check-the-status-of-the-rdp-self-signed-certificate"></a>檢查 RDP 的自我簽署憑證的狀態

1. 如果您仍然無法連線，開啟 憑證 MMC 嵌入式管理單元。 當系統會提示您選取要管理，請選取憑證存放區**電腦帳戶**，然後選取 受影響的電腦。
2. 在 **憑證**下方的資料夾**遠端桌面**，刪除 RDP 的自我簽署的憑證。 
    ![MMC 憑證嵌入式管理單元的遠端桌面憑證。](..\media\troubleshoot-remote-desktop-connections\MMCCert_Delete.png)
3. 在受影響的電腦上重新啟動遠端桌面服務的服務。
4. 重新整理 [憑證] 嵌入式管理單元。
5. 如果尚未重建，RDP 的自我簽署的憑證[檢查 MachineKeys 資料夾的權限](#check-the-permissions-of-the-machinekeys-folder)。

#### <a name="check-the-permissions-of-the-machinekeys-folder"></a>檢查 MachineKeys 資料夾的權限

1. 在受影響的電腦，請開啟 Explorer，然後再瀏覽至**c:\\ProgramData\\Microsoft\\Crypto\\RSA\\** 。
2. 以滑鼠右鍵按一下**MachineKeys**，選取**屬性**，選取**安全性**，然後選取**進階**。
3. 請確定已設定下列權限：
      - 內建\\系統管理員：完全控制
      - 所有人：讀取、 寫入

### <a name="check-the-rdp-listener-port"></a>核取 RDP 接聽程式連接埠

在本機 （用戶端） 的電腦和遠端 （目標） 的電腦中，RDP 接聽程式應該接聽連接埠 3389。 沒有其他應用程式應該使用此連接埠。

> [!IMPORTANT]  
> 請仔細依照本節中的步驟。 如果您未正確修改登錄，可能會發生嚴重問題。 您可以修改它，才能[備份進行還原登錄](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22)在發生問題。

若要檢查或變更 RDP 連接埠，請使用 登錄編輯：

1. 選取 [開始] 中，選取**執行**，然後輸入**regedt32**。
      - 若要連線到遠端電腦，請選取**檔案**，然後選取**連線網路登錄**。
      - 中**選取的電腦**對話方塊方塊中，輸入遠端電腦的名稱，選取**檢查名稱**，然後選取**確定**。
2. 開啟登錄，並瀏覽至**HKEY\_本機\_機器\\SYSTEM\\CurrentControlSet\\控制\\終端機伺服器\\WinStations\\\<接聽程式\>** 。 
    ![RDP 通訊協定的通訊埠編號子機碼。](..\media\troubleshoot-remote-desktop-connections\RegEntry_PortNumber.png)
3. 如果**PortNumber**以外的值**3389**，將它變更為**3389**。 
   > [!IMPORTANT]  
    > 您可以操作使用另一個連接埠的遠端桌面服務。 不過，我們不建議您這麼做。 疑難排解這類組態已超出本文的範圍。
4. 變更連接埠號碼之後，重新啟動遠端桌面服務的服務。

#### <a name="check-that-another-application-is-not-trying-to-use-the-same-port"></a>另一個應用程式沒有嘗試使用相同的連接埠的核取

此程序中，使用具有系統管理權限的 PowerShell 執行個體。 本機電腦，您也可以使用具有系統管理權限的命令提示字元。 不過，此程序使用 PowerShell，因為相同的命令在本機和遠端。

1. 開啟 PowerShell 視窗。 若要連線到遠端電腦，請輸入**Enter-pssession-ComputerName\<電腦名稱\>** 。
2. 輸入下列命令：  
   
     ```powershell  
    cmd /c 'netstat -ano | find "3389"'  
    ```
  
    ![Netstat 命令會產生一份連接埠和接聽的服務。](..\media\troubleshoot-remote-desktop-connections\WPS_netstat.png)
1. 尋找具有 **Listening** 狀態之 TCP 連接埠 3389 (或指派的 RDP 連接埠) 的項目。 
    > [!NOTE]  
   > 使用該連接埠之處理程序或服務的 PID (處理程序識別碼) 會顯示在 [PID] 欄位底下。
1. 若要判斷哪一個應用程式使用連接埠 3389 （或指派的 RDP 連接埠），請輸入下列命令：  
   
     ```powershell  
    cmd /c 'tasklist /svc | find "<pid listening on 3389>"'  
    ```  
  
    ![Tasklist 命令報告特定處理序的詳細的資訊。](..\media\troubleshoot-remote-desktop-connections\WPS_tasklist.png)
1. 尋找與連接埠相關聯之 PID 號碼的項目 (從**netstat**輸出)。 服務或與該 PID 相關聯的處理程序會出現在右邊。
1. 如果應用程式或服務，而非 Remote Desktop Services (TermServ.exe) 使用的連接埠，您可以使用下列方法之一來解決衝突：
      - 設定其他應用程式或服務使用不同的連接埠 （建議選項）。
      - 解除安裝其他應用程式或服務。
      - 設定 rdp 連接至使用不同的連接埠，然後重新啟動遠端桌面服務 （不建議）。

#### <a name="check-whether-a-firewall-is-blocking-the-rdp-port"></a>檢查防火牆是否封鎖 RDP 連接埠

使用**psping**工具來測試是否藉由使用連接埠 3389，您可連線受影響的電腦。

1. 在電腦上不同於受影響的電腦，下載**psping**從<https://live.sysinternals.com/psping.exe>。
2. 開啟命令提示字元 視窗，以系統管理員，將在您安裝所在的目錄**psping**，然後輸入下列命令：  
   
   ```  
   psping -accepteula <computer IP>:3389  
   ```
   
3. 檢查輸出**psping**命令結果如下所示：  
      - **連接到\<電腦 IP\>** :連線到遠端電腦。
      - **(0 %loss)** :所有連接嘗試成功。
      - **在遠端電腦拒絕網路連線**:無法連線到遠端電腦。
      - **(100 %loss)** :所有嘗試連接失敗的詳細資訊。
1. 執行**psping**來測試其能夠連線到受影響的電腦的多部電腦上。
1. 請注意，受影響的電腦是否會封鎖來自其他所有電腦、 其他電腦或僅一部其他電腦的連線。
1. 建議的後續步驟：
      - 請連絡您的網路系統管理員確認網路允許 RDP 流量傳送到受影響的電腦。
      - 若要判斷 RDP 連接埠時，是否要封鎖防火牆調查來源電腦與受影響的電腦 （包括受影響的電腦上的 Windows 防火牆） 之間的任何防火牆的組態。

## <a name="client-cannot-connect-class-not-registered"></a>用戶端無法連線，「 類別未登錄 」

當您嘗試連線到遠端電腦使用用戶端執行 Windows 10 1709年版或更新版本中，用戶端可能無法連接時的遠端桌面工作階段主機伺服器會報告一則訊息包含的 「 類別未登錄 (0x80040154) 」 的錯誤程式碼。

如果嘗試連線的使用者具有必要的使用者設定檔，就會發生此問題。 若要解決此問題，請安裝[2018 年 7 月 24 日 — KB4338817 (OS 組建 16299.579)](https://support.microsoft.com/en-us/help/4338817/windows-10-update-kb4338817) Windows 10 更新。

## <a name="client-cannot-connect-no-licenses-available"></a>用戶端無法連線，沒有可用的授權

這種情況適用於包含 RDSH 伺服器和遠端桌面授權伺服器的部署。

識別哪一種使用者會看到的行為：

- 因為沒有授權可或沒有授權伺服器，就已中斷連線工作階段
- 由於安全性錯誤而拒絕存取

登入 RD 工作階段主機，以網域系統管理員，並開啟 RD 授權診斷程式。 看起來如下所示的訊息：

  - 在寬限期內，針對遠端桌面工作階段主機伺服器已過期，但尚未與任何授權伺服器設定的 RD 工作階段主機伺服器。 RD 工作階段主機伺服器的連接會遭到拒絕，除非授權伺服器設定 RD 工作階段主機伺服器。
  - 授權伺服器\<電腦名稱\>不提供。 這可能網路連線問題所造成，「 遠端桌面授權 」 服務已停止在授權伺服器上，或 RD 授權已不再安裝在電腦上。

這些問題通常與下列的使用者訊息相關聯：

  - 遠端工作階段中斷連線，因為有不可供此電腦的任何遠端桌面用戶端存取使用權。
  - 遠端工作階段已中斷連線，因為沒有可用的遠端桌面授權伺服器提供授權。

在此情況下，[設定 RD 授權服務](#configure-the-rd-licensing-service)。

如果 RD 授權診斷程式還列出其他問題，例如"RDP 通訊協定元件 X.224 通訊協定資料流中偵測到錯誤，並已中斷連線的用戶端，「 可能會影響授權的憑證有問題。 這類問題通常為相關聯使用者的訊息，如下所示：

安全性錯誤，因為用戶端無法連線到終端機伺服器。 確定您已登入網路之後, 再試一次連接到伺服器。

在此情況下，[重新整理之 X509 憑證登錄機碼](#refresh-the-x509-certificate-registry-keys)。

### <a name="configure-the-rd-licensing-service"></a>設定 RD 授權服務

下列程序會使用伺服器管理員進行組態變更。 如需有關如何設定及使用伺服器管理員的資訊，請參閱[伺服器管理員](https://docs.microsoft.com/en-us/windows-server/administration/server-manager/server-manager)。

1. 開啟 [伺服器管理員]，然後瀏覽**Remote Desktop Services**。
2. 在 **部署概觀**，選取**工作**，然後選取**編輯部署內容**。
3. 選取  **RD 授權**，然後選取適當的授權模式，為您的部署 (**每一裝置**或是**每個使用者**)。
4. 輸入 RD 授權伺服器的完整的網域名稱 (FQDN)，然後選取**新增**。
5. 如果您有多部 RD 授權伺服器，請針對每部伺服器重複步驟 4。 
    ![RD 授權伺服器組態選項在 [伺服器管理員] 中。](..\media\troubleshoot-remote-desktop-connections\RDLicensing_Configure.png)

### <a name="refresh-the-x509-certificate-registry-keys"></a>重新整理之 X509 憑證登錄機碼

> [!IMPORTANT]  
> 請仔細依照本節中的步驟。 如果您未正確修改登錄，可能會發生嚴重問題。 您可以修改它，才能[備份進行還原登錄](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22)在發生問題。

若要解決此問題，請備份並移除 X509 憑證登錄機碼，然後重新啟動電腦，然後重新啟用 RD 授權伺服器。 若要這樣做，請執行下列步驟。

> [!NOTE]  
> 每個 RDSH 伺服器上執行下列程序。

1. 開啟登錄編輯程式，並瀏覽至**HKEY\_本機\_機器\\SYSTEM\\CurrentControlSet\\控制\\終端機伺服器\\RCM**.
2. 在 [登錄] 功能表上選取**匯出登錄檔案**。
3. 型別**匯出憑證**中**檔名**方塊，然後按**儲存**。
4. 以滑鼠右鍵按一下每個下列值，選取**刪除**，然後選取**是**以確認刪除：  
      - **[MSSQLSERVER 的通訊協定內容]**
      - **X509 憑證**
      - **X509 憑證識別碼**
      - **X509 Certificate2**
5. 結束登錄編輯器，然後再重新啟動 RDSH 伺服器。

## <a name="user-cannot-authenticate-or-must-authenticate-twice"></a>無法驗證使用者，或必須驗證兩次

有幾個問題，可能會造成影響的使用者驗證的問題。 本節說明下列案例：

  - [使用者拒絕存取的 「 限制的登入類型 」 訊息](#access-denied-restricted-type-of-logon)
  - [使用者或應用程式拒絕存取與事件 16965，「 SAM 資料庫的遠端呼叫已拒絕 」](#access-denied-a-remote-call-to-the-sam-database-has-been-denied)
  - [使用者無法登入，使用智慧卡](#a-user-cannot-sign-in-by-using-a-smart-card)
  - [如果遠端電腦已鎖定，使用者必須輸入密碼兩次](#if-the-remote-pc-is-locked-a-user-needs-to-enter-a-password-twice)
  - [使用者無法登入，並收到 「 驗證錯誤 」 和 「 CredSSP 加密 oracle 補救 」 訊息](#user-cannot-sign-in-and-receives-authentication-error-and-credssp-encryption-oracle-remediation-messages)
  - [更新用戶端電腦之後，某些使用者需要登入兩次](#after-you-update-client-computers-some-users-need-to-sign-in-twice)。
  - [使用者在使用多個 RD 連線代理人 RemoteGuard 的部署上的存取被拒](#users-are-denied-access-on-a-deployment-that-uses-remote-credential-guard-with-multiple-rd-connection-brokers)

### <a name="access-denied-restricted-type-of-logon"></a>拒絕存取，限制的類型的登入

在此情況下，嘗試連線到 Windows 10 或 Windows Server 2016 電腦的 Windows 10 使用者拒絕存取，並出現下列訊息：

> 遠端桌面連線：  
> 系統管理員已限制的登入類型 (網路或互動式)，您可以使用。 如需協助，請連絡您的系統管理員或技術支援。

當網路層級驗證 (NLA) 是為了透過 RDP 連線，而使用者不是成員，就會發生這個問題**Remote Desktop Users**群組。 如果，也會發生**Remote Desktop Users**群組未指派給**從網路存取這台電腦**使用者權限。

若要解決此問題，請遵循下列步驟：

  - [修改使用者的群組成員資格或使用者權限指派](#modify-the-users-group-membership-or-user-rights-assignment)。
  - 關閉 NLA （不建議）。
  - 使用 Windows 10 以外的遠端桌面用戶端。 比方說，Windows 7 用戶端並沒有此問題。

#### <a name="modify-the-users-group-membership-or-user-rights-assignment"></a>修改使用者的群組成員資格或使用者權限指派

如果此問題會影響單一使用者，此問題最簡單的解決方法是將使用者新增至**Remote Desktop Users**群組。

如果使用者已經是此群組的成員 （或多個群組成員是否相同的問題），請檢查遠端的 Windows 10 或 Windows Server 2016 電腦上的使用者權限設定。

1. 開啟群組原則物件編輯器 (GPE)，並連接到遠端電腦的本機原則。
2. 瀏覽至**電腦組態\\Windows 設定\\安全性設定\\本機原則\\使用者權限指派**，以滑鼠右鍵按一下**存取這個從網路的電腦**，然後選取**屬性**。
3. 檢查使用者和群組的清單**Remote Desktop Users** （或父群組）。
4. 如果清單不包含**Remote Desktop Users** (或父群組，例如**Everyone**) 您要將它新增至清單 （如果您在部署中有超過一個或兩台電腦，使用群組原則物件）.  
    例如的預設成員資格**從網路存取這台電腦**包含**Everyone**。 如果您的部署會使用群組原則物件，在移除**Everyone**，您可能需要還原藉由更新要加入的群組原則物件的存取權**Remote Desktop Users**。

### <a name="access-denied-a-remote-call-to-the-sam-database-has-been-denied"></a>拒絕存取，已拒絕在 SAM 資料庫中的遠端呼叫

此行為是最有可能發生，如果您的網域控制站執行 Windows Server 2016 或更新版本，而且使用者嘗試使用自訂的連接應用程式連線。 特別是，存取 Active Directory 中使用者的設定檔資訊的應用程式將被拒絕存取。

此行為起因變更到 Windows。 在 Windows Server 2012 R2 及較早版本中，當使用者登入 「 遠端桌面，遠端連接管理員 (RCM) 連絡人網域控制站 (DC)，來查詢特定 Active Directory 網域中使用者物件的遠端桌面組態服務 (AD DS)。 這項資訊會顯示在使用者的物件屬性中的 Active Directory 使用者和電腦 MMC 嵌入式管理單元的 遠端桌面服務設定檔 索引標籤。

從 Windows Server 2016 開始，RCM 不再會查詢 AD DS 中的使用者物件。 如果您需要 RCM 來查詢 AD DS，因為您正在使用遠端桌面服務屬性時，您必須手動啟用先前的 RCM 行為。

> [!IMPORTANT]  
> 請仔細依照本節中的步驟。 如果您未正確修改登錄，可能會發生嚴重問題。 您可以修改它，才能[備份進行還原登錄](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22)在發生問題。

若要啟用舊版 RCM 行為 RD 工作階段主機伺服器上的，設定下列登錄項目，然後重新啟動**Remote Desktop Services**服務：  
  - **HKEY\_本機\_MACHINE\\軟體\\原則\\Microsoft\\Windows NT\\終端機服務**
  - **HKEY\_本機\_MACHINE\\系統\\CurrentControlSet\\控制\\終端機伺服器\\WinStations\\\<視窗工作站名稱\>\\**  
      - 名稱： **fQueryUserConfigFromDC**
      - 輸入：**Reg\_DWORD**
      - 值：**1** （十進位）

若要啟用舊版的 RCM 行為，而不是 RD 工作階段主機伺服器的伺服器上，設定這些登錄項目和下列額外的登錄項目 （，然後重新啟動服務）：
  - **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server**

如需有關此行為的詳細資訊，請參閱 KB 3200967[變更至 Windows Server 中遠端連線管理員](https://support.microsoft.com/en-us/help/3200967/changes-to-remote-connection-manager-in-windows-server)。

### <a name="a-user-cannot-sign-in-by-using-a-smart-card"></a>使用者無法登入，使用智慧卡

本文說明三種常見的情況下，在其中的使用者無法登入遠端桌面使用智慧卡：  
  - [使用者無法登入分公司使用唯讀網域控制站 (RODC)](#cannot-sign-in-with-a-smart-card-in-a-branch-office-with-a-rodc)
  - [使用者無法登入 Windows Server 2008 SP2 的電腦最近已更新](#cannot-sign-in-with-a-smart-card-to-a-windows-server-2008-sp2-computer)
  - [使用者無法保持登入 最近已更新，在 Windows 電腦和遠端桌面服務服務會變成沒有回應 （懸置）](#cannot-stay-signed-in-with-a-smart-card-and-remote-desktop-services-service-hangs)

#### <a name="cannot-sign-in-with-a-smart-card-in-a-branch-office-with-a-rodc"></a>無法登入與 RODC 的分公司中的智慧卡

包含 RDSH 伺服器會使用 RODC 的分公司站台的部署中，就會發生此問題。 RDSH 伺服器被裝載根網域中。 在新分支站台的使用者隸屬於子網域，並使用智慧卡進行驗證。 RODC 已快取使用者的密碼 (RODC 所屬**允許的 RODC 密碼複寫群組**)。 當使用者嘗試登入 RDSH 伺服器上的工作階段時，他們收到的訊息例如 「 登入嘗試無效。 這是因為不正確的使用者名稱或驗證資訊。 」

這是已知的問題的方式，根目錄 DC 與 RODC 管理的使用者認證加密。 根 DC 會使用加密金鑰來加密認證，並在 RODC 提供給用戶端的一種解密金鑰。 不過，兩個金鑰不相符。

若要解決此問題，請考慮變更您的 DC 拓撲關閉 RODC 上快取的密碼，或將可寫入 DC 部署到分公司站台。 替代方法是將 RDSH 伺服器移至相同的子網域的使用者。 另一個替代方式是讓使用者登入，但沒有智慧卡。 任何這些解決方案可能需要在效能或安全性層級的入侵。

#### <a name="cannot-sign-in-with-a-smart-card-to-a-windows-server-2008-sp2-computer"></a>無法登入到 Windows Server 2008 SP2 的電腦的智慧卡

當使用者登入時 KB4093227 已更新的 Windows Server 2008 SP2 電腦，就會發生此問題 (2018.4B)。 當使用者嘗試使用智慧卡來登入時，使用者被拒絕存取訊息，例如 「 找不到有效的憑證。 檢查已正確插入智慧卡，而且緊密符合。 」 在此同時，Windows Server 電腦的應用程式事件記錄 「 插入的智慧卡從擷取的數位認證時發生錯誤。 無效的簽章。 」

若要解決此問題，請更新 Windows Server 電腦 2018.06 B 重新發行的知識庫 4093227 [Windows 遠端桌面通訊協定 (RDP) 阻斷服務弱點，在 Windows Server 2008 中的安全性更新的描述：2018 年 4 月 10 日](https://support.microsoft.com/en-us/help/4093227/security-update-for-vulnerabilities-in-windows-server-2008)。

#### <a name="cannot-stay-signed-in-with-a-smart-card-and-remote-desktop-services-service-hangs"></a>無法保持登入對智慧卡和遠端桌面服務服務停止回應

當使用者登入 KB 4056446 已更新的 Windows 或 Windows Server 電腦時，就會發生此問題。 一開始，使用者或許可以使用智慧卡，登入系統，但接著收到 「 捨棄\_電子\_NO\_服務 」 錯誤訊息。 在遠端電腦可能會變成沒有回應。

若要解決此問題，請重新啟動遠端電腦。

若要解決此問題，請使用適當的修正更新遠端電腦：

  - Windows Server 2008 SP2:KB 4090928， [Windows 流失 lsm.exe 程序中的控制代碼和智慧卡應用程式可能會顯示 「 捨棄\_E\_NO\_服務 」 錯誤](https://support.microsoft.com/en-us/help/4090928/scard-e-no-service-errors-when-windows-leaks-handles-in-the-lsm-exe)
  - Windows Server 2012 R2：KB 4103724， [2018 5 月 17 日 — KB4103724 （每月彙總套件預覽）](https://support.microsoft.com/en-us/help/4103724/windows-81-update-kb4103724)
  - Windows Server 2016 和 Windows 10 版本 1607年中：KB 4103720， [2018 5 月 17 日 — KB4103720 （OS 組建 14393.2273）](https://support.microsoft.com/en-us/help/4103720/windows-10-update-kb4103720)

### <a name="if-the-remote-pc-is-locked-a-user-needs-to-enter-a-password-twice"></a>如果遠端電腦已鎖定，使用者必須輸入密碼兩次

當使用者嘗試連線到 RDP 連線並需要 NLA 部署中執行 Windows 10 1709年版的遠端桌面，則可能會發生此問題。 根據這些條件，如果遠端桌面已被鎖定，使用者必須輸入其認證，連接時，兩次。

若要解決此問題，請更新 Windows 10 1709年版電腦 KB 4343893 [2018 年 8 月 30 日 — KB4343893 (OS 組建 16299.637)](https://support.microsoft.com/en-us/help/4343893/windows-10-update-kb4343893)。

### <a name="user-cannot-sign-in-and-receives-authentication-error-and-credssp-encryption-oracle-remediation-messages"></a>使用者無法登入，並收到 「 驗證錯誤 」 和 「 CredSSP 加密 oracle 補救 」 訊息

當使用者嘗試登入使用任何版本的 Windows，從 Windows Vista SP2 和更新版本或 Windows Server 2008 SP2 和更新版本時，它們會被拒絕存取，並接收訊息，如下所示：

  - 發生驗證錯誤。 不支援要求的函式。
  - 這可能是因為 CredSSP 加密 oracle 補救

"CredSSP 加密 oracle 補救 」 是指一組在 3 月、 年 4 月和的 2018 年發行的安全性更新。 CredSSP 會處理其他應用程式的驗證要求的驗證提供者。 年 3 月 13，2018 年 「 3B"和後續更新中解決的攻擊中，攻擊者無法轉送目標系統上執行程式碼的使用者認證。

初始更新已新增支援新的群組原則物件**加密 Oracle 補救**，具有下列可能的設定：

  - **易受攻擊**。 使用 CredSSP 的用戶端應用程式可以改為使用不安全的版本中，但這種行為會公開攻擊的遠端桌面。 使用 CredSSP 的服務會接受尚未更新的用戶端。
  - **降低**。 使用 CredSSP 的用戶端應用程式不能改為使用不安全的版本中，但使用 CredSSP 的服務接受尚未更新的用戶端。
  - **強制更新用戶端**。 使用 CredSSP 的用戶端應用程式不能改為使用不安全的版本中，並使用 CredSSP 的服務將不會接受未修補的用戶端。 
    注意:這項設定應該不會部署，直到所有的遠端主機支援的最新版本。

2018 年 5 月 8 日更新變更預設值**加密 Oracle 補救**從設定**Vulnerable**來**緩和**。 使用此項變更之後，已更新的遠端桌面用戶端無法連線到未安裝 （或更新但尚未重新啟動伺服器） 的伺服器。 如需效果的更新和它們所封鎖的通訊類型，請參閱 KB 4093492 [CredSSP 更新 CVE-2018年-0886年](https://support.microsoft.com/en-us/help/4093492/credssp-updates-for-cve-2018-0886-march-13-2018)。

若要解決此問題，請確定所有系統都完全更新，並重新啟動。 如需更新和弱點的詳細資訊的完整清單，請參閱[CVE-2018年-0886年 |CredSSP 的遠端執行程式碼弱點](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-0886)。

若要更新完成之前，請暫時解決此問題，請檢查 KB 4093492 為允許連接類型。 如果沒有任何可行的替代方案，您可以考慮下列方法之一：

  - 受影響的用戶端電腦，設定**加密 Oracle 補救**原則回到**Vulnerable**。
  - 修改中的下列原則**電腦組態\\系統管理範本\\Windows 元件\\Remote Desktop Services\\遠端桌面工作階段主機\\安全性**群組原則資料夾：  
      - **遠端 (RDP) 連線需要使用特定的安全性層級**： 設為**Enabled** ，然後選取**RDP**。
      - **使用網路層級驗證針對遠端連線以要求使用者驗證**： 設為**已停用**。
      > [!IMPORTANT]  
      > 這些修改會降低部署的安全性。 它們應該只是暫時性的如果您使用它們。

如需使用群組原則的詳細資訊，請參閱[修改封鎖的 GPO](#modifying-a-blocking-gpo)。

### <a name="after-you-update-client-computers-some-users-need-to-sign-in-twice"></a>某些使用者更新用戶端電腦之後，需要兩次登入

在某些 Windows 7 或 Windows 10 版本 1709年上的使用者登入遠端桌面工作階段，並立即看到另一個登入畫面。 如果用戶端電腦已收到下列更新，就會發生此問題：

  - Windows 7:KB 4103718， [8，2018 年 — KB4103718 （每月彙總）](https://support.microsoft.com/en-us/help/4103718/windows-7-update-kb4103718)
  - Windows 10 1709年:KB 4103727， [8，2018 年 — KB4103727 （OS 組建 16299.431）](https://support.microsoft.com/en-us/help/4103727/windows-10-update-kb4103727)

若要解決此問題，請確定使用者想要連線到 （以及 RDSH 或 RDVI 伺服器） 的電腦會完全更新起至 2018 年 6 月日止。 這包括下列更新：

  - Windows Server 2016:KB 4284880， [2018 年 6 月 12 日 — KB4284880 （OS 組建 14393.2312）](https://support.microsoft.com/en-us/help/4284880/windows-10-update-kb4284880)
  - Windows Server 2012 R2：KB 4284815， [2018 年 6 月 12 日 — KB4284815 （每月彙總）](https://support.microsoft.com/en-us/help/4284815/windows-81-update-kb4284815)
  - Windows Server 2012:KB 4284855， [2018 年 6 月 12 日 — KB4284855 （每月彙總）](https://support.microsoft.com/en-us/help/4284855/windows-server-2012-update-kb4284855)
  - Windows Server 2008 R2：KB 4284826， [2018 年 6 月 12 日 — KB4284826 （每月彙總）](https://support.microsoft.com/en-us/help/4284826/windows-7-update-kb4284826)
  - Windows Server 2008 SP2:KB4056564， [CredSSP 遠端程式碼執行的弱點可能會在 Windows Server 2008、 Windows Embedded POSReady 2009 中和 Windows Embedded Standard 2009 中的安全性更新的描述：2018 年 3 月 13日日](https://support.microsoft.com/en-us/help/4056564/security-update-for-vulnerabilities-in-windows-server-2008)

### <a name="users-are-denied-access-on-a-deployment-that-uses-remote-credential-guard-with-multiple-rd-connection-brokers"></a>使用者在遠端的 Credential Guard 使用多個 RD 連線代理人的部署上的存取被拒

如果正在使用中的 Windows Defender 遠端 Credential Guard，可在高可用性部署中使用兩個或多個遠端桌面連線代理人，發生此問題。 使用者無法登入遠端桌面。

因為遠端 Credential Guard 使用 Kerberos 進行驗證，並限制 NTLM，就會發生此問題。 不過，在負載平衡高可用性組態中，RD 連線代理人無法支援 Kerberos 的作業。

如果您要使用負載平衡的 RD 連線代理人高可用性組態，您可以藉由停用遠端 Credential Guard 暫時解決此問題。 如需如何管理 Windows Defender 和 Credential Guard 遠端的詳細資訊，請參閱[與 Windows Defender 遠端 Credential Guard 保護遠端桌面認證](https://docs.microsoft.com/en-us/windows/security/identity-protection/remote-credential-guard#enable-windows-defender-remote-credential-guard)。

## <a name="on-connecting-user-receives-remote-desktop-service-is-currently-busy-message"></a>在連接時，使用者會收到 「 遠端桌面服務目前忙碌中 」 訊息

若要判斷適當的回應，此問題，請使用下列步驟：

1. 沒有遠端桌面服務的服務變得沒有回應 （例如，遠端桌面用戶端會顯示以 「 擱置 」，在 [歡迎] 畫面）。  
      - 如果服務變得沒有回應，請參閱[RDSH 伺服器記憶體問題](#rdsh-server-memory-issue)。
      - 如果用戶端似乎通常與服務互動，請繼續下一個步驟。
2. 如果一或多個使用者中斷連線其遠端桌面工作階段，可以使用者重新連線嗎？  
   - 如果，服務會繼續拒絕連線，不論有多少使用者中斷連線工作階段，請參閱[RD 接聽程式問題](#rd-listener-issue)。
   - 如果該服務會開始接受連接的使用者數目之後，再次都中斷連接其工作階段中，[檢查連線限制原則](#check-the-connection-limit-policy)。

### <a name="rdsh-server-memory-issue"></a>RDSH 伺服器記憶體問題

在某些 Windows Server 2012 R2 RDSH 伺服器上發現有記憶體流失。 經過一段時間，這些伺服器會開始拒絕遠端桌面連線和本機主控台登入訊息，如下所示：

> 無法完成您嘗試執行的工作，因為遠端桌面服務目前忙碌中。 請再試一次幾分鐘的時間。 其他使用者應該仍然能夠登入。

遠端桌面用戶端嘗試連線也會變成沒有回應。

若要解決此問題，請重新啟動 RDSH 伺服器。

若要解決此問題，請套用 KB 4093114 [2018 年 4 月 10 日 — KB4093114 （每月彙總）] (c:file:///\\使用者\\v jesits\\AppData\\本機\\Microsoft\\Windows\\INetCache\\Content.Outlook\\FUB8OO45\\年 4 月 %%202018 2010年 — KB4093114 %20\(每月 %20rollup\))，到 RDSH 伺服器。

### <a name="rd-listener-issue"></a>RD 接聽程式問題

在已直接從 Windows Server 2008 R2 到 Windows Server 2012 R2 升級某些 RDSH 伺服器或 Windows Server 2016 上曾經發生問題。 當遠端桌面用戶端連線到 RDSH 伺服器時，RDSH 伺服器會建立使用者工作階段的 RD 接聽程式。 受影響的伺服器保留 RD 的接聽程式會隨著使用者連線，而增加，但永遠不會減少的計數。

若要解決此問題，您可以使用下列方法：

  - 請重新啟動 RDSH 伺服器重設 RD 接聽程式的計數
  - 修改連接限制原則，將它設定為非常大的值。 如需管理連接限制原則的詳細資訊，請參閱 <<c0> [ 檢查連線限制原則](#check-the-connection-limit-policy)。

若要解決此問題，請至 RDSH 伺服器套用下列更新：

  - Windows Server 2012 R2：KB 4343891， [2018 年 8 月 30 日 — KB4343891 （每月彙總套件預覽）](https://support.microsoft.com/en-us/help/4343891/windows-81-update-kb4343891)
  - Windows Server 2016:KB 4343884， [2018 年 8 月 30 日 — KB4343884 （OS 組建 14393.2457）](https://support.microsoft.com/en-us/help/4343884/windows-10-update-kb4343884)

### <a name="check-the-connection-limit-policy"></a>請檢查連線限制原則

您可以在個別的電腦層級，或藉由設定群組原則物件 (GPO) 設定同時的遠端桌面連線的數目限制。 根據預設，未設定的限制。

若要檢查目前的設定，並識別任何現有的 Gpo RDSH 伺服器上，開啟 命令提示字元視窗，以系統管理員身分，並輸入下列命令：
  
```
gpresult /H c:\gpresult.html
```
   
此命令完成時，開啟 gpresult.html 之後，並在**電腦組態\\系統管理範本\\Windows 元件\\Remote Desktop Services\\遠端桌面工作階段主機\\連線**，尋找**限制連線數目**原則。

  - 如果此原則設定為**已停用**，則群組原則不會限制透過 RDP 連線。
  - 如果此原則設定為**Enabled**，然後核取**優勢 GPO**。 如果您需要移除或變更連線限制，請編輯此 GPO。

若要強制執行原則變更，請開啟命令提示字元 視窗，在受影響的電腦，並輸入下列命令：
  
```
gpupdate /force
```
  
## <a name="rd-client-disconnects-and-cannot-reconnect-to-the-same-session"></a>RD 用戶端中斷連線，並無法重新連接至相同的工作階段

遠端桌面用戶端失去遠端桌面連接之後，用戶端無法立即重新連線。 使用者會收到錯誤訊息，如下所示：

  - 安全性錯誤，因為用戶端無法連線到終端機伺服器。 確定您已登入網路之後, 再試一次連接到伺服器。
  - 已中斷連線的遠端桌面。 安全性錯誤，因為用戶端無法連接到遠端電腦。 請確認您已登入網路，並再試一次。

稍後，遠端桌面用戶端重新連線到遠端桌面。 不過，而不是將用戶端連接到原始的工作階段，RDSH 伺服器將用戶端連接至新的工作階段。 檢查 RDSH 伺服器會顯示，原始的工作階段未輸入中斷連線的狀態，但改為會維持使用中。

若要暫時解決此問題，您可以啟用**設定為 keep-alive 連線間隔**中的原則**電腦設定\\系統管理範本\\Windows 元件\\遠端桌面服務\\遠端桌面工作階段主機\\連線**群組原則資料夾。 如果您啟用此原則時，您必須輸入 keep-alive 間隔。 保持連接間隔會決定頻率，以分鐘為單位，伺服器會檢查工作階段狀態。

此問題可能因您的驗證和組態設定的設定不正確。 您可以在伺服器層級，或使用 Gpo 來設定這些設定。 群組原則設定位於**電腦組態\\系統管理範本\\Windows 元件\\Remote Desktop Services\\遠端桌面工作階段主機\\安全性**群組原則資料夾。

1. 若要解決此問題，必須重新啟動遠端桌面連線代理人服務。
2. 底下**連線**，以滑鼠右鍵按一下連線名稱，然後按一下**屬性**。
3. 在**屬性**連線 對話方塊上**一般**索引標籤**安全性**圖層中，選取安全性方法。
4. 在 **加密層級**，按一下您想要的層級。 您可以選取**低**，**用戶端相容**，**高**，或**FIPS 相容**。

> [!NOTE]  
>  - 當用戶端與 RD 工作階段主機伺服器之間的通訊需要最高層級的加密時，使用 FIPS 相容的加密。
>  - 設定群組原則中的任何加密層級設定會覆寫您使用遠端桌面服務組態工具設定的組態。 此外，如果您啟用[系統密碼編譯：使用 FIPS 相容演算法於加密、 雜湊和簽署](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2003/cc780081\(v=ws.10\))原則，此設定會覆寫**設定用戶端連線加密層級**原則。 系統密碼編譯原則處於**電腦組態\\Windows 設定\\安全性設定\\本機原則\\安全性選項**資料夾。
>  - 當您變更加密層級時，新的加密層級會在下次使用者登入時生效。 如果您需要多個層級的一部伺服器上的加密時，安裝多個網路介面卡，並個別設定每張介面卡。
>  - 若要確認該憑證具有對應的私密金鑰，遠端桌面服務組態中，以滑鼠右鍵按一下您想要檢視憑證，請選取的連線**一般**，選取**編輯**，選取您想要檢視，然後選取 憑證**檢視憑證**。 在底部**一般**索引標籤上，陳述式中，「 您有此憑證相對應的私密金鑰 」 應該會出現。 您也可以使用 [憑證] 嵌入式管理單元，以檢視這項資訊。
>  - 符合 FIPS 規範的加密 (**系統密碼編譯：使用 fips 相容方法於加密、 雜湊和簽署**原則或**FIPS 相容**在遠端桌面伺服器組態中設定) 來加密及解密從用戶端在伺服器來回傳送的資料伺服器到用戶端，與美國聯邦資訊處理標準 (FIPS) 140-1 加密演算法、 使用 Microsoft 密碼編譯模組。 如需詳細資訊，請參閱 < [FIPS 140 驗證](https://docs.microsoft.com/en-us/windows/security/threat-protection/fips-140-validation)。
>  - **高**設定加密從伺服器到用戶端和伺服器到用戶端傳送使用強式 128 位元加密的資料。
>  - **用戶端相容**設定加密用戶端和用戶端支援的最大索引鍵強度的伺服器之間傳送資料。
>  - **低**設定加密從用戶端傳送到伺服器使用 56 位元加密的資料。

## <a name="remote-laptop-disconnects-from-wireless-network"></a>遠端的膝上型電腦中斷連線的無線網路

使用 802.1x 的無線網路的遠端桌面用戶端連接到在膝上型電腦時，可能會發生此問題。 偶爾膝上型電腦中斷與無線網路的連線，並不會自動重新連線。

這是無線網路連線的網路驗證設定時，就會發生的已知的問題**使用者驗證**。

若要解決此問題，請將網路驗證設定為**使用者或電腦驗證**或是**電腦驗證**。

 > [!NOTE]  
> 若要變更在單一電腦上的網路驗證設定，您可能需要使用網路和共用中心 控制台來使用新的設定建立新的無線連線。

如需如何設定使用 Gpo 的無線網路設定的完整說明，請參閱[設定的無線網路 (IEEE 802.11) 原則](https://docs.microsoft.com/en-us/windows-server/networking/core-network-guide/cncg/wireless/e-wireless-access-deployment#bkmk_policies)。

## <a name="user-experiences-poor-performance-or-application-problems"></a>使用者體驗不佳的效能或應用程式的問題

本節說明在使用遠端桌面功能時，使用者可能會遇到幾個常見的問題：

  - [間歇性連線至新虛擬機器的 Microsoft Azure 使用者遇到問題](#intermittent-problems-with-new-microsoft-azure-virtual-machines)。
  - [連線到 Windows 10 1709年版的遠端桌面的使用者可能會有播放視訊的問題](#video-playback-issues-on-windows-10-version-1709)。
  - [使用唯讀使用者設定檔連線到 Windows 10 的遠端桌面的使用者不能共用桌面。](#desktop-sharing-issues-on-windows-10)
  - [NLA 停用時，連接到 Windows 10 的遠端桌面使用舊版的 Windows 10 執行的用戶端的使用者會遇到效能不佳](#performance-issues-when-mixing-versions-of-windows-10-if-nla-is-disabled)
  - [當使用者在 遠端桌面工作階段中執行多個應用程式和中斷連線，並經常重新連線時，它們可能會遇到全黑螢幕重新連接。](#black-screen-issue)

### <a name="intermittent-problems-with-new-microsoft-azure-virtual-machines"></a>新的 Microsoft Azure 虛擬機器的間歇性問題

此問題會影響最近佈建的虛擬機器。 使用者連線至虛擬機器後，遠端桌面工作階段可能不會載入所有使用者的設定正確。

若要解決此問題，從虛擬機器中斷連接，等待至少 20 分鐘，然後連接一次。

若要解決此問題，請到虛擬機器，視需要套用下列更新：

  - Windows 10 和 Windows Server 2016:KB 4343884， [2018 年 8 月 30 日 — KB4343884 （OS 組建 14393.2457）](https://support.microsoft.com/en-us/help/4343884/windows-10-update-kb4343884)
  - Windows Server 2012 R2：KB 4343891， [2018 年 8 月 30 日 — KB4343891 （每月彙總套件預覽）](https://support.microsoft.com/en-us/help/4343891/windows-81-update-kb4343891)

### <a name="video-playback-issues-on-windows-10-version-1709"></a>在 Windows 10 1709年版上的視訊播放問題

使用者連線到執行 Windows 10 1709年版的遠端電腦時，就會發生此問題。 當這些使用者播放影片使用 VMR9 (視訊混合使用轉譯器 9) 的轉碼器，播放程式顯示黑色視窗。

這是 Windows 10 1709年版中的已知的問題。 在 Windows 10 1703年版中不會發生問題。

### <a name="desktop-sharing-issues-on-windows-10"></a>桌面共用在 Windows 10 上的問題

當使用者具有僅限讀取使用者設定檔中 （和相關聯的登錄 hive），例如資訊站案例中，就會發生此問題。 當這類使用者連線到遠端電腦執行 Windows 10 版本 1803，它們不能共用他們的桌面。

若要修正此問題，請將 Windows 10 更新 4340917，套用[2018 年 7 月 24 日 — KB4340917 (OS 組建 17134.191)](https://support.microsoft.com/en-us/help/4340917/windows-10-update-kb4340917)。

### <a name="performance-issues-when-mixing-versions-of-windows-10-if-nla-is-disabled"></a>如果已停用 NLA 混合的 Windows 10 版本時的效能問題

NLA 會停用，並執行 Windows 10 的遠端桌面用戶端電腦連線到執行不同版本的 Windows 10 的遠端桌面時，就會發生此問題。 在連線到執行 Windows 10 1803年版或更新版本的遠端桌面時，在執行 Windows 10 1709年版或更早版本的電腦上的遠端桌面用戶端的使用者體驗不佳的效能。

這是因為在連線到 Windows 10，1803年版或更新版本時，較舊的用戶端電腦 NLA 停用時，使用速度較慢的通訊協定。

若要解決此問題，請套用 KB 4340917 [2018 年 7 月 24 日 — KB4340917 (OS 組建 17134.191)](https://support.microsoft.com/en-us/help/4340917/windows-10-update-kb4340917)。

### <a name="black-screen-issue"></a>黑色畫面問題

Windows 8.0、 Windows 8.1，Windows 10 RTM 和 Windows Server 2012 R2 中，就會發生此問題。 使用者啟動遠端桌面，在多個應用程式，然後從工作階段中斷連線。 定期，使用者重新連線到遠端桌面的應用程式互動，並再中斷連線一次。 在某些時候，當使用者重新連線，遠端桌面工作階段只會顯示黑色畫面。 使用者必須結束遠端桌面工作階段，從遠端電腦的主控台 （或 RDSH 伺服器主控台）。 這個動作會停止應用程式。

若要解決此問題，套用下列更新適當地：

  - Windows 8 和 Windows Server 2012:KB4103719， [2018 5 月 17 日 — KB4103719 （每月彙總套件預覽）](https://support.microsoft.com/en-us/help/4103719/windows-server-2012-update-kb4103719)
  - Windows 8.1 和 Windows Server 2012 R2:KB4103724， [2018 5 月 17 日 — KB4103724 （每月彙總套件預覽）](https://support.microsoft.com/en-us/help/4103724/windows-81-update-kb4103724)和 KB 4284863， [2018 年 6 月 21 日 — KB4284863 （每月彙總套件預覽）](https://support.microsoft.com/en-us/help/4284863/windows-81-update-kb4284863)
  - Windows 10：已修正在 KB4284860， [2018 年 6 月 12 日 — KB4284860 （OS 組建 10240.17889）](https://support.microsoft.com/en-us/help/4284860/windows-10-update-kb4284860)
