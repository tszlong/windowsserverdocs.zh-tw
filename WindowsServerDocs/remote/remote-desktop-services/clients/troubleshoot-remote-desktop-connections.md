---
title: 遠端桌面連線疑難排解
description: 疑難排解程序排列徵兆
ms.custom: na
ms.reviewer: rklemen;josh.bender
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: troubleshooting
ms.assetid: ''
author: jobende
manager: ''
ms.author: v-tea;kaushika;rklemen;josh.bender
ms.date: 02/22/2019
ms.localizationpriority: medium
ms.openlocfilehash: 8965df09fd57decf7f4a1b6b89861e5a67034a0a
ms.sourcegitcommit: d3f73936160505a40633ad8dd5931ac5fe3eccdb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/12/2019
ms.locfileid: "9297397"
---
# 遠端桌面連線疑難排解
幾種最常見的遠端桌面服務 (RDS) 問題的簡短說明，請參閱[常見問題集遠端桌面用戶端相關資訊](https://review.docs.microsoft.com/en-us/windows-server/remote/remote-desktop-services/clients/remote-desktop-client-faq)。 本文章說明更進階的數種方式連線問題疑難排解。 這些程序的許多適用於是否您要進行疑難排解簡單的設定，例如一部實體電腦連線到另一部實體電腦或更複雜的設定。 只有在更複雜的多使用者案例中發生問題並解決某些程序。 如需有關在遠端桌面元件，以及如何一起運作的詳細資訊，請參閱[遠端桌面服務架構](https://docs.microsoft.com/en-us/windows-server/remote/remote-desktop-services/desktop-hosting-logical-architecture)。

> [!NOTE]  
> 許多這篇文章中所述的程序需要您存取多部電腦，其中有些可能要從遠端存取。 如需遠端系統管理工具，以及如何設定它們的詳細資訊，請參閱[遠端伺服器管理工具 (RSAT) 的 Windows 作業系統](https://support.microsoft.com/en-us/help/2693643/remote-server-administration-tools-rsat-for-windows-operating-systems)。

在下列清單中，找出您 （或您的使用者） 所遇到的徵狀的類型。

- [遠端桌面用戶端無法連線到遠端桌面，但沒有特定的徵狀或訊息 （一般疑難排解步驟）](#no-specific-symptoms-or-messages-general-troubleshooting-steps)
- [遠端桌面用戶端無法連線到遠端桌面，並會收到 「 類別未登錄 」 的訊息](#client-cannot-connect-class-not-registered)
- [遠端桌面用戶端無法連線到遠端桌面，並會收到 「 沒有可用的授權 」 或 「 安全性錯誤 」 訊息](#client-cannot-connect-no-licenses-available)
- [使用者會收到 「 拒絕存取 」 訊息，或兩次必須提供認證](#user-cannot-authenticate-or-must-authenticate-twice)
- [在連線時，會收到 「 遠端桌面服務是目前忙碌 」 的訊息](#on-connecting-user-receives-remote-desktop-service-is-currently-busy-message)
- [遠端桌面用戶端中斷連線，並無法重新連接到相同的工作階段](#rd-client-disconnects-and-cannot-reconnect-to-the-same-session)
- [使用者連線到遠端透過無線網路，膝上型電腦和膝上型電腦再從網路中斷](#remote-laptop-disconnects-from-wireless-network)。
- [使用者體驗不佳的效能或遠端應用程式的問題](#user-experiences-poor-performance-or-application-problems)

> [!NOTE]  
> 如需目前的 RDP 中斷連線代碼清單，請參閱[ExtendedDisconnectReasonCode 列舉](https://docs.microsoft.com/en-us/windows/desktop/TermServ/extendeddisconnectreasoncode)。 

## 沒有特定的徵狀或訊息 （一般疑難排解步驟）

遠端桌面用戶端無法連線到遠端桌面，但不會提供訊息或其他會有幫助的徵狀識別原因時，請使用下列步驟執行。 若要解決許多這類問題最常見的原因，請使用下列方法：

- [檢查 RDP 通訊協定的狀態](#check-the-status-of-the-rdp-protocol)
- [檢查 RDP 服務的狀態](#check-the-status-of-the-rdp-services)
- [檢查運作 RDP 接聽程式](#check-that-the-rdp-listener-is-functioning)
- [核取 RDP 接聽連接埠](#check-the-rdp-listener-port)

### 檢查 RDP 通訊協定的狀態

#### 檢查 RDP 通訊協定的本機電腦上的狀態

若要檢查，並在本機電腦上的 RDP 通訊協定的狀態變更，請參閱[如何啟用遠端桌面](https://docs.microsoft.com/en-us/windows-server/remote/remote-desktop-services/clients/remote-desktop-allow-access#how-to-enable-remote-desktop)。

> [!NOTE]  
> 如果未提供遠端桌面的選項，請參閱[檢查的群組原則物件是否會封鎖 RDP](#check-whether-a-group-policy-object-gpo-is-blocking-rdp-on-a-local-computer)。

#### 檢查 RDP 通訊協定的遠端電腦上的狀態

> [!IMPORTANT]  
> 請謹慎依照本節中的步驟。 如果您未正確修改登錄，可能會發生嚴重問題。 之前您修改它[備份還原的登錄，](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22)以便在發生問題。

若要檢查，並將 RDP 通訊協定的遠端電腦上的狀態變更，請使用網路登錄連線：

1. 選取 **[開始]**、 選取 [**執行**]，然後輸入**regedt32**。
2. 在 「 登錄編輯程式中，選取 [**檔案**]，然後選取 [**連線網路登錄**。
3. 中**選取電腦**] 對話方塊中，輸入 [遠端電腦的名稱，選取 [**檢查名稱**，，然後選取 **[確定]**。
4. 瀏覽至**HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal 伺服器**。  
   ![登錄編輯程式，顯示 fDenyTSConnections 項目](..\media\troubleshoot-remote-desktop-connections\RegEntry_fDenyTSConnections.png)
   - 如果**fDenyTSConnections**金鑰的值為**0**，然後 RDP 已啟用
   - 如果**fDenyTSConnections**金鑰的值為**1**，則 RDP 會停用
5. 若要啟用 RDP，從**1**變更**fDenyTSConnections**的值為**0**。

#### 檢查 [群組原則物件 (GPO) 是否會在本機電腦上封鎖 RDP

如果您無法將 RDP 使用者介面中開啟，或您變更之後的**fDenyTSConnections**值會還原為**1** ，GPO 可能會覆寫電腦層級的設定。

若要檢查在本機電腦上的群組原則設定，開啟 \ [以系統管理員身分，命令提示字元視窗並輸入下列命令：
```
gpresult /H c:\gpresult.html
```
此命令完成之後，開啟 gpresult.html。 在 [**電腦設定 \\ 系統管理範本 \\windows 元件 Components\\Remote 桌面 Services\\Remote 桌面工作階段 Host\\Connections**，找到 [**允許使用者使用遠端桌面服務，來從遠端連線**的原則。

- 如果**啟用**這個原則設定，群組原則不會封鎖 RDP 連線。
- 如果**已停用**這個原則設定，請檢查**贏利的 GPO**。 這是會封鎖 RDP 連線的 GPO。
![Gpresult.html，其中的範例區隔網域層級 GPO * * 區塊 RDP * * \ [已停用 RDP。](..\media\troubleshoot-remote-desktop-connections\GPResult_RDSH_Connections_GP.png)
   
  ![Gpresult.html，其中的範例區隔 * * \ [本機群組原則 \] * * 停用 RDP。](..\media\troubleshoot-remote-desktop-connections\GPResult_RDSH_Connections_LGP.png)

#### 檢查 GPO 是否會封鎖 RDP 遠端電腦上

若要檢查在遠端電腦上的群組原則設定，則命令為與本機電腦幾乎相同：
```
gpresult /S <computer name> /H c:\gpresult-<computer name>.html
```
此命令會產生的檔案 (**gpresult-\<computer name\>.html**) 做為使用本機電腦版本 (**gpresult.html**) 會使用相同的資訊格式。

#### 修改封鎖 GPO

您可以修改這些設定中的群組原則物件編輯器 (GPE) 與群組原則管理主控台 (GPM)。 如需有關如何使用群組原則的詳細資訊，請參閱[進階群組原則管理](https://docs.microsoft.com/en-us/microsoft-desktop-optimization-pack/agpm/)。

若要修改封鎖原則，使用下列方法的其中一個：

- 在 GPE，存取 （例如本機或網域），適當的 GPO 層級，並瀏覽至 [**電腦設定 \\ 系統管理範本 \\windows 元件 Components\\Remote 桌面 Services\\Remote 桌面工作階段 Host\\Connections**\\允許**使用者使用遠端桌面服務從遠端連線**。  
   1. 設定原則來**啟用**或**未設定**。
   2. 受影響的電腦上開啟以系統管理員身分，在命令提示字元視窗並執行**gpupdate /force**命令。
- 在 GPM，瀏覽至 OU 中封鎖原則套用到受影響的電腦，並刪除該原則從 OU。

### 檢查 RDP 服務的狀態

在本機 （用戶端） 電腦和遠端 （目標） 電腦，應該執行下列服務：

- 遠端桌面服務 (TermService)
- 遠端桌面服務使用者模式的連接埠重新導向器 (UmRdpService)

若要在本機或遠端管理的服務，您可以使用 \ [服務] MMC 嵌入式管理單元中。 您也可以在本機或從遠端使用 PowerShell （如果遠端電腦已設定為接受遠端的 PowerShell 命令）。

![服務 MMC 嵌入式管理單元的遠端桌面服務。 不要修改預設的服務設定。](..\media\troubleshoot-remote-desktop-connections\RDSServiceStatus.png)

任一種在電腦上，如果一或兩個服務未執行，啟動它們。

> [!NOTE]  
> 如果您啟動遠端桌面服務服務，按一下 **[是]** 來自動重新啟動遠端桌面服務使用者模式的連接埠重新導向器服務。

### 檢查運作 RDP 接聽程式

> [!IMPORTANT]  
> 請謹慎依照本節中的步驟。 如果您未正確修改登錄，可能會發生嚴重問題。 之前您修改它[備份還原的登錄，](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22)以便在發生問題。

#### 檢查 RDP 接聽程式的狀態

進行此程序中，使用具有系統管理權限的 PowerShell 執行個體。 本機電腦，您也可以使用具有系統管理權限的命令提示字元。 不過，這個程序使用 PowerShell，因為相同的命令工作兩者本機和遠端。

1. 開啟 PowerShell 視窗。 若要連線至遠端電腦，請輸入**Enter-pssession-ComputerName \<computer name\>**。
2. 輸入**qwinsta**。 
    ![Qwinsta 命令列出接聽的連接埠的電腦上的處理程序。](..\media\troubleshoot-remote-desktop-connections\WPS_qwinsta.png)
3. 如果清單中包含**rdp tcp**且狀態為**接聽**，使用 RDP 接聽程式。 繼續進行[檢查 RDP 接聽連接埠](#check-the-rdp-listener-port)。 否則，繼續在步驟 4。
4. 從工作電腦中匯出 RDP 接聽程式設定。
    1. 受影響的電腦有，因為有相同的作業系統版本的電腦登入及存取該電腦的登錄 （例如，透過使用登錄編輯程式）。
    2. 瀏覽至下列登錄項目：  
        **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server\\WinStations\\RDP Tcp**
    3. 將項目匯出至.reg 檔案。 例如，在 [登錄編輯程式，以滑鼠右鍵按一下項目、 選取**匯出**，，然後輸入檔案名稱匯出設定。
    4. 將匯出的.reg 檔案複製到受影響的電腦。
5. 匯入 RDP 接聽程式設定、 開啟受影響的電腦擁有系統管理權限的 PowerShell 視窗 （或開啟 PowerShell 視窗，並從遠端連線到受影響的電腦）。
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
      其中 \<filename\> 是匯出的.reg 檔案的名稱。
6. 嘗試遠端桌面連線一次來測試設定。 如果仍然無法連線，重新啟動受影響的電腦。
7. 如果您仍然無法連線時，[檢查 RDP 自我簽署憑證的狀態](#check-the-status-of-the-rdp-self-signed-certificate)。

#### 檢查 RDP 自我簽署憑證的狀態

1. 如果仍然無法連接，開啟 \ [憑證 MMC 嵌入式管理單元中。 當系統提示您選取的憑證存放區，以管理，選取**電腦帳戶**，然後選取 [受影響的電腦。
2. 在 [**憑證**] 資料夾底下**遠端桌面**，刪除 RDP 自我簽署的憑證。 
    ![遠端桌面憑證，憑證 MMC 嵌入式管理單元。](..\media\troubleshoot-remote-desktop-connections\MMCCert_Delete.png)
3. 受影響的電腦上重新啟動的遠端桌面服務服務。
4. \ [憑證嵌入式管理單元中重新整理。
5. 如果尚未重新建立，RDP 自我簽署的憑證[檢查 MachineKeys 資料夾的權限](#check-the-permissions-of-the-machinekeys-folder)。

#### 檢查 MachineKeys 資料夾的權限

1. 受影響的電腦上開啟檔案總管]，然後瀏覽到**C:\\ProgramData\\Microsoft\\Crypto\\RSA\\**。
2. 以滑鼠右鍵按一下**MachineKeys**、 選取 [**屬性**]、 選取**安全性**，然後選取**進階**。
3. 請確定已設定下列的權限：
      - Builtin\\Administrators： 完整控制
      - 所有人： 讀取、 寫入

### 核取 RDP 接聽連接埠

在本機 （用戶端） 電腦和遠端 （目標） 電腦上 RDP 接聽程式應接聽連接埠 3389 上中。 沒有其他應用程式應該使用此連接埠。

> [!IMPORTANT]  
> 請謹慎依照本節中的步驟。 如果您未正確修改登錄，可能會發生嚴重問題。 之前您修改它[備份還原的登錄，](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22)以便在發生問題。

若要檢查或變更的 RDP 連接埠，使用登錄編輯程式：

1. 選取 [開始 \]，選取 [**執行**]，然後輸入**regedt32**。
      - 以連線至遠端電腦，選取**檔案**，然後選取 [**連線網路登錄**。
      - 中**選取電腦**] 對話方塊中，輸入 [遠端電腦的名稱，選取 [**檢查名稱**，，然後選取 **[確定]**。
2. 開啟登錄，並瀏覽至**HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server\\WinStations\\\<listener\>**。 
    ![RDP 通訊協定 PortNumber 子機碼。](..\media\troubleshoot-remote-desktop-connections\RegEntry_PortNumber.png)
3. 如果**PortNumber**有**3389**以外的值，則**3389**來加以變更。 
   > [!IMPORTANT]  
    > 您可以操作使用另一個連接埠的遠端桌面服務。 不過，我們不建議您這麼。 疑難排解類設定是超出本文章的範圍。
4. 變更連接埠號碼之後，重新啟動的遠端桌面服務服務。

#### 另一個應用程式不嘗試使用相同的連接埠的核取

進行此程序中，使用具有系統管理權限的 PowerShell 執行個體。 本機電腦，您也可以使用具有系統管理權限的命令提示字元。 不過，這個程序使用 PowerShell，因為相同的命令使用本機和遠端。

1. 開啟 PowerShell 視窗。 若要連線至遠端電腦，請輸入**Enter-pssession-ComputerName \<computer name\>**。
2. 輸入下列命令：  
   
     ```powershell  
    cmd /c 'netstat -ano | find "3389"'  
    ```
  
    ![Netstat 命令會產生一份連接埠和接聽給他們的服務。](..\media\troubleshoot-remote-desktop-connections\WPS_netstat.png)
1. 且狀態為 [**正在聆聽**尋找 TCP 連接埠 3389 的項目 （或受指派的 RDP 連接埠）。 
    > [!NOTE]  
   > 下 PID 欄中出現的處理程序或服務使用的連接埠的 PID （處理程序識別碼）。
1. 若要判斷哪些應用程式使用連接埠 3389 （或受指派的 RDP 連接埠），輸入下列命令：  
   
     ```powershell  
    cmd /c 'tasklist /svc | find "<pid listening on 3389>"'  
    ```  
  
    ![Tasklist 命令會回報特定處理程序的詳細資料。](..\media\troubleshoot-remote-desktop-connections\WPS_tasklist.png)
1. 尋找 PID 數 （從**netstat**輸出） 的連接埠與相關聯的項目。 服務或與該 PID 相關聯的處理程序顯示在右側。
1. 如果應用程式或服務以外遠端桌面服務 (TermServ.exe) 使用連接埠，您可以透過下列方法的其中一個解決衝突：
      - 設定的其他應用程式或服務使用不同的連接埠 （建議選項）。
      - 解除安裝的其他應用程式或服務。
      - 設定要使用不同的連接埠，RDP，然後重新啟動遠端桌面服務服務 （不建議）。

#### 檢查是否有防火牆封鎖 RDP 連接埠

若要測試，您可以透過使用連接埠 3389 擴及受影響的電腦是否使用**psping**工具。

1. 在電腦上此地址不同於受影響的電腦，下載**psping**從<https://live.sysinternals.com/psping.exe>。
2. 開啟 \ [以系統管理員命令提示字元視窗、 變更到您已安裝**psping**，目錄，然後輸入下列命令：  
   
   ```  
   psping -accepteula <computer IP>:3389  
   ```
   
3. 檢查**psping**命令，以取得結果，例如下列的輸出：  
      - **連線到 \<computer IP\>**： 遠端電腦是保持連線。
      - **（0%遺失）**： 所有嘗試連線都成功。
      - **遠端電腦拒絕網路連線**： 遠端電腦不是保持連線。
      - **（100%遺失）**： 所有嘗試連線都失敗。
1. 若要測試能夠連線至受影響的電腦的多部電腦上執行**psping** 。
1. 請注意受影響的電腦是否會封鎖來自所有其他電腦、 其他電腦或只有一個其他電腦連線。
1. 建議的下一個步驟：
      - 請與您的網路系統管理員，確認網路可讓受影響的電腦的 RDP 流量。
      - 若要判斷是否有防火牆封鎖 RDP 連接埠調查來源電腦和受影響的電腦 （包括受影響的電腦上的 Windows 防火牆） 之間的任何防火牆的設定。

## 用戶端無法連線時，「 類別未登錄 」

當您嘗試連線至遠端電腦使用用戶端正在執行 Windows 10，版本 1709年或更新版本，用戶端可能不會連線的遠端桌面工作階段主機伺服器報告訊息時，會包含 「 類別未登錄 (0x80040154) 」 的錯誤碼。

如果嘗試連線的使用者的強制使用者設定檔，就會發生此問題。 若要解決此問題，請安裝[2018 年 7 月 24 日 — KB4338817 (作業系統組建 16299.579)](https://support.microsoft.com/en-us/help/4338817/windows-10-update-kb4338817) Windows 10 更新。

## 用戶端無法連線時，沒有可用的授權

這種情況適用於包含 RDSH 伺服器和遠端桌面授權伺服器的部署。

找出哪些類型的使用者會看到的行為：

- 工作階段已中斷，因為沒有授權或沒有授權伺服器可用
- 因為安全性錯誤拒絕存取

RD 工作階段主機網域系統管理員身分登入，然後開啟 RD 授權 Diagnoser。 尋找下列訊息：

  - 在寬限期針對遠端桌面工作階段主機伺服器已過期，但不是已使用的任何授權伺服器設定 RD 工作階段主機伺服器。 除非授權伺服器已設定 RD 工作階段主機伺服器的 RD 工作階段主機伺服器的連線將會遭到拒絕。
  - 授權伺服器 \<computer name\> 就無法使用。 這可能因為網路連線問題、 遠端桌面授權服務已停止在授權伺服器上，或 RD 授權不再安裝在電腦上。

這些問題傾向於下列使用者訊息相關聯：

  - 遠端工作階段已中斷，因為不沒有可用的此電腦的任何遠端桌面用戶端存取使用權。
  - 遠端工作階段已中斷，因為沒有遠端桌面授權伺服器可用來提供授權。

在此案例中，[設定 RD 授權服務](#configure-the-rd-licensing-service)。

如果 RD 授權 Diagnoser 會列出其他問題，例如 「 RDP 通訊協定元件 X.224 通訊協定資料流中偵測到錯誤及已中斷連線，用戶端 」 那里可能會影響授權憑證有問題。 這類問題傾向於與使用者的訊息，如下列相關聯：

安全性錯誤，因為用戶端無法連線到終端伺服器。 進行確定您已登入網路之後, 再試一次連線到伺服器。

在此情況下，[重新整理 X509 憑證登錄機碼](#refresh-the-x509-certificate-registry-keys)。

### 設定 RD 授權服務

下列程序會使用伺服器管理員來進行設定變更。 如需如何設定及使用伺服器管理員資訊，請參閱[伺服器管理員](https://docs.microsoft.com/en-us/windows-server/administration/server-manager/server-manager)。

1. 開啟伺服器管理員 \]，然後瀏覽**遠端桌面服務**。
2. 上**部署概觀**，選取**工作**]，然後選取 [**編輯部署內容**。
3. 選取**RD 授權**，並選取適當的授權模式為您的部署 （**每一裝置**或**每一使用者**）。
4. 輸入您的 RD 授權伺服器的完整的網域名稱 (FQDN)，然後選取 [**新增]**。
5. 如果您有一個以上的 RD 授權伺服器，請針對每個伺服器重複步驟 4。 
    ![RD 授權伺服器設定選項伺服器管理員中。](..\media\troubleshoot-remote-desktop-connections\RDLicensing_Configure.png)

### 重新整理 X509 憑證登錄機碼

> [!IMPORTANT]  
> 請謹慎依照本節中的步驟。 如果您未正確修改登錄，可能會發生嚴重問題。 之前您修改它[備份還原的登錄，](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22)以便在發生問題。

若要解決這個問題，備份，並移除 X509 憑證登錄機碼，然後重新啟動電腦，然後重新啟動 RD 授權伺服器。 若要這樣做，請依照下列步驟。

> [!NOTE]  
> 每個 RDSH 伺服器上，執行下列程序。

1. 開啟登錄編輯程式，並瀏覽至**HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server\\RCM**。
2. 在 [登錄] 功能表中，選取**匯出登錄檔案**。
3. 在**檔案名稱**] 方塊中輸入**匯出憑證**，然後選取 [**儲存**。
4. 以滑鼠右鍵按一下每個下列值，選取 [**刪除**]，然後選取 **[是]** 來確認刪除：  
      - **憑證**
      - **X509 憑證**
      - **X509 憑證識別碼**
      - **X509 Certificate2**
5. 結束登錄編輯程式，並重新 RDSH 伺服器。

## 使用者無法驗證或必須兩次進行驗證

有可能會造成影響使用者驗證問題的幾個問題。 本節可解決下列情況：

  - [使用者是以 「 受限制的登入類型 」 的訊息的存取被拒](#access-denied-restricted-type-of-logon)
  - [使用者或應用程式拒絕存取與事件 16965，「 遠端呼叫 SAM 資料庫已被拒絕 」](#access-denied-a-remote-call-to-the-sam-database-has-been-denied)
  - [使用者無法使用登入智慧卡](#a-user-cannot-sign-in-by-using-a-smart-card)
  - [如果遠端電腦鎖定時，使用者需要輸入密碼兩次](#if-the-remote-pc-is-locked-a-user-needs-to-enter-a-password-twice)
  - [使用者無法登入，並會收到 「 驗證錯誤 」 和 「 CredSSP 加密 oracle 補救 」 訊息](#user-cannot-sign-in-and-receives-authentication-error-and-credssp-encryption-oracle-remediation-messages)
  - [某些使用者在更新用戶端電腦後需要兩次登入](#after-you-update-client-computers-some-users-need-to-sign-in-twice)。
  - [使用者無法存取上 RemoteGuard 使用多個 RD 連線代理人的部署](#users-are-denied-access-on-a-deployment-that-uses-remote-credential-guard-with-multiple-rd-connection-brokers)

### 存取被拒，受限制的登入類型

在此情況下，嘗試連線到 Windows 10 或 Windows Server 2016 的電腦的 Windows 10 使用者拒絕存取包含以下訊息：

> 遠端桌面連線：  
> 系統管理員已限制您的登入類型 (網路或互動式)，您也可以使用。 如需協助，請連絡您的系統管理員或技術支援。

當網路層級驗證 (NLA) 是必要項目 RDP 連線，且使用者不是**遠端桌面使用者**群組的成員，就會發生此問題。 如果**遠端桌面使用者**群組尚未指派到**此電腦從網路存取**使用者的權限時，它也會發生。

若要解決此問題，請依照這些步驟的其中之一：

  - [修改使用者的群組成員資格或使用者權限指派](#modify-the-users-group-membership-or-user-rights-assignment)。
  - 關閉 NLA （不建議）。
  - 使用 Windows 10 以外的遠端桌面用戶端。 例如，Windows 7 用戶端沒有這個問題。

#### 修改使用者的群組成員資格或使用者權限指派

如果這個問題會影響單一使用者，此問題最簡單的解決方案是將使用者新增至**遠端桌面使用者**群組。

如果使用者已經是此群組的成員 （或多個群組成員如果有相同的問題），，檢查使用者權限設定遠端的 Windows 10 或 Windows Server 2016 電腦上。

1. 開啟群組原則物件編輯器 (GPE)，然後連線至遠端電腦的本機原則。
2. 瀏覽到**電腦 Configuration\\Windows Settings\\Security Settings\\Local Policies\\User 權限指派**，**存取這台電腦從網路**，以滑鼠右鍵按一下，然後選取**屬性**。
3. 檢查清單的使用者及群組**遠端桌面使用者**（或父群組）。
4. 如果清單中不包含**遠端桌面使用者**（或父群組，例如**每個人**） 您需要將它新增到清單 （如果您的部署中有超過一或兩部電腦，使用群組原則物件）。  
    例如，**存取這台電腦與網路**的預設成員資格包含**所有人**。 如果您的部署移除**每個人都**使用群組原則物件，您可能需要更新來新增**遠端桌面使用者**的群組原則物件來還原存取。

### 存取被拒，遭到拒絕遠端呼叫 SAM 資料庫

此行為是最有可能發生，如果您的網域控制站執行 Windows Server 2016 或更新版本，且使用者嘗試使用自訂的連線應用程式連線。 尤其是，存取使用者的設定檔資訊在 Active Directory 中的應用程式將會遭到拒絕存取。

這種行為全責到 Windows 的變更。 在 Windows Server 2012 R2 和較舊版本的版本中，當使用者登入遠端桌面，遠端連線管理員 (RCM) 連絡人的網域控制站 (DC) 查詢特定在 Active Directory 網域中之使用者物件上的遠端桌面的組態服務 (AD DS)。 這項資訊會顯示在使用者的物件屬性中的 Active Directory 使用者和電腦 \] MMC 嵌入式管理單元中的 [遠端桌面服務設定檔] 索引標籤。

從 Windows Server 2016 中，RCM 不再查詢 AD DS 中的使用者物件。 如果您需要 RCM 來查詢 AD DS，因為您使用遠端桌面服務屬性，您必須以手動方式啟用先前的 RCM 行為。

> [!IMPORTANT]  
> 請謹慎依照本節中的步驟。 如果您未正確修改登錄，可能會發生嚴重問題。 之前您修改它[備份還原的登錄，](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22)以便在發生問題。

若要啟用舊版 RCM 行為 RD 工作階段主機伺服器上的，設定下列登錄項目，然後重新啟動的**遠端桌面服務**服務：  
  - **HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Policies\\Microsoft\\Windows NT\\Terminal 服務**
  - **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server\\WinStations\\\<Winstation name\>\\**  
      - 名稱： **fQueryUserConfigFromDC**
      - 類型： **Reg\_DWORD**
      - 值： **1** （十進位）

若要啟用舊版 RCM 行為以外的 RD 工作階段主機伺服器的伺服器上，設定下列登錄項目與下列額外的登錄項目 （，然後重新啟動服務）：
  - **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal 伺服器**

如需有關此行為的詳細資訊，請參閱 KB 3200967[變更至 Windows Server 中遠端連線管理員](https://support.microsoft.com/en-us/help/3200967/changes-to-remote-connection-manager-in-windows-server)。

### 使用者無法使用登入智慧卡

這篇文章說明三個常見的情況下，使用者無法登入遠端桌面使用智慧卡：  
  - [使用者無法登入分公司使用唯讀網域控制站 (RODC)](#cannot-sign-in-with-a-smart-card-in-a-branch-office-with-a-rodc)
  - [使用者無法登入到最近已更新的 Windows Server 2008 SP2 電腦](#cannot-sign-in-with-a-smart-card-to-a-windows-server-2008-sp2-computer)
  - [使用者無法保持登入 Windows 電腦已最近更新，以及遠端桌面服務服務會變成沒有回應 （當機）](#cannot-stay-signed-in-with-a-smart-card-and-remote-desktop-services-service-hangs)

#### 無法使用智慧卡與 RODC 分公司中登入

包含在使用 RODC 分支站台 RDSH 伺服器的部署中發生此問題。 RDSH 伺服器被裝載於根網域。 使用者在分支站台屬於子網域，並使用智慧卡進行驗證。 RODC 已設定為快取使用者密碼 （RODC 所屬**允許 RODC 密碼複寫群組**）。 當使用者嘗試登入 RDSH 伺服器上的工作階段時，它們接收訊息例如 「 登入嘗試是無效。 這可能是因為不正確的使用者名稱或驗證資訊。 」

這是已知的問題的方式，根 DC 和 RODC 管理加密的使用者認證。 根 DC 使用的加密金鑰來加密認證，並 RODC 提供給用戶端的解密金鑰。 不過，兩個金鑰不相符。

若要解決此問題，請考慮變更您的 DC 拓撲關閉 RODC 的快取密碼，或可寫入的 DC 部署至分支的網站。 替代方法是將 RDSH 伺服器移至相同的子網域使用者。 另一個替代方法是讓使用者不需要智慧卡登入。 這些解決方案的任何可能需要折衷效能或的安全性層級中。

#### 無法使用智慧卡與 Windows Server 2008 SP2 電腦登入

當使用者登入已更新為使用 KB4093227 的 Windows Server 2008 SP2 電腦時，會發生此問題 (2018.4B)。 當使用者嘗試使用智慧卡登入時，他們會拒絕存取訊息，例如 「 找不到有效的憑證。 檢查已正確插入智慧卡，而且符合緊密。 」 在此同時，Windows Server 電腦應用程式事件記錄 」 從插入的智慧卡擷取數位憑證時發生錯誤。 無效簽章。 」

若要解決此問題，請使用 2018.06Bre 更新的 Windows Server 電腦-發行版本的 KB 4093227[描述 Windows 遠端桌面通訊協定 (RDP) 拒絕服務弱點，在 Windows Server 2008 的安全性更新： 2018 年 4 月 10 日](https://support.microsoft.com/en-us/help/4093227/security-update-for-vulnerabilities-in-windows-server-2008)。

#### 不能保持已簽署使用智慧卡和遠端桌面服務服務停止回應

當使用者登入 Windows 或 Windows Server 的電腦與 KB 4056446 已更新，就會發生此問題。 首先，使用者就能夠使用智慧卡，登入系統，但然後會收到 「 SCARD\_E\_NO\_SERVICE 」 錯誤訊息。 遠端電腦可能會變成沒有回應。

若要解決此問題，重新啟動的遠端電腦。

若要解決此問題，請使用適當的修正程式更新的遠端電腦：

  - Windows Server 2008 SP2: KB 4090928， [Windows 會洩漏 lsm.exe 處理程序中的控制代碼與智慧卡應用程式可能會顯示 「 SCARD\_E\_NO\_SERVICE 」 錯誤](https://support.microsoft.com/en-us/help/4090928/scard-e-no-service-errors-when-windows-leaks-handles-in-the-lsm-exe)
  - Windows Server 2012 R2: KB 4103724， [2018 年月 17 日 — KB4103724 （每月彙總套件的預覽）](https://support.microsoft.com/en-us/help/4103724/windows-81-update-kb4103724)
  - Windows Server 2016 和 Windows 10，版本 1607年: KB 4103720 [2018 年月 17 日 — KB4103720 (作業系統組建 14393.2273)](https://support.microsoft.com/en-us/help/4103720/windows-10-update-kb4103720)

### 如果遠端電腦鎖定時，使用者需要輸入密碼兩次

當使用者嘗試連線到執行 Windows 10 版本 1709年在 RDP 連線並需要 NLA 部署遠端桌面時，可能會發生此問題。 這些條件下，如果遠端桌面已鎖定，使用者必須輸入其認證連線時，兩次。

若要解決此問題，使用 KB 4343893 更新 Windows 10 版本 1709年的電腦[2018 年 8 月 30 日 — KB4343893 (作業系統組建 16299.637)](https://support.microsoft.com/en-us/help/4343893/windows-10-update-kb4343893)。

### 使用者無法登入，並會收到 「 驗證錯誤 」 和 「 CredSSP 加密 oracle 補救 」 訊息

當使用者嘗試使用任何版本的 Windows 從 Windows Vista SP2 及更新版本或 Windows Server 2008 SP2 和更新版本登入時，他們會拒絕存取和接收訊息，例如下列：

  - 驗證錯誤。 不支援要求的功能。
  - 這可能是因為 CredSSP 加密 oracle 補救措施

「 CredSSP 加密 oracle 補救 」 指的是一組發行於 3、 4 月及 2018 年的安全性更新。 CredSSP 是處理其他應用程式的驗證要求驗證提供者。 3 月 13 2018 年日，「 3b\ 」 和後續的更新解決在其中攻擊者轉接的使用者認證來目標系統上執行的程式碼入侵。

初始更新新增對新的群組原則物件，**加密 Oracle 補救**，有可能的設定下列支援：

  - **很容易受到鍵盤**。 使用 CredSSP 的用戶端應用程式可以改為不安全的版本中，但這種行為會公開遠端桌面的攻擊。 使用 CredSSP 服務接受尚未更新的用戶端。
  - **降低**。 使用 CredSSP 的用戶端應用程式無法回復為不安全的版本中，但使用 CredSSP 服務接受尚未更新的用戶端。
  - **強制更新用戶端**。 使用 CredSSP 的用戶端應用程式無法回復為不安全的版本中，並使用 CredSSP 的服務不會接受未修補的用戶端。 
    注意： 此設定不應該部署直到所有的遠端主機支援的最新版本。

2018 年 8，更新會從**Vulnerable**預設**加密 Oracle 補救**設定變更為**降低**。 透過這項變更的地方，已更新的遠端桌面用戶端無法連線到伺服器，不需要它們 （或更新的伺服器，尚未重新啟動）。 如需有關效果的更新和它們封鎖的通訊的類型，請參閱 KB 4093492， [CVE-2017-2018年-0886 CredSSP 更新](https://support.microsoft.com/en-us/help/4093492/credssp-updates-for-cve-2018-0886-march-13-2018)。

若要解決此問題，請確定所有系統是完全更新並重新啟動。 更新及弱點的詳細資訊的完整清單，請參閱[CVE-2017-2018年-0886年 |CredSSP 的遠端執行程式碼弱點](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-0886)。

若要解決此問題，更新完成前，檢查允許連線類型的 KB 4093492。 如果有任何可行的替代方案，您可能會考慮下列方法的其中一個：

  - 受影響的用戶端電腦，請回到**Vulnerable**設定**加密 Oracle 補救**原則。
  - 修改下列原則 [**電腦設定 \\ 系統管理範本 \\windows 元件 Components\\Remote 桌面 Services\\Remote 桌面工作階段 Host\\Security**群組原則] 資料夾中：  
      - **需要使用特定的安全性層級的遠端 (RDP) 連線**： 設為**Enabled** ，然後選取**RDP**。
      - **需要使用網路層級驗證的遠端連線的使用者驗證**： 設為**已停用**。
      > [!IMPORTANT]  
      > 這些修改減少部署的安全性。 它們只應該是暫時性的如果完全使用。

如需有關使用 「 群組原則的詳細資訊，請參閱[修改封鎖的 GPO](#modifying-a-blocking-gpo)。

### 更新用戶端電腦之後，某些使用者需要兩次登入

某些 Windows 7 或 Windows 10 版本 1709年上的使用者登入遠端桌面工作階段，並立即看到第二個登入畫面。 如果用戶端電腦已收到的下列更新，就會發生此問題：

  - Windows 7: KB 4103718， [8 2018 日 — KB4103718 （每月彙總套件）](https://support.microsoft.com/en-us/help/4103718/windows-7-update-kb4103718)
  - Windows 10 1709年: KB 4103727， [8 2018 日 — KB4103727 （作業系統組建 16299.431）](https://support.microsoft.com/en-us/help/4103727/windows-10-update-kb4103727)

若要解決此問題，請確定使用者想要連線到 （以及 RDSH 或 RDVI 伺服器） 的電腦已完整更新透過 2018 年 6 月日。 這包括下列更新：

  - Windows Server 2016: KB 4284880， [2018 年 6 月 12 日 — KB4284880 （作業系統組建 14393.2312）](https://support.microsoft.com/en-us/help/4284880/windows-10-update-kb4284880)
  - Windows Server 2012 R2: KB 4284815， [2018 年 6 月 12 日 — KB4284815 （每月彙總套件）](https://support.microsoft.com/en-us/help/4284815/windows-81-update-kb4284815)
  - Windows Server 2012: KB 4284855， [2018 年 6 月 12 日 — KB4284855 （每月彙總套件）](https://support.microsoft.com/en-us/help/4284855/windows-server-2012-update-kb4284855)
  - Windows Server 2008 R2: KB 4284826， [2018 年 6 月 12 日 — KB4284826 （每月彙總套件）](https://support.microsoft.com/en-us/help/4284826/windows-7-update-kb4284826)
  - Windows Server 2008 SP2: KB4056564[描述在 Windows Server 2008、 Windows Embedded POSReady 2009 和 Windows Embedded 標準 2009 CredSSP 遠端程式碼執行的弱點的安全性更新： 2018 年 3 月 13 日](https://support.microsoft.com/en-us/help/4056564/security-update-for-vulnerabilities-in-windows-server-2008)

### 使用者無法存取上部署使用 Remote Credential Guard 搭配多個 RD 連線代理人

如果 Windows Defender Remote Credential Guard 正在使用中，可在高可用性部署中使用兩個或多個遠端桌面連線代理人，發生此問題。 遠端桌面，使用者無法登入。

因為 Remote Credential Guard 進行驗證，會使用 Kerberos 和 NTLM 會限制，就會發生此問題。 不過，在負載平衡與高可用性設定中，RD 連線代理人不支援 Kerberos 作業。

如果您需要以搭配負載平衡 RD 連線代理人高可用性設定，您可以透過停用 Remote Credential Guard 解決此問題。 如需有關如何管理 Windows Defender Remote Credential Guard 的詳細資訊，請參閱[使用 Windows Defender Remote Credential Guard 保護遠端桌面認證](https://docs.microsoft.com/en-us/windows/security/identity-protection/remote-credential-guard#enable-windows-defender-remote-credential-guard)。

## 在連線時，使用者會收到 「 遠端桌面服務是目前忙碌 」 的訊息

若要判斷此問題的適當回應，使用下列步驟：

1. 沒有遠端桌面服務服務變成沒有回應 （例如，遠端桌面用戶端會顯示 「 停止回應 」 在歡迎畫面）。  
      - 如果服務沒有回應，請參閱[RDSH 伺服器的記憶體問題](#rdsh-server-memory-issue)。
      - 如果用戶端顯示一般互動與服務，請繼續下一個步驟。
2. 如果一或多個使用者中斷連線的遠端桌面工作階段，便可以使用者連線一次？  
   - 如果服務會繼續拒絕無論有多少使用者中斷連接他們的工作階段連線，請參閱[RD 接聽程式問題](#rd-listener-issue)。
   - 如果服務開始接受連線的使用者數之後再次已中斷他們的工作階段，[檢查連線的限制原則](#check-the-connection-limit-policy)。

### RDSH 伺服器的記憶體問題

某些 Windows Server 2012 R2 RDSH 伺服器上找到記憶體流失。 經過一段時間，這些伺服器開始拒絕遠端桌面連線和本機主控台登入以下列訊息：

> 因為遠端桌面服務目前忙碌，無法完成您嘗試執行的工作。 請再試一次在幾分鐘的時間。 其他使用者應該仍無法登入。

遠端桌面用戶端嘗試連線也會變成沒有回應。

若要解決此問題，重新啟動 RDSH 伺服器。

若要解決此問題，套用 KB 4093114 [2018 年 4 月 10 日-KB4093114 （每月彙總套件）] (file:///C:\\Users\\v-jesits\\AppData\\Local\\Microsoft\\Windows\\INetCache\\Content.Outlook\\FUB8OO45\\April%2010,%202018—KB4093114%20\(Monthly%20Rollup\))，到 RDSH 伺服器。

### RD 接聽程式問題

已升級至 Windows Server 2012 R2 的 Windows Server 2008 R2 直接從的一些 RDSH 伺服器或 Windows Server 2016 上曾經發生問題。 當遠端桌面用戶端連線到 RDSH 伺服器時，RDSH 伺服器會為使用者工作階段建立 RD 接聽程式。 受影響的伺服器保持計數會隨著使用者連線時，增加的 RD 接聽程式，但永遠不會降低。

若要解決此問題，您可以使用下列方法：

  - 重新啟動 RDSH 伺服器以重設的 RD 接聽程式計數
  - 修改連線的限制原則，請將它設定為非常大型的值。 如需有關如何管理連線的限制原則的詳細資訊，請參閱[檢查連線的限制原則](#check-the-connection-limit-policy)。

若要解決此問題，請將下列更新套用至 RDSH 伺服器：

  - Windows Server 2012 R2: KB 4343891， [2018 年 8 月 30 日 — KB4343891 （每月彙總套件的預覽）](https://support.microsoft.com/en-us/help/4343891/windows-81-update-kb4343891)
  - Windows Server 2016: KB 4343884， [2018 年 8 月 30 日 — KB4343884 （作業系統組建 14393.2457）](https://support.microsoft.com/en-us/help/4343884/windows-10-update-kb4343884)

### 檢查連線的限制原則

在個別電腦層級，或藉由設定群組原則物件 (GPO)，您可以設定限制同時遠端桌面連線的數目。 根據預設，不會設定的限制。

若要檢查目前的設定，並找出 RDSH 伺服器上任何現有的 Gpo，開啟 \ [以系統管理員命令提示字元視窗並輸入下列命令：
  
```
gpresult /H c:\gpresult.html
```
   
此命令完成之後，開啟 gpresult.html，然後在**電腦設定 \\ 系統管理範本 \\windows 元件 Components\\Remote 桌面 Services\\Remote 桌面工作階段 Host\\Connections**，找出**連線數目限制**原則。

  - 如果**已停用**這個原則設定，群組原則不會限制 RDP 連線。
  - 如果**啟用**這個原則設定，然後檢查**贏利的 GPO**。 如果您需要移除或變更連線的限制，編輯此 GPO。

若要強制執行原則變更，開啟受影響的電腦上的命令提示字元視窗並輸入下列命令：
  
```
gpupdate /force
```
  
## RD 用戶端中斷連線，並無法重新連接到相同的工作階段

遠端桌面用戶端失去的遠端桌面連線之後，用戶端無法立即重新連線。 使用者會收到下列錯誤訊息：

  - 安全性錯誤，因為用戶端無法連線到終端伺服器。 進行確定您已登入網路之後, 再試一次連線到伺服器。
  - 遠端桌面連線中斷。 安全性錯誤，因為用戶端無法連線至遠端電腦。 確認您已登入網路，並再試一次。

在稍後，遠端桌面用戶端重新連線至遠端桌面。 不過，而不是原始的工作階段連接用戶端，RDSH 伺服器會連線到新的工作階段用戶端。 檢查 RDSH 伺服器揭露的原始的工作階段並未輸入中斷連線的狀態，但改為都會維持有效。

若要解決此問題，您可以啟用**設定保持連線間隔**原則在電腦設定 \\ 系統管理範本 \\windows 元件 Components\\Remote 桌面 Services\\Remote 桌面工作階段 Host\\Connections **** 群組原則資料夾。 如果您啟用這項原則，您必須輸入持續連線的間隔。 持續連線的間隔決定頻率，以分鐘為單位，伺服器會檢查工作階段狀態。

此問題可能因您驗證及組態設定的設定錯誤。 在伺服器層級，或使用 Gpo，您可以設定這些設定。 群組原則設定可在**電腦設定 \\ 系統管理範本 \\windows 元件 Components\\Remote 桌面 Services\\Remote 桌面工作階段 Host\\Security**群組原則] 資料夾。

1. 在 RD 工作階段主機伺服器上，開啟 [遠端桌面工作階段主機設定。
2. 在**連線**，以滑鼠右鍵按一下 [連線的名稱，然後按一下 \ [**內容**。
3. 適用於連線，在**一般**] 索引標籤**的安全性**層級，[**屬性**] 對話方塊中選取的安全性方法。
4. 在**加密層級**中，按一下您想要的層級。 您可以選取**低**、**用戶端相容**、**高**，或**FIPS 相容**。

> [!NOTE]  
>  - 當用戶端和 RD 工作階段主機伺服器之間的通訊需要最高層級的加密時，使用 FIPS 相容的加密。
>  - 您在群組原則中設定任何加密層級設定覆寫您使用遠端桌面服務設定工具設定的設定。 此外，如果您啟用[系統加密編譯： 使用 FIPS 相容演算法於加密、 雜湊及簽章](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2003/cc780081\(v=ws.10\))原則，這項設定會覆寫**設定用戶端連線加密層級**的原則。 系統密碼編譯原則是在**電腦 Configuration\\Windows Settings\\Security Settings\\Local Policies\\Security 選項**資料夾中。
>  - 當您變更加密層級時，將新的加密層級會生效的下次使用者登入。 如果您需要的加密一部伺服器上的多個層級，安裝多個網路介面卡，並分別設定每個介面卡。
>  - 若要確認該憑證有對應的私密金鑰，在遠端桌面服務設定中，以滑鼠右鍵按一下您要檢視的憑證，選取 [**一般**]、 選取 [**編輯**]，選取您想要的憑證的連線檢視，然後再選取 [**檢視憑證**。 在 [**一般**] 索引標籤下方，陳述式中，「 您有一個對應到此憑證的私密金鑰 」 應該會出現。 您也可以使用 \ [憑證嵌入式管理單元中檢視這項資訊。
>  - FIPS 相容加密 (**系統加密編譯： 使用 FIPS 相容演算法於加密、 雜湊及簽章**原則或遠端桌面的伺服器設定的 [ **FIPS 相容**設定) 會加密和解密資料從傳送用戶端到伺服器，並從伺服器以使用的用戶端，聯邦資訊處理標準 (FIPS) 140-1 加密演算法，與 Microsoft 的密碼編譯模組。 如需詳細資訊，請參閱[FIPS 140 驗證](https://docs.microsoft.com/en-us/windows/security/threat-protection/fips-140-validation)。
>  - **高**設定會加密傳送從伺服器用戶端和伺服器用戶端以使用強式 128 位元加密的資料。
>  - **用戶端相容**的設定會加密在用戶端和伺服器的最大金鑰強度用戶端支援之間傳送的資料。
>  - **低**] 設定會加密從用戶端傳送至伺服器使用 56 位元加密的資料。

## 從無線網路中斷連線的遠端膝上型電腦

遠端桌面用戶端將連接到膝上型電腦使用 802.1 x 無線網路時，可能會發生此問題。 間歇性，膝上型電腦從無線網路中斷，並不會自動重新連線。

這是已知的問題發生時的無線網路連線的網路驗證設定為**使用者驗證**。

若要解決此問題，請將網路驗證設定為**使用者或電腦驗證**或**電腦的驗證**。

 > [!NOTE]  
> 若要變更單一電腦上的網路驗證設定，您可能需要使用 [網路和共用中心] 控制台中建立新的無線連線使用新的設定。

如需如何設定使用 Gpo 的無線網路設定的完整描述，請參閱[設定無線網路 (IEEE 802.11) 原則](https://docs.microsoft.com/en-us/windows-server/networking/core-network-guide/cncg/wireless/e-wireless-access-deployment#bkmk_policies)。

## 使用者體驗不佳的效能或應用程式的問題

本節可解決他們使用遠端桌面的功能時，使用者可能會遇到的數個常見問題：

  - [連線到虛擬機器間歇性 Microsoft Azure 的使用者遇到的問題](#intermittent-problems-with-new-microsoft-azure-virtual-machines)。
  - [至 Windows 10 版本 1709年遠端桌面連線的使用者可能會有播放視訊的問題](#video-playback-issues-on-windows-10-version-1709)。
  - [具有唯讀的使用者設定檔至 Windows 10 的遠端桌面連線的使用者無法共用他們的桌面。](#desktop-sharing-issues-on-windows-10)
  - [Windows 10 使用舊版的 Windows 10 執行的用戶端的遠端桌面連線的使用者體驗不佳的效能，NLA 停用時](#performance-issues-when-mixing-versions-of-windows-10-if-nla-is-disabled)
  - [當使用者在遠端桌面工作階段中執行多個應用程式和中斷連線，以及經常重新連線時，它們可能會遇到一個黑色畫面上重新連線。](#black-screen-issue)

### 使用新的 Microsoft Azure 虛擬機器斷斷續續的問題

這個問題會影響已經最近佈建的虛擬機器。 使用者連線到虛擬機器之後，遠端桌面工作階段可能不會載入所有使用者的設定正確。

若要解決此問題，從虛擬機器中斷連接，請至少 20 分鐘，稍後再重新連線。

若要解決此問題，將套用到虛擬機器，做為適當的下列更新：

  - Windows 10 和 Windows Server 2016: 4343884，KB [2018 年 8 月 30 日 — KB4343884 （作業系統組建 14393.2457）](https://support.microsoft.com/en-us/help/4343884/windows-10-update-kb4343884)
  - Windows Server 2012 R2: KB 4343891， [2018 年 8 月 30 日 — KB4343891 （每月彙總套件的預覽）](https://support.microsoft.com/en-us/help/4343891/windows-81-update-kb4343891)

### 在 Windows 10 版本 1709年的視訊播放問題

當使用者連線至遠端電腦執行 Windows 10，版本 1709年時，就會發生此問題。 當這些使用者播放視訊使用 VMR9 (視訊 MixingRenderer9) 的轉碼器，玩家會顯示黑色視窗。

這是 Windows 10，版本 1709年中的已知的問題。 Windows 10 版本 1703年中不會發生問題。

### 桌面共用 Windows 10 上的問題

當使用者具有唯讀的使用者設定檔中 （和相關聯的登錄區），例如 kiosk 案例中，就會發生此問題。 當這類使用者連接到遠端電腦執行 Windows 10，版本 1803 起，它們無法共用他們的桌面。

若要修正此問題，適用於 Windows 10 更新 4340917， [2018 年 7 月 24 日 — KB4340917 (作業系統組建 17134.191)](https://support.microsoft.com/en-us/help/4340917/windows-10-update-kb4340917)。

### 如果已停用 NLA 混合版本的 Windows 10 時的效能問題

NLA 已停用，並執行 Windows 10 的遠端桌面用戶端電腦連線到執行不同版本的 Windows 10 的遠端桌面時，就會發生此問題。 連接到執行 Windows 10，版本 1803年或更新版本的遠端桌面時，在執行 Windows 10，版本 1709年或較舊版本的電腦上的遠端桌面用戶端的使用者體驗效能不佳。

這是因為 NLA 停用時，較舊的用戶端電腦連線至 Windows 10 版本 1803年或更新版本時使用較慢的通訊協定。

若要解決此問題，套用 KB 4340917 [2018 年 7 月 24 日 — KB4340917 (作業系統組建 17134.191)](https://support.microsoft.com/en-us/help/4340917/windows-10-update-kb4340917)。

### 黑色畫面問題

Windows 8.0、 Windows 8.1、 Windows 10 RTM 和 Windows Server 2012 R2 中，就會發生此問題。 使用者啟動遠端桌面，在多個應用程式，然後從工作階段中斷。 定期，使用者重新連線至遠端桌面應用程式，互動，並再次中斷連線。 有些時候，當使用者重新連線，遠端桌面工作階段就只會顯示一個黑色畫面。 使用者必須結束遠端桌面工作階段，從遠端電腦的主控台 （或 RDSH 伺服器主控台）。 這個動作會停止應用程式。

若要解決此問題，套用適當的下列更新：

  - Windows 8 和 Windows Server 2012: KB4103719， [2018 年月 17 日 — KB4103719 （每月彙總套件的預覽）](https://support.microsoft.com/en-us/help/4103719/windows-server-2012-update-kb4103719)
  - Windows 8.1 和 Windows Server 2012 R2: KB4103724， [2018 年月 17 日 — KB4103724 （每月彙總套件的預覽）](https://support.microsoft.com/en-us/help/4103724/windows-81-update-kb4103724) KB 4284863，以及[2018 年 6 月 21 日 — KB4284863 （每月彙總套件的預覽）](https://support.microsoft.com/en-us/help/4284863/windows-81-update-kb4284863)
  - Windows 10： 固定 KB4284860，在[2018 年 6 月 12 日 — KB4284860 （作業系統組建 10240.17889）](https://support.microsoft.com/en-us/help/4284860/windows-10-update-kb4284860)
