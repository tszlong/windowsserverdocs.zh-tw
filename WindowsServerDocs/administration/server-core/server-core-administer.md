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
ms.sourcegitcommit: 4df1bc940338219316627ad03f0525462a05a606
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/19/2018
ms.locfileid: "8977995"
---
# 管理 Server Core 伺服器

>適用於：Windows Server (半年度管道) 和 Windows Server 2016

Server Core 沒有 UI，因為您需要使用 Windows PowerShell cmdlet、 命令列工具或遠端工具來執行基本的系統管理工作。 下列各節概述的 PowerShell cmdlet 和命令用於基本的工作。 您也可以使用[Windows Admin Center](../../manage/windows-admin-center/overview.md)，目前在公用預覽，統一的管理入口網站來管理您的安裝。 

## 使用 PowerShell cmdlet 的系統管理工作
您可以使用下列資訊來執行基本的系統管理工作，使用 Windows PowerShell cmdlet。

### 設定靜態 IP 位址
當您安裝 Server Core 伺服器時，預設就會有 A DHCP 位址。 如果您需要為靜態 IP 位址，您可以設定它使用下列步驟。

若要檢視您目前的網路設定，請使用**Get NetIPConfiguration**。

若要檢視您已經在使用的 IP 位址，請使用**Get-netipaddress**。

若要設定靜態 IP 位址，執行下列動作： 

1. 執行**Get NetIPInterface**。 
2. 請注意您 IP 的介面或**InterfaceDescription**字串的**IfIndex**欄中的數字。 如果您有多個網路介面卡，請注意的數字或字串對應到您想要設定的靜態 IP 位址的介面。
3. 執行下列 cmdlet 來設定靜態 IP 位址：

   ```powershell
   New-NetIPaddress -InterfaceIndex 12 -IPAddress 192.0.2.2 -PrefixLength 24 -DefaultGateway 192.0.2.1
   ```

   其中：
   - **InterfaceIndex**是**IfIndex**與步驟 2 的值。 （在我們的範例，12）
   - **IPAddress**是您想要設定的靜態 IP 位址。 （在範例中，191.0.2.2）
   - **PrefixLength**是您要設定的 IP 位址的首碼長度 （另一個表單的子網路遮罩）。 （如我們的範例中，24）
   - **DefaultGateway**是預設閘道 IP 位址。 （如我們的範例中，192.0.2.1）
4. 若要設定的 DNS 用戶端伺服器位址，請執行下列 cmdlet: 

   ```powershell
   Set-DNSClientServerAddress –InterfaceIndex 12 -ServerAddresses 192.0.2.4
   ```
   
   其中：
   - **InterfaceIndex**是 IfIndex 與步驟 2 的值。
   - **ServerAddresses**是您的 DNS 伺服器的 IP 位址。
5. 若要新增多個 DNS 伺服器，請執行下列 cmdlet: 

   ```powershell
   Set-DNSClientServerAddress –InterfaceIndex 12 -ServerAddresses 192.0.2.4,192.0.2.5
   ```

   其中，在此範例中， **192.0.2.4**和**192.0.2.5**是這兩個 IP 位址的 DNS 伺服器。

如果您需要切換成使用 DHCP，執行**組 DnsClientServerAddress – InterfaceIndex 12 – ResetServerAddresses**。

### 加入網域
您可以使用下列 cmdlet，將電腦加入網域。

1. 執行**新增電腦**。 將會提示您提供這兩個認證以加入網域和網域名稱。
2. 如果您需要將網域使用者帳戶新增到本機 Administrators 群組，請在命令提示字元 （不在 PowerShell 視窗） 中執行下列命令：

   ```
   net localgroup administrators /add <DomainName>\<UserName>
   ```
3. 重新啟動電腦。 您可以執行**重新啟動電腦**。

### 重新命名伺服器
您可以使用下列步驟來重新命名伺服器。

1. 判斷目前的**主機名稱**或**ipconfig**命令的伺服器名稱。
2. 執行**重新命名電腦-ComputerName \<new_name\ >**。
3. 重新啟動電腦。

### 啟動伺服器

執行**slmgr.vbs – ipk\<productkey\ >**。 然後執行**slmgr.vbs /ato –**。 如果啟動成功，您將不會收到的訊息。

> [!NOTE]
> 您也可以啟用伺服器透過電話，使用[金鑰管理服務 (KMS) 伺服器](../../get-started/server-2016-activation.md)，或從遠端。 若要從遠端啟用，請從遠端電腦，執行下列 cmdlet: 

>```powershell
>**cscript windows\system32\slmgr.vbs <ServerName> <UserName> <password>:-ato**
>```
 
### 設定 Windows 防火牆

您可以使用 Windows PowerShell cmdlet 和指令碼的 Server Core 電腦上的本機設定 Windows 防火牆。 您可以使用它來設定 Windows 防火牆的 cmdlet，請參閱[NetSecurity](/powershell/module/netsecurity/?view=win10-ps) 。

### 啟用 Windows PowerShell 遠端執行功能

您可以啟用 Windows PowerShell 遠端執行功能，在其中命令型別在 Windows PowerShell 中執行另一部電腦上的一部電腦。 啟用與**Enable-psremoting**的 Windows PowerShell 遠端執行功能。

如需詳細資訊，請參閱[有關遠端常見問題集](/powershell/module/microsoft.powershell.core/about/about_remote_faq?view=powershell-5.1)
</br>

## 從命令列的系統管理工作
您可以使用下列的參考資訊，從命令列執行系統管理工作。

### 設定及安裝
|工作 | 命令 |
|-----|-------|
|設定本機系統管理員密碼| **淨使用者系統管理員** \* |
|將電腦加入網域| **netdom 加入 %computername%****/domain:\<domain\ > /userd:\<domain\\username\ > /passwordd:**\* <br> 重新啟動電腦。|
|確認已變更網域| **set** |
|從網域移除電腦|**netdom 移除 \ < computername\ >**| 
|將使用者新增到本機系統管理員群組|**net 熟知的系統管理員 / 新增 \ < domain\\username\ >** |
|移除使用者從本機系統管理員群組|**淨熟知的系統管理員 /delete \ < domain\\username\ >** |
|將使用者新增到本機電腦|**淨使用者 \ < domain\username\ > * / 新增** |
|新增群組到本機電腦|**淨熟知 \ < 群組名稱 \ > / 新增**|
|變更已加入網域的電腦名稱|**netdom renamecomputer %computername%/NewName:\<new 電腦名稱 \ > /userd:\<domain\\username\ > /passwordd:** * |
|確認新的電腦名稱|**set**| 
|變更工作群組中的電腦的名稱|**netdom renamecomputer \ < currentcomputername\ > /NewName: \ < newcomputername\ >** <br>重新啟動電腦。|
|停用分頁檔案管理|**wmic 電腦系統的位置名稱 ="< computername\ >"設定 AutomaticManagedPagefile = False**| 
|設定分頁檔|**wmic pagefileset 其中名稱 ="< 路徑/filename\ >"設定 InitialSize = \ < initialsize\ >，MaximumSize = \ < maxsize\ >** <br>*檔案路徑/名稱*所在的路徑和*initialsize*分頁檔的名稱是開始分頁檔，位元組為單位的大小而*maxsize*是分頁檔，以位元組為單位的大小上限。|
|變更為靜態 IP 位址|**ipconfig/所有** <br>記錄的相關資訊，或將它重新導向至文字檔案 (**ipconfig/所有 > ipconfig.txt**)。<br>**netsh 介面 ipv4 顯示介面**<br>確認介面清單。<br>**netsh 介面 ipv4 設定位址名稱 \ < 識別碼從介面 list\ > 來源 = 靜態位址 = \ < 慣用的 IP \ [email address\ > 閘道 = \ < 閘道 \ [email address\ >**<br>執行**ipconfig/所有**到 vierfy DHCP 功能已啟用已設定為**否**。|
|設定靜態的 DNS 位址。|**netsh 介面 ipv4 新增 dnsserver 名稱 \<name 或網路介面 card\ ID = > 位址 = \<IP 地址的主要 DNS server\ > 索引 = 1 <br> **netsh 介面 ipv4 新增 dnsserver 名稱 = \<name 的次要 DNS server\ >地址 = 次要 DNS server\ \<IP 位址 > 索引 = 2 \] * * <br> 重複執行適當地新增額外的伺服器。<br>執行**ipconfig/所有**來確認位址是正確的。|
|從靜態 IP 位址變更提供 DHCP IP 位址|**netsh 介面 ipv4 設定位址名稱 = \ < 本機層中的 IP 位址 > 來源 = DHCP** <br>執行**Ipconfig/所有**驗證 DCHP 啟用是否已設定為 **[是]**。|
|輸入產品金鑰|**slmgr.vbs – ipk \ < 產品 key\ >**| 
|啟動本機伺服器|**slmgr.vbs /ato**| 
|從遠端啟動伺服器|**cscript slmgr.vbs – ipk \ < 產品 key\ > \ < 伺服器名稱 \ > \ < username\ > \ < password\ >** <br>**cscript slmgr.vbs /ato \ < servername\ > \ < username\ > \ < password\ >** <br>透過執行，以取得電腦的 GUID **cscript slmgr.vbs-未** <br> 執行**cscript slmgr.vbs-dli \<GUID\ >** <br>驗證授權狀態會設定為**已授權 （啟用）**。


### 網路功能及防火牆

|工作|命令| 
|----|-------|
|設定您要使用 proxy 伺服器的伺服器|**netsh Winhttp 設定 proxy \ < servername\ >: \ < 連接埠 \<sequence >** <br>**注意：** Server Core 安裝無法存取網際網路，透過都需要密碼來允許連線的 proxy。|
|設定您的伺服器來略過 proxy 的網際網路位址|**netsh winttp 設定 proxy \ < servername\ >: \ < 連接埠 \<sequence > 略過清單 ="< 不可以 >"**| 
|顯示或修改 IPSEC 設定|**netsh ipsec**| 
|顯示或修改 NAP 設定|**netsh nap**| 
|顯示或修改實際地址翻譯的 IP|**arp**| 
|顯示或設定的本機的路由表|**路由**| 
|檢視或設定 DNS 伺服器設定|**nslookup**| 
|顯示通訊協定統計資料和目前的 TCP/IP 網路連線|**netstat**| 
|顯示通訊協定統計資料和目前的 TCP/IP 連線使用 NetBIOS 透過 TCP/IP (NBT)|**nbtstat**| 
|顯示躍點變得網路連線|**pathping**| 
|追蹤躍點變得網路連線|**tracert**| 
|顯示多點傳送的路由器的設定|**mrinfo**| 
|啟用遠端管理的防火牆|**netsh advfirewall 防火牆設定規則群組 = 「 Windows 防火牆遠端管理 」 的新啟用 = 是**| 
 

### 更新、 錯誤報告，和意見反應

|工作|命令| 
|----|-------|
|安裝更新|**wusa \ < update\ >.msu /quiet**| 
|清單已安裝的更新|**systeminfo**| 
|移除更新|**展開 /f:\* \ < update\ >.msu c:\test** <br>瀏覽至 c:\test\] 和 [開啟 \ < update\ >.xml 在文字編輯器中的。<br>取代**安裝****移除**，並儲存檔案。<br>**pkgmgr /n:\ < update\ >.xml**|
|設定自動更新|若要確認目前的設定: * * cscript %systemroot%\system32\scregedit.wsf /AU /v * *<br>若要啟用自動更新： **cscript scregedit.wsf /AU 4** <br>若要停用自動更新： **cscript %systemroot%\system32\scregedit.wsf /AU 1**| 
|啟用錯誤報告|若要確認目前的設定： **serverWerOptin /query** <br>若要自動傳送詳細的報告： **serverWerOptin / 詳細** <br>若要自動傳送摘要報告： **serverWerOptin /summary** <br>若要停用錯誤報告： **serverWerOptin /disable**|
|參與 「 客戶經驗改進計畫 (CEIP)|若要確認目前的設定： **serverCEIPOptin /query** <br>若要啟用 CEIP: **serverCEIPOptin 情況下** <br>若要停用 CEIP: **serverCEIPOptin /disable**|

### 服務、 處理程序和效能

|工作|命令| 
|----|-------|
|列出正在執行的服務|**sc 查詢**或**net start**| 
|啟動服務|**sc 開始 \<service name\ >** 或**net 開始 \<service name\ >**| 
|停止服務|**sc stop \<service name\ >** 或**net stop \<service name\ >**| 
|擷取執行應用程式及相關聯的處理程序的清單|**Tasklist**||強制停止處理程序|執行**tasklist**擷取的程序識別碼 (PID)，然後執行**taskkill /PID \<process ID\ >**|
|啟動工作管理員|**taskmgr**| 
|建立和管理事件追蹤工作階段和效能記錄檔|若要建立計數器、 追蹤、 設定資料收集或 API: **logman ceate** <br>查詢的資料收集器屬性： **logman 查詢** <br>開始或停止資料收集： **logman start\ | 停止** <br>若要刪除的行程： **logman 刪除** <br> 若要更新收集器的屬性： **logman 更新** <br>若要從 XML 檔案匯入資料收集器集，或將它匯出為 XML 檔案： **logman import\ | 匯出**|

### 事件記錄檔

|工作|命令| 
|----|-------|
|清單事件記錄檔|**wevtutil el**| 
|查詢中指定的記錄檔的事件|**wevtutil qe /f:text \ < 記錄名稱 \ >**| 
|匯出事件記錄檔|**wevtutil epl \ < 記錄名稱 \ >**| 
|清除事件記錄檔|**wevtutil cl \ < 記錄名稱 \ >**| 


### 磁碟和檔案系統

|工作|命令|
|----|-------|
|管理磁碟分割|如需命令的完整清單，請執行**diskpart /？**|  
|管理軟體 RAID|如需命令的完整清單，請執行**diskraid /？**|  
|管理磁碟區掛接點|如需命令的完整清單，請執行**mountvol /？**| 
|重組磁碟區|如需命令的完整清單，請執行**磁碟重組 /？**|  
|轉換成 NTFS 檔案系統的磁碟區|**將轉換 \ < 磁碟區 letter\ > /FS:NTFS**| 
|壓縮的檔案|如需命令的完整清單，請執行**壓縮 /？**|  
|管理開啟的檔案|如需命令的完整清單，請執行**openfiles /？**|  
|管理 VSS 資料夾|如需命令的完整清單，請執行**vssadmin /？**| 
|管理檔案系統|如需命令的完整清單，請執行**fsutil /？**||確認檔案的簽章|**sigverif /？**| 
|取得檔案或資料夾的擁有權|如需命令的完整清單，請執行**icacls /？**| 
 
### 硬體

|工作|命令| 
|----|-------|
|新增新的硬體裝置的驅動程式|將驅動程式複製到在 %homedrive%\\\ < 驅動程式 folder\ > 資料夾。 執行**pnputil-i-%homedrive%\\\<driver folder\ > \\\<driver\ >.inf**|
|移除硬體裝置的驅動程式|如需載入的驅動程式清單，請執行**sc 查詢類型 = 驅動程式**。 然後執行**sc 刪除 \<service_name\ >**|
