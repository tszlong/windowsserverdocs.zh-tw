---
title: nfsadmin
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 955f6d93379802444d542ea571f98b69b9191f5c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837109"
---
# <a name="nfsadmin"></a>nfsadmin

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

您可以使用**nfsadmin**來管理 Server for NFS 和 Client for NFS。  
  
## <a name="syntax"></a>語法  
**nfsadmin server** `[` *computerName*`] [`\-u *UserName*`[`\-p *密碼*`]]` \-l  
  
**nfsadmin server** `[` *computerName*`] [`\-u *UserName* `[` \-p *密碼*`]]` \-r `{`*用戶端*`|`所有`}`  
  
**nfsadmin server** `[` *computerName*`] [`\-u *UserName* `[` \-p *密碼*`]] {`啟動`|`停止`}`  
  
**nfsadmin server** `[` *computerName*`] [`\-u *UserName* `[` \-p *密碼*`]]` config*選項*`[...]`  
  
**nfsadmin server** `[` *computerName*`] [`\-u *UserName* `[` \-p *密碼*`]]` creategroup*名稱*  
  
**nfsadmin server** `[` *computerName*`] [`\-u *UserName* `[` \-p *密碼*`]]` listgroups  
  
**nfsadmin server** `[` *computerName*`] [`\-u *UserName* `[` \-p *密碼*`]]` deletegroup*名稱*  
  
**nfsadmin server** `[` *computerName*`] [`\-u *UserName* `[` \-p *密碼*`]]` renamegroup *OldName NewName*  
  
**nfsadmin server** `[` *computerName*`] [`\-u *UserName* `[` \-p *密碼*`]]` addmembers*主機名稱*`[...]`  
  
**nfsadmin server** `[` *computerName*`] [`\-u *UserName* `[` \-p *密碼*`]]` listmembers  
  
**nfsadmin server** `[` *computerName*`] [`\-u *UserName* `[` \-p *密碼*`]]` deletemembers*群組主機*`[...]`  
  
**nfsadmin client** `[` *computerName*`] [`\-u *UserName* `[` \-p *密碼*`]] {`啟動`|`停止`}`  
  
**nfsadmin client** `[` *computerName*`] [`\-u *UserName* `[` \-p *密碼*`]]` config*選項*`[...]`  
  
## <a name="description"></a>描述  
**Nfsadmin**命令\-列公用程式 Server for NFS 或 Client for NFS 管理本機或遠端電腦上執行 Microsoft Services for Network File System \(NFS\)。 如果您沒有必要的權限的帳戶登入，您可以指定的使用者名稱和密碼的帳戶。 所執行的動作**nfsadmin**取決於您所提供的命令引數。  
  
除了服務之外\-特定的命令列引數和選項， **nfsadmin**接受下列：  
  
*computerName*  
指定您想要管理的遠端電腦。 您可以指定使用 Windows 網際網路名稱服務的電腦\(WINS\)名稱或網域名稱系統\(DNS\)名稱，或網際網路通訊協定\(IP\)位址。  
  
**\-u** *UserName*  
指定所要使用的認證使用者的使用者名稱。 可能需要將網域名稱新增至表單中的使用者名稱*網域***\\***使用者名稱*  
  
**\-p** *密碼*  
指定使用指定之使用者的密碼 **\-u**選項。 如果您指定 **\-u**選項，但是省略 **\-p**選項，系統會提示輸入使用者的密碼。  
  
#### <a name="administering-server-for-nfs"></a>管理 Server for NFS  
使用**nfsadmin server**命令來管理 Server for NFS。 特定的動作所**nfsadmin server**時間取決於您所指定的引數的命令選項：  
  
**\-l**  
列出所有用戶端所保留的鎖定。  
  
**\-r** {*client* | **all**}  
釋放所持有的鎖定*用戶端*或者，如果**所有**指定，則所有用戶端。  
  
**start**  
啟動 Server for NFS 服務。  
  
**stop**  
停止 Server for NFS 服務。  
  
**config**  
指定 Server for NFS 的一般設定。 您必須提供至少其中一個選項搭配**config**命令引數：  
  
**mapsvr\=***server*  
設定組*server*為 Server for NFS 的使用者名稱對應伺服器。 雖然此選項將會繼續支援與舊版相容，您應該使用**sfuadmin**公用程式改為。  
  
**auditlocation\=**{**eventlog** | **file** | **both** | **none**}  
指定是否將稽核事件，並會記錄事件。 其中一個下列的引數是必要項目。  
  
**eventlog**  
指定只在事件檢視器應用程式記錄檔，將會記錄稽核的事件。  
  
**file**  
指定記錄稽核的事件，只有在所指定的檔案**config fname**。  
  
**both**  
指定稽核的事件將會記錄在事件檢視器應用程式記錄檔，以及所指定的檔案**config fname**。  
  
**none**  
指定將不會稽核事件。  
  
**fname\=***file*  
設定所指定的檔案*檔案*為稽核檔案。 預設值是 %sfudir\\記錄\\nfssvr.log  
  
**fsize\=**\=*size*  
設定組*大小*最大的大小，以 mb 為單位的稽核檔案。 預設大小上限為 7 MB。  
  
**稽核\=**\[**\+**|**\-**\]**掛接** \[**\+** | **\-** \]**讀取** \[ **\+**| **\-** \]**撰寫** \[ **\+** | **\-**  \]**建立** \[ **\+** | **\-** \]**刪除** \[ **\+** | **\-** \]**鎖定** \[ **\+** | **\-** \]**所有**  
指定要記錄的事件。 若要開始記錄的事件，請輸入一個加號\( **\+** \)之前的事件名稱; 若要停止記錄事件中，輸入減號\( **\-**\)事件名稱前面。 如果省略符號，則會假設加號。 請勿使用**所有**與任何其他的事件名稱。  
  
**lockperiod\=***seconds*  
指定 Server for NFS 的 NFS 伺服器的連線已遺失並再重新建立之後，或重新啟動 Server for NFS 服務之後回收鎖定等候的秒數。  
  
Portmapprotocol\={TCP |UDP |TCP\+UDP  
指定連接埠對應支援哪些傳輸通訊協定。 預設值是**TCP\+UDP**。  
  
mountprotocol\={TCP | UDP | TCP\+UDP}  
指定的傳輸通訊協定掛接支援。 預設值是**TCP\+UDP**。  
  
nfsprotocol\={TCP | UDP | TCP\+UDP}  
指定的傳輸通訊協定的網路檔案系統\(NFS\)支援。 預設值是**TCP\+UDP**  
  
nlmprotocol\={TCP | UDP | TCP\+UDP}  
指定的傳輸通訊協定網路鎖定管理員\(NLM\)支援。 預設值是**TCP\+UDP**。  
  
nsmprotocol\={TCP | UDP | TCP\+UDP}  
指定的傳輸通訊協定的網路狀態管理員\(NSM\)支援。 預設值是**TCP\+UDP**。  
  
**enableV3\=**{**yes** | **no**}  
指定是否將支援 NFS 版本 3 通訊協定。 預設值是**是**。  
  
**renewauth\=**{**yes** | **沒有**}  
指定用戶端連線是否會需要重新驗證所指定的期間之後**config renewauthinterval**。 預設值是**沒有**。  
  
**renewauthinterval\=***seconds*  
指定用戶端會強制重新驗證，如果之前經過的秒數**config renewauth**設為**是**。 預設值為 600 秒。  
  
**dircache\=***size*  
指定的大小，以 kb 為單位的目錄快取。 指定數字*大小*必須介於 4 到 128 之間的 4 的倍數。 預設目錄\-快取大小為 128 KB。  
  
**translationfile**\=\[file\]  
指定檔案，其中包含對應資訊取代的檔案名稱中的字元時將它們從 Windows 移\-到 UNIX\-基礎檔案系統。 如果*檔案*未指定，就會停用檔案名稱字元轉譯。 如果值**translationfile**會變更，您必須重新啟動伺服器，變更才會生效。  
  
**dotfileshidden**\={**yes** | **no**}  
指定檔案是否建立名稱以句號開頭\(。\)會標示為隱藏 Windows 檔案系統中，並因此的 NFS 用戶端隱藏。 預設值是**沒有**。  
  
**casesensitivelookups\=**{**yes** | **沒有**}  
指定是否會區分大小寫目錄查閱\(需要完全相符的字元大小寫\)。  
  
您也需要停用 Windows 核心案例\-為了讓 Server for NFS 才能支援案例的不區分大小寫\-敏感的檔案名稱。 您可以停用 Windows 核心案例\-藉由清除下列登錄機碼設為 0 的不區分大小寫：  
  
HKLM\\SYSTEM\\CurrentControlSet\\Control\\Session Manager\\kernel  
  
DWOrd obcaseinsensitive   
  
> [!IMPORTANT]  
> 本節僅適用於 Windows Server 2008 R2、 Windows Server 2008 和 Windows Server 2003。 本節不適用於 Windows Server 2012 R2 或 Windows Server 2012。  
  
**ntfscase\=**{**lower** | **upper** | **preserve**}  
指定是否會以小寫字母、 大寫或儲存在目錄中的表單中傳回的 NTFS 檔案系統中的檔案名稱中的字元大小寫。 預設值是**保留**。 無法變更此設定，如果**casesensitivelookups**設為**是**。  
  
**creategroup** *name*  
建立新的用戶端群組，並賦予它指定*名稱*。  
  
**listgroups**  
顯示所有的用戶端群組的名稱。  
  
**deletegroup** *name*  
移除所指定的用戶端群組*名稱*。  
  
**renamegroup** *OldName NewName*  
所指定的用戶端群組的名稱變更*OldName*到*NewName*  
  
**addmembers** *名稱主機*\[...\]  
新增*主機*所指定的用戶端群組*名稱*。  
  
**listmembers** *名稱*  
列出所指定的用戶端群組中的主機電腦*名稱*。  
  
**deletemembers** *群組主機*\[...\]  
移除指定的用戶端*主機*從所指定的用戶端群組*群組*。  
  
如果您未指定的命令選項或引數**nfsadmin server**會顯示目前的 Server for NFS 組態設定。  
  
#### <a name="administering-client-for-nfs"></a>管理 NFS 用戶端  
使用**nfsadmin client**管理 NFS 用戶端的命令。 特定的動作所**nfsadmin client**時間取決於您所指定的命令引數：  
  
**start**  
啟動 Client for NFS 服務。  
  
**stop**  
停止 Client for NFS 服務。  
  
**config**  
指定 NFS 用戶端的一般設定。 您必須提供至少其中一個選項搭配**config**命令引數：  
  
**fileaccess\=***mode*  
-   指定網路檔案系統上建立檔案的預設權限模式\(NFS\)伺服器。 *模式*引數包含三個位數，範圍介於 0 到 7\(內含\)表示的預設權限授與使用者、 群組和其他\(分別\)。 數字會轉譯為 UNIX\-樣式權限，如下所示：0\=無、 1\=x，2\=w、 3\=wx，4\=r、 5\=rx，6\=rw，而 7\=rwx。 例如， **fileaccess\=750**提供給擁有者的 rwx 權限、 rx 權限給群組和其他項目沒有存取權限。  
  
**mapsvr\=***server*  
設定組*server*為 NFS 用戶端的使用者名稱對應伺服器。 雖然此選項將會繼續支援與舊版相容，您應該使用**sfuadmin**公用程式改為。  
  
**mtype\=**{**hard** | **soft**}  
指定預設掛接類型。 至於硬掛接，NFS 用戶端會繼續重試失敗的 RPC，直到成功為止。 軟掛接，NFS 用戶端傳回失敗後重試呼叫呼叫的應用程式所指定的次數**重試**選項。  
  
**retry\=***number*  
指定嘗試進行軟掛接連線的次數。 此值必須介於 1 到 10 （含)。 預設值為 1。  
  
**timeout\=***seconds*  
指定要等候連接的秒數\(遠端程序呼叫\)。 此值必須是 0.8、 0.9 或從 1 到 60 （含) 的整數。 預設值為 0.8。  
  
**Protocol\={TCP | UDP | TCP\+UDP}**  
指定的傳輸通訊協定的用戶端支援。 預設值是**TCP\+UDP**  
  
**rsize\=***size*  
指定的大小，以 kb 為單位讀取的緩衝區。 此值可以是 0.5、 1、 2、 4、 8、 16 或 32。 預設值為 32。  
  
**wsize\=***size*  
指定的大小，以 kb 為單位的寫入緩衝。 此值可以是 0.5、 1、 2、 4、 8、 16 或 32。 預設值為 32。  
  
**perf\=default**  
將下列的效能設定還原為預設值：  
  
-   **mtype**  
  
-   **retry**  
  
-   **timeout**  
  
-   **rsize**  
  
-   **wsize**  
  
**fileaccess\=***mode*  
指定網路檔案系統上建立檔案的預設權限模式\(NFS\)伺服器。 *模式*引數包含三個位數，範圍介於 0 到 7\(內含\)表示的預設權限授與使用者、 群組和其他\(分別\)。 數字會轉譯為 UNIX\-樣式權限，如下所示：0\=無、 1\=x，2\=w、 3\=wx，4\=r、 5\=rx，6\=rw，而 7\=rwx。 例如， **fileaccess\=750**提供給擁有者的 rwx 權限、 rx 權限給群組和其他項目沒有存取權限。  
  
如果您未指定的命令選項或引數**nfsadmin client**會顯示目前的用戶端 for NFS 組態設定。  
  

