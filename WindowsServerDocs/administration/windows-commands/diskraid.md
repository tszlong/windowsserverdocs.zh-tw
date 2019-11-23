---
title: diskraid
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 20aef1e5-7641-47cf-b4eb-cda117f65b6e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f72e91f856da3b24e7450381b293f4b365d914f3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377804"
---
# <a name="diskraid"></a>diskraid



DiskRAID 是一種命令列工具，可讓您設定和管理獨立（或便宜）磁片（RAID）儲存子系統的重複陣列。

RAID 是一種用來標準化和分類容錯磁片系統的方法。 RAID 層級提供各種效能、可靠性和成本組合。 RAID 通常是在伺服器上使用。 有些伺服器會提供三個 RAID 層級：層級0（等量）、層級1（鏡像）和層級5（以同位方式分割）。

硬體 RAID 子系統會使用邏輯單元編號（LUN），區別實體可定址的儲存單位。 LUN 物件必須至少有一個 plex，而且可以有任意數目的額外 plex。 每個 plex 都包含 LUN 物件上的資料複本。 可以在 LUN 物件中新增和移除 plex。

大部分的 DiskRAID 命令都是在特定的主機匯流排介面卡（HBA）埠、啟動器介面卡、啟動器入口網站、提供者、子系統、控制器、埠、磁片磁碟機、LUN、目標入口網站、目標或目標入口網站群組上操作。 您可以使用 SELECT 命令來選取物件。 選取的物件被視為具有焦點。 焦點簡化了常見的設定工作，例如在同一個子系統中建立多個 Lun。

> [!NOTE]
> DiskRAID 命令列工具僅適用于支援虛擬磁碟服務（VDS）的存放子系統。

## <a name="diskraid-commands"></a>DiskRAID 命令

若要查看命令語法，請按一下命令：
-   [載入](#BKMK_1)
-   [起來](#BKMK_2)
-   [automagic](#BKMK_3)
-   [break](#BKMK_4)
-   [dh-chap](#BKMK_5)
-   [建立](#BKMK_6)
-   [delete](#BKMK_7)
-   [資訊](#BKMK_8)
-   [中斷關聯](#BKMK_9)
-   [exit](#BKMK_10)
-   [extend](#BKMK_11)
-   [flushcache](#BKMK_12)
-   [說明](#BKMK_13)
-   [importtarget](#BKMK_14)
-   [啟動器](#BKMK_15)
-   [invalidatecache](#BKMK_16)
-   [lbpolicy](#BKMK_18)
-   [list](#BKMK_19)
-   [登入](#BKMK_20)
-   [登出](#BKMK_21)
-   [保養](#BKMK_22)
-   [name](#BKMK_23)
-   [離線](#BKMK_24)
-   [線上](#BKMK_25)
-   [recover](#BKMK_26)
-   [reenumerate](#BKMK_27)
-   [恢復](#BKMK_28)
-   [rem](#BKMK_29)
-   [取消](#BKMK_30)
-   [replace](#BKMK_31)
-   [啟動](#BKMK_32)
-   [請](#BKMK_33)
-   [setflag](#BKMK_34)
-   [shrink](#BKMK_shrink)
-   [待命](#BKMK_35)
-   [取消遮罩](#BKMK_36)

### <a name="BKMK_1"></a>載入

將現有的 LUN 新增至目前選取的 LUN，或將 iSCSI 目標入口網站新增至目前選取的 iSCSI 目標入口網站群組。

#### <a name="syntax"></a>語法

```
add plex lun=n [noerr]
add tpgroup tportal=n [noerr]
```

#### <a name="parameters"></a>Parameters

**plex lun**=*n*

指定要新增為目前所選 LUN 之 plex 的 LUN 編號。

> [!CAUTION]
> 將會刪除 LUN 上新增為 plex 的所有資料。

**tpgroup 管理入口網站 =** <em>n</em>

指定要新增至目前所選取 iSCSI 目標入口網站群組的 iSCSI 目標入口網站編號。

**noerr**

指定在執行此作業時所發生的任何失敗都會被忽略。 這在腳本模式中很有用。

### <a name="BKMK_2"></a>起來

針對目前選取的 LUN，將指定的控制器埠清單設定為作用中（其他控制器埠會變成非作用中），或將指定的控制器埠新增至目前所選 LUN 的現有作用中控制器埠清單，或將目前所選取 LUN 的指定 iSCSI 目標。

#### <a name="syntax"></a>語法

```
associate controllers [add] <n>[,<n> [,…]]
associate ports [add] <n-m>[,<n-m>[,…]]
associate targets [add] <n>[,<n> [,…]]
```

#### <a name="parameters"></a>Parameters

**控制器**

僅與 VDS 1.0 提供者搭配使用。 新增或取代與目前所選 LUN 相關聯的控制器清單。

**埠**

僅與 VDS 1.1 提供者搭配使用。 新增或取代與目前所選 LUN 相關聯的控制器埠清單。

**攻擊**

僅與 VDS 1.1 提供者搭配使用。 新增或取代與目前所選 LUN 相關聯的 iSCSI 目標清單。

**載入**

對於 VDS 1.0 提供者，會將指定的控制器新增至與 LUN 相關聯的現有控制器清單。 如果未指定此參數，控制器清單會取代與此 LUN 相關聯的現有控制器清單。

對於 VDS 1.1 提供者，會將指定的控制器埠新增至與 LUN 相關聯的現有控制器埠清單。 如果未指定此參數，控制器埠的清單會取代與此 LUN 相關聯的現有控制器埠清單。
```
<n>[,<n> [, ...]]
```
用於**控制器**或**目標**參數。 指定要設定為作用中或建立關聯的控制器或 iSCSI 目標數目。
```
<n-m>[,<n-m>[,…]]
```
用於**埠**參數。 使用控制器編號（*n*）和埠號碼（*m*）配對，指定要設定作用中的控制器埠。

#### <a name="example"></a>範例

下列範例示範如何將埠關聯並新增至使用 VDS 1.1 提供者的 LUN：
```
DISKRAID> SEL LUN 5
LUN 5 is now the selected LUN.

DISKRAID> ASSOCIATE PORTS 0-0,0-1
Controller port associations changed.
(Controller ports active after this command: Ctlr 0 Port 0, Ctlr 0 Port 1)

DISKRAID> ASSOCIATE PORTS ADD 1-1
Controller port associations changed.
(Controller ports active after this command: Ctlr 0 Port 0, Ctlr 0 Port 1, Ctlr 1 Port 1)
```

### <a name="BKMK_3"></a>automagic

設定或清除旗標，提供有關如何設定 LUN 的提示給提供者。 不搭配任何參數使用時， **automagic**作業會顯示旗標清單。

#### <a name="syntax"></a>語法

```
automagic {set | clear | apply} all <flag=value> [<flag=value> [...]]
```

#### <a name="parameters"></a>Parameters

**set**

將指定的旗標設定為指定的值。

**clear**

清除指定的旗標。 **All**關鍵字會清除所有的 automagic 旗標。

**套用**

將目前的旗標套用至選取的 LUN。

\<旗標 >

旗標是以三個字母的縮寫來識別。

|旗標|描述|
|----|-----------|
|FCR|需要快速損毀復原|
|FTL|容錯|
|MSR|大部分的讀取|
|MXD|磁片磁碟機數目上限|
|MXS|預期的大小上限|
|TNSNAMES.ORA|最佳的讀取對齊|
|OR|最佳讀取大小|
|OSR|針對連續讀取優化|
|OSW|針對連續寫入優化|
|OWA|最佳寫入對齊|
|OWS.JS|最佳寫入大小|
|RBP|重建優先順序|
|RBV|讀回驗證已啟用|
|RMP|已啟用重新對應|
|STS|等量大小|
|WTC|已啟用寫入快取|
|YNK|可|

### <a name="BKMK_4"></a>崩潰

從目前選取的 LUN 移除 plex。 並不會保留該 plex 和其包含的資料，而且可能會回收磁片區範圍。

#### <a name="syntax"></a>語法

```
break plex=<plex_number> [noerr]
```

#### <a name="parameters"></a>Parameters

**複雜**

指定要移除之 plex 的編號。 將不會保留該 plex 和其包含的資料，而且將會回收此 plex 所使用的資源。 LUN 中包含的資料不保證是一致的。 如果您想要保留此 plex，請使用磁碟區陰影複製服務（VSS）。

**noerr**

指定在執行此作業時所發生的任何失敗都會被忽略。 這在腳本模式中很有用。

#### <a name="remarks"></a>備註

> [!NOTE]
> 使用**break**命令之前，您必須先選取鏡像 LUN。

> [!CAUTION]
> 將會刪除該 plex 上的所有資料。

> [!CAUTION]
> 原始 LUN 上包含的所有資料都不保證一致。

### <a name="BKMK_5"></a>dh-chap

設定挑戰交握驗證通訊協定（CHAP）共用密碼，讓 iSCSI 啟動器和 iSCSI 目標可以彼此通訊。

#### <a name="syntax"></a>語法

```
chap initiator set secret=[<secret>] [target=<target>]
chap initiator remember secret=[<secret>] target=<target>
chap target set secret=[<secret>] [initiator=<initiatorname>]
chap target remember secret=[<secret>] initiator=<initiatorname>
```

#### <a name="parameters"></a>Parameters

**起始端集合**

當起始端驗證目標時，設定用於相互 CHAP 驗證的本機 iSCSI 啟動器服務中的共用密碼。

**起始端記住**

將 iSCSI 目標的 CHAP 密碼與本機 iSCSI 啟動器服務通訊，讓起始端服務可以使用秘密，在 CHAP 驗證期間對目標進行驗證。

**目標集**

設定目前選取的 iSCSI 目標中的共用密碼，在目標驗證啟動器時用於 CHAP 驗證。

**目標記憶**

將 iSCSI 啟動器的 CHAP 密碼與目前的集中式 iSCSI 目標通訊，讓目標可以使用秘密，以便在相互 CHAP 驗證期間向啟動器驗證其本身。

**密碼**

指定要使用的密碼。 如果空白，將會清除密碼。

**設定**

指定目前所選子系統中與密碼相關聯的目標。 這是在啟動者上設定密碼時的選擇性選項，並將其登出指出將會針對所有尚未擁有相關聯秘密的目標使用該密碼。

**initiatorname**

指定要與密碼相關聯的啟動器 iSCSI 名稱。 這是設定目標上的秘密並讓它退出時，這是選擇性的，這表示密碼將用於所有尚未擁有相關聯秘密的啟動器。

### <a name="BKMK_6"></a>建立

在目前選取的子系統上建立新的 LUN 或 iSCSI 目標，或在目前選取的目標上建立目標入口網站群組。 您可以使用 [ **DiskRAID 清單**] 命令來查看實際的系結。

#### <a name="syntax"></a>語法

```
create lun simple [size=<n>] [drives=<n>] [noerr]
create lun stripe [size=<n>] [drives=<n, n> [,...]]  [stripesize=<n>] [noerr]
create lun raid [size=<n>] [drives=<n, n> [,...]] [stripesize=<n>] [noerr]
create lun mirror [size=<n>] [drives=<n, n> [,...]] [stripesize=<n>] [noerr]
create lun automagic size=<n> [noerr]
create target name=<name> [iscsiname=<iscsiname>] [noerr]
create tpgroup [noerr]
```

#### <a name="parameter"></a>參數

**simple**

建立簡單的 LUN。

**stripe**

建立等量 LUN。

**陣列**

建立具有同位的等量 LUN。

**對稱**

建立鏡像 LUN。

**automagic**

使用目前作用中的*automagic*提示，建立 LUN。 如需詳細資訊，請參閱**automagic**子命令。

**大小**=

指定 LUN 總大小（以 mb 為單位）。 如果未指定**size =** 參數，則建立的 LUN 會是所有指定磁片磁碟機所允許的最大可能大小。

提供者通常會建立至少與所要求大小一樣大的 LUN，但在某些情況下，提供者可能必須進位到下一個最大的大小。 例如，如果將 size 指定為. 99 GB，而提供者只能配置 GB 的磁片區，則產生的 LUN 會是 1 GB。

若要使用其他單位來指定大小，請使用下列其中一個可辨識的尾碼，緊接在大小之後：
-   **B**代表 byte。
-   **Kb**表示千位元組。
-   **Mb （mb** ）。
-   **Gb （gb** ）。
-   **Tb** ，用於 tb。
-   **Pb （pb** ）。

**磁片磁碟機**=

指定要用來建立 LUN 之磁片磁碟機的*drive_number* 。 如果未指定**size =** 參數，則建立的 LUN 會是所有指定磁片磁碟機所允許的最大可能大小。 如果已指定**size =** 參數，提供者會從指定的磁片磁碟機清單中選取磁片磁碟機，以建立 LUN。 提供者會嘗試依照指定的順序來使用磁片磁碟機。

**stripesize**=

指定*stripe*或*RAID* LUN 的大小（以 mb 為單位）。 建立 LUN 之後，即無法變更 stripesize。

若要使用其他單位來指定大小，請使用下列其中一個可辨識的尾碼，緊接在大小之後：
-   **B**代表 byte。
-   **Kb**表示千位元組。
-   **Mb （mb** ）。
-   **Gb （gb** ）。
-   **Tb** ，用於 tb。
-   **Pb （pb** ）。

**設定**

在目前選取的子系統上建立新的 iSCSI 目標。

**name**

提供目標的易記名稱。

**iscsiname**

提供目標的 iSCSI 名稱，而且可以省略，讓提供者產生名稱。

**tpgroup**

在目前選取的目標上建立新的 iSCSI 目標入口網站群組。

**noerr**

指定在執行此作業時所發生的任何失敗都會被忽略。 這在腳本模式中很有用。

#### <a name="remarks"></a>備註

-   必須指定**size**= 或**磁片磁碟機**= 參數。 它們也可以一起使用。
-   建立之後，就無法變更 LUN 的等量大小。

### <a name="BKMK_7"></a>delete

刪除目前選取的 LUN、iSCSI 目標（只要沒有任何與 iSCSI 目標相關聯的 Lun）或 iSCSI 目標入口網站群組。

#### <a name="syntax"></a>語法

```
delete lun [uninstall] [noerr]
delete target [noerr]
delete tpgroup [noerr]
```

#### <a name="parameters"></a>Parameters

**lun**

刪除目前選取的 LUN 和其上的所有資料。

**uninstall**

指定在刪除 LUN 之前，會清除與 LUN 相關聯之本機系統上的磁片。

**設定**

如果沒有與目標相關聯的 Lun，則刪除目前選取的 iSCSI 目標。

**tpgroup**

刪除目前選取的 iSCSI 目標入口網站群組。

**noerr**

指定在執行此作業時所發生的任何失敗都會被忽略。 這在腳本模式中很有用。

### <a name="BKMK_8"></a>資訊

顯示指定類型之目前選取物件的詳細資訊。

#### <a name="syntax"></a>語法

```
Detail {hbaport | iadapter | iportal | provider | subsystem | controller | port | drive | lun | tportal | target | tpgroup} [verbose]
```

#### <a name="parameters"></a>Parameters

**hbaport**

列出目前選取的主機匯流排介面卡（HBA）埠的詳細資訊。

**iadapter**

列出目前選取的 iSCSI 啟動器介面卡的詳細資訊。

**iportal**

列出目前選取的 iSCSI 啟動器入口網站的詳細資訊。

**那裡**

列出目前所選提供者的詳細資訊。

**子系統**

列出目前所選子系統的詳細資訊。

**控制器**

列出目前所選控制器的詳細資訊。

**移植**

列出目前選取的控制器埠的詳細資訊。

**硬碟磁碟機**

列出目前所選磁片磁碟機的詳細資訊，包括佔用的 Lun。

**lun**

列出目前所選 LUN 的詳細資訊，包括參與的磁片磁碟機。 輸出會因 LUN 是否為光纖通道或 iSCSI 子系統的一部分而略有不同。 如果 [未遮罩的主機] 清單只包含星號，這表示 LUN 不會對所有主機取消遮罩。

**管理入口網站**

列出目前選取的 iSCSI 目標入口網站的詳細資訊。

**設定**

列出目前選取的 iSCSI 目標的詳細資訊。

**tpgroup**

列出目前選取的 iSCSI 目標入口網站群組的詳細資訊。

**冗余**

僅用於 LUN 參數。 列出其他資訊，包括其 plex。

### <a name="BKMK_9"></a>中斷關聯

針對目前選取的 LUN，將指定的控制器埠清單設定為非作用中（其他控制器埠不會受到影響），或分離目前所選 LUN 的指定 iSCSI 目標清單。

#### <a name="syntax"></a>語法

```
dissociate controllers <n> [,<n> [,...]]
dissociate ports <n-m>[,<n-m>[,…]]
dissociate targets <n> [,<n> [,…]]
```

#### <a name="parameter"></a>參數

**控制器**

僅與 VDS 1.0 提供者搭配使用。 從與目前選取的 LUN 相關聯的控制器清單中移除控制器。

**埠**

僅與 VDS 1.1 提供者搭配使用。 從與目前選取的 LUN 相關聯的控制器埠清單中移除控制器埠。

**攻擊**

僅與 VDS 1.1 提供者搭配使用。 從與目前選取的 LUN 相關聯的 iSCSI 目標清單中移除目標。
```
<n> [,<n> [,…]]
```
用於**控制器**或**目標**參數。 指定要設定為非作用中或中斷關聯的控制器或 iSCSI 目標數目。
```
<n-m>[,<n-m>[,…]]
```
用於**埠**參數。 使用控制器編號（*n*）和埠號碼（*m*）配對，指定要設定為非使用中的控制器埠。

#### <a name="example"></a>範例

```
DISKRAID> SEL LUN 5
LUN 5 is now the selected LUN.

DISKRAID> ASSOCIATE PORTS 0-0,0-1
Controller port associations changed.
(Controller ports active after this command: Ctlr 0 Port 0, Ctlr 0 Port 1)

DISKRAID> ASSOCIATE PORTS ADD 1-1
Controller port associations changed.
(Controller ports active after this command: Ctlr 0 Port 0, Ctlr 0 Port 1, Ctlr 1 Port 1)

DISKRAID> DISSOCIATE PORTS 0-0,1-1
Controller port associations changed.
(Controller ports active after this command: Ctlr 0 Port 1)
```

### <a name="BKMK_10"></a>退出

結束 DiskRAID。

#### <a name="syntax"></a>語法

```
exit
```

### <a name="BKMK_11"></a>延遲

藉由將磁區新增至 LUN 的結尾，來擴充目前選取的 LUN。 並非所有提供者都支援擴充 Lun。 不會擴充 LUN 上包含的任何磁片區或檔案系統。 擴充 LUN 之後，您應該使用**DiskPart extend**命令來擴充相關聯的磁片上結構。

#### <a name="syntax"></a>語法

```
extend lun [size=<LUN_size>] [drives=<drive_number>, [<drive_number>, ...]] [noerr]
```

#### <a name="parameters"></a>Parameters

**大小 =**

指定擴充 LUN 的大小（以 mb 為單位）。 如果未指定**size =** 參數，LUN 會以所有指定磁片磁碟機所允許的最大可能大小來擴充。 如果已指定**size =** 參數，提供者會從**磁片磁碟機 =** 參數所指定的清單中選取磁片磁碟機，以建立 LUN。

若要使用其他單位來指定大小，請使用下列其中一個可辨識的尾碼，緊接在大小之後：
-   **B**代表 byte。
-   **Kb**表示千位元組。
-   **Mb （mb** ）。
-   **Gb （gb** ）。
-   **Tb （tb** ）
-   **Pb （pb** ）

**磁片磁碟機 =**

指定建立 LUN 時要使用之磁片磁碟機的 \<drive_number >。 如果未指定**size =** 參數，則建立的 LUN 會是所有指定磁片磁碟機所允許的最大可能大小。 提供者會盡可能以指定的順序使用磁片磁碟機。

**noerr**

指定應該忽略執行此作業時所發生的任何失敗。 這在腳本模式中很有用。

#### <a name="remarks"></a>備註

必須指定*大小*或 \<磁片磁碟機 > 參數。 它們也可以一起使用。

### <a name="BKMK_12"></a>flushcache

清除目前選取的控制器上的快取。

#### <a name="syntax"></a>語法

```
flushcache controller
```

### <a name="BKMK_13"></a>説明

顯示所有 DiskRAID 命令的清單。

#### <a name="syntax"></a>語法

```
help
```

### <a name="BKMK_14"></a>importtarget

抓取或設定目前所選子系統的目前磁碟區陰影複製服務（VSS）匯入目標。

#### <a name="syntax"></a>語法

```
importtarget subsystem [set target]
```

#### <a name="parameter"></a>參數

**設定目標**

若已指定，會將目前選取的目標設定為目前所選子系統的 VSS 匯入目標。 如果未指定，命令會抓取目前所選子系統所設定的目前 VSS 匯入目標。

### <a name="BKMK_15"></a>啟動器

抓取本機 iSCSI 啟動器的相關資訊。

#### <a name="syntax"></a>語法

```
initiator
```

### <a name="BKMK_16"></a>invalidatecache

使目前選取的控制器上的快取失效。

#### <a name="syntax"></a>語法

```
invalidatecache controller
```

### <a name="BKMK_18"></a>lbpolicy

在目前選取的 LUN 上設定負載平衡原則。

#### <a name="syntax"></a>語法

```
lbpolicy set lun type=<type> [paths=<path>-{primary | <weight>}[,<path>-{primary | <weight>}[,…]]]
lbpolicy set lun paths=<path>-{primary | <weight>}[,<path>-{primary | <weight>}[,…]]
```

#### <a name="parameters"></a>Parameters

**type**

指定負載平衡原則。 如果未指定類型，則必須指定**path**參數。 類型可以是下列其中一項：

**容錯移轉**：使用一個主要路徑搭配其他路徑作為備份路徑。

**ROUNDROBIN**：以迴圈配置資源方式使用所有路徑，這會依序嘗試每個路徑。

**SUBSETROUNDROBIN**：以迴圈配置方式使用所有主要路徑;只有在所有主要路徑都失敗時，才會使用備份路徑。

**DYNLQD**：使用作用中要求數目最少的路徑。

**加權**：使用具有最小權數的路徑（每個路徑都必須指派權數）。

**LEASTBLOCKS**：使用具有最少區塊的路徑。

**VENDORSPECIFIC**：使用特定廠商的原則。

**路線圖**

指定路徑為**主要**或具有特定的 \<權數 >。 未指定的任何路徑都會隱含地設定為備份。 列出的任何路徑都必須是目前所選取 LUN 的其中一個路徑。

### <a name="BKMK_19"></a>名單

顯示指定類型的物件清單。

#### <a name="syntax"></a>語法

```
List {hbaports | iadapters | iportals | providers | subsystems | controllers | ports | drives | LUNs | tportals | targets | tpgroups}
```

#### <a name="parameters"></a>Parameters

**hbaports**

列出有關 VDS 已知的所有 HBA 埠的摘要資訊。 目前選取的 HBA 埠以星號（*）標示。

**iadapters**

列出有關 VDS 已知的所有 iSCSI 啟動器介面卡的摘要資訊。 目前選取的啟動器介面卡以星號（*）標示。

**iportals**

列出目前所選啟動器介面卡中所有 iSCSI 啟動器入口網站的摘要資訊。 目前選取的啟動器入口網站會以星號（*）標示。

**提供者**

列出有關 VDS 已知的每個提供者的摘要資訊。 目前選取的提供者會以星號（*）標示。

**分系統**

列出系統中每個子系統的摘要資訊。 目前選取的子系統會以星號（*）標示。

**控制器**

列出目前所選子系統中每個控制器的相關摘要資訊。 目前選取的控制器會以星號（*）標示。

**埠**

列出目前選取的控制器中，每個控制器埠的相關摘要資訊。 目前選取的埠以星號（*）標示。

**硬**

列出目前所選子系統中每個磁片磁碟機的摘要資訊。 目前選取的磁片磁碟機以星號（*）標示。

**lun**

列出目前所選子系統中每個 LUN 的相關摘要資訊。 目前選取的 LUN 會以星號（*）標示。

**tportals**

列出目前所選子系統中所有 iSCSI 目標入口網站的摘要資訊。 目前選取的目標入口網站會以星號（*）標示。

**攻擊**

列出目前所選子系統中所有 iSCSI 目標的摘要資訊。 目前選取的目標會以星號（*）標示。

**tpgroups**

列出目前所選目標中所有 iSCSI 目標入口網站群組的摘要資訊。 目前選取的入口網站群組會以星號（*）標示。

### <a name="BKMK_20"></a>登入

將指定的 iSCSI 啟動器介面卡記錄到目前選取的 iSCSI 目標。

#### <a name="syntax"></a>語法

```
login target iadapter=<iadapter> [type={manual | persistent | boot}] [chap={none | oneway | mutual}] [iportal=<iportal>] [tportal=<tportal>] [<flag> [<flag> […]]]
```

#### <a name="parameters"></a>Parameters

**type**

指定要執行的登入類型： [**手動**]、[**持續**] 或 [**開機**]。 如果未指定，則會執行手動登入。

**手動-手動**登入。

**持續**性-重新開機電腦時，自動使用相同的登入。

**開機**-（此選項適用于未來的開發，目前不會使用）<em>。</em>

**dh-chap**

指定要使用的 CHAP 驗證類型： [**無**]、[**單向**chap] 或 [**相互**chap]。如果未指定，則不會使用驗證。

**管理入口網站**

在目前選取的子系統中，指定用於登入的選擇性目標入口網站。

**iportal**

在指定的啟動器介面卡中，指定要用於登入的選用啟動器入口網站。

\<旗標 >

由三個字母的縮寫所識別：

**Ip**：需要 IPsec

**EMP**：啟用多重路徑

**EHD**：啟用標頭摘要

**EDD**：啟用資料摘要

### <a name="BKMK_21"></a>登出

從目前選取的 iSCSI 目標記錄指定的 iSCSI 啟動器介面卡。

#### <a name="syntax"></a>語法

```
logout target iadapter= <iadapter>
```

#### <a name="parameters"></a>Parameters

**iadapter**

指定要登出之登入會話的啟動器介面卡。

### <a name="BKMK_22"></a>保養

在目前選取的指定類型物件上執行維護作業。

#### <a name="syntax"></a>語法

```
maintenance <object operation> [count=<iteration>]
```

#### <a name="parameters"></a>Parameters

\<物件 >

指定要在其上執行作業的物件類型。 *物件*類型可以是**子系統**、**控制器**、**埠、磁片磁碟機**或**LUN**。

\<作業 >

指定要執行的維護作業。 *操作*類型可以是**spinup**、 **spindown**、**閃爍**、**嗶聲**或**ping**。 必須*指定*作業。

**計數 =**

指定重複*作業的次數。* 這通常與**閃爍**、**嗶聲**或**ping**搭配使用。

### <a name="BKMK_23"></a>檔案名

將目前選取的子系統、LUN 或 iSCSI 目標的易記名稱設定為指定的名稱。

#### <a name="syntax"></a>語法

```
name {subsystem | lun | target} [<name>]
```

#### <a name="parameter"></a>參數

\<名稱 >

指定子系統、LUN 或目標的名稱。 名稱的長度必須少於64個字元。 如果未提供任何名稱，則會刪除現有的名稱（如果有的話）。

### <a name="BKMK_24"></a>離線

將指定類型之目前選取物件的狀態設定為 [**離線**]。

#### <a name="syntax"></a>語法

```
offline <object>
```

#### <a name="parameter"></a>參數

\<物件 >

指定要在其上執行這項作業的物件類型。 \<物件 >

類型可以是**子系統**、**控制器**、**磁片磁碟機**、 **LUN**或**管理入口網站**。

### <a name="BKMK_25"></a>線上

將指定類型之所選物件的狀態設定為 [**線上**]。 如果物件是**hbaport**，則會將目前所選 HBA 埠的路徑狀態變更為 [**線上**]。

#### <a name="syntax"></a>語法

```
online <object> 
```

#### <a name="parameter"></a>參數

\<物件 >

指定要在其上執行這項作業的物件類型。 \<物件 >

類型可以是**hbaport**、**子系統**、**控制器**、**磁片磁碟機**、 **LUN**或**管理入口網站**。

### <a name="BKMK_26"></a>recover

執行必要的作業，例如重新同步或熱備份，以修復目前選取的容錯 LUN。 例如，復原可能會導致熱備件系結至具有故障磁片或其他磁片區重新配置的 RAID 集。

#### <a name="syntax"></a>語法

```
recover <lun>
```

### <a name="BKMK_27"></a>reenumerate

Reenumerates 指定之類型的物件。 如果您使用 [擴充 LUN] 命令，則必須先使用 [重新整理] 命令來更新磁片大小，再使用 reenumerate 命令。

#### <a name="syntax"></a>語法

```
reenumerate {subsystems | drives}
```

#### <a name="parameters"></a>Parameters

**分系統**

查詢提供者，以探索目前所選提供者中新增的任何新子系統。

**硬**

查詢內部 i/o 匯流排，以探索目前所選子系統中新增的任何新磁片磁碟機。

### <a name="BKMK_28"></a>恢復

重新整理目前所選提供者的內部資料。

#### <a name="syntax"></a>語法

```
refresh provider
```

### <a name="BKMK_29"></a>剩餘

用來批註腳本。

#### <a name="syntax"></a>語法

```
Rem <comment>
```

### <a name="BKMK_30"></a>取消

從目前選取的目標入口網站群組移除指定的 iSCSI 目標入口網站。

#### <a name="syntax"></a>語法

```
remove tpgroup tportal=<tportal> [noerr]
```

#### <a name="parameter"></a>參數

**tpgroup 管理入口網站 =** \<管理入口網站 >

指定要移除的 iSCSI 目標入口網站。

**noerr**

指定應該忽略執行此作業時所發生的任何失敗。 這在腳本模式中很有用。

### <a name="BKMK_31"></a>取代

以目前選取的磁片磁碟機取代指定的磁片磁碟機。

#### <a name="syntax"></a>語法

```
replace drive=<drive_number>
```

#### <a name="parameter"></a>參數

**磁片磁碟機 =**

指定要取代之磁片磁碟機的 \<drive_number >。

#### <a name="remarks"></a>備註

-   指定的磁片磁碟機可能不是目前選取的磁片磁碟機。

### <a name="BKMK_32"></a>啟動

重設目前選取的控制器或埠。

#### <a name="syntax"></a>語法

```
Reset {controller | port}
```

#### <a name="parameters"></a>Parameters

**控制器**

重設控制器。

**移植**

重設埠。

### <a name="BKMK_33"></a>請

顯示或變更目前選取的物件。

#### <a name="syntax"></a>語法

```
Select {hbaport | iadapter | iportal | provider | subsystem | controller | port | drive | lun | tportal | target | tpgroup } [<n>]
```

#### <a name="parameters"></a>Parameters

**目標**

指定要選取的物件類型。 \<物件 > 類型可以是**提供者**、**子系統**、**控制器**、**磁片磁碟機**或**LUN**。

**hbaport** [\<n >]

將焦點設定為指定的本機 HBA 埠。 如果未指定 HBA 埠，此命令會顯示目前選取的 HBA 埠（如果有的話）。 指定不正確 HBA 埠索引會導致沒有焦點的 HBA 埠。 選取 HBA 埠會取消選取任何所選的啟動器介面卡和啟動器入口網站。

**iadapter** [\<n >]

將焦點設定為指定的本機 iSCSI 啟動器介面卡。 如果未指定啟動器介面卡，此命令會顯示目前選取的啟動器介面卡（如果有的話）。 指定不正確啟動器介面卡索引會導致沒有焦點的啟動器介面卡。 選取啟動器介面卡，會取消選取任何所選的 HBA 埠和啟動器入口網站。

**iportal** [\<n >]

將焦點設定為所選 iSCSI 啟動器介面卡內指定的本機 iSCSI 啟動器入口網站。 如果未指定啟動器入口網站，此命令會顯示目前選取的啟動器入口網站（如果有的話）。 指定不正確啟動器入口網站索引會導致沒有選取的啟動器入口網站。

**提供者**[\<n >]

將焦點設定為指定的提供者。 如果未指定提供者，此命令會顯示目前選取的提供者（如果有的話）。 指定不正確提供者索引會導致沒有焦點提供者。

**子系統**[\<n >]

將焦點設定為指定的子系統。 如果未指定任何子系統，此命令會顯示具有焦點的子系統（如果有的話）。 指定不正確子系統索引會導致沒有焦點子系統。 選取子系統會隱含地選取其相關聯的提供者。

**控制器**[\<n >]

將焦點設定為目前所選子系統內的指定控制器。 如果未指定任何控制器，此命令會顯示目前選取的控制器（如果有的話）。 指定不正確控制器索引會導致沒有焦點控制器。 選取控制器會取消選取任何所選的控制器埠、磁片磁碟機、Lun、目標入口網站、目標和目標入口網站群組。

**埠**[\<n >]

將焦點設定為目前所選控制器內的指定控制器埠。 如果未指定埠，此命令會顯示目前選取的埠（如果有的話）。 指定不正確埠索引會導致沒有選取的埠。

**磁片磁碟機**[\<n >]

將焦點設定到目前所選子系統內的指定磁片磁碟機或實體主軸。 如果未指定任何磁片磁碟機，此命令會顯示目前選取的磁片磁碟機（如果有的話）。 指定不正確磁片磁碟機索引會導致沒有焦點磁片磁碟機。 選取磁片磁碟機會取消選取任何所選的控制器、控制器埠、Lun、目標入口網站、目標和目標入口網站群組。

**lun** [\<n >]

將焦點設定為目前所選子系統內的指定 LUN。 如果未指定任何 LUN，此命令會顯示目前選取的 LUN （如果有的話）。 指定不正確 LUN 索引會導致沒有選取的 LUN。 選取 LUN 會取消選取任何所選的控制器、控制器埠、磁片磁碟機、目標入口網站、目標和目標入口網站群組。

**管理入口網站**[\<n >]

將焦點設定為目前所選子系統中指定的 iSCSI 目標入口網站。 如果未指定目標入口網站，此命令會顯示目前選取的目標入口網站（如果有的話）。 指定不正確目標入口網站索引會導致沒有選取的目標入口網站。 選取目標入口網站會將任何控制器、控制器埠、磁片磁碟機、Lun、目標和目標入口網站群組取消選取。

**目標**[\<n >]

將焦點設定為目前所選子系統內的指定 iSCSI 目標。 如果未指定任何目標，此命令會顯示目前選取的目標（如果有的話）。 指定不正確目標索引會導致沒有選取的目標。 選取目標會取消選取任何控制器、控制器埠、磁片磁碟機、Lun、目標入口網站和目標入口網站群組。

**tpgroup** [\<n >]

將焦點設定為目前所選取 iSCSI 目標內的指定 iSCSI 目標入口網站群組。 如果未指定目標入口網站群組，此命令會顯示目前選取的目標入口網站群組（如果有的話）。 指定不正確目標入口網站群組索引會導致沒有焦點的目標入口網站群組。

[\<n >]

指定要選取的 \<物件編號 >。 如果指定的 <object number> 無效，則會清除指定類型之物件的任何現有選取專案。 如果未指定 <object number>，則會顯示目前的物件。

### <a name="BKMK_34"></a>setflag

將目前選取的磁片磁碟機設定為熱備件。

#### <a name="syntax"></a>語法

```
setflag drive hotspare={true | false}
```

#### <a name="parameters"></a>Parameters

**true**

選取目前選取的磁片磁碟機作為熱備件。

**false**

將目前選取的磁片磁碟機取消選取為熱備件。

#### <a name="remarks"></a>備註

熱備件無法用於一般 LUN 系結作業。 它們只保留給錯誤處理。 磁片磁碟機目前不得系結到任何現有的 LUN。

### <a name="BKMK_shrink"></a>排

縮小所選 LUN 的大小。

#### <a name="syntax"></a>語法

```
shrink lun size=<n> [noerr]
```

#### <a name="parameters"></a>Parameters

**大小 =**

指定所需的空間量（以 mb 為單位），以減少 LUN 的大小。 若要使用其他單位來指定大小，請在大小之後立即使用其中一個可辨識的尾碼（B、KB、MB、GB、TB 和 PB）。

**noerr**

指定在執行此作業時所發生的任何失敗都會被忽略。 這在腳本模式中很有用。

### <a name="BKMK_35"></a>待命

將目前選取的主機匯流排介面卡（HBA）埠的路徑狀態變更為待命。

#### <a name="syntax"></a>語法

```
standby hbaport
```

#### <a name="parameters"></a>Parameters

**hbaport**

將目前選取的主機匯流排介面卡（HBA）埠的路徑狀態變更為待命。

### <a name="BKMK_36"></a>取消遮罩

使目前選取的 Lun 可從指定的主機存取。

#### <a name="syntax"></a>語法

```
unmask LUN {all | none | [add] wwn=<hexadecimal_number> [;<hexadecimal_number> [;…]] | [add] initiator=<initiator>[;<initiator>[;…]]} [uninstall]
```

#### <a name="parameters"></a>Parameters

**這**

指定 LUN 應可從所有主機進行存取。 不過，您無法將 LUN 取消遮罩至 iSCSI 子系統中的所有目標。

> [!IMPORTANT]
> 您必須先登出目標，才能執行全部取消遮罩命令。

**無**

指定 LUN 不應可供任何主機存取。

> [!IMPORTANT]
> 您必須先登出目標，才能執行「取消遮罩 LUN NONE」命令。

**載入**

指定必須將指定的主機新增至可存取此 LUN 的現有主機清單。 如果未指定此參數，則提供的主機清單會取代可存取此 LUN 的現有主機清單。

**WWN =**

指定十六進位數位的清單，代表可供存取 LUN 或主機的全球名稱。 若要遮罩/取消遮罩光纖通道子系統中一組特定的主機，您可以針對感興趣的主機電腦上的埠，輸入以分號分隔的 WWN 清單。

**起始端 =**

指定要讓目前選取的 LUN 可供存取的 iSCSI 啟動器清單。 若要對 iSCSI 子系統中的一組特定主機遮罩/取消遮罩，您可以在感興趣的主機電腦上輸入啟動器的 iSCSI 啟動器名稱清單（以分號分隔）。

**uninstall**

若已指定，則會先卸載與本機系統上的 LUN 相關聯的磁片，然後再遮罩 LUN。

## <a name="scripting-diskraid"></a>腳本處理 DiskRAID

在任何執行 Windows Server 2008 或 Windows Server 2003 的電腦上，可以使用相關聯的 VDS 硬體提供者編寫 DiskRAID 的腳本。 若要叫用 DiskRAID 腳本，請在命令提示字元中輸入：
```
diskraid /s <script.txt>
```
根據預設，如果腳本發生問題，則 DiskRAID 會停止處理命令，並傳回錯誤碼。 若要繼續執行腳本並忽略錯誤，請在命令上包含 NOERR 參數。 這種作法可讓您使用單一腳本來刪除子系統中的所有 Lun，而不論 Lun 總數為何。 並非所有命令都支援 NOERR 參數。 無論您是否包含 NOERR 參數，錯誤一律會在命令語法錯誤中傳回。

### <a name="diskraid-error-codes"></a>DiskRAID 錯誤碼

|錯誤碼|錯誤描述|
|----------|-----------------|
|0|未發生任何錯誤。 整個腳本執行時不會失敗。|
|1|發生嚴重例外狀況。|
|2|在 DiskRAID 命令列上指定的引數不正確。|
|3|DiskRAID 無法開啟指定的腳本或輸出檔。|
|4|其中一個 DiskRAID 使用的服務傳回失敗。|
|5|發生命令語法錯誤。 腳本失敗，因為物件未正確選取或對該命令使用的無效。|

## <a name="example-interactively-view-status-of-subsystem"></a>範例：以互動方式查看子系統的狀態

如果您想要在電腦上查看子系統0的狀態，請在命令列中輸入下列程式碼：
```
diskraid
```
按 ENTER。 隨即顯示下列內容：
```
Microsoft Diskraid version 5.2.xxxx
Copyright (©) 2003 Microsoft Corporation
On computer: COMPUTER_NAME
```
若要選取子系統0，請在 DiskRAID 提示字元中輸入下列命令：
```
select subsystem 0
```
按 ENTER。 會顯示類似下列的輸出：
```
Subsystem 0 is now the selected subsystem.

DISKRAID> list drives

  Drive ###  Status      Health          Size      Free    Bus  Slot  Flags
  ---------  ----------  ------------  --------  --------  ---  ----  -----
  Drive 0    Online      Healthy         107 GB    107 GB    0     1
  Drive 1    Offline     Healthy          29 GB     29 GB    1     0
  Drive 2    Online      Healthy         107 GB    107 GB    0     2
  Drive 3    Not Ready   Healthy          19 GB     19 GB    1     1
```
若要結束 DiskRAID，請在 DiskRAID 提示字元中輸入下列命令：
```
exit
```