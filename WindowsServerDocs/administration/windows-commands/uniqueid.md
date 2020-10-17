---
title: uniqueid
description: Uniqueid 的參考文章，可顯示或設定 GUID 磁碟分割表格 (GPT) 識別碼或主開機記錄 (MBR) 具有焦點磁片的簽章。
ms.topic: reference
ms.assetid: 64235a4a-b91c-46da-b9b0-68ee90571c2a
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 73721316747ffd0380f178622d37dc4fdbefc2fb
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156291"
---
# <a name="uniqueid"></a>uniqueid

針對具有焦點的基本或動態磁碟，顯示或設定 GUID 磁碟分割表格 (GPT) 識別碼或主開機記錄 (MBR) 簽章。 必須選取基本或動態磁碟，此操作才會成功。 使用 [ [選取磁片] 命令](select-disk.md) 來選取磁片，並將焦點移至該磁片。

## <a name="syntax"></a>語法

```
uniqueid disk [id={<dword> | <GUID>}] [noerr]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| 識別碼 =`{<dword> | <GUID>}` | 若為 MBR 磁碟，這個參數會以十六進位形式為簽章指定4位元組 (DWORD) 值。 若為 GPT 磁片，這個參數會指定識別碼的 GUID。 |
| noerr | 僅適合執行指令。 發生錯誤時，DiskPart 會繼續處理命令，就像未發生錯誤一樣。 如果沒有這個參數，錯誤會導致 DiskPart 結束，並產生錯誤碼。 |

## <a name="examples"></a>範例

若要顯示具有焦點之 MBR 磁碟的簽章，請輸入：

```
uniqueid disk
```

若要將具有焦點的 MBR 磁碟簽章設定為 DWORD 值 *5f1b2c36*，請輸入：

```
uniqueid disk id=5f1b2c36
```

若要將具有焦點的 GPT 磁片識別碼設定為 GUID 值 baf784e7-6bbd-4cfb-aaac-e86c96e166ee，請輸入：

```
uniqueid disk id=baf784e7-6bbd-4cfb-aaac-e86c96e166ee
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [選取磁片命令](select-disk.md)
