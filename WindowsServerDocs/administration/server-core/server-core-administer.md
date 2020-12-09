---
title: 管理 Server Core
description: 瞭解如何管理 Windows Server 的 Server Core 安裝
ms.mktglfcycl: manage
ms.sitesec: library
author: pronichkin
ms.author: artemp
ms.localizationpriority: medium
ms.date: 12/18/2018
ms.openlocfilehash: 2cdddf706d03aa5b1aa1239b58fd4d8a9f4beed2
ms.sourcegitcommit: d08965d64f4a40ac20bc81b14f2d2ea89c48c5c8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/08/2020
ms.locfileid: "96864317"
---
# <a name="administer-a-server-core-server"></a>管理 Server Core 伺服器

>適用于： Windows Server 2019、Windows Server 2016 和 Windows Server (半年通道) 

因為 Server Core 沒有 UI，所以您需要使用 Windows PowerShell Cmdlet、命令列工具或遠端工具來執行基本的系統管理工作。 下列各節概述用於基本工作的 PowerShell Cmdlet 和命令。 您也可以使用 [Windows Admin Center](../../manage/windows-admin-center/overview.md)（目前處於公開預覽狀態的整合管理入口網站）來管理您的安裝。

## <a name="administrative-tasks-using-powershell-cmdlets"></a>使用 PowerShell Cmdlet 的管理工作
使用下列資訊，利用 Windows PowerShell Cmdlet 來執行基本的系統管理工作。

### <a name="set-a-static-ip-address"></a>設定靜態 IP 位址
當您安裝 Server Core 伺服器時，預設會有 DHCP 位址。 如果您需要靜態 IP 位址，您可以使用下列步驟來設定它。

若要查看目前的網路設定，請使用 **NetIPConfiguration**。

若要查看您已經在使用的 IP 位址，請使用 **new-netipaddress**。

若要設定靜態 IP 位址，請執行下列動作：

1. 執行 **Get-get-netipinterface**。
2. 請注意 IP 介面或 **InterfaceDescription** 字串的 **IfIndex** 資料行中的數位。 如果您有一個以上的網路介面卡，請記下您想要設定其靜態 IP 位址之介面的對應數位或字串。
3. 執行下列 Cmdlet 來設定靜態 IP 位址：

   ```powershell
   New-NetIPaddress -InterfaceIndex 12 -IPAddress 192.0.2.2 -PrefixLength 24 -DefaultGateway 192.0.2.1
   ```

   其中：
   - **InterfaceIndex** 是步驟2中的 **IfIndex** 值。  (在我們的範例中為 12) 
   - **IPAddress** 是您想要設定的靜態 IP 位址。  (在我們的範例中，191.0.2.2) 
   - **PrefixLength** 是前置長度 (另一種形式的子網路遮罩，) 您要設定的 IP 位址。  (在我們的範例中為 24) 
   - **DefaultGateway** 是預設閘道的 IP 位址。  (範例中的為 192.0.2.1) 
4. 執行下列 Cmdlet 來設定 DNS 用戶端伺服器位址：

   ```powershell
   Set-DNSClientServerAddress –InterfaceIndex 12 -ServerAddresses 192.0.2.4
   ```

   其中：
   - **InterfaceIndex** 是步驟2中的 IfIndex 值。
   - **ServerAddresses** 是您 DNS 伺服器的 IP 位址。
5. 若要新增多部 DNS 伺服器，請執行下列 Cmdlet：

   ```powershell
   Set-DNSClientServerAddress –InterfaceIndex 12 -ServerAddresses 192.0.2.4,192.0.2.5
   ```

   在此範例中， **192.0.2.4** 和 **192.0.2.5** 都是 DNS 伺服器的 IP 位址。

如果您需要切換到使用 DHCP，請執行 **Set-set-dnsclientserveraddress – InterfaceIndex 12 – ResetServerAddresses**。

### <a name="join-a-domain"></a>加入網域
使用下列 Cmdlet 將電腦加入網域。

1. 執行 [ **新增電腦**]。 系統會提示您輸入這兩個認證來加入網域和功能變數名稱。
2. 如果您需要將網域使用者帳戶新增至本機系統管理員群組，請在命令提示字元中執行下列命令， (不在 PowerShell 視窗中) ：

   ```
   net localgroup administrators /add <DomainName>\<UserName>
   ```
3. 重新啟動電腦。 您可以執行 **重新開機電腦** 來執行此動作。

### <a name="rename-the-server"></a>重新命名伺服器
使用下列步驟來重新命名伺服器。

1. 使用 **hostname** 或 **ipconfig** 命令判斷伺服器的目前名稱。
2. 執行 **重新命名-電腦- \<new_name\> ComputerName**。
3. 重新啟動電腦。

### <a name="activate-the-server"></a>啟用伺服器

執行 **slmgr.vbs – ipk \<productkey\>**。 然後執行 **slmgr.vbs – ato**。 如果啟用成功，您將不會收到訊息。

> [!NOTE]
> 您也可以透過電話、使用 [ (KMS) server 的金鑰管理服務](../../get-started/server-2016-activation.md)，或從遠端啟用伺服器。 若要從遠端啟用，請從遠端電腦執行下列 Cmdlet：
>
> ```
> cscript windows\system32\slmgr.vbs <ServerName> <UserName> <password>:-ato
> ```

### <a name="configure-windows-firewall"></a>設定 Windows 防火牆

您可以在 Server Core 電腦上，在本機使用 Windows PowerShell Cmdlet 和指令檔設定 Windows 防火牆。 如需可用於設定 Windows 防火牆的 Cmdlet，請參閱 [NetSecurity](/powershell/module/netsecurity/) 。

### <a name="enable-windows-powershell-remoting"></a>啟用 Windows PowerShell 遠端執行功能

您可以啟用 Windows PowerShell 遠端執行功能，這個功能可讓在一部電腦上輸入的 Windows PowerShell 命令在另一部電腦上執行。 使用 **>enable-psremoting** 啟用 Windows PowerShell 遠端處理。

如需詳細資訊，請參閱 [關於遠端常見問題](/powershell/module/microsoft.powershell.core/about/about_remote_faq?view=powershell-5.1)。

## <a name="administrative-tasks-from-the-command-line"></a>從命令列管理工作
您可以使用下列參考資訊，從命令列執行系統管理工作。

### <a name="configuration-and-installation"></a>設定與安裝

|                             Task                              |                                                                                                                                                                                                                 Command                                                                                                                                                                                                                 |
|---------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|             設定本機系統管理密碼             |                                                                                                                                                                                                      **net 使用者系統管理員**\*                                                                                                                                                                                                      |
|                  將電腦加入網域                  |                                                                                                                                                       **netdom join% computername%** **/domain： \<domain\> /userd： \<domain\\username\> /passwordd：**\* <br> 重新啟動電腦。                                                                                                                                                        |
|              確認網域已變更              |                                                                                                                                                                                                                 **set**                                                                                                                                                                                                                 |
|                從網域移除電腦                |                                                                                                                                                                                                   **netdom 移除 \<computername\>**                                                                                                                                                                                                    |
|         將使用者新增至本機系統管理員群組          |                                                                                                                                                                                       **net localgroup 系統管理員/add \<domain\\username\>**                                                                                                                                                                                       |
|       從本機 Administrators 群組移除使用者       |                                                                                                                                                                                     **net localgroup Administrator/delete \<domain\\username\>**                                                                                                                                                                                      |
|               將使用者新增至本機電腦                |                                                                                                                                                                                                **net user \<domain\username\> \* /add**                                                                                                                                                                                                 |
|               將群組新增至本機電腦               |                                                                                                                                                                                                 **net localgroup \<group name\> /add**                                                                                                                                                                                                  |
|          變更加入網域之電腦的名稱          |                                                                                                                                                           **netdom renamecomputer% computername%/NewName： \<new computer name\> /userd： \<domain\\username\> /passwordd：**\*                                                                                                                                                            |
|                 確認新的電腦名稱                 |                                                                                                                                                                                                                 **set**                                                                                                                                                                                                                 |
|         變更工作群組中電腦的名稱         |                                                                                                                                                                **netdom renamecomputer \<currentcomputername\> /NewName：\<newcomputername\>** <br>重新啟動電腦。                                                                                                                                                                 |
|                停用分頁檔管理                 |                                                                                                                                                                        **在 name = " \<computername\> " Set AutomaticManagedPagefile = False 的情況下，wmic**                                                                                                                                                                         |
|                   設定分頁檔                   |                                                            **wmic pagefileset，其中 name = " \<path/filename\> " Set InitialSize = \<initialsize\> ，MaximumSize =\<maxsize\>** <br>其中 *path/filename* 是分頁檔案的路徑和名稱， *initialsize* 是分頁檔案的開始大小（以位元組為單位），而 *maxsize* 是分頁檔的大小上限（以位元組為單位）。                                                             |
|                 變更到靜態 IP 位址                 | **ipconfig/all** <br>請記錄相關資訊，或將其重新導向至文字檔 (**ipconfig/all >ipconfig.txt**) 。<br>**netsh interface ipv4 show interfaces**<br>確認有介面清單。<br>**netsh interface ipv4 set address \<Name ID from interface list\> source = static address = \<preferred IP address\> gateway =\<gateway address\>**<br>執行 **ipconfig/all** ，確認 [DHCP 已啟用] 設定為 [ **否**]。 |
|                   設定靜態 DNS 位址。                   |   <strong>netsh interface ipv4 add dnsserver name = \<name or ID of the network interface card\> address = \<IP address of the primary DNS server\> index = 1 <br></strong>netsh interface ipv4 add dnsserver name = \<name of secondary DNS server\> address = \<IP address of the secondary DNS server\> index = 2\*\* <br> 視需要重複上述步驟以新增其他伺服器。<br>執行 **ipconfig/all** 以驗證位址是否正確。   |
| 從靜態 IP 位址變更為 DHCP 提供的 IP 位址 |                                                                                                                                      **netsh interface ipv4 set address name = \<IP address of local system\> source = DHCP** <br>執行 **ipconfig/all** 來確認 [DCHP 已啟用] 設定為 **[是]**。                                                                                                                                      |
|                      輸入產品金鑰                      |                                                                                                                                                                                                   **slmgr.vbs – ipk \<product key\>**                                                                                                                                                                                                    |
|                  從本機啟用伺服器                  |                                                                                                                                                                                                           **slmgr.vbs -ato**                                                                                                                                                                                                            |
|                 從遠端啟用伺服器                  |                                            **cscript slmgr.vbs – ipk \<product key\>\<server name\>\<username\>\<password\>** <br>**cscript slmgr.vbs-ato \<servername\> \<username\>\<password\>** <br>藉由執行 **cscript slmgr.vbs** 來取得電腦的 GUID <br> 執行 **cscript slmgr.vbs-dli \<GUID\>** <br>確認 [授權狀態] 設定為 [已 **啟用授權 (])**。                                             |

### <a name="networking-and-firewall"></a>網路功能和防火牆

|Task|Command|
|----|-------|
|設定您的伺服器以使用 proxy 伺服器|**netsh WinHTTP set proxy \<servername\> ：\<port number\>** <br>**注意：** Server Core 安裝無法透過需要密碼以允許連線的 proxy 存取網際網路。|
|設定您的伺服器以略過網際網路位址的 proxy|**netsh winHTTP set proxy \<servername\> ： \<port number\> 略過清單 = " \<local\> "**|
|顯示或修改 IPSEC 設定|**netsh ipsec**|
|顯示或修改 NAP 設定|**netsh nap**|
|顯示或修改實體位址轉譯的 IP|**arp**|
|顯示或設定本機路由表|**route**|
|查看或設定 DNS 伺服器設定|**nslookup**|
|顯示通訊協定統計資料以及目前的 TCP/IP 網路連線|**netstat**|
|使用 NetBIOS over TCP/IP (NBT) 顯示通訊協定統計資料和目前的 TCP/IP 連接|**nbtstat**|
|顯示網路連接的躍點|**pathping**|
|網路連接的追蹤躍點|**tracert**|
|顯示多點傳送路由器的設定|**mrinfo**|
|啟用防火牆的遠端系統管理|**netsh advfirewall firewall set rule group = "Windows Defender 防火牆 Remote Management" new enable = yes**|


### <a name="updates-error-reporting-and-feedback"></a>更新、錯誤報告及意見反應

|                               Task                                |                                                                                                                               Command                                                                                                                                |
|-------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                         安裝更新                         |                                                                                                                    **wusa.exe \<update\> .msu/quiet**                                                                                                                    |
|                      列出已安裝的更新                       |                                                                                                                            **systeminfo**                                                                                                                            |
|                         移除更新                          |                                 **展開/f： \* \<update\> .msu c:\test** <br>瀏覽到 c:\test\，然後使用文字編輯器開啟 \<update\>.xml。<br>以 [**移除**] 取代 [**安裝**]，然後儲存檔案。<br>**pkgmgr/n： \<update\> .xml**                                 |
|                    設定自動更新                    |          若要驗證目前的設定： **cscript%systemroot%\system32\scregedit.wsf/AU/v \* \* <br> 以啟用自動更新： \* \* cscript scregedit.wsf. w2kmiguser.wsf/AU 4** <br>停用自動更新： **cscript%SYSTEMROOT%\SYSTEM32\SCREGEDIT.WSF/AU 1**          |
|                      啟用錯誤報告                       | 確認目前的設定： **serverWerOptin/query** <br>自動傳送詳細報告： **serverWerOptin/detailed** <br>若要自動傳送摘要報告： **serverWerOptin/summary** <br>停用錯誤報表： **serverWerOptin/disable** |
| 參與客戶經驗改進計畫 (CEIP) |                                                     確認目前的設定： **serverCEIPOptin/query** <br>若要啟用 CEIP： **serverCEIPOptin/enable** <br>停用 CEIP： **serverCEIPOptin/disable**                                                      |

### <a name="services-processes-and-performance"></a>服務、程序以及效能

|                               Task                               |                                                                                                                                                                                                             Command                                                                                                                                                                                                              |
|------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                    列出正在執行的服務                     |                                                                                                                                                                                                  **sc 查詢** 或 **net start**                                                                                                                                                                                                   |
|                         啟動服務                          |                                                                                                                                                                                 **sc start \<service name\>** 或 **net start \<service name\>**                                                                                                                                                                                  |
|                          停止服務                          |                                                                                                                                                                                  **sc stop \<service name\>** 或 **net stop \<service name\>**                                                                                                                                                                                   |
| 擷取執行中應用程式與相關處理程序的清單 |                                                                                                                                                                                                           **tasklist**                                                                                                                                                                                                           |
|                        啟動工作管理員                        |                                                                                                                                                                                                           **taskmgr**                                                                                                                                                                                                            |
|    建立和管理事件追蹤會話和效能記錄    | 若要建立計數器、追蹤、設定資料收集或 API： **logman 建立** <br>若要查詢資料收集器屬性： **logman query** <br>若要啟動或停止資料收集： **logman 開始 \| 停止** <br>刪除收集器： **logman delete** <br> 更新收集器的屬性： **logman 更新** <br>若要從 XML 檔案匯入資料收集器集合，或將它匯出至 XML 檔案： **logman 匯入 \| 匯出** |

### <a name="event-logs"></a>事件記錄檔

|Task|Command|
|----|-------|
|列出事件記錄檔|**wevtutil el**|
|查詢指定之記錄檔中的事件|**wevtutil qe/f： text \<log name\>**|
|匯出事件記錄檔|**wevtutil epl \<log name\>**|
|清除事件記錄檔|**wevtutil cl \<log name\>**|


### <a name="disk-and-file-system"></a>磁碟和檔案系統

|                   Task                   |                        Command                        |
|------------------------------------------|-------------------------------------------------------|
|          管理磁碟分割          | 如需完整的命令清單，請執行 **diskpart/？**  |
|           管理軟體 RAID           | 如需完整的命令清單，請執行 **diskraid/？**  |
|        管理磁碟區掛接點        | 如需完整的命令清單，請執行 **mountvol/？**  |
|           重組磁碟區            |  如需完整的命令清單，請執行 **defrag/？**   |
| 將磁碟區轉換為 NTFS 檔案系統 |        **轉換 \<volume letter\> /fs： NTFS**         |
|              壓縮檔案              |  如需完整的命令清單，請執行 **compact/？**  |
|          管理開啟的檔案           | 如需完整的命令清單，請執行 **openfiles/？** |
|          管理 VSS 資料夾          | 如需完整的命令清單，請執行 **vssadmin/？**  |
|        管理檔案系統        |  如需完整的命令清單，請執行 **fsutil/？**   |
|    取得檔案或資料夾的擁有權    |  如需完整的命令清單，請執行 **icacls/？**   |

### <a name="hardware"></a>硬體

|Task|Command|
|----|-------|
|新增新硬體裝置的驅動程式|將驅動程式複製到位於% homedrive% 的資料夾 \\ \<driver folder\> 。 執行 **pnputil-i-a% homedrive% \\ \<driver folder\> \\ \<driver\> .inf**|
|移除硬體裝置的驅動程式|如需載入的驅動程式清單，請執行 **sc query type = driver**。 然後執行 **sc delete \<service_name\>**|
