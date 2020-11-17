---
title: 針對一般遠端桌面連線進行疑難排解
description: 針對遠端桌面連線的「類別未登錄」錯誤進行疑難排解。
ms.reviewer: rklemen
ms.topic: troubleshooting
author: kaushika-msft
manager: dcscontentpm
ms.author: delhan
ms.date: 07/24/2019
ms.localizationpriority: medium
ms.openlocfilehash: bfed6f0c22a4949291d12cc417a15d0aa435ae00
ms.sourcegitcommit: b39ea3b83280f00e5bb100df0dc8beaf1fb55be2
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/11/2020
ms.locfileid: "94520511"
---
# <a name="general-remote-desktop-connection-troubleshooting"></a>針對一般遠端桌面連線進行疑難排解

當遠端桌面用戶端無法連線到遠端桌面，但未提供可協助找出原因的訊息或其他徵兆，請使用下列步驟。

## <a name="check-the-status-of-the-rdp-protocol"></a>檢查 RDP 通訊協定的狀態

### <a name="check-the-status-of-the-rdp-protocol-on-a-local-computer"></a>檢查本機電腦上 RDP 通訊協定的狀態

若要檢查並變更本機電腦上 RDP 通訊協定的狀態，請參閱[如何啟用遠端桌面](../clients/remote-desktop-allow-access.md#how-to-enable-remote-desktop)。

> [!NOTE]  
> 若無法使用遠端桌面選項，請參閱[檢查群組原則物件是否封鎖 RDP](#check-whether-a-group-policy-object-gpo-is-blocking-rdp-on-a-local-computer)。

### <a name="check-the-status-of-the-rdp-protocol-on-a-remote-computer"></a>檢查遠端電腦上 RDP 通訊協定的狀態

> [!IMPORTANT]  
> 請仔細遵循本節的指示。 如果您未正確修改登錄，就會發生嚴重問題。 在開始修改登錄之前，請先 [備份登錄](https://support.microsoft.com/help/322756)，以便在發生問題時進行還原。

若要檢查並變更遠端電腦上 RDP 通訊協定的狀態，請使用網路登錄連線：

1. 首先，移至 [開始]  功能表，然後選取 [執行]  。 在出現的文字方塊中輸入 **regedt32**。
2. 在登錄編輯程式中，選取 [檔案]  ，然後選取 [連線網路登錄]  。
3. 在 [選取電腦]  對話方塊中，輸入遠端電腦的名稱，選取 [檢查名稱]  ，然後選取 [確定]  。
4. 巡覽至 **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server**。  
   ![登錄編輯程式，顯示 fDenyTSConnections 項目](../media/troubleshoot-remote-desktop-connections/RegEntry_fDenyTSConnections.png)
   - 如果 **fDenyTSConnections** 金鑰值為 **0**，則會啟用 RDP。
   - 如果 **fDenyTSConnections** 金鑰值為 **1**，則會停用 RDP。
5. 若要啟用 RDP，請將 **fDenyTSConnections** 的值從 **1** 變更為 **0**。

### <a name="check-whether-a-group-policy-object-gpo-is-blocking-rdp-on-a-local-computer"></a>檢查群組原則物件 (GPO) 是否會封鎖本機電腦上的 RDP

如果您無法開啟使用者介面中的 RDP，或 **fDenyTSConnections** 的值在您變更後還原成 **1**，則 GPO 可能覆寫電腦層級設定。

若要檢查本機電腦上的群組原則設定，請以系統管理員身分開啟命令提示字元視窗，並輸入下列命令：

```cmd
gpresult /H c:\gpresult.html
```

此命令完成後，開啟 gpresult.html。 在 **電腦設定\\系統管理範本\\Windows 元件\\遠端桌面服務\\遠端桌面工作階段主機\\連線** 中，尋找 **允許使用者使用遠端桌面服務從遠端連線** 原則。

- 如果此原則的設定為 **已啟用**，則群組原則不會封鎖 RDP 連線。
- 如果此原則的設定為 **已停用**，請檢查 **優勢 GPO**。 這是封鎖 RDP 連線的 GPO。
  ![Gpresult.html 的範例區段，在其中網域層級 GPO **封鎖 RDP** 會停用 RDP。](../media/troubleshoot-remote-desktop-connections/GPResult_RDSH_Connections_GP.png)
   
  ![Gpresult.html 的範例區段，在其中 **本機群組原則** 會停用 RDP。](../media/troubleshoot-remote-desktop-connections/GPResult_RDSH_Connections_LGP.png)

### <a name="check-whether-a-gpo-is-blocking-rdp-on-a-remote-computer"></a>檢查 GPO 是否會封鎖遠端電腦上的 RDP

若要檢查遠端電腦上的群組原則設定，命令與用於本機電腦幾乎一樣：

```cmd
gpresult /S <computer name> /H c:\gpresult-<computer name>.html
```

此命令產生的檔案 (**gpresult-\<computer name\>.html**) 與本機電腦版本 (**gpresult.html**) 會使用相同的資訊格式。

### <a name="modifying-a-blocking-gpo"></a>修改封鎖 GPO

您可以在群組原則物件編輯器 (GPE) 和群組原則管理主控台 (GPM) 中修改這些設定。 如需如何使用群組原則的詳細資訊，請參閱[進階群組原則管理](/microsoft-desktop-optimization-pack/agpm/)。

若要修改封鎖原則，請使用下列其中一種方法：

- 在 GPE 中，存取適當的 GPO 層級 (例如本機或網域)，並巡覽至 **電腦設定** > **系統管理範本** > **Windows 元件** > **遠端桌面服務** > **遠端桌面工作階段主機** > **連線** > **允許使用者使用遠端桌面服務從遠端連線**。  
   1. 將原則設為 **已啟用** 或 **未設定**。
   2. 在受影響的電腦上，以系統管理員身分開啟命令提示字元視窗，並執行 **gpupdate /force** 命令。
- 在 GPM 中巡覽到組織單位 (OU)，其中的封鎖原則已套用至受影響的電腦，從 OU 中刪除該原則。

## <a name="check-the-status-of-the-rdp-services"></a>檢查 RDP 服務的狀態

在本機 (用戶端) 電腦和遠端 (目標) 電腦上，應執行下列服務：

- 遠端桌面服務 (TermService)
- 遠端桌面服務使用者模式連接埠重新導向器 (UmRdpService)

您可以使用 [服務] MMC 嵌入式管理單元，在本機或遠端管理服務。 您也可以在本機或遠端使用 PowerShell 來管理服務 (如果遠端電腦設定為可接受遠端的 PowerShell Cmdlet)。

![[服務] MMC 嵌入式管理單元中的遠端桌面服務。 請勿修改預設的服務設定。](../media/troubleshoot-remote-desktop-connections/RDSServiceStatus.png)

在其中一台電腦上，如果其中一個或兩個服務皆未執行，請將其啟動。

> [!NOTE]  
> 如果您啟動「遠端桌面服務」服務，按一下 [是]  以自動重新啟動「遠端桌面服務使用者模式連接埠重新導向器」服務。

## <a name="check-that-the-rdp-listener-is-functioning"></a>檢查 RDP 接聽程式是否正常運作

> [!IMPORTANT]  
> 請仔細遵循本節的指示。 如果您未正確修改登錄，就會發生嚴重問題。 在開始修改登錄之前，請先[備份登錄](https://support.microsoft.com/help/322756)，以便在發生問題時進行還原。

### <a name="check-the-status-of-the-rdp-listener"></a>檢查 RDP 接聽程式的狀態

針對此程序，使用具有系統管理權限的 PowerShell 執行個體。 針對本機電腦，您也可以使用具有系統管理權限的命令提示字元。 不過，此程序使用 PowerShell，因為相同的 Cmdlet 在本機和遠端皆可運作。

1. 若要連線到遠端電腦，請執行下列 Cmdlet：

   ```powershell
   Enter-PSSession -ComputerName <computer name>
   ```

2. 輸入 **qwinsta**。 
    ![qwinsta 命令會列出電腦連接埠上接聽的處理程序。](../media/troubleshoot-remote-desktop-connections/WPS_qwinsta.png)
3. 如果清單包含 **rdp-tcp** 且狀態為 **接聽**，則 RDP 接聽程式運作正常。 繼續[檢查 RDP 接聽程式連接埠](#check-the-rdp-listener-port)。 否則，請繼續執行步驟 4。
4. 從工作電腦匯出 RDP 接聽程式設定。
    1. 登入與受影響電腦具有相同作業系統版本的電腦，並存取該電腦的登錄 (例如，藉由使用登錄編輯程式)。
    2. 巡覽至下列登錄項目：  
        **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server\\WinStations\\RDP-Tcp**
    3. 將項目匯出為 .reg 檔案。 例如，在登錄編輯程式中，以滑鼠右鍵按一下項目，選取 [匯出]  ，然後輸入匯出設定的檔案名稱。
    4. 將匯出的 .reg 檔案複製到受影響的電腦。
5. 若要匯入 RDP 接聽程式設定，請在受影響的電腦上開啟具有系統管理權限的 PowerShell 視窗 (或開啟 PowerShell 視窗並從遠端連線到受影響的電腦)。
   1. 若要備份現有的登錄項目，請輸入下列 Cmdlet：  
   
      ```powershell  
      cmd /c 'reg export "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-tcp" C:\Rdp-tcp-backup.reg'   
      ```  
   

   2. 若要移除現有的登錄項目，請輸入下列 Cmdlet：  
   
      ```powershell  
      Remove-Item -path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-tcp' -Recurse -Force  
      ```
   
   3. 若要匯入新的登錄項目，並重新啟動服務，請輸入下列 Cmdlet：  
   
      ```powershell  
      cmd /c 'regedit /s c:\<filename>.reg'  
      Restart-Service TermService -Force  
      ```
      
      將 \<filename\> 取代為匯出的 .reg 檔案名稱。

6. 再次嘗試遠端桌面連線以測試設定。 如果您仍然無法連線，請重新啟動受影響的電腦。
7. 如果您仍然無法連線，請[檢查 RDP 自我簽署憑證的狀態](#check-the-status-of-the-rdp-self-signed-certificate)。

### <a name="check-the-status-of-the-rdp-self-signed-certificate"></a>檢查 RDP 自我簽署憑證的狀態

1. 如果您仍然無法連線，請開啟 [憑證] MMC 嵌入式管理單元。 當系統提示您選取要管理的憑證存放區，請選取 **電腦帳戶**，然後選取受影響的電腦。
2. 在 [遠端桌面]  下方的 [憑證]  資料夾中，刪除 RDP 自我簽署憑證。 
    ![MMC 憑證嵌入式管理單元中的遠端桌面憑證。](../media/troubleshoot-remote-desktop-connections/MMCCert_Delete.png)
3. 在受影響的電腦上，重新啟動「遠端桌面服務」服務。
4. 重新整理 [憑證] 嵌入式管理單元。
5. 如果 RDP 自我簽署憑證尚未重建，請[檢查 MachineKeys 資料夾的權限](#check-the-permissions-of-the-machinekeys-folder)。

### <a name="check-the-permissions-of-the-machinekeys-folder"></a>檢查 MachineKeys 資料夾的權限

1. 在受影響的電腦上開啟 Explorer，然後巡覽至 **C:\\ProgramData\\Microsoft\\Crypto\\RSA\\** 。
2. 以滑鼠右鍵按一下 **MachineKeys**，選取 [屬性]  ，選取 [安全性]  ，然後選取 [進階]  。
3. 確認已設定下列權限：
      - 內建\\系統管理員：完全控制
      - 所有人：讀取、寫入

## <a name="check-the-rdp-listener-port"></a>檢查 RDP 接聽程式連接埠

在本機 (用戶端) 電腦和遠端 (目標) 電腦上，RDP 接聽程式應在連接埠 3389 上進行接聽。 其他應用程式不應使用此連接埠。

> [!IMPORTANT]  
> 請仔細遵循本節的指示。 如果您未正確修改登錄，就會發生嚴重問題。 在開始修改登錄之前，請先[備份登錄](https://support.microsoft.com/help/322756)，以便在發生問題時進行還原。

若要檢查或變更 RDP 連接埠，請使用登錄編輯程式：

1. 移至 [開始] 功能表並選取 [執行]  ，然後在出現的文字方塊中輸入 **regedt32**。
      - 若要連線至遠端電腦，選取 [檔案]  ，然後選取 [連線網路登錄]  。
      - 在 [選取電腦]  對話方塊中，輸入遠端電腦的名稱，選取 [檢查名稱]  ，然後選取 [確定]  。
2. 開啟登錄並巡覽至 **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server\\WinStations\\\<listener\>** 。 
    ![RDP 通訊協定的 PortNumber 子機碼。](../media/troubleshoot-remote-desktop-connections/RegEntry_PortNumber.png)
3. 如果 **PortNumber** 的值不是 **3389**，將其變更為 **3389**。 
   > [!IMPORTANT]  
    > 您可以使用其他連接埠來操作遠端桌面服務。 不過，我們不建議您這麼做。 本文並未說明如何對該類型的設定進行疑難排解。
4. 變更連接埠號碼之後，重新啟動「遠端桌面服務」服務。

### <a name="check-that-another-application-isnt-trying-to-use-the-same-port"></a>確認其他應用程式並未嘗試使用同一個連接埠

針對此程序，使用具有系統管理權限的 PowerShell 執行個體。 針對本機電腦，您也可以使用具有系統管理權限的命令提示字元。 不過，此程序使用 PowerShell，因為相同的 Cmdlet 在本機和遠端皆可運作。

1. 開啟 PowerShell 視窗。 若要連線到遠端電腦，請輸入 **Enter-PSSession -ComputerName \<computer name\>** 。
2. 輸入下列命令：  
   
     ```powershell  
    cmd /c 'netstat -ano | find "3389"'  
    ```
  
    ![Netstat 命令會產生連接埠和接聽服務的清單。](../media/troubleshoot-remote-desktop-connections/WPS_netstat.png)
3. 尋找具有 **Listening** 狀態之 TCP 連接埠 3389 (或指派的 RDP 連接埠) 的項目。 
    > [!NOTE]  
   > 對於使用該連接埠的處理程序或服務，其處理程序識別碼 (PID) 會顯示在 [PID] 欄位底下。
4. 若要判斷哪個應用程式正在使用連接埠 3389 (或指派的 RDP 連接埠)，請輸入下列命令：  
   
     ```powershell  
    cmd /c 'tasklist /svc | find "<pid listening on 3389>"'  
    ```  
  
    ![Tasklist 命令會回報特定處理程序的詳細資料。](../media/troubleshoot-remote-desktop-connections/WPS_tasklist.png)
5. 尋找與此連接埠相關聯之 PID 號碼的項目 (根據 **netstat** 輸出)。 與該 PID 相關聯的服務或處理程序會顯示在右欄。
6. 如果應用程式或遠端桌面服務 (TermServ.exe) 以外的服務正在使用該連接埠，您可以使用下列方法之一來解決衝突：
      - 設定其他應用程式或服務使用不同的連接埠 (建議)。
      - 解除安裝其他應用程式或服務。
      - 設定 RDP 使用不同的連接埠，然後重新啟動「遠端桌面服務」服務 (不建議)。

### <a name="check-whether-a-firewall-is-blocking-the-rdp-port"></a>檢查防火牆是否封鎖 RDP 連接埠

使用 **psping** 工具測試是否可藉由使用連接埠 3389 來觸達受影響的電腦。

1. 移至另一部未受影響的電腦，然後從 <https://live.sysinternals.com/psping.exe> 下載 **psping**。
2. 以系統管理員身分開啟命令提示字元視窗，變更至您安裝 **psping** 所在的目錄，然後輸入下列命令：  
   
   ```  
   psping -accepteula <computer IP>:3389  
   ```
   
3. 檢查 **psping** 命令的輸出是否有如下結果：  
      - **連線至\<computer IP\>** ：遠端電腦可連線。
      - **(0% 遺失)** ：所有連線嘗試皆成功。
      - **遠端電腦拒絕網路連線**：遠端電腦無法連線。
      - **(100% 遺失)** ：所有連線嘗試皆失敗。
1. 在多部電腦上執行 **psping** 來測試能否連線到受影響的電腦。
1. 請注意受影響的電腦是否封鎖來自所有其他電腦、部分其他電腦，或者僅一部其他電腦的連線。
1. 建議的後續步驟：
      - 請連絡網路系統管理員，確認網路允許 RDP 流量傳送到受影響的電腦。
      - 調查來源電腦與受影響電腦之間的任何防火牆設定 (包括受影響電腦上的 Windows 防火牆)，以判斷是否有防火牆封鎖 RDP 連接埠。
