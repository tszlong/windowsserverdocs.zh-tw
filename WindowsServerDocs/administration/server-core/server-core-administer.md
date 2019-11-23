---
title: 管理 Server Core
description: 瞭解如何管理 Windows Server 的 Server Core 安裝
ms.prod: windows-server
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.author: elizapo
ms.localizationpriority: medium
ms.date: 12/18/2018
ms.openlocfilehash: bcc4bf7b3fbdbff1aed2c8dd07b90346fe9eebab
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383429"
---
# <a name="administer-a-server-core-server"></a>管理 Server Core 伺服器

>適用于： Windows Server 2019、Windows Server 2016 和 Windows Server （半年通道）

因為 Server Core 沒有 UI，所以您需要使用 Windows PowerShell Cmdlet、命令列工具或遠端工具來執行基本管理工作。 下列各節概述用於基本工作的 PowerShell Cmdlet 和命令。 您也可以使用[Windows 管理中心](../../manage/windows-admin-center/overview.md)（目前處於公開預覽狀態的統一管理入口網站）來管理您的安裝。 

## <a name="administrative-tasks-using-powershell-cmdlets"></a>使用 PowerShell Cmdlet 的系統管理工作
使用下列資訊來執行 Windows PowerShell Cmdlet 的基本管理工作。

### <a name="set-a-static-ip-address"></a>設定靜態 IP 位址
當您安裝 Server Core 伺服器時，預設會有 DHCP 位址。 如果您需要靜態 IP 位址，您可以使用下列步驟來設定它。

若要查看目前的網路設定，請使用**NetIPConfiguration**。

若要查看您已在使用的 IP 位址，請使用**new-netipaddress**。

若要設定靜態 IP 位址，請執行下列動作： 

1. 執行**get-netipinterface**。 
2. 請記下 IP 介面的**IfIndex**資料行中的數位或**InterfaceDescription**字串。 如果您有多張網路介面卡，請記下您想要設定靜態 IP 位址之介面的對應數位或字串。
3. 執行下列 Cmdlet 來設定靜態 IP 位址：

   ```powershell
   New-NetIPaddress -InterfaceIndex 12 -IPAddress 192.0.2.2 -PrefixLength 24 -DefaultGateway 192.0.2.1
   ```

   其中：
   - **InterfaceIndex**是步驟2中的**IfIndex**值。 （在我們的範例中為12）
   - **IPAddress**是您想要設定的靜態 IP 位址。 （在我們的範例中為191.0.2.2）
   - **PrefixLength**是您所設定之 IP 位址的前置長度（子網路遮罩的另一種形式）。 （在本例中為24）
   - **DefaultGateway**是預設閘道的 IP 位址。 （在我們的範例中為為192.0.2.1）
4. 執行下列 Cmdlet 來設定 DNS 用戶端伺服器位址： 

   ```powershell
   Set-DNSClientServerAddress –InterfaceIndex 12 -ServerAddresses 192.0.2.4
   ```
   
   其中：
   - **InterfaceIndex**是步驟2中的 IfIndex 值。
   - **ServerAddresses**是 DNS 伺服器的 IP 位址。
5. 若要新增多部 DNS 伺服器，請執行下列 Cmdlet： 

   ```powershell
   Set-DNSClientServerAddress –InterfaceIndex 12 -ServerAddresses 192.0.2.4,192.0.2.5
   ```

   其中，在此範例中， **192.0.2.4**和**192.0.2.5**都是 DNS 伺服器的 IP 位址。

如果您需要切換為使用 DHCP，請執行**Set-DnsClientServerAddress – InterfaceIndex 12 – ResetServerAddresses**。

### <a name="join-a-domain"></a>加入網域
使用下列 Cmdlet 將電腦加入網域。

1. 執行 [**新增電腦**]。 系統會提示您輸入這兩個認證，以加入網域和功能變數名稱。
2. 如果您需要將網域使用者帳戶新增至本機系統管理員群組，請在命令提示字元中執行下列命令（不在 PowerShell 視窗中）：

   ```
   net localgroup administrators /add <DomainName>\<UserName>
   ```
3. 重新啟動電腦。 您可以執行 [**重新開機-電腦**] 來完成此動作。

### <a name="rename-the-server"></a>重新命名伺服器
請使用下列步驟來重新命名伺服器。

1. 使用**hostname**或**ipconfig**命令來判斷伺服器的目前名稱。
2. 執行**重新命名-Computer-ComputerName \<new_name\>** 。
3. 重新啟動電腦。

### <a name="activate-the-server"></a>啟用伺服器

執行**slmgr.vbs – ipk\<productkey\>** 。 然後執行**slmgr.vbs – ato**。 如果啟用成功，您就不會收到訊息。

> [!NOTE]
> 您也可以透過電話、使用[金鑰管理服務（KMS）伺服器](../../get-started/server-2016-activation.md)或從遠端啟動伺服器。 若要從遠端啟用，請從遠端電腦執行下列 Cmdlet： 
> 
> ```
> cscript windows\system32\slmgr.vbs <ServerName> <UserName> <password>:-ato
> ```
 
### <a name="configure-windows-firewall"></a>設定 Windows 防火牆

您可以在 Server Core 電腦上，在本機使用 Windows PowerShell Cmdlet 和指令檔設定 Windows 防火牆。 如需可用來設定 Windows 防火牆的 Cmdlet，請參閱[NetSecurity](/powershell/module/netsecurity/?view=win10-ps) 。

### <a name="enable-windows-powershell-remoting"></a>啟用 Windows PowerShell 遠端執行功能

您可以啟用 Windows PowerShell 遠端執行功能，這個功能可讓在一部電腦上輸入的 Windows PowerShell 命令在另一部電腦上執行。 使用**Enable PSRemoting**啟用 Windows PowerShell 遠端功能。

如需詳細資訊，請參閱[關於遠端常見問題](/powershell/module/microsoft.powershell.core/about/about_remote_faq?view=powershell-5.1)。

## <a name="administrative-tasks-from-the-command-line"></a>命令列中的系統管理工作
使用下列參考資訊，從命令列執行系統管理工作。

### <a name="configuration-and-installation"></a>設定與安裝

|                             工作                              |                                                                                                                                                                                                                 命令                                                                                                                                                                                                                 |
|---------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|             設定本機系統管理密碼             |                                                                                                                                                                                                      **net 使用者系統管理員**\*                                                                                                                                                                                                      |
|                  將電腦加入網域                  |                                                                                                                                                       **netdom join% computername%** **/domain：\<網域\>/userd：\<網域\\使用者名稱\>/passwordd：** \* <br> 重新啟動電腦。                                                                                                                                                        |
|              確認網域已變更              |                                                                                                                                                                                                                 **set**                                                                                                                                                                                                                 |
|                從網域移除電腦                |                                                                                                                                                                                                   **netdom 移除 \<computername\>**                                                                                                                                                                                                    |
|         將使用者新增至本機系統管理員群組          |                                                                                                                                                                                       **net localgroup Administrators/add \<網域\\使用者名稱\>**                                                                                                                                                                                       |
|       從本機 Administrators 群組移除使用者       |                                                                                                                                                                                     **net localgroup Administrators/delete \<網域\\使用者名稱\>**                                                                                                                                                                                      |
|               將使用者新增至本機電腦                |                                                                                                                                                                                                **net user \<網域 \ 使用者名稱\> \*/add**                                                                                                                                                                                                 |
|               將群組新增至本機電腦               |                                                                                                                                                                                                 **net localgroup \<組名\>/add**                                                                                                                                                                                                  |
|          變更加入網域之電腦的名稱          |                                                                                                                                                           **netdom renamecomputer% computername%/NewName：\<新的電腦名稱稱\>/userd：\<網域\\username\>/passwordd：** \*                                                                                                                                                            |
|                 確認新的電腦名稱                 |                                                                                                                                                                                                                 **set**                                                                                                                                                                                                                 |
|         變更工作群組中電腦的名稱         |                                                                                                                                                                **netdom renamecomputer \<currentcomputername\>/NewName：\<newcomputername\>** <br>重新啟動電腦。                                                                                                                                                                 |
|                停用分頁檔管理                 |                                                                                                                                                                        **wmic，其中 name = "\<computername\>" set AutomaticManagedPagefile = False**                                                                                                                                                                         |
|                   設定分頁檔                   |                                                            **wmic pagefileset，其中 name = "\<path/filename\>" set InitialSize =\<InitialSize\>，MaximumSize =\<maxsize\>** <br>其中*path/filename*是分頁檔案的路徑和名稱， *initialsize*是分頁檔案的開始大小（以位元組為單位）， *maxsize*是分頁檔案的大小上限（以位元組為單位）。                                                             |
|                 變更到靜態 IP 位址                 | **ipconfig/all** <br>記錄相關資訊，或將其重新導向至文字檔（**ipconfig/all > ipconfig .txt**）。<br>**netsh interface ipv4 show 介面**<br>確認有介面清單。<br>**從介面清單中 \<識別碼的 netsh 介面 ipv4 設定位址名稱\> 來源 = 靜態位址 =\<慣用的 IP 位址\> 閘道 =\<閘道位址\>**<br>執行**ipconfig/all** ，確認 [DHCP 已啟用] 已設定為 [**否**]。 |
|                   設定靜態 DNS 位址。                   |   <strong>netsh interface ipv4 add dnsserver name =\<name 或 network 介面卡的識別碼\> address =\<主要 DNS 伺服器的 IP 位址\> index = 1 <br></strong>netsh interface ipv4 add dnsserver name =\<name of 次要 DNS 伺服器\> address =\<次要 DNS 伺服器的 IP 位址\> index = 2\*\* <br> 視需要重複上述步驟，以新增額外的伺服器。<br>執行**ipconfig/all**以確認位址是否正確。   |
| 從靜態 IP 位址變更為 DHCP 提供的 IP 位址 |                                                                                                                                      **netsh interface ipv4 set address name =\<local system 的 IP 位址\> source = DHCP** <br>執行**ipconfig/all** ，確認 [DCHP 已啟用] 已設定為 **[是]** 。                                                                                                                                      |
|                      輸入產品金鑰                      |                                                                                                                                                                                                   **slmgr.vbs – ipk \<產品金鑰\>**                                                                                                                                                                                                    |
|                  從本機啟用伺服器                  |                                                                                                                                                                                                           **slmgr.vbs-ato**                                                                                                                                                                                                            |
|                 從遠端啟用伺服器                  |                                            **cscript slmgr.vbs – ipk \<產品金鑰\>\<伺服器名稱\>\<username\>\<password\>** <br>**cscript slmgr.vbs-ato \<servername\> \<使用者名稱\> \<密碼\>** <br>藉由執行**cscript slmgr.vbs**來取得電腦的 GUID <br> 執行**cscript slmgr.vbs-dli \<GUID\>** <br>確認 [授權狀態] 已設定為 **[已授權（已啟用）** ]。                                             |

### <a name="networking-and-firewall"></a>網路功能和防火牆

|工作|命令| 
|----|-------|
|將您的伺服器設定為使用 proxy 伺服器|**netsh WinHTTP set proxy \<servername\>：\<埠號碼\>** <br>**注意：** Server Core 安裝無法透過需要密碼的 proxy 來存取網際網路，以允許連接。|
|將您的伺服器設定為略過網際網路位址的 proxy|**netsh winHTTP 設定 proxy \<servername\>：\<埠號碼\> 略過-list = "\<local\>"**| 
|顯示或修改 IPSEC 設定|**netsh ipsec**| 
|顯示或修改 NAP 設定|**netsh nap**| 
|顯示或修改 IP 至實體位址轉譯|**arp**| 
|顯示或設定本機路由表|**料**| 
|查看或設定 DNS 伺服器設定|**nslookup**| 
|顯示通訊協定統計資料以及目前的 TCP/IP 網路連線|**netstat**| 
|使用 NetBIOS over TCP/IP （NBT）顯示通訊協定統計資料和目前的 TCP/IP 連接|**nbtstat**| 
|顯示網路連線的躍點|**pathping**| 
|網路連接的追蹤躍點|**tracert**| 
|顯示多點傳送路由器的設定|**mrinfo**| 
|啟用防火牆的遠端系統管理|**netsh advfirewall 防火牆設定規則群組 = "Windows Defender 防火牆遠端系統管理" new enable = yes**| 
 

### <a name="updates-error-reporting-and-feedback"></a>更新、錯誤報表和意見反應

|                               工作                                |                                                                                                                               命令                                                                                                                                |
|-------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                         安裝更新                         |                                                                                                                    **wusa \<更新\>.msu/quiet**                                                                                                                    |
|                      列出已安裝的更新                       |                                                                                                                            **systeminfo**                                                                                                                            |
|                         移除更新                          |                                 **展開/f：\* \<更新\>.msu c:\test** <br>流覽至 c:\test\，然後在文字編輯器中開啟 \<更新\>。<br>以 [**移除**] 取代**Install** ，並儲存檔案。<br>**pkgmgr/n：\<更新\>.xml**                                 |
|                    設定自動更新                    |          若要確認目前的設定： **cscript%SYSTEMROOT%\SYSTEM32\SCREGEDIT.WSF/AU/v \*\*<br>啟用自動更新： \*\*cscript scregedit.wsf. MANAGE-BDE.WSF/AU 4** <br>若要停用自動更新： **cscript%SYSTEMROOT%\SYSTEM32\SCREGEDIT.WSF/AU 1**          |
|                      啟用錯誤報告                       | 若要驗證目前的設定： **serverWerOptin/query** <br>若要自動傳送詳細報告： **serverWerOptin/detailed** <br>若要自動傳送摘要報告： **serverWerOptin/summary** <br>若要停用錯誤報表： **serverWerOptin/disable** |
| 參與客戶經驗改進計畫 (CEIP) |                                                     若要驗證目前的設定： **serverCEIPOptin/query** <br>若要啟用 CEIP： **serverCEIPOptin/enable** <br>若要停用 CEIP： **serverCEIPOptin/disable**                                                      |

### <a name="services-processes-and-performance"></a>服務、程序以及效能

|                               工作                               |                                                                                                                                                                                                             命令                                                                                                                                                                                                              |
|------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                    列出正在執行的服務                     |                                                                                                                                                                                                  **sc 查詢**或**net start**                                                                                                                                                                                                   |
|                         啟動服務                          |                                                                                                                                                                                 **sc start \<服務名稱\>** 或**net start \<服務名稱\>**                                                                                                                                                                                  |
|                          停止服務                          |                                                                                                                                                                                  **sc 停止 \<服務名稱\>** 或**net stop \<服務名稱\>**                                                                                                                                                                                   |
| 擷取執行中應用程式與相關處理程序的清單 |                                                                                                                                                                                                           **tasklist**                                                                                                                                                                                                           |
|                        啟動工作管理員                        |                                                                                                                                                                                                           **taskmgr**                                                                                                                                                                                                            |
|    建立和管理事件追蹤會話和效能記錄    | 若要建立計數器、追蹤、設定資料收集或 API： **logman 建立** <br>若要查詢資料收集器屬性： **logman query** <br>若要啟動或停止資料收集： **logman start\|stop** <br>若要刪除收集器： **logman delete** <br> 更新收集器的屬性： **logman 更新** <br>若要從 XML 檔案匯入資料收集器集合，或將它匯出至 XML 檔案： **logman 匯入\|匯出** |

### <a name="event-logs"></a>事件記錄檔

|工作|命令| 
|----|-------|
|列出事件記錄檔|**wevtutil el**| 
|查詢指定記錄中的事件|**wevtutil qe/f：文字 \<記錄檔名稱\>**| 
|匯出事件記錄檔|**wevtutil epl \<記錄檔名稱\>**| 
|清除事件記錄檔|**wevtutil cl \<記錄檔名稱\>**| 


### <a name="disk-and-file-system"></a>磁碟和檔案系統

|                   工作                   |                        命令                        |
|------------------------------------------|-------------------------------------------------------|
|          管理磁碟分割          | 如需完整的命令清單，請執行**diskpart/？**  |
|           管理軟體 RAID           | 如需完整的命令清單，請執行**diskraid/？**  |
|        管理磁碟區掛接點        | 如需完整的命令清單，請執行**mountvol/？**  |
|           重組磁碟區            |  如需完整的命令清單，請執行**磁碟重組/？**   |
| 將磁碟區轉換為 NTFS 檔案系統 |        **轉換 \<的 volume 字母\>/FS： NTFS**         |
|              壓縮檔案              |  如需完整的命令清單，請執行**compact/？**  |
|          管理開啟的檔案           | 如需完整的命令清單，請執行**openfiles/？** |
|          管理 VSS 資料夾          | 如需完整的命令清單，請執行**vssadmin/？**  |
|        管理檔案系統        |  如需完整的命令清單，請執行**fsutil/？**   |
|    取得檔案或資料夾的擁有權    |  如需完整的命令清單，請執行**icacls/？**   |
 
### <a name="hardware"></a>硬體

|工作|命令| 
|----|-------|
|新增新硬體裝置的驅動程式|將驅動程式複製到位於% homedrive%\\\<驅動程式資料夾\>的資料夾。 執行**pnputil-i-a% homedrive%\\\<驅動程式資料夾\>\\\<驅動程式\>.inf**|
|移除硬體裝置的驅動程式|如需已載入的驅動程式清單，請執行**sc query type = driver**。 然後執行**sc delete \<service_name\>**|
