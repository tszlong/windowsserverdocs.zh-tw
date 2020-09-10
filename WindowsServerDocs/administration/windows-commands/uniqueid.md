---
title: 唯一
description: Uniqueid 的參考文章，可顯示或設定 GUID 磁碟分割表格 (GPT) 識別碼或主開機記錄 (MBR) 具有焦點磁片的簽章。
ms.topic: reference
ms.assetid: 64235a4a-b91c-46da-b9b0-68ee90571c2a
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: de379b25edf83212e25b34e7c8594ef03090ca9a
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89626575"
---
# <a name="uniqueid"></a>唯一

針對具有焦點的磁片，顯示或設定 GUID 磁碟分割表格 (GPT) 識別碼或主開機記錄 (MBR) 簽章。

> [!IMPORTANT]
> 所有版本的 Windows Vista 都無法使用此 DiskPart 命令。

## <a name="syntax"></a>語法

```
uniqueid disk [id={<dword> | <GUID>}] [noerr]
```

### <a name="parameters"></a>參數

|  參數   |                                                                                             描述                                                                                              |
|--------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 識別碼 = {\<dword> |                                                                                               <GUID>}                                                                                                |
|    noerr     | 僅適合執行指令。 遇到錯誤時，DiskPart 會像沒有發生錯誤一般繼續處理命令。 如果沒有這個參數，錯誤會導致 DiskPart 結束，並產生錯誤碼。 |

## <a name="remarks"></a>備註

-   此命令適用于基本和動態磁碟。
-   必須選取磁片，此命令才會成功。 使用 [ **選取磁片** ] 命令來選取磁片，並將焦點移至該磁片。

## <a name="examples"></a>範例

若要顯示具有焦點之 MBR 磁碟的簽章，請輸入：
```
uniqueid disk
```
若要將具有焦點的 MBR 磁碟簽章設定為5f1b2c36，請輸入：
```
uniqueid disk id=5f1b2c36
```
若要將具有焦點的 GPT 磁片識別碼設定為 baf784e7-6bbd-4cfb-aaac-e86c96e166ee，請輸入：
```
uniqueid disk id=baf784e7-6bbd-4cfb-aaac-e86c96e166ee
```

## <a name="additional-references"></a>其他參考資料

