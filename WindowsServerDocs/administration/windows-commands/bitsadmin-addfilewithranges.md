---
title: bitsadmin addfilewithranges
description: Bitsadmin addfilewithranges 命令的參考文章，此命令會將檔案新增至指定的作業。 BITS 會從遠端檔案下載指定的範圍。
ms.topic: reference
ms.assetid: df0ce0bf-dff1-4a48-a16f-fd2f4d5f7189
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 08f9031ebd6ffe2e1480e59e5e357a33b9895766
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89027846"
---
# <a name="bitsadmin-addfilewithranges"></a>bitsadmin addfilewithranges

將檔案加入至指定的作業。 BITS 會從遠端檔案下載指定的範圍。 此參數僅適用于下載作業。

## <a name="syntax"></a>語法

```
bitsadmin /addfilewithranges <job> <remoteURL> <localname> <rangelist>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| 作業 | 作業的顯示名稱或 GUID。 |
| remoteURL | 伺服器上檔案的 URL。 |
| localname | 本機電腦上的檔案名。 必須包含檔案的絕對路徑。 |
| rangelist | 位移：長度配對的逗號分隔清單。 使用冒號將位移值與長度值隔開。 例如，的值會 `0:100,2000:100,5000:eof` 告訴 BITS 將100個位元組從位移0、100個位元組（位移2000）到個位元組，並將剩餘的位元組從位移5000傳送至檔案結尾。 |

## <a name="remarks"></a>備註

- Token **eof** 是中的位移和長度配對內的有效長度值 `<rangelist>` 。 它會指示服務讀取至指定檔案的結尾。

- `addfilewithranges`如果指定零長度的範圍與使用相同位移的另一個範圍，命令將會失敗並出現錯誤碼0x8020002c，例如：

    `c:\bits>bitsadmin /addfilewithranges j2 http://bitsdc/dload/1k.zip c:\1k.zip 100:0,100:5`

    **錯誤訊息：** 無法將檔案新增至作業-0x8020002c。 位元組範圍清單包含一些重迭的範圍，但不受支援。

    因應措施 **：** 請勿先指定長度為零的範圍。 例如，使用 `bitsadmin /addfilewithranges j2 http://bitsdc/dload/1k.zip c:\1k.zip 100:5,100:0`

## <a name="examples"></a>範例

若要從位移0、100位元組（位移2000）到位元組，以及從位移5000到檔案結尾的剩餘位元組之間傳輸100位元組：

```
bitsadmin /addfilewithranges http://downloadsrv/10mb.zip c:\10mb.zip 0:100,2000:100,5000:eof
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
