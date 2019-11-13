---
title: nfsadmin
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7375b2cf-c6b8-45b5-abf6-6c10e462defd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2658cf610e4328d382b9224f4230d68a022d1cc3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373228"
---
# <a name="nfsadmin"></a>nfsadmin

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

您可以使用**nfsadmin**來管理 SERVER for Nfs 和 CLIENT for nfs。  
  
## <a name="syntax"></a>語法  
**nfsadmin 伺服器**`[`*computerName*`] [`\-u 使用者*名稱*`[`\-p*密碼*`]]` \-l  
  
**nfsadmin 伺服器**`[`*computerName*`] [`\-u 使用者*名稱*`[`\-p*密碼*`]]` \-r `{`*用戶端*`|` 全部`}`  
  
**nfsadmin 伺服器**`[`*computerName*`] [`\-u 使用者*名稱*`[`\-p*密碼*`]] {`開始 `|` 停止`}`  
  
**nfsadmin 伺服器**`[`*computerName*`] [`\-u 使用者*名稱*`[`\-p*密碼*`]]` config*選項*`[...]`  
  
**nfsadmin 伺服器**`[`*computerName*`] [`\-u *UserName* `[`\-p *Password*`]]` creategroup *Name*  
  
**nfsadmin 伺服器**`[`*computerName*`] [`\-u 使用者*名稱*`[`\-p*密碼*`]]` listgroups  
  
**nfsadmin 伺服器**`[`*computerName*`] [`\-u *UserName* `[`\-p *Password*`]]` deletegroup *Name*  
  
**nfsadmin 伺服器**`[`*computerName*`] [`\-u 使用者*名稱*`[`\-p*密碼*`]]` renamegroup *OldName NewName*  
  
**nfsadmin server** `[`*computerName*`] [`\-u *UserName* `[`\-p *Password*`]]` addmembers *Name Host*`[...]`  
  
**nfsadmin 伺服器**`[`*computerName*`] [`\-u 使用者*名稱*`[`\-p*密碼*`]]` listmembers  
  
**nfsadmin 伺服器**`[`*computerName*`] [`\-u 使用者*名稱*`[`\-p*密碼*`]]` deletemembers*群組主機*`[...]`  
  
**nfsadmin 用戶端**`[`*computerName*`] [`\-u 使用者*名稱*`[`\-p*密碼*`]] {`開始 `|` 停止`}`  
  
**nfsadmin 用戶端**`[`*computerName*`] [`\-u 使用者*名稱*`[`\-p*密碼*`]]` config*選項*`[...]`  
  
## <a name="description"></a>描述  
**Nfsadmin**命令\-line 公用程式會在執行 Microsoft Services For Network File SYSTEM \(NFS\)的本機或遠端電腦上，管理 Server for Nfs 或 CLIENT for nfs。 如果您使用沒有必要許可權的帳戶登入，則可以指定執行之帳戶的使用者名稱和密碼。 **Nfsadmin**所執行的動作取決於您提供的命令引數。  
  
除了服務\-特定的命令引數和選項之外， **nfsadmin**也接受下列各項：  
  
*名稱*  
指定您想要管理的遠端電腦。 您可以使用 Windows 網際網路名稱服務 \(WINS\) 名稱或網域名稱系統 \(DNS\) 名稱，或透過網際網路通訊協定 \(IP\) 位址來指定電腦。  
  
**\-u**使用者*名稱*  
指定要使用其認證之使用者的使用者名稱。 可能需要將功能變數名稱新增至格式為<em>domain</em> **\\** <em>UserName</em>的使用者名稱  
  
**\-p** *密碼*  
指定使用 **\-u**選項指定之使用者的密碼。 如果您指定 [ **\-u** ] 選項，但省略 [ **\-p** ] 選項，系統會提示您輸入使用者的密碼。  
  
#### <a name="administering-server-for-nfs"></a>管理 Server for NFS  
使用**nfsadmin server**命令來管理 SERVER for NFS。 **Nfsadmin 伺服器**所採取的特定動作取決於您指定的命令選項或引數：  
  
**\-l**  
列出用戶端持有的所有鎖定。  
  
**\-r** {*client* | **all**}  
釋放*用戶端*所持有的鎖定，如果所有用戶端已指定**all** ，則為。  
  
**開始**  
啟動 Server for NFS 服務。  
  
**stop**  
停止 Server for NFS 服務。  
  
**web.config**  
指定 Server for NFS 的一般設定。 您必須使用**config**命令引數提供下列其中一個選項：  
  
**mapsvr\=** <em>伺服器</em>  
將*伺服器*設定為 SERVER for NFS 的消費者名稱對應伺服器。 雖然此選項會繼續支援以符合舊版的相容性，但您應該改用**sfuadmin**公用程式。  
  
**auditlocation\=** {**eventlog** | **檔案** | **都**是 | **none**}  
指定是否要審核事件以及記錄事件的位置。 需要下列其中一個引數。  
  
**eventlog**  
指定只會將已審核的事件記錄在事件檢視器應用程式記錄檔中。  
  
**文字檔**  
指定只會將已審核的事件記錄在**config fname**所指定的檔案中。  
  
**既**  
指定已審核的事件將會記錄在事件檢視器應用程式記錄檔中，以及**config fname**所指定的檔案中。  
  
**無**  
指定不會審核事件。  
  
**fname\=** 檔案  
將檔案所指定的檔案*設定為 audit*檔案。 預設值為% sfudir%\\log\\nfssvr  
  
**fsize\=** \=*大小*  
將 [*大小*] 設定為 audit 檔案的大小上限（以 mb 為單位）。 預設的大小上限為 7 MB。  
  
**audit\=** \[ **\+** | **\-** \]**掛接**\[ **\+** | **\-** \]**閱讀**\[ **\+|\-** \]**寫入**\[ **\+** | **\-** \]**建立**\[ **\+** | **\-\]** **刪除**\[ **\+** | **\-** \]**鎖定**\[ **\+|\-** \]**全部**  
指定要記錄的事件。 若要開始記錄事件，請在事件名稱前面輸入加號 \( **\+** \);若要停止記錄事件，請在事件名稱前面輸入減號 \( **\-** \)。 如果省略正負號，則會假設為加號。 請勿將**all**與任何其他事件名稱一起使用。  
  
**lockperiod\=** <em>秒</em>  
指定 Server for nfs 的連線遺失後，或在 Server for nfs 服務重新開機之後，伺服器會等待回收鎖定的秒數。  
  
Portmapprotocol\={TCP |UDP |TCP\+UDP  
指定 Portmap 支援的傳輸通訊協定。 預設設定為 [ **TCP\+UDP**]。  
  
mountprotocol\={TCP |UDP |TCP\+UDP}  
指定掛接支援的傳輸通訊協定。 預設設定為 [ **TCP\+UDP**]。  
  
nfsprotocol\={TCP |UDP |TCP\+UDP}  
指定 \(NFS\) 支援的傳輸通訊協定網路檔案系統。 預設設定為**TCP\+UDP**  
  
nlmprotocol\={TCP |UDP |TCP\+UDP}  
指定網路鎖定管理員 \(NLM\) 支援的傳輸通訊協定。 預設設定為 [ **TCP\+UDP**]。  
  
nsmprotocol\={TCP |UDP |TCP\+UDP}  
指定 \(NSM\) 支援的傳輸通訊協定網路狀態管理員。 預設設定為 [ **TCP\+UDP**]。  
  
**enableV3\=** {**yes** | **否**}  
指定是否支援 NFS 第3版的通訊協定。 預設設定為 **[是]** 。  
  
**renewauth\=** {**yes** | **否**}  
指定在**config renewauthinterval**指定的期間之後，是否需要重新驗證用戶端連接。 預設設定為 [**否**]。  
  
**renewauthinterval\=** <em>秒</em>  
指定如果設定**renewauth**設為 **[是]** ，則在強制重新驗證用戶端之前經過的秒數。 預設值為600秒。  
  
**dircache\=** <em>大小</em>  
指定目錄快取的大小（以 kb 為單位）。 指定為*size*的數位必須是4到128之間的4倍數。 預設目錄\-快取大小為 128 KB。  
  
**translationfile**\=\[檔\]  
指定一個檔案，其中包含在將檔案從 Windows\-移至以 UNIX\-為基礎的檔案系統時，用來取代檔案名稱中之字元的對應資訊。 如果未指定*file* ，則會停用檔案名字元轉譯。 如果**translationfile**的值已變更，您必須重新開機伺服器，變更才會生效。  
  
**dotfileshidden**\={**yes** | **否**}  
指定以名稱開頭的檔案是否 \(。\) 在 Windows 檔案系統中會標示為隱藏，因此會在 NFS 用戶端中隱藏。 預設設定為 [**否**]。  
  
**casesensitivelookups\=** {**yes** | **否**}  
指定目錄查閱是否會區分大小寫，\(需要完全符合字元大小寫\)。  
  
您也需要停用 Windows 核心案例\-不區分，讓 Server for NFS 支援區分大小寫\-機密檔案名。 您可以將下列登錄機碼清除為0，以停用 Windows 核心案例\-不區分大小寫：  
  
HKLM\\SYSTEM\\CurrentControlSet\\控制\\會話管理員\\核心  
  
DWOrd obcaseinsensitive   
  
> [!IMPORTANT]  
> 本節僅適用于 Windows Server 2008 R2、Windows Server 2008 和 Windows Server 2003。 本節不適用於 Windows Server 2012 R2 或 Windows Server 2012。  
  
**ntfscase\=** {**lower** | **上限** | **保留**}  
指定是否要以小寫、大寫或儲存在目錄中的格式，傳回 NTFS 檔案系統中檔案名稱的字元大小寫。 預設設定為 [**保留**]。 如果 [ **casesensitivelookups** ] 設定為 **[是]** ，則無法變更此設定。  
  
**creategroup** *名稱*  
建立新的用戶端群組，並提供指定的*名稱*。  
  
**listgroups**  
顯示所有用戶端群組的名稱。  
  
**deletegroup** *名稱*  
移除 [*名稱*] 所指定的用戶端群組。  
  
**renamegroup** *OldName NewName*  
將*OldName*所指定的用戶端群組名稱變更為*NewName*  
  
**addmembers** *名稱主機*\[...\]  
將*主機*新增至 [*名稱*] 所指定的用戶端群組。  
  
**listmembers** *名稱*  
列出依 [*名稱*] 指定之用戶端群組中的主機電腦。  
  
**deletemembers** *群組主機*\[...\]  
從*group*指定的用戶端群組移除*主機*所指定的用戶端。  
  
如果您未指定命令選項或引數， **nfsadmin 伺服器**會顯示目前的 NFS 伺服器設定設定。  
  
#### <a name="administering-client-for-nfs"></a>管理 NFS 用戶端  
使用**nfsadmin 客戶**端命令來管理 NFS 用戶端。 **Nfsadmin 用戶端**所採取的特定動作取決於您指定的命令引數：  
  
**開始**  
啟動 Client for NFS 服務。  
  
**stop**  
停止 NFS 用戶端服務。  
  
**web.config**  
指定 Client for NFS 的一般設定。 您必須使用**config**命令引數提供下列其中一個選項：  
  
**fileaccess\=** <em>模式</em>  
-   指定在網路檔案系統 \(NFS\) 伺服器上建立之檔案的預設許可權模式。 *Mode*引數是由0到7的三個數字組成，\(內含\) 代表授與使用者、群組和其他 \(分別\)的預設許可權。 數位會轉譯為 UNIX\-樣式許可權，如下所示： 0\=無，1\=x，2\=w，3\=wx，4\=r，5\=rx，6\=rw 和 7\=rwx。 例如， **fileaccess\=750**提供擁有者的 rwx 許可權、對群組的 rx 許可權，以及對其他人沒有存取權限。  
  
**mapsvr\=** <em>伺服器</em>  
將*伺服器*設定為 CLIENT for NFS 的消費者名稱對應伺服器。 雖然此選項會繼續支援以符合舊版的相容性，但您應該改用**sfuadmin**公用程式。  
  
**mtype\=** {**hard** | **軟**性}  
指定預設掛接類型。 若是硬掛接，NFS 用戶端會繼續重試失敗的 RPC，直到成功為止。 若為軟掛接，則在重試呼叫由**重試**選項指定的次數之後，NFS 用戶端會傳回呼叫應用程式失敗。  
  
**重試\=** <em>號碼</em>  
指定嘗試建立軟掛接連接的次數。 這個值必須介於1到10（含）之間。 預設值為 1。  
  
**timeout\=** <em>秒</em>  
指定等候連接 \(遠端程序呼叫\)的秒數。 這個值必須是0.8、0.9 或1到60（含）之間的整數。 預設值為0.8。  
  
**通訊協定\={TCP |UDP |TCP\+UDP}**  
指定用戶端支援的傳輸通訊協定。 預設設定為**TCP\+UDP**  
  
**rsize\=** <em>大小</em>  
指定讀取緩衝區的大小（以 kb 為單位）。 此值可以是0.5、1、2、4、8、16或32。 預設值為32。  
  
**wsize\=** <em>大小</em>  
指定寫入緩衝區的大小（以 kb 為單位）。 此值可以是0.5、1、2、4、8、16或32。 預設值為32。  
  
**效能\=預設值**  
將下列效能設定還原為預設值：  
  
-   **mtype**  
  
-   **再**  
  
-   **timeout**  
  
-   **rsize**  
  
-   **wsize**  
  
**fileaccess\=** <em>模式</em>  
指定在網路檔案系統 \(NFS\) 伺服器上建立之檔案的預設許可權模式。 *Mode*引數是由0到7的三個數字組成，\(內含\) 代表授與使用者、群組和其他 \(分別\)的預設許可權。 數位會轉譯為 UNIX\-樣式許可權，如下所示： 0\=無，1\=x，2\=w，3\=wx，4\=r，5\=rx，6\=rw 和 7\=rwx。 例如， **fileaccess\=750**提供擁有者的 rwx 許可權、對群組的 rx 許可權，以及對其他人沒有存取權限。  
  
如果您未指定命令選項或引數， **nfsadmin 用戶端**會顯示目前的 NFS 用戶端設定。  
  

