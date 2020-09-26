---
title: set id
description: Diskpart set id 命令的參考文章，此命令會變更具有焦點之分割區的分割區類型欄位。
ms.topic: reference
ms.assetid: 5793d7ad-827e-4285-b2c6-ae60eeb0e886
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 0f25498a89cdb7347a40825af70621d5cd09440a
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/26/2020
ms.locfileid: "91389082"
---
# <a name="set-id-diskpart"></a>將識別碼設定 (Diskpart) 

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

變更具有焦點之資料分割的資料分割類型欄位。 此命令不適用於動態磁碟或 Microsoft 保留的磁碟分割。

> [!IMPORTANT]
> 此命令僅供原始設備製造商使用 (Oem) 。 使用此參數變更磁碟分割類型欄位可能會導致您的電腦失敗或無法開機。 除非您是 OEM 或有使用 gpt 磁片的經驗，否則您不應該使用此參數變更 gpt 磁片上的磁碟分割類型欄位。 相反地，請一律使用 [create partition efi](create-partition-efi.md) 命令來建立 efi 系統磁碟分割、 [建立磁碟分割 msr](create-partition-msr.md) 命令以建立 Microsoft 保留的磁碟分割，並使用 [create partition primary](create-partition-primary.md) 命令，而不使用 ID 參數在 gpt 磁片上建立主要磁碟分割。

## <a name="syntax"></a>語法

```
set id={ <byte> | <GUID> } [override] [noerr]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
|--|--|
| `<byte>` | 若為主開機記錄 (MBR) 磁片，請為磁碟分割指定類型欄位的新值（以十六進位形式）。 您可以使用這個參數指定任何磁碟分割類型 **位元組** ，但類型0x42 （指定 LDM 分割區）除外。 請注意，指定十六進位的資料分割類型時，會省略前置的0x。 |
| `<GUID>` | 針對 GUID 磁碟分割表格 (gpt) 磁片，為數據分割的 [類型] 欄位指定新的 GUID 值。 辨識的 Guid 包括：<ul><li>**EFI 系統磁碟分割：** c12a7328-f81f-11d2-ba4b-00a0c93ec93b</li><li>**基本資料分割：** ebd0a0a2-b9e5-4433-87c0-68b6b72699c7</li></ul>您可以使用這個參數指定任何磁碟分割類型 GUID，但下列情況除外：<ul><li>**Microsoft 保留的磁碟分割：** e3c9e316-0b5c-4db8-817d-f92df00215ae</li><li>**動態磁碟上的 LDM 中繼資料磁碟分割：** 5808c8aa-7e8f-42e0-85d2-e1e90434cfb3</li><li>**動態磁碟上的 LDM 資料磁碟分割：** af9b60a0-1431-4f62-bc68-3311714a69ad</li><li>叢集**中繼資料磁碟分割：** db97dba9-0840-4bae-97f0-ffb9a327c7e1</li></ul> |
| override | 強制卸載磁片區上的檔案系統，再變更磁碟分割類型。 當您執行「 **設定識別碼** 」命令時，DiskPart 會嘗試鎖定並卸載磁片區上的檔案系統。 如果未指定覆 **寫** ，而且鎖定檔案系統的呼叫失敗 (例如，因為有開啟的控制碼) ，所以作業失敗。 如果指定了 **override** ，則即使鎖定檔案系統的呼叫失敗，以及對磁片區開啟的控制碼都將停止有效，DiskPart 仍會強制卸載。 |
| noerr | 僅供腳本使用。 遇到錯誤時，DiskPart 會像沒有發生錯誤一般繼續處理命令。 如果沒有這個參數，錯誤會導致 DiskPart 結束，並產生錯誤碼。 |

## <a name="remarks"></a>備註

- 除了先前所述的限制以外，DiskPart 也不會檢查您 (指定的值是否有效，除非確定它是十六進位格式的位元組或 GUID) 。

## <a name="examples"></a>範例

若要將類型欄位設定為 *0x07* 並強制檔案系統卸載，請輸入：

```
set id=0x07 override
```

若要將類型欄位設定為基本資料分割，請輸入：

```
set id=ebd0a0a2-b9e5-4433-87c0-68b6b72699c7
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)
