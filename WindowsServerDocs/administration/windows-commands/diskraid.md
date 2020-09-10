---
title: Diskraid
description: 適用于 Diskraid 命令列工具的參考文章，可讓您設定和管理獨立 (的多餘陣列，或 (RAID) 儲存子系統的低成本) 磁片。
ms.topic: reference
ms.assetid: 20aef1e5-7641-47cf-b4eb-cda117f65b6e
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 49d190f257c93a026f29188fa26af7409c611f44
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89627165"
---
# <a name="diskraid"></a>Diskraid

**Diskraid** 是一種命令列工具，可讓您設定和管理獨立 (的多餘陣列，或 (RAID) 儲存子系統的低成本) 磁片。

RAID 通常會在伺服器上用來標準化及分類容錯磁片系統。 RAID 層級提供各種效能、可靠性及成本的混合。 有些伺服器提供三個 RAID 層級：層級 0 (等量) 、層級 1 (鏡像) ，以及層級 5 (與同位檢查) 等量分割。

硬體 RAID 子系統會使用邏輯單元編號 (LUN) 來區分實體可定址的儲存單位。 LUN 物件至少必須有一個 plex，而且可以有任意數目的其他 plex。 每個 plex 都包含 LUN 物件上的資料複本。 您可以在 LUN 物件中新增和移除 plex。

大部分的 Diskraid 命令都是在特定的主機匯流排介面卡上操作， (HBA) 埠、啟動器介面卡、啟動器入口網站、提供者、子系統、控制器、埠、磁片磁碟機、LUN、目標入口網站、目標或目標入口網站群組。 您可以使用 **select** 命令來選取物件。 選取的物件會被視為具有焦點。 焦點可簡化常見的設定工作，例如在相同子系統內建立多個 Lun。

> [!NOTE]
> Diskraid 命令列工具只適用于支援虛擬磁碟服務 (VDS) 的儲存子系統。

## <a name="diskraid-commands"></a>Diskraid 命令

您可以從 Diskraid 工具中使用下列命令。

### <a name="add"></a>add

將現有的 LUN 新增至目前選取的 LUN，或將 iSCSI 目標入口網站新增至目前選取的 iSCSI 目標入口網站群組。

#### <a name="syntax"></a>語法

```
add plex lun=n [noerr]
add tpgroup tportal=n [noerr]
```

##### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| plex lun =`<n>` | 指定要新增為目前所選 LUN 之 plex 的 LUN 編號。 注意：將會刪除 LUN 上新增為 plex 的所有資料。 |
| tpgroup 管理入口網站 =`<n>` | 指定要新增至目前所選 iSCSI 目標入口網站群組的 iSCSI 目標入口網站編號。 |
| noerr | 僅適合執行指令。 當發生錯誤時，Diskraid 會繼續處理命令，就像未發生錯誤一樣。 |

### <a name="associate"></a>副

針對目前選取的 LUN，將指定的控制器埠清單設定為作用中， (其他控制器埠變成非使用中) ，或將指定的控制器埠新增到目前所選 LUN 的現有作用中控制器埠清單，或將指定的 iSCSI 目標與目前選取的 LUN 相關聯。

#### <a name="syntax"></a>語法

```
associate controllers [add] <n>[,<n> [,…]]
associate ports [add] <n-m>[,<n-m>[,…]]
associate targets [add] <n>[,<n> [,…]]
```

##### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| controller | 新增或取代與目前選取之 LUN 相關聯的控制器清單。 只能搭配 VDS 1.0 提供者使用。 |
| 連接埠 | 新增或取代與目前選取之 LUN 相關聯的控制器埠清單。 只能搭配 VDS 1.1 提供者使用。 |
| 目標 | 新增或取代與目前選取之 LUN 相關聯的 iSCSI 目標清單。 只能搭配 VDS 1.1 提供者使用。 |
| add | **如果使用 VDS 1.0 提供者：** 將指定的控制器新增至與 LUN 相關聯的現有控制器清單。 如果未指定此參數，控制器清單會取代與此 LUN 相關聯的現有控制器清單。<p>**如果使用 VDS 1.1 提供者：** 將指定的控制器埠新增至與 LUN 相關聯的現有控制器埠清單。 如果未指定此參數，控制器埠清單會取代與此 LUN 相關聯的現有控制器埠清單。 |
| `<n>[,<n> [, ...]]` | 搭配 **控制器** 或 **目標** 參數使用。 指定要設為作用中或相關聯的控制器或 iSCSI 目標數目。 |
| `<n-m>[,<n-m>[,…]]` | 搭配 **埠** 參數使用。 使用控制器編號 (*n*) 和埠號碼 (*m*) 配對，指定要設定使用中的控制器埠。 |

#### <a name="example"></a>範例

若要建立埠的關聯，並將其新增至使用 VDS 1.1 提供者的 LUN：

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

### <a name="automagic"></a>automagic

設定或清除旗標，為提供者提供有關如何設定 LUN 的提示。 使用時不含任何參數， **automagic** 作業會顯示旗標清單。

#### <a name="syntax"></a>語法

```
automagic {set | clear | apply} all <flag=value> [<flag=value> [...]]
```

##### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| set | 將指定的旗標設定為指定的值。 |
| clear | 清除指定的旗標。 **All**關鍵字會清除所有 automagic 旗標。 |
| apply | 將目前的旗標套用至選取的 LUN。 |
| `<flag>` | 旗標以三個字母的縮寫來識別，包括：<ul><li>**FCR** -需要快速損毀復原</li><li>**FTL** -容錯</li><li>**MSR** -大部分讀取</li><li>**MXD** -最大磁片磁碟機</li><li>**MXS** -預期大小上限</li><li>**Tnsnames.ora** -最佳讀取對齊</li><li>**Or** -最佳讀取大小</li><li>**OSR** -針對連續讀取優化</li><li>**OSW** -針對順序寫入優化</li><li> **OWA** -最佳寫入調整</li><li>**OWS** -最佳寫入大小</li><li>**RBP** -重建優先順序</li><li>**RBV** -已啟用讀回確認</li><li>**RMP** -已啟用重新對應</li><li>**STS** -帶大尺寸</li><li>**WTC** -已啟用寫入快取</li><li>**YNK** -可移動</li></ul> |

### <a name="break"></a>break

從目前選取的 LUN 移除此 plex。 不會保留該 plex 及其包含的資料，而且可能會回收磁片區的範圍。

> [!CAUTION]
> 您必須先選取鏡像 LUN，才能使用此命令。 將會刪除該 plex 上的所有資料。 原始 LUN 上包含的所有資料都不保證一致。

#### <a name="syntax"></a>語法

```
break plex=<plex_number> [noerr]
```

##### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| 叢 | 指定要移除之 plex 的編號。 將不會保留其所包含的 plex 和資料，而且將會回收此 plex 所使用的資源。 LUN 上所含的資料不保證是一致的。 如果您想要保留此 plex，請使用磁碟區陰影複製服務 (VSS) 。 |
| noerr | 僅適合執行指令。 當發生錯誤時，Diskraid 會繼續處理命令，就像未發生錯誤一樣。 |

### <a name="chap"></a>章

將挑戰交握驗證通訊協定設定 (CHAP) 共用密碼，讓 iSCSI 啟動器和 iSCSI 目標可以彼此通訊。

#### <a name="syntax"></a>語法

```
chap initiator set secret=[<secret>] [target=<target>]
chap initiator remember secret=[<secret>] target=<target>
chap target set secret=[<secret>] [initiator=<initiatorname>]
chap target remember secret=[<secret>] initiator=<initiatorname>
```

##### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| 啟動器集合 | 設定本機 iSCSI 啟動器服務中的共用密碼，以便在起始端驗證目標時用於相互 CHAP 驗證。 |
| 啟動器記住 | 將 iSCSI 目標的 CHAP 秘密傳達給本機 iSCSI 啟動器服務，讓起始端服務可以使用秘密，在 CHAP 驗證期間對目標進行驗證。 |
| 目標集 | 設定目前選取的 iSCSI 目標中的共用密碼，在目標驗證啟動器時用於 CHAP 驗證。 |
| 目標記住 | 將 iSCSI 啟動器的 CHAP 秘密傳達給目前的焦點 iSCSI 目標，讓目標可以使用密碼，以便在相互 CHAP 驗證期間對啟動器進行驗證。 |
| secret | 指定要使用的秘密。 如果空白，則會清除秘密。 |
| 目標 | 在目前選取的子系統中，指定要與秘密建立關聯的目標。 這是在起始端設定秘密並離開時的選擇性選項，表示密碼將會用於尚未有相關密碼的所有目標。 |
| initiatorname | 指定要與秘密相關聯的啟動器 iSCSI 名稱。 當您在目標上設定秘密，並將其留出時，這是選擇性的，表示密碼將用於所有尚未有相關密碼的啟動器。 |

### <a name="create"></a>建立

在目前選取的子系統上建立新的 LUN 或 iSCSI 目標，或在目前選取的目標上建立目標入口網站群組。 您可以使用 [ **Diskraid 清單** ] 命令來查看實際的系結。

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

##### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| simple | 建立簡單的 LUN。 |
| 帶狀線 | 建立等量 LUN。 |
| Raid | 建立具有同位的等量 LUN。 |
| mirror | 建立鏡像 LUN。 |
| automagic | 使用目前作用中的 *automagic* 提示來建立 LUN。 如需詳細資訊，請參閱本文中的 **automagic** 子命令。 |
| size= | 指定 LUN 大小總計（以 mb 為單位）。 必須指定 **size**= 或 **磁片磁碟機**= 參數。 它們也可以一起使用。 如果未指定 **size =** 參數，則建立的 LUN 會是所有指定磁片磁碟機所允許的最大可能大小。<p>提供者通常會建立至少與所要求大小一樣大的 LUN，但是在某些情況下，提供者可能必須四捨五入至下一個最大的大小。 例如，如果將大小指定為 99 GB，而提供者只能配置 GB 的磁片區，則產生的 LUN 會是 1 GB。 若要使用其他單位來指定大小，請在大小之後立即使用下列其中一個可辨識的尾碼：<ul><li>**B** 位元組</li><li>**Kb** -kb</li><li>**Mb** -mb</li><li>**Gb** -gb</li><li>**Tb** -tb</li><li>**Pb** -pb。</li></ul> |
| 磁片磁碟機 = | 指定要用來建立 LUN 之磁片磁碟機的 *drive_number* 。 必須指定 **size**= 或 **磁片磁碟機**= 參數。 它們也可以一起使用。 如果未指定 **size =** 參數，則建立的 LUN 會是所有指定磁片磁碟機所允許的最大可能大小。 如果指定 **size =** 參數，提供者會從指定的磁片磁碟機清單中選取磁片磁碟機，以建立 LUN。 提供者會嘗試使用盡可能指定的順序來使用磁片磁碟機。 |
| stripesize = | 指定 *stripe* 或 *raid* LUN 的大小（以 mb 為單位）。 建立 LUN 之後，就無法變更 stripesize。 若要使用其他單位來指定大小，請在大小之後立即使用下列其中一個可辨識的尾碼：<ul><li>**B** 位元組</li><li>**Kb** -kb</li><li>**Mb** -mb</li><li>**Gb** -gb</li><li>**Tb** -tb</li><li>**Pb** -pb。</li></ul> |
| 目標 | 在目前選取的子系統上建立新的 iSCSI 目標。 |
| 名稱 | 提供目標的易記名稱。 |
| iscsiname | 提供目標的 iSCSI 名稱，而且可以省略，讓提供者產生名稱。 |
| tpgroup | 在目前選取的目標上建立新的 iSCSI 目標入口網站群組。 |
| noerr | 僅適合執行指令。 當發生錯誤時，Diskraid 會繼續處理命令，就像未發生錯誤一樣。 |

### <a name="delete"></a>delete

只要沒有任何 Lun 與 iSCSI 目標) 或 iSCSI 目標入口網站群組相關聯，就會刪除目前選取的 LUN （iSCSI 目標 (）。

#### <a name="syntax"></a>語法

```
delete lun [uninstall] [noerr]
delete target [noerr]
delete tpgroup [noerr]
```

##### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| lun | 刪除目前選取的 LUN 和其上的所有資料。 |
| uninstall | 指定在刪除 LUN 之前，將會清除與 LUN 相關聯之本機系統上的磁片。 |
| 目標 | 如果沒有與目標相關聯的 Lun，則刪除目前選取的 iSCSI 目標。 |
| tpgroup | 刪除目前選取的 iSCSI 目標入口網站群組。 |
| noerr | 僅適合執行指令。 當發生錯誤時，Diskraid 會繼續處理命令，就像未發生錯誤一樣。 |

### <a name="detail"></a>詳細資料

顯示目前選取之物件的相關詳細資訊。

#### <a name="syntax"></a>語法

```
detail {hbaport | iadapter | iportal | provider | subsystem | controller | port | drive | lun | tportal | target | tpgroup} [verbose]
```

##### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| hbaport | 列出目前選取的主機匯流排介面卡 (HBA) 埠的詳細資訊。 |
| iadapter | 列出目前所選 iSCSI 啟動器介面卡的詳細資訊。 |
| iportal | 列出目前所選 iSCSI 啟動器入口網站的詳細資訊。 |
| provider | 列出目前所選提供者的詳細資訊。 |
| 子系統 | 列出目前所選子系統的詳細資訊。 |
| controller | 列出目前所選控制器的詳細資訊。 |
| 連接埠 | 列出目前所選控制器埠的詳細資訊。 |
| 磁碟機 | 列出目前所選磁片磁碟機的詳細資訊，包括佔用的 Lun。 |
| lun | 列出目前所選 LUN 的詳細資訊，包括參與的磁片磁碟機。 輸出會因 LUN 是光纖通道或 iSCSI 子系統的一部分而稍有不同。 如果未遮罩的主機清單中只包含星號，這表示 LUN 不會被遮罩到所有主機。 |
| 管理入口網站 | 列出目前所選 iSCSI 目標入口網站的詳細資訊。 |
| 目標 | 列出目前所選 iSCSI 目標的詳細資訊。 |
| tpgroup | 列出目前所選 iSCSI 目標入口網站群組的詳細資訊。 |
| verbose | 僅用於 LUN 參數。 列出其他資訊，包括其 plex。 |

### <a name="dissociate"></a>中斷

針對目前選取的 LUN，將指定的控制器埠清單設定為非作用中 (其他控制器埠不會受到影響) ，或分離目前所選 LUN 的指定 iSCSI 目標清單。

#### <a name="syntax"></a>語法

```
dissociate controllers <n> [,<n> [,...]]
dissociate ports <n-m>[,<n-m>[,…]]
dissociate targets <n> [,<n> [,…]]
```

##### <a name="parameter"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| controllers | 從與目前選取之 LUN 相關聯的控制器清單中移除控制器。 只能搭配 VDS 1.0 提供者使用。 |
| 連接埠 | 從與目前選取之 LUN 相關聯的控制器埠清單中移除控制器埠。 只能搭配 VDS 1.1 提供者使用。 |
| 目標 | 從與目前選取之 LUN 相關聯的 iSCSI 目標清單中移除目標。 只能搭配 VDS 1.1 提供者使用。 |
| `<n> [,<n> [,…]]` | 搭配 **控制器** 或 **目標** 參數使用。 指定要設定為非作用中或中斷關聯的控制器或 iSCSI 目標數目。 |
| `<n-m>[,<n-m>[,…]]` | 搭配 **埠** 參數使用。 使用控制器編號 (*n*) 和埠號碼 (*m*) 配對，指定要設定為非作用中的控制器埠。 |

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

### <a name="exit"></a>exit

結束 Diskraid。

#### <a name="syntax"></a>語法

```
exit
```

### <a name="extend"></a>extend

藉由將磁區新增至 LUN 的結尾，來擴充目前選取的 LUN。 並非所有提供者都支援擴充 Lun。 不會擴充 LUN 上包含的任何磁片區或檔案系統。 擴充 LUN 之後，您應該使用 **DiskPart 擴充** 命令來擴充相關聯的磁片上結構。

#### <a name="syntax"></a>語法

```
extend lun [size=<LUN_size>] [drives=<drive_number>, [<drive_number>, ...]] [noerr]
```

##### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| 大小 | 指定擴充 LUN 的大小（以 mb 為單位）。 必須指定 *大小* 或 `<drive>` 參數。 它們也可以一起使用。 如果未指定 **size =** 參數，則會以所有指定磁片磁碟機所允許的最大可能大小來擴充 LUN。 如果指定 **size =** 參數，提供者會從 **磁片磁碟機 =** 參數所指定的清單中選取磁片磁碟機，以建立 LUN。 若要使用其他單位來指定大小，請在大小之後立即使用下列其中一個可辨識的尾碼：<ul><li>**B** 位元組</li><li>**Kb** -kb</li><li>**Mb** -mb</li><li>**Gb** -gb</li><li>**Tb** -tb</li><li>**Pb** -pb。</li></ul> |
| 磁片磁碟機 = | 指定 `<drive_number>` 建立 LUN 時要使用的磁片磁碟機。 必須指定 *大小* 或 `<drive>` 參數。 它們也可以一起使用。 如果未指定 **size =** 參數，則建立的 LUN 會是所有指定磁片磁碟機所允許的最大可能大小。 提供者會盡可能以指定的順序使用磁片磁碟機。 |
| noerr | 僅適合執行指令。 當發生錯誤時，Diskraid 會繼續處理命令，就像未發生錯誤一樣。 |

### <a name="flushcache"></a>flushcache

清除目前所選控制器上的快取。

#### <a name="syntax"></a>語法

```
flushcache controller
```

### <a name="help"></a>help

顯示所有 Diskraid 命令的清單。

#### <a name="syntax"></a>語法

```
help
```

### <a name="importtarget"></a>importtarget

取得或設定目前的磁碟區陰影複製服務 (VSS) 匯入目標，此為目前選取的子系統所設定。

#### <a name="syntax"></a>語法

```
importtarget subsystem [set target]
```

##### <a name="parameter"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| 設定目標 | 如果有指定，會將目前選取的目標設定為目前所選子系統的 VSS 匯入目標。 如果未指定，此命令會抓取目前所選子系統所設定的目前 VSS 匯入目標。 |

### <a name="initiator"></a>起始端

抓取本機 iSCSI 啟動器的相關資訊。

#### <a name="syntax"></a>語法

```
initiator
```

### <a name="invalidatecache"></a>invalidatecache

使目前選取之控制器上的快取失效。

#### <a name="syntax"></a>語法

```
invalidatecache controller
```

### <a name="lbpolicy"></a>lbpolicy

在目前選取的 LUN 上設定負載平衡原則。

#### <a name="syntax"></a>語法

```
lbpolicy set lun type=<type> [paths=<path>-{primary | <weight>}[,<path>-{primary | <weight>}[,…]]]
lbpolicy set lun paths=<path>-{primary | <weight>}[,<path>-{primary | <weight>}[,…]]
```

##### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| type | 指定負載平衡原則。 如果未指定類型，則必須指定 **path** 參數。 Type 可以是下列其中之一：<ul><li>**容錯移轉** -使用一個主要路徑與其他路徑作為備份路徑。</li><li>**ROUNDROBIN** -以迴圈配置資源的方式使用所有路徑，以依序嘗試每個路徑。</li><li>**SUBSETROUNDROBIN** -以迴圈配置資源的方式使用所有主要路徑;只有在所有主要路徑都失敗時，才會使用備份路徑。</li><li>**DYNLQD** -使用具有最少使用中要求數目的路徑。<li><li>**加權** -使用具有最小權數的路徑 (每個路徑都必須被指派權數) 。</li><li>**LEASTBLOCKS** -使用具有最少區塊的路徑。</li><li>**VENDORSPECIFIC** -使用廠商特定的原則。</li></ul> |
| path | 指定路徑是否為 **主要** 路徑，或是否有特定路徑 `<weight>` 。 未指定的任何路徑都會隱含地設定為備份。 列出的任何路徑都必須是目前選取的 LUN 路徑之一。 |

### <a name="list"></a>list

顯示指定類型的物件清單。

#### <a name="syntax"></a>語法

```
list {hbaports | iadapters | iportals | providers | subsystems | controllers | ports | drives | LUNs | tportals | targets | tpgroups}
```

##### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| hbaports | 列出 VDS 已知的所有 HBA 埠的相關摘要資訊。 目前選取的 HBA 埠會以星號標示 ( * ) 。 |
| iadapters | 列出所有已知于 VDS 的 iSCSI 啟動器介面卡的摘要資訊。 目前選取的啟動器介面卡會以星號標示 ( * ) 。 |
| iportals | 列出目前所選啟動器介面卡中所有 iSCSI 啟動器入口網站的相關摘要資訊。 目前選取的啟動器入口網站會以星號標示 ( * ) 。 |
| 提供者 | 列出有關 VDS 已知的每個提供者的摘要資訊。 目前選取的提供者會以星號標示 ( * ) 。 |
| 子系統 | 列出系統中每個子系統的相關摘要資訊。 目前選取的子系統會以星號標示 ( * ) 。 |
| controllers | 列出目前所選子系統中每個控制器的相關摘要資訊。 目前選取的控制器會以星號標示 ( * ) 。 |
| 連接埠 | 列出目前所選控制器中每個控制器埠的相關摘要資訊。 目前選取的埠會以星號標示 ( * ) 。 |
| 磁碟機 | 列出目前所選子系統中每個磁片磁碟機的相關摘要資訊。 目前選取的磁片磁碟機會以星號標示 ( * ) 。 |
| Lun | 列出目前所選子系統中每個 LUN 的相關摘要資訊。 目前選取的 LUN 會以星號標示 ( * ) 。 |
| tportals | 列出目前所選子系統中所有 iSCSI 目標入口網站的相關摘要資訊。 目前選取的目標入口網站會以星號標示 ( * ) 。 |
| 目標 | 列出目前所選子系統中所有 iSCSI 目標的相關摘要資訊。 目前選取的目標會以星號標示 ( * ) 。 |
| tpgroups | 列出目前所選目標中所有 iSCSI 目標入口網站群組的相關摘要資訊。 目前選取的入口網站群組會以星號標示 ( * ) 。 |

### <a name="login"></a>login

將指定的 iSCSI 啟動器介面卡記錄到目前選取的 iSCSI 目標。

#### <a name="syntax"></a>語法

```
login target iadapter=<iadapter> [type={manual | persistent | boot}] [chap={none | oneway | mutual}] [iportal=<iportal>] [tportal=<tportal>] [<flag> [<flag> […]]]
```

##### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| type | 指定要執行的登入類型： **手動** 或 **持續**性。 如果未指定，則會執行手動登入。 |
| manual | 手動登入。 另外還有一個 **開機** 選項，適用于未來開發且目前未使用。 |
| 持續 | 重新開機電腦時，會自動使用相同的登入。 |
| 章 | 指定要使用的 CHAP 驗證類型： **無**、 **單向** chap 或 **相互** chap;如果未指定，將不會使用任何驗證。 |
| 管理入口網站 | 在目前選取的子系統中，指定用於登入的選擇性目標入口網站。 |
| iportal | 在指定的啟動器介面卡中，指定用於登入的選擇性啟動器入口網站。 |
| `<flag>` | 以三個字母的縮寫識別：<ul><li>**Ip** -需要 IPsec</li><li>**EMP** -啟用多重路徑</li><li>**EHD** -啟用標頭摘要</li><li>**EDD** -啟用資料摘要</li></ul> |

### <a name="logout"></a>logout

從目前選取的 iSCSI 目標記錄指定的 iSCSI 啟動器介面卡。

#### <a name="syntax"></a>語法

```
logout target iadapter= <iadapter>
```

##### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| iadapter | 指定具有要登出之登入會話的啟動器介面卡。 |

### <a name="maintenance"></a>維護

在目前選取的物件上，針對指定的類型執行維護作業。

#### <a name="syntax"></a>語法

```
maintenance <object operation> [count=<iteration>]
```

##### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<object>` | 指定要在其上執行作業的物件類型。 *物件*類型可以是**子系統**、**控制器**、**埠、磁片磁碟機**或**LUN**。 |
| `<operation>` | 指定要執行的維護作業。 作業 *類型可以* 是 **spinup**、 **spindown**、 **閃爍**、 **嗶聲** 或 **偵測**。 必須指定 *操作* 。 |
| 計數 = | 指定重複 *操作*的次數。 這通常用於 **閃爍**、 **嗶聲**或 **偵測**。 |

### <a name="name"></a>名稱

將目前所選子系統、LUN 或 iSCSI 目標的易記名稱設定為指定的名稱。

#### <a name="syntax"></a>語法

```
name {subsystem | lun | target} [<name>]
```

##### <a name="parameter"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<name>` | 指定子系統、LUN 或目標的名稱。 名稱的長度必須小於64個字元。 如果未提供名稱，則會刪除現有的名稱（如果有的話）。 |

### <a name="offline"></a>離線

將所指定類型之目前選取物件的狀態設定為 [ **離線**]。

#### <a name="syntax"></a>語法

```
offline <object>
```

##### <a name="parameter"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<object>` | 指定要執行這項作業的物件類型。 類型可以是： **子系統**、 **控制器**、 **磁片磁碟機**、 **LUN**或 **管理入口網站**。 |

### <a name="online"></a>線上

將指定之類型的選取物件狀態設定為 [ **線上**]。 如果物件是 **hbaport**，則會將目前所選 HBA 埠的路徑狀態變更為「 **線上**」。

#### <a name="syntax"></a>語法

```
online <object>
```

##### <a name="parameter"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<object>` | 指定要執行這項作業的物件類型。 此類型可以是： **hbaport**、 **子系統**、 **控制器**、 **磁片磁碟機**、 **LUN**或 **管理入口網站**。 |

### <a name="recover"></a>recover

執行必要的作業，例如重新同步處理或熱備份，以修復目前選取的容錯 LUN。 例如，復原可能會導致熱備用裝置系結至具有故障磁片或其他磁片區重新配置的 RAID 組。

#### <a name="syntax"></a>語法

```
recover <lun>
```

### <a name="reenumerate"></a>reenumerate

Reenumerates 指定類型的物件。 如果您使用 [擴充 LUN] 命令，則必須先使用 [重新整理] 命令來更新磁片大小，然後再使用 reenumerate 命令。

#### <a name="syntax"></a>語法

```
reenumerate {subsystems | drives}
```

##### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| 子系統 | 查詢提供者以探索目前所選提供者中已新增的任何新子系統。 |
| 磁碟機 | 查詢內部 i/o 匯流排，以探索在目前選取的子系統中新增的任何新磁片磁碟機。 |

### <a name="refresh"></a>refresh

重新整理目前所選提供者的內部資料。

#### <a name="syntax"></a>語法

```
refresh provider
```

### <a name="rem"></a>rem

用來批註腳本。

#### <a name="syntax"></a>語法

```
Rem <comment>
```

### <a name="remove"></a>remove

從目前選取的目標入口網站群組移除指定的 iSCSI 目標入口網站。

#### <a name="syntax"></a>語法

```
remove tpgroup tportal=<tportal> [noerr]
```

##### <a name="parameter"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| tpgroup 管理入口網站 =`<tportal>` | 指定要移除的 iSCSI 目標入口網站。 |
| noerr | 僅適合執行指令。 當發生錯誤時，Diskraid 會繼續處理命令，就像未發生錯誤一樣。 |

### <a name="replace"></a>取代

以目前選取的磁片磁碟機取代指定的磁片磁碟機。 指定的磁片磁碟機可能不是目前選取的磁片磁碟機。

#### <a name="syntax"></a>語法

```
replace drive=<drive_number>
```

##### <a name="parameter"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| 磁片磁碟機 = | 指定 `<drive_number>` 要取代之磁片磁碟機的。 |

### <a name="reset"></a>reset

重設目前選取的控制器或埠。

#### <a name="syntax"></a>語法

```
reset {controller | port}
```

##### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| controller | 重設控制器。 |
| 連接埠 | 重設埠。 |

### <a name="select"></a>select

顯示或變更目前選取的物件。

#### <a name="syntax"></a>語法

```
select {hbaport | iadapter | iportal | provider | subsystem | controller | port | drive | lun | tportal | target | tpgroup } [<n>]
```

##### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| 物件 (object) | 指定要選取的物件類型，包括： **提供者**、 **子系統**、 **控制器**、 **磁片磁碟機**或 **LUN**。 |
| hbaport `[<n>]` | 將焦點設定為指定的本機 HBA 埠。 如果未指定 HBA 埠，此命令會顯示目前選取的 HBA 埠 (是否有任何) 。 指定不正確 HBA 埠索引會導致沒有焦點的 HBA 埠。 選取 HBA 埠可取消選取任何選取的啟動器介面卡和起始端入口網站。 |
| iadapter `[<n>]` | 將焦點設定為指定的本機 iSCSI 啟動器介面卡。 如果未指定任何起始端介面卡，則命令會顯示目前選取的啟動器介面卡 (是否有任何) 。 指定不正確啟動者介面卡索引會導致沒有焦點的啟動器介面卡。 選取啟動器介面卡可取消選取任何選取的 HBA 埠和起始端入口網站。 |
| iportal `[<n>]` | 將焦點設定為所選 iSCSI 啟動器介面卡內指定的本機 iSCSI 啟動器入口網站。 如果未指定任何起始端入口網站，此命令會顯示目前選取的啟動器入口網站 (是否有任何) 。 指定不正確啟動者入口網站索引會導致沒有任何選取的啟動器入口網站。 |
| 供應商 `[<n>]` | 將焦點設定為指定的提供者。 如果未指定提供者，此命令會顯示目前選取的提供者 (是否有任何) 。 指定不正確提供者索引會導致沒有焦點提供者。 |
| 子系統 `[<n>]` | 將焦點設定為指定的子系統。 如果未指定子系統，此命令會顯示具有焦點的子系統 (如果有任何) 。 指定不正確子系統索引會導致沒有焦點子系統。 選取子系統時，會隱含地選取其相關聯的提供者。 |
| 控制器 `[<n>]` | 將焦點設定到目前所選子系統內的指定控制器。 如果未指定控制器，此命令會顯示目前選取的控制器 (是否有任何) 。 指定不正確控制器索引會導致沒有焦點控制器。 選取控制器會取消選取任何選取的控制器埠、磁片磁碟機、Lun、目標入口網站、目標和目標入口網站群組。 |
| 港口 `[<n>]` | 將焦點設定為目前選取之控制器內的指定控制器埠。 如果未指定埠，此命令會顯示目前選取的埠 (是否有任何) 。 指定不正確埠索引會導致沒有選取的埠。 |
| 驅動 `[<n>]` | 將焦點設定到目前所選子系統內的指定磁片磁碟機或實體主軸。 如果未指定磁片磁碟機，則此命令會顯示目前選取的磁片磁碟機 (是否有任何) 。 指定不正確磁片磁碟機索引會導致沒有焦點磁片磁碟機。 選取磁片磁碟機，取消選取任何選取的控制器、控制器埠、Lun、目標入口網站、目標和目標入口網站群組。 |
| 倫 `[<n>]` | 將焦點設定到目前所選子系統內的指定 LUN。 如果未指定任何 LUN，此命令會顯示目前選取的 LUN (是否有任何) 。 指定不正確 LUN 索引會導致沒有選取的 LUN。 選取 LUN 會取消選取任何選取的控制器、控制器埠、磁片磁碟機、目標入口網站、目標和目標入口網站群組。 |
| 管理入口網站 `[<n>]` | 將焦點設定到目前所選子系統內的指定 iSCSI 目標入口網站。 如果未指定目標入口網站，此命令會顯示目前選取的目標入口網站 (是否有任何) 。 指定不正確目標入口網站索引會導致沒有選取的目標入口網站。 選取目標入口網站會取消選取任何控制器、控制器埠、磁片磁碟機、Lun、目標和目標入口網站群組。 |
| 目標 `[<n>]` | 將焦點設定到目前所選子系統內的指定 iSCSI 目標。 如果未指定目標，則命令會顯示目前選取的目標 (如果有任何) 。 指定不正確目標索引會導致沒有選取的目標。 選取目標會取消選取任何控制器、控制器埠、磁片磁碟機、Lun、目標入口網站，以及目標入口網站群組。 |
| tpgroup `[<n>]` | 將焦點設定到目前所選 iSCSI 目標內的指定 iSCSI 目標入口網站群組。 如果未指定目標入口網站群組，此命令會顯示目前選取的目標入口網站群組 (是否有任何) 。 指定不正確目標入口網站群組索引會導致沒有焦點的目標入口網站群組。 |
|`[<n>]` | 指定 `<object number>` 要選取的。 如果指定的無效 `<object number>` ，則會清除指定類型之物件的任何現有選項。 如果未 `<object number>` 指定，則會顯示目前的物件。

### <a name="setflag"></a>setflag

將目前選取的磁片磁碟機設定為熱備用。 熱備件無法用於一般 LUN 系結作業。 它們只保留給錯誤處理。 磁片磁碟機目前不得系結至任何現有的 LUN。

#### <a name="syntax"></a>語法

```
setflag drive hotspare={true | false}
```

##### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| true | 選取目前選取的磁片磁碟機做為熱備用。 |
| false | 將目前選取的磁片磁碟機取消選取為熱備用。 |

### <a name="shrink"></a>shrink

減少所選 LUN 的大小。

#### <a name="syntax"></a>語法

```
shrink lun size=<n> [noerr]
```

##### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| 大小 | 指定所需的空間量（以 (mb 為單位），) 以減少 LUN 的大小。 若要使用其他單位來指定大小，請在大小之後立即使用下列其中一個可辨識的尾碼：<ul><li>**B** 位元組</li><li>**Kb** -kb</li><li>**Mb** -mb</li><li>**Gb** -gb</li><li>**Tb** -tb</li><li>**Pb** -pb。 |
| noerr | 僅適合執行指令。 當發生錯誤時，Diskraid 會繼續處理命令，就像未發生錯誤一樣。 |

### <a name="standby"></a>待命

將路徑的狀態變更為目前選取的主機匯流排介面卡， (HBA) 埠變成待命。

#### <a name="syntax"></a>語法

```
standby hbaport
```

##### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| hbaport | 將路徑的狀態變更為目前選取的主機匯流排介面卡， (HBA) 埠變成待命。 |

### <a name="unmask"></a>揭露

使目前選取的 Lun 可從指定的主機存取。

#### <a name="syntax"></a>語法

```
unmask lun {all | none | [add] wwn=<hexadecimal_number> [;<hexadecimal_number> [;…]] | [add] initiator=<initiator>[;<initiator>[;…]]} [uninstall]
```

##### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| all | 指定應讓 LUN 可從所有主機存取。 不過，您無法將 LUN 取消遮罩至 iSCSI 子系統中的所有目標。<P>您必須在執行命令之前登出目標 `unmask lun all` 。 |
| 無 | 指定不應該讓任何主機存取 LUN。<P>您必須在執行命令之前登出目標 `unmask lun none` 。 |
| add | 指定必須將指定的主機新增到可存取此 LUN 的現有主機清單。 如果未指定此參數，則提供的主機清單會取代可從中存取此 LUN 的現有主機清單。 |
| wwn = | 指定十六進位數位的清單，這些數位代表要讓 LUN 或主機可供存取的世界範圍名稱。 若要遮罩/取消遮罩光纖通道子系統中的一組特定主機，您可以針對感興趣的主機機器上的埠，輸入以分號分隔的 WWN 清單。 |
| 啟動器 = | 指定要讓目前選取的 LUN 成為可存取的 iSCSI 啟動器清單。 若要遮罩/取消遮罩 iSCSI 子系統中的一組特定主機，您可以在感興趣的主機電腦上，為啟動器輸入以分號分隔的 iSCSI 啟動器名稱清單。 |
| uninstall | 如果有指定，則會先卸載與本機系統上的 LUN 相關聯的磁片，然後再遮罩該 LUN。 |

## <a name="scripting-diskraid"></a>腳本處理 Diskraid

您可以使用相關的 VDS 硬體提供者，在任何執行支援的 Windows Server 版本的電腦上編寫 Diskraid 的腳本。 若要叫用 Diskraid 腳本，請在命令提示字元中輸入：

```
diskraid /s <script.txt>
```

根據預設，如果腳本中有問題，則會停止處理命令並傳回錯誤碼。 若要繼續執行腳本並忽略錯誤，請在命令中包含 **noerr** 參數。 如此一來，就能使用單一腳本刪除子系統中的所有 Lun，而不考慮 Lun 的總數目。 並非所有命令都支援 **noerr** 參數。 無論您是否包含了 **noerr** 參數，都一律會在命令語法錯誤時傳回錯誤。

## <a name="diskraid-error-codes"></a>Diskraid 錯誤碼

| 錯誤碼 | 錯誤說明 |
| ---------- | ----------------- |
| 0 | 未發生任何錯誤。 執行整個指令檔過程中未發生失敗。 |
| 1 | 發生嚴重例外狀況。 |
| 2 | 在 Diskraid 命令列上指定的引數不正確。 |
| 3 | Diskraid 無法開啟指定的腳本或輸出檔。 |
| 4 | 其中一個 Diskraid 使用的服務傳回失敗。 |
| 5 | 發生命令語法錯誤。 指令檔失敗的原因是，未正確選取物件或物件對於該命令無效。 |

## <a name="example"></a>範例

若要查看電腦上子系統0的狀態，請輸入：

```
diskraid
```

按下 ENTER 和輸出，如下所示：

```
Microsoft Diskraid version 5.2.xxxx
Copyright (©) 2003 Microsoft Corporation
On computer: COMPUTER_NAME
```

若要選取子系統0，請在 Diskraid 提示字元中輸入下列命令：

```
select subsystem 0
```

按下 ENTER 和輸出，如下所示：

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

若要結束 Diskraid，請在 Diskraid 提示字元中輸入下列命令：

```
exit
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)