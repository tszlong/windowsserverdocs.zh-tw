---
title: fsutil dirty
description: Fsutil dirty 命令的參考文章，可查詢或設定磁片區的中途位。
manager: dmoss
ms.author: toklima
author: toklima
ms.assetid: 385a2a7c-d6bd-4f11-9c18-fca0413f9e97
ms.topic: reference
ms.date: 10/16/2017
ms.openlocfilehash: fa63ec550821f99dffa59f092bb0f0523bfb948b
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89030146"
---
# <a name="fsutil-dirty"></a>fsutil dirty

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows 10、Windows Server 2012 R2、Windows 8.1、Windows Server 2012、Windows 8

查詢或設定磁片區的中途位。 設定磁片區的中途位時， **autochk** 會在下次電腦重新開機時自動檢查磁片區是否有錯誤。

## <a name="syntax"></a>語法

```
fsutil dirty {query | set} <volumepath>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| 查詢 | 查詢指定磁片區的變更位。 |
| set | 設定指定的磁片區變更位。 |
| `<volumepath>` | 指定磁片磁碟機名稱，後面接著冒號或 GUID，格式如下： `volume{GUID}` 。 |

#### <a name="remarks"></a>備註

- 磁片區的中途位表示檔案系統可能處於不一致的狀態。 您可以設定中途位，因為：

    - 磁片區已上線，且有未完成的變更。

    - 對磁片區進行了變更，而電腦在變更認可到磁片之前已關閉。

    - 在磁片區上偵測到損毀。

- 如果在電腦重新開機時設定了中途位，則會執行 **chkdsk** 來確認檔案系統完整性，並嘗試修正磁片區的任何問題。

### <a name="examples"></a>範例

若要在磁片磁碟機 C 上查詢中途的位，請輸入：

```
fsutil dirty query c:
```

- 如果磁片區已變更，則會顯示下列輸出： `Volume C: is dirty`

- 如果磁片區未變更，則會顯示下列輸出： `Volume C: is not dirty`

若要在磁片磁碟機 C 上設定中途位，請輸入：

```
fsutil dirty set C:
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [fsutil](fsutil.md)
