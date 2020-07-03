---
title: nfsadmin
description: Nfsadmin 命令的參考文章，它會管理 Server for NFS 和 Client for NFS。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7375b2cf-c6b8-45b5-abf6-6c10e462defd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ad88594d534c64c0651fcc4e094fef669a02f16c
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85932330"
---
# <a name="nfsadmin"></a>nfsadmin

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

一種命令列公用程式，可在執行 Microsoft Services for Network File System （NFS）的本機或遠端電腦上，管理 Server for NFS 或 Client for NFS。 使用不含參數的情況下，nfsadmin 伺服器會顯示目前的 NFS 伺服器設定設定，而 nfsadmin 用戶端會顯示目前的 NFS 用戶端設定。

## <a name="syntax"></a>Syntax

```
nfsadmin server [computername] [-u Username [-p Password]] -l
nfsadmin server [computername] [-u Username [-p Password]] -r {client | all}
nfsadmin server [computername] [-u Username [-p Password]] {start | stop}
nfsadmin server [computername] [-u Username [-p Password]] config option[...]
nfsadmin server [computername] [-u Username [-p Password]] creategroup <name>
nfsadmin server [computername] [-u Username [-p Password]] listgroups
nfsadmin server [computername] [-u Username [-p Password]] deletegroup <name>
nfsadmin server [computername] [-u Username [-p Password]] renamegroup <oldname> <newname>
nfsadmin server [computername] [-u Username [-p Password]] addmembers <hostname>[...]
nfsadmin server [computername] [-u Username [-p Password]] listmembers
nfsadmin server [computername] [-u Username [-p Password]] deletemembers <hostname><groupname>[...]
nfsadmin client [computername] [-u Username [-p Password]] {start | stop}
nfsadmin client [computername] [-u Username [-p Password]] config option[...]
```

### <a name="general-parameters"></a>一般參數

| 參數 | 說明 |
| --------- | ----------- |
| computername | 指定您要管理的遠端電腦。 您可以使用 Windows 網際網路名稱服務（WINS）名稱或網域名稱系統（DNS）名稱或網際網路通訊協定（IP）位址來指定電腦。 |
| -u 使用者名稱 | 指定要使用其認證之使用者的使用者名稱。 可能需要將功能變數名稱新增至使用者名稱，格式為「網域*\*使用者名稱」。 |
| -p 密碼 | 使用 **-u**選項指定使用者的密碼。 如果您指定 **-u**選項，但省略 **-p**選項，系統會提示您輸入使用者的密碼。 |

### <a name="server-for-nfs-related-parameters"></a>Server for NFS 相關參數

| 參數 | 說明 |
| --------- | ----------- |
| -l | 列出用戶端持有的所有鎖定。 |
| -r`{client|all}` | 釋放用戶端所持有的鎖定，如果所有用戶端已指定 all，則為。 |
| start | 啟動 Server for NFS 服務。 |
| stop | 停止 Server for NFS 服務。 |
| config | 指定 Server for NFS 的一般設定。 您必須使用**config**命令引數提供下列其中一個選項：<ul><li>**mapsvr = `<server>` **-將伺服器設定為 Server for NFS 的消費者名稱對應伺服器。 雖然此選項會繼續支援以符合舊版的相容性，但您應該改用 sfuadmin 公用程式。</li><li>**auditlocation = `{eventlog|file|both|none}` **-指定是否要審核事件以及記錄事件的位置。 需要下列其中一個引數：<ul><li>**eventlog** -指定僅將經過審核的事件記錄在事件檢視器應用程式記錄檔中。</li><li>檔案 **-指定**只會將已審核的事件記錄在所指定的檔案中 `config fname` 。</li><li>**both** -指定已審核的事件將會記錄在事件檢視器應用程式記錄檔中，以及所指定的檔案中 `config fname` 。</li><li>**無**-指定不審核事件。</li></ul><li>**fname = `<file>` **-將檔案所指定的檔案設定為 audit 檔案。 預設值為 **%sfudir%\log \\ nfssvr。**</li><li>**fsize = `<size>` **-將大小設定為 audit 檔案的大小上限（以 mb 為單位）。 預設的大小上限為**7 MB**。</li><li>**`audit=[+|-]mount [+|-]read [+|-]write [+|-]create [+|-]delete [+|-]locking [+|-]all`**-指定要記錄的事件。 若要開始記錄事件，請在事件名稱前面輸入一個加號（ **+** ）; 若要停止記錄事件，請在 **-** 事件名稱前面輸入減號（）。 如果省略正負號，則 **+** 會假設為正負號。 請勿將**all**與任何其他事件名稱一起使用。</li><li>**lockperiod = `<seconds>` **-指定 Server for nfs 連線遺失後，或在 Server for nfs 服務重新開機之後，伺服器將等待回收鎖定的秒數。</li><li>**portmapprotocol = `{TCP|UDP|TCP+UDP}` **-指定 Portmap 支援的傳輸通訊協定。 預設設定為**TCP + UDP**。</li><li>**mountprotocol = `{TCP|UDP|TCP+UDP}` **-指定掛接支援的傳輸通訊協定。 預設設定為**TCP + UDP**。</li><li>**nfsprotocol = `{TCP|UDP|TCP+UDP}` **-指定網路檔案系統（NFS）支援的傳輸通訊協定。 預設設定為**TCP + UDP**</li><li>**nlmprotocol = `{TCP|UDP|TCP+UDP}` **-指定網路鎖定管理員（NLM）支援的傳輸通訊協定。 預設設定為**TCP + UDP**。</li><li>**nsmprotocol = `{TCP|UDP|TCP+UDP}` **-指定網路狀態管理員（NSM）支援的傳輸通訊協定。 預設設定為**TCP + UDP**。</li><li>**enableV3 = `{yes|no}` **-指定是否支援 NFS 第3版通訊協定。 預設設定為 **[是]**。</li><li>**renewauth = `{yes|no}` **-指定是否需要在 config renewauthinterval 指定的期間之後重新驗證用戶端連接。 預設設定為 [**否**]。</li><li>**renewauthinterval = `<seconds>` **-指定如果 `config renewauth` 設定為 **[是]**，則在強制重新驗證用戶端之前經過的秒數。 預設值為**600 秒**。</li><li>**dircache = `<size>` **-指定目錄快取的大小（以 kb 為單位）。 指定為 size 的數位必須是4到128之間的4倍數。 預設的目錄快取大小為**128 KB**。</li><li>**translationfile = `<file>` **-指定一個檔案，其中包含在將檔案從 Windows 架構移至 UNIX 檔案系統時，用來取代檔案名稱中字元的對應資訊。 如果未指定 file，則會停用檔案名字元轉譯。 如果**translationfile**的值已變更，您必須重新開機伺服器，變更才會生效。</li><li>**dotfileshidden = `{yes|no}` **-指定名稱開頭為句點（.）的檔案在 Windows 檔案系統中是否標示為隱藏，並因此從 NFS 用戶端隱藏。 預設設定為 [**否**]。</li><li>**casesensitivelookups = `{yes|no}` **-指定目錄查閱是否區分大小寫（需要完全符合的字元大小寫）。<p>您也必須停用 Windows 核心不區分大小寫，以支援區分大小寫的檔案名。 若要支援區分大小寫，請將登錄機碼的**DWord**值變更 `HKLM\SYSTEM\CurrentControlSet\Control\Session Manager\kernel` 為**0**。</li><li>**ntfscase = `{lower|upper|preserve}` **-指定是否要以小寫、大寫或儲存在目錄中的格式，傳回 NTFS 檔案系統中檔案名稱的字元大小寫。 預設設定為 [**保留**]。 如果 [ **casesensitivelookups** ] 設定為 **[是]**，則無法變更此設定。</li></ul> |
| creategroup`<name>` | 建立新的用戶端群組，並提供指定的名稱。 |
| listgroups | 顯示所有用戶端群組的名稱。 |
| deletegroup`<name>` | 移除 [名稱] 所指定的用戶端群組。 |
| renamegroup `<oldname>``<newname>` | 將*oldname*所指定的用戶端群組名稱變更為*newname*。 |
| addmembers`<hostname>[...]` | 將*主機*新增至 [*名稱*] 所指定的用戶端群組。 |
| listmembers`<name>` | 列出依 [*名稱*] 指定之用戶端群組中的主機電腦。 |
| deletemembers`<hostname><groupname>[...]` | 從*group*指定的用戶端群組移除*主機*所指定的用戶端。 |

### <a name="client-for-nfs-related-parameters"></a>NFS 相關的用戶端參數

| 參數 | 說明 |
| --------- | ----------- |
| start | 啟動 Client for NFS 服務。 |
| stop | 停止 NFS 用戶端服務。 |
| config | 指定 Client for NFS 的一般設定。 您必須使用**config**命令引數提供下列其中一個選項：<ul><li>**fileaccess = `<mode>` **-指定在網路檔案系統（NFS）伺服器上建立之檔案的預設許可權模式。 **Mode**引數是由三個數字組成，介於0到7（含）之間，代表授與使用者、群組和其他的預設許可權。 數位會轉譯為 UNIX 樣式許可權，如下所示： *0 = 無*、 *1 = x （執行）*、 *2 = w （僅限寫入）*、 *3 = wx （write 和 execute）*、 *4 = r （唯讀）*、 *5 = rx （讀取和執行）*、 *6 = rw （讀取和寫入）*，以及*7 = rwx （讀取、寫入和執行）*。 例如，授與擁有者的 `fileaccess=750` 讀取、寫入和執行許可權、群組的讀取和執行許可權，以及對其他人沒有存取權限。</li><li>**mapsvr = `<server>` **-將伺服器設定為 Client for NFS 的消費者名稱對應伺服器。 雖然此選項會繼續支援以符合舊版的相容性，但您應該改用 sfuadmin 公用程式。</li><li>**mtype = `{hard|soft}` **-指定預設掛接類型。 若是硬掛接，NFS 用戶端會繼續重試失敗的 RPC，直到成功為止。 若為軟掛接，則在重試呼叫由重試選項指定的次數之後，NFS 用戶端會傳回呼叫應用程式失敗。</li><li>**重試 `<number>` =**-指定嘗試建立軟掛接連接的次數。 這個值必須介於1到10（含）之間。 預設為 **1**。</li><li>**timeout = `<seconds>` **-指定等候連接的秒數（遠端程序呼叫）。 這個值必須是*0.8*、 *0.9*或*1 到 60*（含）之間的整數。 預設值為**0.8**。</li><li>**通訊協定 `{TCP|UDP|TCP+UDP}` =**-指定用戶端支援的傳輸通訊協定。 預設設定為**TCP + UDP**。</li><li>**rsize = `<size>` **-指定讀取緩衝區的大小（以 kb 為單位）。 此值可以是*0.5、1、2、4、8、16*或*32*。 預設值為**32**。</li><li>**wsize = `<size>` **-指定寫入緩衝區的大小（以 kb 為單位）。 此值可以是*0.5、1、2、4、8、16*或*32*。 預設值為**32**。</li><li>**perf = default** -將下列效能設定還原為預設值： *mtype*、 *retry*、 *timeout*、 *rsize*或*wsize*。 |

### <a name="examples"></a>範例

若要停止 Server for NFS 或 Client for NFS，請輸入：

```
nfsadmin server stop
nfsadmin client stop
```

若要啟動 Server for NFS 或 Client for NFS，請輸入：

```
nfsadmin server start
nfsadmin client start
```

若要將 Server for NFS 設定為不區分大小寫，請輸入：

```
nfsadmin server config casesensitive=no
```

若要將 Client for NFS 設定為區分大小寫，請輸入：

```
nfsadmin client config casesensitive=yes
```

若要顯示所有目前的 Server for NFS 或 Client for NFS 選項，請輸入：

```
nfsadmin server config
nfsadmin client config
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [NFS Cmdlet 參考](https://docs.microsoft.com/powershell/module/nfs)
