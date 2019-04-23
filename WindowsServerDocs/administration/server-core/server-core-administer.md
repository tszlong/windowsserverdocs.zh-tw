---
title: 管理 Server Core
description: 了解如何管理 Windows Server 的 Server Core 安裝
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.author: elizapo
ms.localizationpriority: medium
ms.date: 12/18/2018
ms.openlocfilehash: 39fbb92645d39a46613f2142d0258c78a6ba425b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842659"
---
# <a name="administer-a-server-core-server"></a>管理 Server Core 伺服器

>適用於：Windows Server （半年通道） 和 Windows Server 2016

因為 Server Core 沒有 UI，您需要使用 Windows PowerShell cmdlet、 命令列工具或遠端工具來執行基本管理工作。 下列各節說明的 PowerShell cmdlet 和命令用來執行基本工作。 您也可以使用[Windows Admin Center](../../manage/windows-admin-center/overview.md)，統一的管理入口網站，目前處於公開預覽狀態，來管理您的安裝。 

## <a name="administrative-tasks-using-powershell-cmdlets"></a>使用 PowerShell cmdlet 的系統管理工作
使用下列資訊來執行基本的系統管理工作，使用 Windows PowerShell cmdlet。

### <a name="set-a-static-ip-address"></a>設定靜態 IP 位址
當您安裝在 Server Core 伺服器時，依預設它的 DHCP 位址。 如果您需要靜態 IP 位址，您可以設定它使用下列步驟。

若要檢視目前的網路設定，請使用**Get NetIPConfiguration**。

若要檢視您已經使用的 IP 位址，請使用**Get NetIPAddress**。

若要設定靜態 IP 位址，執行下列作業： 

1. 執行**Get-netipinterface**。 
2. 請注意中的之數字**IfIndex** IP 介面的資料行或**InterfaceDescription**字串。 如果您有多張網路介面卡，請注意數字或字串對應至您想要設定靜態 IP 位址的介面。
3. 執行下列 cmdlet 來設定靜態 IP 位址：

   ```powershell
   New-NetIPaddress -InterfaceIndex 12 -IPAddress 192.0.2.2 -PrefixLength 24 -DefaultGateway 192.0.2.1
   ```

   其中：
   - **InterfaceIndex**的值**IfIndex**從步驟 2。 （在我們範例中為 12）
   - **IPAddress**是您想要設定靜態 IP 位址。 （在本例中，191.0.2.2）
   - **PrefixLength**是您所設定的 IP 位址的首碼長度 （子網路遮罩的另一個形式）。 （適用於我們範例中為 24）
   - **DefaultGateway**是預設閘道的 IP 位址。 （適用於我們範例中為 192.0.2.1）
4. 執行下列 cmdlet 來設定 DNS 用戶端伺服器位址： 

   ```powershell
   Set-DNSClientServerAddress –InterfaceIndex 12 -ServerAddresses 192.0.2.4
   ```
   
   其中：
   - **InterfaceIndex** IfIndex 步驟 2 中的值。
   - **ServerAddresses**是您的 DNS 伺服器的 IP 位址。
5. 若要新增多部 DNS 伺服器，請執行下列 cmdlet: 

   ```powershell
   Set-DNSClientServerAddress –InterfaceIndex 12 -ServerAddresses 192.0.2.4,192.0.2.5
   ```

   在此範例中的位置**192.0.2.4**並**192.0.2.5 都**是這兩個 DNS 伺服器的 IP 位址。

如果您需要改為使用 DHCP，執行**組 DnsClientServerAddress – InterfaceIndex 12 – ResetServerAddresses**。

### <a name="join-a-domain"></a>加入網域
您可以使用下列 cmdlet 來將電腦加入網域。

1. 執行**新增電腦**。 這兩種認證以加入網域和網域名稱，將會提示您。
2. 如果您需要將網域使用者帳戶新增至本機 Administrators 群組，請在命令提示字元 （不在 [PowerShell] 視窗中） 執行下列命令：

   ```
   net localgroup administrators /add <DomainName>\<UserName>
   ```
3. 重新啟動電腦。 您可以藉由執行**Restart-computer**。

### <a name="rename-the-server"></a>重新命名伺服器
您可以使用下列步驟，重新命名伺服器。

1. 判斷目前的伺服器名稱**hostname**或是**ipconfig**命令。
2. 執行**Rename-computer-ComputerName \<new_name\>**。
3. 重新啟動電腦。

### <a name="activate-the-server"></a>啟用伺服器

執行**slmgr.vbs – ipk\<productkey\>**。 然後執行**slmgr.vbs – ato**。 如果啟用成功時，您將不會收到的訊息。

> [!NOTE]
> 您也可以啟用伺服器電話，使用[金鑰管理服務 (KMS) 伺服器](../../get-started/server-2016-activation.md)，或遠端電腦上。 若要從遠端啟動，請從遠端電腦執行下列 cmdlet: 

>```powershell
>**cscript windows\system32\slmgr.vbs <ServerName> <UserName> <password>:-ato**
>```
 
### <a name="configure-windows-firewall"></a>設定 Windows 防火牆

您可以在 Server Core 電腦上，在本機使用 Windows PowerShell Cmdlet 和指令檔設定 Windows 防火牆。 請參閱[NetSecurity](/powershell/module/netsecurity/?view=win10-ps) cmdlet 可用來設定 Windows 防火牆。

### <a name="enable-windows-powershell-remoting"></a>啟用 Windows PowerShell 遠端執行功能

您可以啟用 Windows PowerShell 遠端執行功能，這個功能可讓在一部電腦上輸入的 Windows PowerShell 命令在另一部電腦上執行。 啟用與 Windows PowerShell 遠端**Enable-psremoting**。

如需詳細資訊，請參閱[關於遠端常見問題集](/powershell/module/microsoft.powershell.core/about/about_remote_faq?view=powershell-5.1)
</br>

## <a name="administrative-tasks-from-the-command-line"></a>從命令列系統管理工作
若要從命令列執行系統管理工作，使用下列參考資訊。

### <a name="configuration-and-installation"></a>設定與安裝
|工作 | 命令 |
|-----|-------|
|設定本機系統管理密碼| **net 使用者系統管理員** \* |
|將電腦加入網域| **netdom join %computername%** **/domain:\<網域\>/userd:\<網域\\username\> /passwordd:**\* <br> 重新啟動電腦。|
|確認網域已變更| **set** |
|從網域移除電腦|**netdom 移除\<computername\>**| 
|將使用者新增至本機 Administrators 群組|**net localgroup 系統管理員 / 新增\<網域\\使用者名稱\>** |
|從本機 Administrators 群組移除使用者|**net localgroup 管理員 /delete\<網域\\使用者名稱\>** |
|將使用者新增至本機電腦|**net 使用者\<domain\username\> * /** |
|將群組新增至本機電腦|**net localgroup\<群組名稱\>/add**|
|變更加入網域之電腦的名稱|**netdom renamecomputer %computername%/NewName:\<新的電腦名稱\>/userd:\<網域\\username\> /passwordd:** * |
|確認新的電腦名稱|**set**| 
|變更工作群組中電腦的名稱|**netdom renamecomputer \<currentcomputername\> /NewName:\<newcomputername\>** <br>重新啟動電腦。|
|停用分頁檔管理|**wmic computersystem where name="\<computername\>" set AutomaticManagedPagefile=False**| 
|設定分頁檔|**wmic pagefileset where name=”\<path/filename\>” set InitialSize=\<initialsize\>,MaximumSize=\<maxsize\>** <br>其中*路徑/filename*是分頁檔的名稱與路徑*initialsize*是起始分頁檔的大小，以位元組為單位，並*maxsize*的大小上限分頁檔，以位元組為單位。|
|變更到靜態 IP 位址|**ipconfig /all** <br>記錄的相關資訊，或將它重新導向至文字檔案 (**ipconfig /all > ipconfig.txt**)。<br>**netsh 介面 ipv4 顯示介面**<br>確認有介面清單。<br>**netsh interface ipv4 設定位址名稱\<識別碼，從介面清單\>來源 = 靜態位址 =\<慣用的 IP 位址\>閘道 =\<閘道位址\>**<br>執行**ipconfig /all**來設定為 已啟用 DHCP 的 vierfy **No**。|
|設定靜態 DNS 位址。|**netsh interface ipv4 新增 dnsserver 名稱 =\<網路介面卡的名稱或識別碼\>地址 =\<的主要 DNS 伺服器的 IP 位址\>索引 = 1 <br>** netsh interface ipv4 新增dnsserver 名稱 =\<次要 DNS 伺服器的名稱\>地址 =\<次要 DNS 伺服器 IP 位址\>索引 = 2 * * <br> 重複執行視需要新增額外的伺服器。<br>執行**ipconfig /all**確認位址正確無誤。|
|從靜態 IP 位址變更為 DHCP 提供的 IP 位址|**netsh interface ipv4 設定位址名稱 =\<的本機系統的 IP 位址\>來源 = DHCP** <br>執行**Ipconfig /all**若要確認 DHCP 啟用已設定為**是**。|
|輸入產品金鑰|**slmgr.vbs –ipk \<product key\>**| 
|從本機啟用伺服器|**slmgr.vbs -ato**| 
|從遠端啟用伺服器|**cscript slmgr.vbs – ipk\<產品金鑰\>\<伺服器名稱\>\<username\>\<密碼\>** <br>**cscript slmgr.vbs -ato \<servername\> \<username\> \<password\>** <br>藉由執行取得電腦的 GUID **cscript slmgr.vbs-未** <br> 執行**cscript slmgr.vbs-dli \<GUID\>** <br>確認授權狀態會設為 **（啟動） 的已授權**。


### <a name="networking-and-firewall"></a>網路功能和防火牆

|工作|命令| 
|----|-------|
|將伺服器設定為使用 proxy 伺服器|**netsh Winhttp 設定 proxy \<servername\>:\<連接埠號碼\>** <br>**注意：** Server Core 安裝無法透過需要的密碼，才能允許連線的 proxy 存取網際網路。|
|將伺服器設定為略過網際網路位址的 proxy|**netsh winttp set proxy \<servername\>:\<port number\> bypass-list="\<local\>"**| 
|顯示或修改 IPSEC 設定|**netsh ipsec**| 
|顯示或修改 NAP 設定|**netsh nap**| 
|顯示或修改 IP 位址轉譯為實體位址|**arp**| 
|顯示或設定本機路由表|**route**| 
|檢視或設定 DNS 伺服器設定|**nslookup**| 
|顯示通訊協定統計資料以及目前的 TCP/IP 網路連線|**netstat**| 
|顯示通訊協定統計資料和目前的 TCP/IP 連線使用 NetBIOS over TCP/IP (NBT)|**nbtstat**| 
|顯示網路連線的躍點|**pathping**| 
|追蹤網路連線的躍點|**tracert**| 
|顯示多點傳送路由器的設定|**mrinfo**| 
|啟用防火牆的遠端管理|**netsh advfirewall 防火牆設定的規則群組 = 「 Windows 防火牆遠端管理 」 的新 enable = yes**| 
 

### <a name="updates-error-reporting-and-feedback"></a>更新、 錯誤報告及意見反應

|工作|命令| 
|----|-------|
|安裝更新|**wusa\<更新\>.msu /quiet**| 
|列出已安裝的更新|**systeminfo**| 
|移除更新|**展開 /f\* \<更新\>.msu c:\test** <br>瀏覽至 c:\test\，並開啟\<更新\>在文字編輯器中的.xml。<br>取代**安裝**具有**移除**並儲存檔案。<br>**pkgmgr /n:\<update\>.xml**|
|設定自動更新|若要確認目前的設定: * * cscript %systemroot%\system32\scregedit.wsf /AU /v * *<br>若要啟用自動更新： **cscript scregedit.wsf /AU 4** <br>若要停用自動更新： **cscript %systemroot%\system32\scregedit.wsf /AU 1**| 
|啟用錯誤報告|若要確認目前的設定： **serverWerOptin /query** <br>若要自動傳送詳細的報告： **serverWerOptin / 詳細** <br>若要自動傳送摘要報告： **serverWerOptin 沒有指定 /summary** <br>若要停用錯誤報告： **serverWerOptin /disable**|
|參與客戶經驗改進計畫 (CEIP)|若要確認目前的設定： **serverCEIPOptin /query** <br>若要啟用 CEIP: **serverCEIPOptin /enable** <br>若要停用 CEIP: **serverCEIPOptin /disable**|

### <a name="services-processes-and-performance"></a>服務、程序以及效能

|工作|命令| 
|----|-------|
|列出執行中的服務|**sc 查詢**或**net start**| 
|啟動服務|**sc start\<服務名稱\>** 或是**net start\<服務名稱\>**| 
|停止服務|**sc stop\<服務名稱\>** 或是**net stop\<服務名稱\>**| 
|擷取執行中應用程式與相關處理程序的清單|**tasklist**||強制停止處理程序|執行**tasklist**擷取處理序識別碼 (PID)，然後執行**taskkill /PID\<處理序識別碼\>**|
|啟動工作管理員|**taskmgr**| 
|建立和管理事件追蹤工作階段和效能記錄檔|若要建立計數器、 追蹤、 設定資料收集或 API: **logman 建立** <br>查詢資料收集器屬性： **logman 查詢** <br>若要啟動或停止資料收集： **logman start\|停止** <br>若要刪除收集器： **logman 刪除** <br> 若要更新收集器的屬性： **logman 更新** <br>從 XML 檔案匯入資料收集器集合工具，或將它匯出至 XML 檔案： **logman 匯入\|匯出**|

### <a name="event-logs"></a>事件記錄檔

|工作|命令| 
|----|-------|
|清單事件記錄檔|**wevtutil el**| 
|查詢中指定的記錄檔的事件|**wevtutil qe /f:text \<log name\>**| 
|匯出事件記錄檔|**wevtutil epl\<記錄檔名稱\>**| 
|清除事件記錄檔|**wevtutil cl\<記錄檔名稱\>**| 


### <a name="disk-and-file-system"></a>磁碟和檔案系統

|工作|命令|
|----|-------|
|管理磁碟分割|命令的完整清單，針對執行**diskpart /？**|  
|管理軟體 RAID|命令的完整清單，針對執行**diskraid /？**|  
|管理磁碟區掛接點|命令的完整清單，針對執行**mountvol /？**| 
|重組磁碟區|命令的完整清單，針對執行**重組 /？**|  
|將磁碟區轉換為 NTFS 檔案系統|**轉換\<磁碟區代號\>/fs: ntfs**| 
|壓縮檔案|命令的完整清單，針對執行**壓縮 /？**|  
|管理開啟的檔案|命令的完整清單，針對執行**openfiles /？**|  
|管理 VSS 資料夾|命令的完整清單，針對執行**vssadmin /？**| 
|管理檔案系統|命令的完整清單，針對執行**fsutil /？**||驗證檔案簽章|**sigverif /?**| 
|取得檔案或資料夾的擁有權|命令的完整清單，針對執行**icacls /？**| 
 
### <a name="hardware"></a>硬體

|工作|命令| 
|----|-------|
|新增新硬體裝置的驅動程式|將驅動程式複製到 %homedrive%資料夾\\\<驅動程式資料夾\>。 執行**pnputil-i-a %homedrive%\\\<驅動程式資料夾\>\\\<驅動程式\>.inf**|
|移除硬體裝置的驅動程式|如需載入的驅動程式的清單，執行**sc 查詢類型 = 驅動程式**。 然後執行**sc delete \<service_name\>**|
