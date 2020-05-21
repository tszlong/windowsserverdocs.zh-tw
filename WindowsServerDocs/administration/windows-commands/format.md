---
title: format
description: Format 命令的參考主題，其格式化磁片以接受 Windows 檔案。
ms.prod: windows-server
manager: dongill
ms.author: jgerend
ms.technology: storage
ms.topic: article
ms.assetid: 51ec7423-9a01-4219-868a-25d69cdcc832
author: jasongerend
ms.date: 10/16/2017
ms.openlocfilehash: abe05c4bf8174fd76cedc29f2bce1c83587c3d8a
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/16/2020
ms.locfileid: "83436983"
---
# <a name="format"></a>格式

> 適用于： Windows 10、Windows Server 2016

格式化磁片以接受 Windows 檔案。 您必須是 Administrators 群組的成員，才能格式化硬碟。

> [!NOTE]
> 您也可以從修復主控台使用**format**命令搭配不同的參數。 如需有關修復主控台的詳細資訊，請參閱[Windows 修復環境（WINDOWS RE）](https://docs.microsoft.com/windows-hardware/manufacture/desktop/windows-recovery-environment--windows-re--technical-reference)。

## <a name="syntax"></a>語法

```
format <volume> [/fs:{FAT|FAT32|NTFS}] [/v:<label>] [/q] [/a:<unitsize>] [/c] [/x] [/p:<passes>]
format <volume> [/v:<label>] [/q] [/f:<size>] [/p:<passes>]
format <volume> [/v:<label>] [/q] [/t:<tracks> /n:<sectors>] [/p:<passes>]
format <volume> [/v:<label>] [/q] [/p:<passes>]
format <volume> [/q]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<volume>` | 指定您想要格式化之磁片磁碟機的掛接點、磁片區名稱或磁碟機號（後面接著冒號）。 如果您未指定下列任何命令列選項， **format**會使用磁片區類型來判斷磁片的預設格式。 |
| /fs： {FAT | FAT32 | 格式化 | 指定檔案系統的類型（FAT、FAT32、NTFS）。 |
| /v:`<label>` | 指定磁碟區標籤。 如果您省略 **/v**命令列選項，或在未指定磁片區標籤的情況下使用它，**格式**會在格式化完成之後提示您輸入磁片區標籤。 使用語法 **/v:** 以避免出現磁碟區標籤的提示。 如果您使用單一 **format** 命令來格式化多個磁碟，即會為所有磁碟指定相同的磁碟區標籤。 |
| /a`<unitsize>` | 指定要在 FAT、FAT32 或 NTFS 磁片區上使用的配置單位大小。 如果您未指定*unitsize*，則會根據磁片區大小來選擇。 針對一般用途，強烈建議使用預設設定。 下列清單顯示 NTFS、FAT 和 FAT32 *unitsize*的有效值：<ul><li>512</li><li>1024</li><li>2048</li><li>4096</li><li>8192</li><li>1.6 萬</li><li>32 K</li><li>64K</li></ul>FAT 和 FAT32 也針對大於 512 位元組的磁區大小支援 128 K 和 256 K。 |
| /q | 執行快速格式化。 刪除先前格式化之磁片區的檔案資料表和根目錄，但不會針對錯誤區域執行磁區內的掃描。 您應該使用 **/q**命令列選項，只將您知道的先前格式化磁片區格式化為良好狀況。 請注意，**/q** 會覆寫 **/p**。 |
| /f`<size>` | 指定要格式化的磁碟片大小。 可能的話，請使用此命令列選項，而不是 **/t**和 **/n**命令列選項。 Windows 接受下列大小值：<ul><li>1440或1440k 或1440kb</li><li>1.44 或 1.44 m 或 1.44 mb</li><li>1.44-MB、雙面、四密度、3.5-寸磁片</li></ul> |
| /t:`<tracks>` | 指定磁碟上的磁軌數量。 可能的話，請改用 **/f**命令列選項。 如果您使用 **/t** 選項，也必須使用 **/n** 選項。 這些選項可一起提供另一個方法來指定要格式化的磁碟大小。 這個選項不能搭配 **/f** 選項使用。 |
| /n`<sectors>` | 指定每個磁軌的磁區數目。可能的話，請使用 **/f**命令列選項，而不是 **/n**。 如果您使用 **/n**，也必須使用 **/t**。 這兩個選項可一起提供另一個方法來指定要格式化的磁碟大小。 這個選項不能搭配 **/f** 選項使用。 |
| /p`<passes>` | 以指定的次數在磁碟區上的每個磁區填入零。 這個選項不能搭配 **/q** 選項使用。 |
| /C | 僅限 NTFS。 預設會壓縮在新磁碟區上建立的檔案。 |
| /x | 如有必要，會在格式化之前卸載磁片區。 磁碟區的任何開啟控制代碼將不再有效。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- **Format**命令會為磁片建立新的根目錄和檔案系統。 它也可以檢查磁片上的錯誤區域，也可以刪除磁片上的所有資料。 若要能夠使用新的磁片，您必須先使用此命令來格式化磁片。

- 格式化磁片之後，**格式**會顯示下列訊息：

    `Volume label (11 characters, ENTER for none)?`

    若要新增磁片區標籤，請輸入最多11個字元（包含空格）。 如果您不想要將磁片區標籤新增至磁片，請按 ENTER。

- 當您使用**format**命令格式化硬碟時，會顯示類似下列的警告訊息：

  ```
  WARNING, ALL DATA ON NON-REMOVABLE DISK
  DRIVE x: WILL BE LOST!
  Proceed with Format (Y/N)? _
  ```

  若要格式化硬碟，請按**Y**;如果您不想要格式化磁片，請按**N**。

- FAT 檔案系統會將叢集數目限制在65526以上。 FAT32 檔案系統會將叢集數目限制在65527到4177917之間。

- 大於 4096 的配置單位大小不支援 NTFS 壓縮。

  > [!NOTE]
  > 如果**格式**判斷無法使用指定的叢集大小來符合先前的需求，則會立即停止處理。

- 格式化完成時， **format**會顯示訊息，顯示磁碟空間總計、標示為有瑕疵的空格，以及您檔案的可用空間。

- 您可以使用 **/q**命令列選項來加速格式化程式。 只有當硬碟上沒有損壞的磁區時，才能使用此選項。

- 您不應該在使用**subst**命令備妥的磁片磁碟機上使用**format**命令。 您無法透過網路格式化磁片。

- 下表列出每個結束代碼及其意義的簡短描述。

  | 結束代碼 | 描述 |
  | --------- | ----------- |
  | 0 | 格式化作業成功。 |
  | 1 | 提供的參數不正確。 |
  | 4 | 發生嚴重錯誤（這是0、1或5以外的任何錯誤）。 |
  | 5 | 使用者按下 N 以回應「繼續進行格式（Y/N）？」 以停止該程序。 |

  您可以使用 ERRORLEVEL 環境變數搭配 **if** 批次命令來檢查這些結束代碼。

### <a name="examples"></a>範例

若要使用預設大小來格式化磁碟機 A 中的新磁碟片，請輸入：

```
format a:
```

若要在磁碟機 A 中先前已格式化的磁碟片上執行快速格式化作業，請輸入：

```
format a: /q
```

若要格式化磁片磁碟機 A 中的軟碟，並為其指派磁片區標籤*資料*，請輸入：

```
format a: /v:DATA
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](https://technet.microsoft.com/library/cc771080.aspx)
