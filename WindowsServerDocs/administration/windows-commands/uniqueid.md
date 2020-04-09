---
title: uniqueid
description: 適用于 uniqueid 的 Windows 命令主題，它會顯示或設定具有焦點之磁片的 GUID 磁碟分割表格（GPT）識別碼或主要開機記錄（MBR）簽章。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 64235a4a-b91c-46da-b9b0-68ee90571c2a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 29d7bf0498e76d5192e986aadabb77d575a8102b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80832311"
---
# <a name="uniqueid"></a>uniqueid

顯示或設定具有焦點之磁片的 GUID 磁碟分割表格（GPT）識別碼或主要開機記錄（MBR）簽章。

> [!IMPORTANT]
> Windows Vista 的任何版本都無法使用此 DiskPart 命令。

## <a name="syntax"></a>語法

```
uniqueid disk [id={<dword> | <GUID>}] [noerr]
```

### <a name="parameters"></a>參數

|  參數   |                                                                                             描述                                                                                              |
|--------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 識別碼 = {\<dword > |                                                                                               <GUID>}                                                                                                |
|    noerr     | 僅適合執行指令。 遇到錯誤時，DiskPart 會像沒有發生錯誤一般繼續處理命令。 若沒有此參數，錯誤會導致 DiskPart 結束，錯誤碼為。 |

## <a name="remarks"></a>備註

-   此命令適用于基本和動態磁碟。
-   必須選取磁片，此命令才會成功。 使用 [**選取磁片**] 命令來選取磁片，並將焦點移至它。

## <a name="examples"></a><a name=BKMK_examples></a>典型

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

