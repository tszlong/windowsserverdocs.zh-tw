---
title: diskraid
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: a565a1d5fa1bc3ff57d1578fb54cfa4553e3bb26
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818869"
---
# <a name="diskraid"></a>diskraid



DiskRAID 是命令列工具，可讓您設定和管理的獨立的 （或低成本的） 的磁碟 (RAID) 的儲存子系統的備援陣列。

RAID 是用來標準化及分類容錯磁碟系統的方法。 RAID 層級提供各種效能、 可靠性及成本的混合。 RAID 通常用在伺服器上。 有些伺服器提供三種 RAID 層級：層級 0 （串接） 」、 「 層級 1 （鏡像） 和 「 層級 5 （具同位檢查的條狀配置） 」。

硬體 RAID 子系統使用邏輯單元編號 (LUN)，不會彼此區分實際可定址存放裝置單位。 LUN 物件必須有至少一個網狀的磁碟區，並可以有任意數目的其他 plex。 每個網狀磁碟區包含一份 LUN 物件上的資料。 Plex 可以加入，並從 LUN 物件中移除。

大部分的 DiskRAID 命令作用於特定的主機匯流排介面卡 (HBA) 連接埠、 啟動器介面卡、 啟動器入口網站、 提供者、 子系統、 控制器、 連接埠、 磁碟機、 LUN、 目標入口網站、 目標或目標入口網站群組。 您可以使用 SELECT 命令來選取物件。 選取的物件即具有焦點。 焦點可簡化常見的組態工作，例如建立多個 Lun 使用相同的子系統內。

> [!NOTE]
> DiskRAID 命令列工具只適用於支援虛擬磁碟服務 (VDS) 的儲存子系統。

## <a name="diskraid-commands"></a>DiskRAID 命令

若要檢視命令語法，請按一下命令：
-   [add](#BKMK_1)
-   [associate](#BKMK_2)
-   [automagic](#BKMK_3)
-   [break](#BKMK_4)
-   [chap](#BKMK_5)
-   [create](#BKMK_6)
-   [delete](#BKMK_7)
-   [detail](#BKMK_8)
-   [dissociate](#BKMK_9)
-   [exit](#BKMK_10)
-   [extend](#BKMK_11)
-   [flushcache](#BKMK_12)
-   [說明](#BKMK_13)
-   [importtarget](#BKMK_14)
-   [initiator](#BKMK_15)
-   [invalidatecache](#BKMK_16)
-   [lbpolicy](#BKMK_18)
-   [list](#BKMK_19)
-   [login](#BKMK_20)
-   [logout](#BKMK_21)
-   [maintenance](#BKMK_22)
-   [name](#BKMK_23)
-   [offline](#BKMK_24)
-   [online](#BKMK_25)
-   [recover](#BKMK_26)
-   [reenumerate](#BKMK_27)
-   [refresh](#BKMK_28)
-   [rem](#BKMK_29)
-   [remove](#BKMK_30)
-   [replace](#BKMK_31)
-   [reset](#BKMK_32)
-   [select](#BKMK_33)
-   [setflag](#BKMK_34)
-   [shrink](#BKMK_shrink)
-   [standby](#BKMK_35)
-   [unmask](#BKMK_36)

### <a name="BKMK_1"></a>add

將現有的 LUN 新增至目前選取的 LUN，或將 iSCSI 目標入口網站新增至目前選取的 iSCSI 目標入口網站群組。

#### <a name="syntax"></a>語法

```
add plex lun=n [noerr]
add tpgroup tportal=n [noerr]
```

#### <a name="parameters"></a>參數

**網狀 lun**=*n*

指定要將目前所選的 LUN 新增為一個網狀結構的 LUN 編號。

> [!CAUTION]
> 將刪除新增為一個網狀結構的 LUN 上的所有資料。

**tpgroup 網站 = * * * n*

指定的 iSCSI 目標入口網站號碼加入目前選取的 iSCSI 目標入口網站群組。

**noerr**

指定執行此作業時，會發生任何失敗將會被忽略。 這是用於指令碼模式。

### <a name="BKMK_2"></a>associate

設定指定的控制器清單的連接埠為作用中目前選取的 LUN （其他連接埠會變成非作用中控制器），或將指定的控制器的連接埠新增至現有的作用中控制器連接埠的清單中，針對目前所選的 LUN，或產生關聯指定的 iSCSI 目標，針對目前所選的 LUN。

#### <a name="syntax"></a>語法

```
associate controllers [add] <n>[,<n> [,…]]
associate ports [add] <n-m>[,<n-m>[,…]]
associate targets [add] <n>[,<n> [,…]]
```

#### <a name="parameters"></a>參數

**controllers**

VDS 1.0 提供者搭配使用。 若要新增或取代目前所選的 LUN 相關聯的控制站的清單。

**ports**

VDS 1.1 提供者搭配使用。 若要新增或取代目前所選的 LUN 相關聯的控制站連接埠清單。

**targets**

VDS 1.1 提供者搭配使用。 若要新增或取代目前所選的 LUN 相關聯的 iSCSI 目標的清單。

**add**

VDS 1.0 提供者，會將指定的控制器新增至現有的 LUN 相關聯的控制站的清單。 如果未指定此參數，控制站的清單會取代現有的這個 LUN 相關聯的控制站的清單。

VDS 1.1 提供者，會將指定的控制器的連接埠新增至現有的控制站與 LUN 關聯的連接埠的清單。 如果未指定此參數，控制站的連接埠清單會取代現有的控制站與這個 LUN 關聯的連接埠的清單。
```
<n>[,<n> [, ...]]
```
搭配**控制器**或是**目標**參數。 指定控制器或 iSCSI 目標設定為使用中或關聯的數的字。
```
<n-m>[,<n-m>[,…]]
```
搭配**連接埠**參數。 指定的控制器連接埠，以設定使用中使用控制器編號 (*n*) 和連接埠號碼 (*m*) 組。

#### <a name="example"></a>範例

下列範例示範如何建立關聯，並將連接埠新增至 LUN 使用一個 VDS 1.1 提供者：
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

設定或清除旗標，提供有關如何設定 LUN 的提供者的提示。 不搭配任何參數， **automagic**作業會顯示一份旗標。

#### <a name="syntax"></a>語法

```
automagic {set | clear | apply} all <flag=value> [<flag=value> [...]]
```

#### <a name="parameters"></a>參數

**set**

將指定的旗標設定為指定的值。

**clear**

清除指定的旗標。 **所有**關鍵字會清除所有 automagic 旗標。

**apply**

適用於目前的旗標的所選的 LUN。

\<flag>

旗標會識別三個字母的縮寫。

|Flag|描述|
|----|-----------|
|FCR|所需的快速的當機復原|
|FTL|具有容錯功能|
|MSR|大部分的讀取|
|MXD|最大的磁碟機|
|MXS|預期的大小上限|
|ORA|最佳讀取對齊方式|
|OR|最佳讀取大小|
|OSR|針對循序讀取最佳化|
|OSW|為循序寫入進行最佳化|
|OWA|最佳寫入對齊方式|
|OWS|最佳的寫入大小|
|RBP|重建優先順序|
|RBV|讀取回確認啟用|
|RMP|啟用重新對應|
|STS|等量磁碟區大小|
|WTC|直接寫入式快取已啟用|
|YNK|卸除式|

### <a name="BKMK_4"></a>中斷

移除目前選取的 LUN 網狀磁碟區。 不會保留網狀磁碟區和它所包含的資料，並可能已經被回收的磁碟機範圍。

#### <a name="syntax"></a>語法

```
break plex=<plex_number> [noerr]
```

#### <a name="parameters"></a>參數

**plex**

指定要移除的數目。 網狀磁碟區和它所包含的資料不會保留，並將回收這個網狀的磁碟區所使用的資源。 包含的 LUN 上的資料不保證一致。 如果您想要保留這個網狀的磁碟區，請使用 磁碟區陰影複製服務 (VSS)。

**noerr**

指定執行此作業時，會發生任何失敗將會被忽略。 這是用於指令碼模式。

#### <a name="remarks"></a>備註

> [!NOTE]
> 您必須使用之前先選取鏡像的 LUN**中斷**命令。

> [!CAUTION]
> 將刪除網狀磁碟區上的所有資料。

> [!CAUTION]
> 包含在原始的 LUN 上的所有資料不保證一致。

### <a name="BKMK_5"></a>chap

設定 Challenge Handshake 驗證通訊協定 (CHAP) 共用祕密，因此，iSCSI 啟動器和 iSCSI 目標可以與彼此進行通訊。

#### <a name="syntax"></a>語法

```
chap initiator set secret=[<secret>] [target=<target>]
chap initiator remember secret=[<secret>] target=<target>
chap target set secret=[<secret>] [initiator=<initiatorname>]
chap target remember secret=[<secret>] initiator=<initiatorname>
```

#### <a name="parameters"></a>參數

**啟動者設定**

設定共用的密碼在本機的 iSCSI 啟動器服務使用相互 CHAP 驗證啟動器驗證目標時。

**請記住啟動器**

通訊的 iSCSI 目標，以便在本機 iSCSI 啟動器服務的 CHAP 密碼，以便起始端服務可以使用的密碼，以便在 CHAP 驗證期間向目標驗證自己。

**target set**

在使用 CHAP 驗證目標驗證啟動器時的目前選取的 iSCSI 目標設定的共用的密碼。

**請記住目標**

通訊是到目前的焦點中 iSCSI 目標的 iSCSI 啟動器的 CHAP 密碼，好讓目標可以使用密碼，以便向啟動器相互 CHAP 驗證期間驗證自己。

**secret**

指定要使用的密碼。 如果是空的將會清除密碼。

**target**

密碼相關聯的目前選取的子系統在指定的目標。 在起始端上設定密碼時，這是選擇性，並留下任何它指出密碼將用於所有的目標已經沒有相關聯的密碼。

**initiatorname**

指定起始端會使用 iSCSI 名稱相關聯的密碼。 這是選擇性的在目標上設定密碼時，它留下表示密碼將用於還沒有相關聯的密碼的所有啟動器。

### <a name="BKMK_6"></a>建立

在目前選取的子系統上建立新的 LUN 或 iSCSI 目標，或目前選取的目標上建立目標入口網站群組。 您可以檢視實際的繫結使用**DiskRAID 清單**命令。

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

建立等量的 LUN。

**RAID**

建立等量的 LUN 具同位檢查。

**mirror**

建立鏡像的 LUN。

**automagic**

建立 LUN，使用*automagic*目前作用中的提示。 請參閱**automagic**子命令，如需詳細資訊。

**size**=

指定 LUN 大小總計 （mb）。 如果**大小 =** 未指定參數，建立 LUN 將會允許所有指定的磁碟機的最大可能大小。

提供者通常會建立 LUN 至少與所要求的大小一樣大，但提供者可能要捨入為下一個最大大小，在某些情況下。 比方說，如果大小指定為.99 GB 和提供者只能配置 GB 延伸磁碟區，所產生的 LUN 會是 1 GB。

若要指定使用其他單位的大小，請大小後可立即使用其中一個可辨識下列尾碼：
-   **B**個位元組。
-   **KB**的 kb。
-   **MB**的 mb 數。
-   **GB**的 gb。
-   **TB**的 tb。
-   **PB**為 pb。

**磁碟機**=

指定*drive_number*来用來建立 LUN 磁碟機。 如果**大小 =** 未指定參數，建立 LUN 是所有指定的磁碟機所允許的最大可能大小。 如果**大小 =** 參數指定，則提供者會從指定的磁碟機清單中，建立 LUN 選取磁碟機。 提供者會嘗試盡可能指定的順序中使用的磁碟機。

**stripesize**=

指定的大小以 mb 為單位，如*stripe*或是*RAID* LUN。 建立 LUN 之後，就無法變更 stripesize。

若要指定使用其他單位的大小，請大小後可立即使用其中一個可辨識下列尾碼：
-   **B**個位元組。
-   **KB**的 kb。
-   **MB**的 mb 數。
-   **GB**的 gb。
-   **TB**的 tb。
-   **PB**為 pb。

**target**

在目前選取的子系統上建立新的 iSCSI 目標。

**name**

提供目標的易記名稱。

**iscsiname**

提供 iSCSI 目標的名稱，而且可以省略讓提供者產生的名稱。

**tpgroup**

在目前選取的目標上建立新的 iSCSI 目標入口網站群組。

**noerr**

指定執行此作業時，會發生任何失敗將會被忽略。 這是用於指令碼模式。

#### <a name="remarks"></a>備註

-   任一**大小**= 或**磁碟機**= 必須指定參數。 它們也可用在一起。
-   建立之後，就無法變更 LUN 的等量磁碟區大小。

### <a name="BKMK_7"></a>delete

刪除目前選取的 LUN，iSCSI 目標 （只要有沒有任何相關聯的 iSCSI 目標的 Lun） 或 iSCSI 目標入口網站群組。

#### <a name="syntax"></a>語法

```
delete lun [uninstall] [noerr]
delete target [noerr]
delete tpgroup [noerr]
```

#### <a name="parameters"></a>參數

**lun**

刪除目前選取的 LUN 和其上的所有資料。

**uninstall**

指定，與 LUN 關聯的本機系統上的磁碟將會清除前 LUN 會被刪除。

**target**

如果沒有 Lun 與目標相關聯，請刪除目前選取的 iSCSI 目標。

**tpgroup**

刪除目前選取的 iSCSI 目標入口網站群組。

**noerr**

指定執行此作業時，會發生任何失敗將會被忽略。 這是用於指令碼模式。

### <a name="BKMK_8"></a>詳細資料

會顯示有關目前選取的物件，指定型別的詳細的資訊。

#### <a name="syntax"></a>語法

```
Detail {hbaport | iadapter | iportal | provider | subsystem | controller | port | drive | lun | tportal | target | tpgroup} [verbose]
```

#### <a name="parameters"></a>參數

**hbaport**

列出有關目前選取的主機匯流排介面卡 (HBA) 連接埠的詳細的資訊。

**iadapter**

列出有關目前選取的 iSCSI 啟動器介面卡的詳細的資訊。

**iportal**

列出有關目前選取的 iSCSI 啟動器入口網站的詳細的資訊。

**provider**

列出有關目前選取的提供者的詳細的資訊。

**subsystem**

列出有關目前選取的子系統的詳細的資訊。

**controller**

列出有關目前選取的控制器的詳細的資訊。

**port**

列出有關目前選取的控制器通訊埠的詳細的資訊。

**drive**

列出有關目前選取的磁碟機，包括佔據的 Lun 的詳細的資訊。

**lun**

清單的詳細的資訊的目前選取的 LUN，包括提供磁碟機。 輸出稍有不同，根據 LUN 是光纖通道或 iSCSI 子系統的一部分。 如果未經遮罩處理的主機清單會包含只有一個星號，這表示 LUN 會揭露給所有主機。

**tportal**

列出有關目前選取的 iSCSI 目標入口網站的詳細的資訊。

**target**

列出有關目前選取的 iSCSI 目標的詳細的資訊。

**tpgroup**

列出有關目前選取的 iSCSI 目標入口網站群組的詳細的資訊。

**verbose**

只能搭配 LUN 參數使用。 列出其他的資訊，包括其 plex。

### <a name="BKMK_9"></a>dissociate

集合指定為非作用中控制器的連接埠清單，針對目前所選的 LUN （另一個控制器不會受到影響的連接埠），或針對目前所選的 LUN 分離指定的 iSCSI 目標清單。

#### <a name="syntax"></a>語法

```
dissociate controllers <n> [,<n> [,...]]
dissociate ports <n-m>[,<n-m>[,…]]
dissociate targets <n> [,<n> [,…]]
```

#### <a name="parameter"></a>參數

**controllers**

VDS 1.0 提供者搭配使用。 移除目前選取的 LUN 相關聯的控制站的清單中的控制站。

**ports**

VDS 1.1 提供者搭配使用。 從控制器相關聯的目前選取的 LUN 的連接埠的清單中移除控制器的連接埠。

**targets**

VDS 1.1 提供者搭配使用。 從 iSCSI 目標與目前所選的 LUN 相關聯的清單中移除目標。
```
<n> [,<n> [,…]]
```
搭配**控制器**或是**目標**參數。 指定控制器或 iSCSI 目標設定為非作用中或中斷關聯的數的字。
```
<n-m>[,<n-m>[,…]]
```
搭配**連接埠**參數。 指定使用控制器編號設定為非作用中控制器的連接埠 (*n*) 和連接埠號碼 (*m*) 組。

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

### <a name="BKMK_10"></a>exit

結束 DiskRAID。

#### <a name="syntax"></a>語法

```
exit
```

### <a name="BKMK_11"></a>extend

LUN 的結尾加入磁區，以擴充目前選取的 LUN。 並非所有提供者支援擴充 Lun。 不會延伸任何磁碟區或 LUN 上包含的檔案系統。 擴充 LUN 之後，您應該擴充使用相關聯的磁碟內存結構**DiskPart 擴充**命令。

#### <a name="syntax"></a>語法

```
extend lun [size=<LUN_size>] [drives=<drive_number>, [<drive_number>, ...]] [noerr]
```

#### <a name="parameters"></a>參數

**size=**

指定的大小以 mb 為單位來擴充 LUN。 如果**大小 =** 參數未指定，所有指定的磁碟機所允許的最大可能大小來擴充 LUN。 如果**大小 =** 參數指定，則提供者從所指定的清單中選取磁碟機**磁碟機 =** 參數，建立 LUN。

若要指定使用其他單位的大小，請大小後可立即使用其中一個可辨識下列尾碼：
-   **B**個位元組。
-   **KB**的 kb。
-   **MB**的 mb 數。
-   **GB**的 gb。
-   **TB**的 tb
-   **PB**為 pb

**drives=**

指定\<drive_number > 針對建立 LUN 時要使用的磁碟機。 如果**大小 =** 未指定參數，建立 LUN 是所有指定的磁碟機所允許的最大可能大小。 提供者會使用磁碟機，盡可能指定的順序。

**noerr**

指定應該忽略執行此作業時，會發生任何失敗。 這是用於指令碼模式。

#### <a name="remarks"></a>備註

任一*大小*或\<磁碟機 > 必須指定參數。 它們也可用在一起。

### <a name="BKMK_12"></a>flushcache

清除目前選取的控制站上快取。

#### <a name="syntax"></a>語法

```
flushcache controller
```

### <a name="BKMK_13"></a>說明

顯示所有 DiskRAID 命令的清單。

#### <a name="syntax"></a>語法

```
help
```

### <a name="BKMK_14"></a>importtarget

擷取或設定目前的磁碟區陰影複製服務 (VSS) 匯入目標設定為目前所選的子系統。

#### <a name="syntax"></a>語法

```
importtarget subsystem [set target]
```

#### <a name="parameter"></a>參數

**設定目標**

若指定，請至 VSS 匯入目標，目前選取的子系統設定目前選取的目標。 如果未指定，命令會擷取目前的 VSS 匯入目標設定為目前所選的子系統。

### <a name="BKMK_15"></a>啟動者

擷取本機 iSCSI 啟動器的相關資訊。

#### <a name="syntax"></a>語法

```
initiator
```

### <a name="BKMK_16"></a>invalidatecache

目前選取的控制器上的快取會導致無效。

#### <a name="syntax"></a>語法

```
invalidatecache controller
```

### <a name="BKMK_18"></a>lbpolicy

設定目前選取的 LUN 上的負載平衡原則。

#### <a name="syntax"></a>語法

```
lbpolicy set lun type=<type> [paths=<path>-{primary | <weight>}[,<path>-{primary | <weight>}[,…]]]
lbpolicy set lun paths=<path>-{primary | <weight>}[,<path>-{primary | <weight>}[,…]]
```

#### <a name="parameters"></a>參數

**type**

指定負載平衡原則。 如果未指定類型，則**路徑**必須指定參數。 類型可以是下列其中一項：

**容錯移轉**:使用其他正在備份路徑的路徑中的一個主要路徑。

**循環配置資源**:依序嘗試每個路徑的循環配置資源方式，會使用所有的路徑。

**SUBSETROUNDROBIN**:所有主要路徑用於循環配置資源方式;所有主要路徑失效時，才使用備份的路徑。

**DYNLQD**:會使用具有最少的路徑使用中要求的數目。

**加權**:具有最低加權 （每個路徑必須被指派一個權數），會使用的路徑。

**LEASTBLOCKS**:使用最少的區塊中的路徑。

**VENDORSPECIFIC**:使用廠商專屬的原則。

**paths**

指定路徑是否**主要**或具有特定\<權數 >。 未指定任何路徑以隱含方式設定為備份。 任何列出的路徑必須是目前選取的 LUN 途徑的其中之一。

### <a name="BKMK_19"></a>list

顯示指定的型別之物件的清單。

#### <a name="syntax"></a>語法

```
List {hbaports | iadapters | iportals | providers | subsystems | controllers | ports | drives | LUNs | tportals | targets | tpgroups}
```

#### <a name="parameters"></a>參數

**hbaports**

列出所有已知 VDS 的 HBA 連接埠的相關摘要資訊。 目前選取的 HBA 連接埠會標示星號 （*）。

**iadapters**

列出所有已知 VDS 的 iSCSI 啟動器介面卡的摘要資訊。 目前選取的啟動器介面卡會以星號 （*） 標示。

**iportals**

列出所有的 iSCSI 啟動器入口網站中目前選取的啟動器介面卡的摘要資訊。 目前選取的啟動者入口網站會以星號 （*） 標示。

**提供者**

列出每個提供者知道 VDS 的相關摘要資訊。 目前選取的提供者會以星號 （*） 標示。

**subsystems**

列出系統中的每個子系統的摘要資訊。 目前選取的子系統會標示星號 （*）。

**controllers**

列出目前選取的子系統中的每個控制站的相關摘要資訊。 目前選取的控制器會以星號 （*） 標示。

**ports**

列出目前選取的控制器中的每個控制站連接埠的相關摘要資訊。 目前選取的連接埠會標示星號 （*）。

**drives**

列出目前選取的子系統中的每個磁碟機的摘要資訊。 目前選取的磁碟機是以星號 （*） 標示。

**luns**

列出目前選取的子系統中的每個 LUN 的摘要資訊。 目前選取的 LUN 會標示星號 （*）。

**tportals**

列出所有目前選取的子系統中的 iSCSI 目標入口網站的相關摘要資訊。 目前選取的目標入口網站會以星號 （*） 標示。

**targets**

列出所有目前選取的子系統中的 iSCSI 目標的摘要資訊。 目前選取的目標是以星號 （*） 標示。

**tpgroups**

列出所有 iSCSI 目標入口網站中的群組目前選取的目標的摘要資訊。 目前選取的入口網站群組會標示星號 （*）。

### <a name="BKMK_20"></a>登入

到目前所選的 iSCSI 目標記錄指定的 iSCSI 啟動器介面卡。

#### <a name="syntax"></a>語法

```
login target iadapter=<iadapter> [type={manual | persistent | boot}] [chap={none | oneway | mutual}] [iportal=<iportal>] [tportal=<tportal>] [<flag> [<flag> […]]]
```

#### <a name="parameters"></a>參數

**type**

指定要執行的登入的類型：**手動**，**持續**，或**開機**。 如果未指定，將會執行手動登入。

**手動**-登入以手動方式。

**持續性**-自動重新啟動電腦時，使用相同的登入。

**開機**-(這個選項適用於未來的開發，目前未使用 *。*)

**chap**

指定要使用 CHAP 驗證類型：**無**， **oneway** CHAP、 或**相互**CHAP; 如果未指定，則會使用任何驗證。

**tportal**

指定目前選取要用於登入子系統中的選擇性目標入口網站。

**iportal**

指定選擇性的啟動者入口網站中指定的啟動器介面卡，用於登入。

\<flag>

以三個字母縮寫來識別：

**IP**:需要 IPsec

**EMP**:啟用多重路徑

**EHD**:啟用標頭摘要

**EDD**:啟用資料摘要

### <a name="BKMK_21"></a>登出

記錄指定的 iSCSI 啟動器介面卡，從目前選取的 iSCSI 目標。

#### <a name="syntax"></a>語法

```
logout target iadapter= <iadapter>
```

#### <a name="parameters"></a>參數

**iadapter**

從登出與登入工作階段指定的啟動器介面卡。

### <a name="BKMK_22"></a>維護

執行指定之型別的目前選取的物件上的維護作業。

#### <a name="syntax"></a>語法

```
maintenance <object operation> [count=<iteration>]
```

#### <a name="parameters"></a>參數

\<object>

指定要執行此作業的物件類型。 *物件*型別可以是**子系統**，**控制器**，**連接埠，磁碟機**或**LUN**。

\<operation>

指定要執行的維護作業。 *作業*型別可以是**spinup**， **spindown**，**閃爍**，**嗶聲**或**ping**. *作業*必須指定。

**count=**

指定要重複的次數*作業*。 這通常會搭配**閃爍**，**嗶聲**，或**ping**。

### <a name="BKMK_23"></a>name

設定目前選取的子系統、 LUN 或 iSCSI 目標的易記名稱指定的名稱。

#### <a name="syntax"></a>語法

```
name {subsystem | lun | target} [<name>]
```

#### <a name="parameter"></a>參數

\<name>

指定的子系統，LUN 或目標的名稱。 名稱必須少於 64 個字元的長度。 如果未提供名稱，現有的名稱，如果有的話，會刪除。

### <a name="BKMK_24"></a>offline

設定目前選取的物件，指定型別的狀態**離線**。

#### <a name="syntax"></a>語法

```
offline <object>
```

#### <a name="parameter"></a>參數

\<object>

指定要執行這項作業的物件類型。 \<物件 >

類型可以是**子系統**，**控制站**，**磁碟機**， **LUN**，或**網站**。

### <a name="BKMK_25"></a>online

設定指定的型別所選取物件的狀態**線上**。 如果物件是**hbaport**，路徑的狀態變更為 以目前選取的 HBA 連接埠**線上**。

#### <a name="syntax"></a>語法

```
online <object> 
```

#### <a name="parameter"></a>參數

\<object>

指定要執行這項作業的物件類型。 \<物件 >

類型可以是**hbaport**，**子系統**，**控制器**，**磁碟機**， **LUN**，或**網站**。

### <a name="BKMK_26"></a>recover

執行必要的例如重新同步處理 」 或 「 最忙碌的運轉時，若要修復的目前選取的容錯 LUN 的作業。 例如，復原可能會造成的熱備援，繫結至具有失敗的磁碟或其他磁碟範圍重新配置的 RAID 集。

#### <a name="syntax"></a>語法

```
recover <lun>
```

### <a name="BKMK_27"></a>reenumerate

Reenumerates 指定型別的物件。 如果您使用擴充 LUN 命令時，您必須使用重新整理 命令之前使用 reenumerate 命令更新的磁碟大小。

#### <a name="syntax"></a>語法

```
reenumerate {subsystems | drives}
```

#### <a name="parameters"></a>參數

**subsystems**

查詢來探索任何新的子系統，在目前選取的提供者中所新增的提供者。

**drives**

查詢內部 I/O 匯流排來探索新的磁碟機加入目前選取的子系統中。

### <a name="BKMK_28"></a>refresh

內部的資料重新整理目前選取的提供者。

#### <a name="syntax"></a>語法

```
refresh provider
```

### <a name="BKMK_29"></a>rem

用來註解指令碼。

#### <a name="syntax"></a>語法

```
Rem <comment>
```

### <a name="BKMK_30"></a>remove

從目前選取的目標入口網站群組中移除指定的 iSCSI 目標入口網站。

#### <a name="syntax"></a>語法

```
remove tpgroup tportal=<tportal> [noerr]
```

#### <a name="parameter"></a>參數

**tpgroup tportal=** \<tportal>

指定要移除的 iSCSI 目標入口網站。

**noerr**

指定應該忽略執行此作業時，會發生任何失敗。 這是用於指令碼模式。

### <a name="BKMK_31"></a>replace

指定的磁碟機取代目前選取的磁碟機。

#### <a name="syntax"></a>語法

```
replace drive=<drive_number>
```

#### <a name="parameter"></a>參數

**drive=**

指定\<drive_number > 更換磁碟機。

#### <a name="remarks"></a>備註

-   指定的磁碟機可能不是目前選取的磁碟機。

### <a name="BKMK_32"></a>reset

重設目前所選的控制器或連接埠。

#### <a name="syntax"></a>語法

```
Reset {controller | port}
```

#### <a name="parameters"></a>參數

**controller**

重設控制器。

**port**

重設連接埠。

### <a name="BKMK_33"></a>select

顯示或變更目前選取的物件。

#### <a name="syntax"></a>語法

```
Select {hbaport | iadapter | iportal | provider | subsystem | controller | port | drive | lun | tportal | target | tpgroup } [<n>]
```

#### <a name="parameters"></a>參數

**object**

指定要選取物件的類型。 \<物件 > 類型可以是**提供者**，**子系統**，**控制器**，**磁碟機**，或**LUN**.

**hbaport** [\<n>]

將焦點設定至指定的本機 HBA 通訊埠。 如果未不指定任何 HBA 通訊埠，則命令會顯示目前選取的 HBA 通訊埠 （如果有的話）。 指定 HBA 連接埠索引若無效會導致沒有聚焦的 HBA 通訊埠。 取消選取 HBA 通訊埠選取任何所選的啟動器介面卡和起始端入口網站。

**iadapter** [\<n>]

將焦點設定至指定的本機 iSCSI 啟動器介面卡。 如果未不指定任何啟動器介面卡，則命令會顯示目前所選的啟動器介面卡 （如果有的話）。 指定無效的啟動器介面卡索引會導致沒有聚焦的啟動器介面卡。 選取的啟動器介面卡會取消選取任何選取的 HBA 連接埠和起始端入口網站。

**iportal** [\<n>]

將焦點設定至指定的本機 iSCSI 啟動器入口網站內選取的 iSCSI 啟動器介面卡。 如果未不指定任何啟動器入口網站，則命令會顯示目前選取的啟動者入口網站 （如果有的話）。 指定無效的啟動者入口網站索引會導致未選取的啟動者入口網站。

**provider** [\<n>]

將焦點設定至指定的提供者。 如果未不指定任何提供者，則命令會顯示目前選取的提供者 （如果有的話）。 指定無效的提供者索引會導致沒有聚焦的提供者。

**subsystem** [\<n>]

將焦點設定至指定的子系統。 如果未不指定任何子系統，則此命令會顯示具有焦點的子系統，（如果有的話）。 指定無效的子系統索引會導致未設為焦點的子系統。 隱含地選取子系統會選取其相關聯的提供者。

**controller** [\<n>]

將焦點設定至指定的控制器內的目前選取的子系統。 如果未不指定任何控制器，則命令會顯示目前選取的控制器 （如果有的話）。 指定無效的控制站索引會導致沒有聚焦的控制站。 選取控制站會取消選取任何選取的控制器的連接埠、 磁碟機、 Lun、 目標入口網站、 目標和入口網站的目標群組。

**port** [\<n>]

將焦點設定至目前選取的控制器內指定的控制器連接埠。 如果未不指定任何連接埠，則命令會顯示目前選取的連接埠 （如果有的話）。 指定連接埠無效索引會導致選取的連接埠。

**drive** [\<n>]

將焦點設定至指定的磁碟機或實體磁針，目前選取的子系統內。 如果未不指定任何磁碟機，則命令會顯示目前選取的磁碟機 （如果有的話）。 指定無效的磁碟機索引會不造成任何設為焦點的磁碟機。 選取磁碟機，取消選取任何所選的控制器、 控制器的連接埠、 Lun、 目標入口網站、 目標和入口網站的目標群組。

**lun** [\<n>]

將焦點設定到目前所選的子系統內指定的 LUN。 如果未不指定任何 LUN，則命令會顯示目前選取的 LUN （如果有的話）。 指定 LUN 索引若無效會不導致任何所選的 LUN。 選取 LUN 會取消選取任何所選的控制器、 控制器的連接埠、 磁碟機、 目標入口網站、 目標和入口網站的目標群組。

**tportal** [\<n>]

將焦點設定至指定的 iSCSI 目標入口網站目前所選的子系統內。 如果未不指定任何目標入口網站，則命令會顯示目前選取的目標入口網站 （如果有的話）。 指定無效的目標入口網站索引會導致未選取的目標入口網站。 選取目標入口網站會取消選取任何控制器、 控制器的連接埠、 磁碟機、 Lun、 目標和入口網站的目標群組。

**target** [\<n>]

將焦點設定至目前選取的子系統內指定的 iSCSI 目標。 如果未指定目標，此命令會顯示目前選取的目標，（如果有的話）。 指定無效的目標索引會導致選取的目標。 選取的目標，取消選取任何控制器、 控制器的連接埠、 磁碟機、 Lun、 目標入口網站和入口網站的目標群組。

**tpgroup** [\<n>]

將焦點設定至指定的 iSCSI 目標入口網站群組內的目前選取的 iSCSI 目標。 如果未不指定任何目標入口網站的群組，此命令會顯示目前選取的目標入口網站群組 （如果有的話）。 指定無效的目標入口網站的群組索引會導致沒有焦點在 目標入口網站的群組。

[\<n>]

指定\<物件 > 來選取。 如果<object number>指定為不是有效的會清除任何現有的選取項目之指定型別的物件。 如果沒有<object number>指定，會顯示目前的物件。

### <a name="BKMK_34"></a>setflag

設定目前選取的磁碟機為熱備援。

#### <a name="syntax"></a>語法

```
setflag drive hotspare={true | false}
```

#### <a name="parameters"></a>參數

**true**

選取目前選取的磁碟機做為熱備援。

**false**

為熱備援，取消選取目前選取的磁碟機。

#### <a name="remarks"></a>備註

熱備援無法用於一般的 LUN 繫結作業。 它們是保留供錯誤只處理。 磁碟機必須未被目前繫結至任何現有的 LUN。

### <a name="BKMK_shrink"></a>壓縮

降低所選 LUN 的大小。

#### <a name="syntax"></a>語法

```
shrink lun size=<n> [noerr]
```

#### <a name="parameters"></a>參數

**size=**

依指定所需的空間 (mb)，以減少大小的 lun 數量。 若要指定使用其他單位的大小，使用其中一個可辨識的尾碼 （B，KB、 MB、 GB、 TB 和 PB） 大小的後面。

**noerr**

指定執行此作業時，會發生任何失敗將會被忽略。 這是用於指令碼模式。

### <a name="BKMK_35"></a>待命模式

路徑的狀態變更為 待命模式的目前選取的主機匯流排介面卡 (HBA) 連接埠。

#### <a name="syntax"></a>語法

```
standby hbaport
```

#### <a name="parameters"></a>參數

**hbaport**

路徑的狀態變更為 待命模式的目前選取的主機匯流排介面卡 (HBA) 連接埠。

### <a name="BKMK_36"></a>取消遮罩

可從指定的主機可存取目前所選的 Lun。

#### <a name="syntax"></a>語法

```
unmask LUN {all | none | [add] wwn=<hexadecimal_number> [;<hexadecimal_number> [;…]] | [add] initiator=<initiator>[;<initiator>[;…]]} [uninstall]
```

#### <a name="parameters"></a>參數

**all**

指定的 LUN 應可從所有主機存取。 不過，您無法取消遮罩 LUN 至 iSCSI 子系統中的所有目標。

> [!IMPORTANT]
> 您必須登出之前執行所有取消遮罩的命令目標。

**none**

指定不應至任何主機存取 LUN。

> [!IMPORTANT]
> 您必須登出再執行取消遮罩 LUN 無命令目標。

**add**

指定指定的主機必須加入現有的這個 LUN 會從可存取的主機清單。 如果未指定此參數，提供的主控件清單會取代現有的這個 LUN 會從可存取的主機清單。

**WWN=**

指定十六進位數字，代表從中 LUN 或主機應在可存取的全球名稱的清單。 遮罩/取消遮罩為一組特定的主機光纖通道子系統中，您可以在感興趣的主機電腦上，輸入 WWN 的連接埠的分號分隔清單。

**initiator=**

指定要在目前選取的 LUN 應在可存取的 iSCSI 啟動器的清單。 遮罩/取消遮罩為一組特定的 iSCSI 子系統中的主機，您可以輸入感興趣的主機電腦上的啟動器的 iSCSI 啟動器名稱的分號分隔清單。

**uninstall**

如果指定，會解除安裝前 LUN 會遮罩相關聯的本機系統上的 LUN 的磁碟。

## <a name="scripting-diskraid"></a>指令碼 DiskRAID

DiskRAID 可以編寫指令碼相關聯的 VDS 硬體提供者與執行 Windows Server 2008 或 Windows Server 2003 的電腦上。 若要叫用 DiskRAID 指令碼，在命令提示字元中輸入：
```
diskraid /s <script.txt>
```
根據預設，DiskRAID 停止處理命令，並傳回錯誤碼，如果指令碼中的問題。 若要繼續執行指令碼，並忽略錯誤，包括 NOERR 參數上的命令。 這可讓這類有用的作法，以使用單一指令碼刪除 Lun 總數無論子系統中的所有 Lun。 並非所有的命令支援 NOERR 參數。 在命令語法錯誤，不論是否已包含 NOERR 參數，一律會傳回錯誤

### <a name="diskraid-error-codes"></a>DiskRAID 錯誤碼

|錯誤碼|錯誤描述|
|----------|-----------------|
|0|不發生任何錯誤。 將整個指令碼執行，而不會失敗。|
|1|發生嚴重的例外狀況。|
|2|DiskRAID 命令列上指定的引數不正確。|
|3|DiskRAID 無法開啟指定的指令碼或輸出檔。|
|4|其中一個 DiskRAID 所使用的服務傳回失敗。|
|5|發生命令語法錯誤。 指令碼失敗，因為未正確選取物件，或對該命令的使用不正確。|

## <a name="example-interactively-view-status-of-subsystem"></a>範例：以互動方式檢視狀態的子系統

如果您想要檢視您電腦上的子系統 0 的狀態，請在命令列中輸入下列：
```
diskraid
```
按 ENTER 鍵。 會顯示下列對話方塊：
```
Microsoft Diskraid version 5.2.xxxx
Copyright (©) 2003 Microsoft Corporation
On computer: COMPUTER_NAME
```
若要選取子系統 0，在 DiskRAID 提示字元中輸入下列命令：
```
select subsystem 0
```
按 ENTER 鍵。 會顯示類似下面的輸出：
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
若要結束 DiskRAID，在 DiskRAID 提示字元中輸入下列命令：
```
exit
```