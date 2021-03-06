---
title: fsutil objectid
description: Fsutil objectid 命令的參考文章，管理物件識別碼以追蹤其他物件，例如檔案、目錄和連結。
manager: dmoss
ms.author: toklima
author: toklima
ms.assetid: 693ab895-9d0c-47c1-9f52-df5cd287842a
ms.topic: reference
ms.date: 10/16/2017
ms.openlocfilehash: 27b7048ebb659c29bd6aa7d41c0be9b26deca547
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89037336"
---
# <a name="fsutil-objectid"></a>fsutil objectid

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows 10、Windows Server 2012 R2、Windows 8.1、Windows Server 2012、Windows 8

管理 (Oid) 的物件識別碼，也就是分散式連結追蹤所使用的內建物件 (DLT) 用戶端服務和檔案複寫服務 (FRS) ）來追蹤其他物件，例如檔案、目錄和連結。 大部分的程式都看不到物件識別碼，絕對不應該修改。

> [!WARNING]
> 請勿刪除、設定或以其他方式修改物件識別碼。 刪除或設定物件識別碼可能會導致檔案的部分遺失，而不包括整個磁片區的資料。 此外，您可能會在分散式連結追蹤 (DLT) 用戶端服務和檔案複寫服務 (FRS) 中造成不良行為。

## <a name="syntax"></a>語法

```
fsutil objectid [create] <filename>
fsutil objectid [delete] <filename>
fsutil objectid [query] <filename>
fsutil objectid [set] <objectID> <birthvolumeID> <birthobjectID> <domainID> <filename>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| 建立 | 如果指定的檔案不存在，則建立物件識別碼。 如果檔案已經有物件識別碼，這個子命令就相當於 **查詢** 子命令。 |
| delete | 刪除物件識別碼。 |
| 查詢 | 查詢物件識別碼。 |
| set | 設定物件識別碼。 |
| `<objectID>` | 設定特定檔案的16位元組十六進位識別碼，該識別碼在磁片區中保證是唯一的。 分散式連結追蹤會使用物件識別碼 (DLT) 用戶端服務和檔案複寫服務 (FRS) 來識別檔案。 |
| `<birthvolumeID>` | 指出第一次取得物件識別碼時，檔案所在的磁片區。 此值是由 DLT 用戶端服務所使用的16位元組十六進位識別碼。 |
| `<birthobjectID>` | 指出檔案的原始物件識別碼 (當) 移動檔案時， *objectID* 可能會變更。 此值是由 DLT 用戶端服務所使用的16位元組十六進位識別碼。 |
| `<domainID>` | 16位元組的十六進位網域識別碼。 目前不使用此值，且必須設定為全部為零。 |
| `<filename>` | 指定檔案的完整路徑，包括檔案名和副檔名，例如 *C:\documents\filename.txt*。 |

#### <a name="remarks"></a>備註

- 任何具有物件識別碼的檔案也有出生磁片區識別碼、出生物件識別碼和網域識別碼。 當您移動檔案時，物件識別碼可能會變更，但出生數量和出生物件識別碼會維持不變。 此行為可讓 Windows 作業系統一律尋找檔案，不論其移動位置為何。

### <a name="examples"></a>範例

若要建立物件識別碼，請輸入：

`fsutil objectid create c:\temp\sample.txt`

若要刪除物件識別碼，請輸入：

`fsutil objectid delete c:\temp\sample.txt`

若要查詢物件識別碼，請輸入：

`fsutil objectid query c:\temp\sample.txt`

若要設定物件識別碼，請輸入：

`fsutil objectid set 40dff02fc9b4d4118f120090273fa9fc f86ad6865fe8d21183910008c709d19e 40dff02fc9b4d4118f120090273fa9fc 00000000000000000000000000000000 c:\temp\sample.txt`

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [fsutil](fsutil.md)
