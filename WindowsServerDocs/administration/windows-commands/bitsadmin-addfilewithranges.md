---
title: bitsadmin addfilewithranges
description: Bitsadmin addfilewithranges 命令的參考主題，它會將檔案新增至指定的作業。 BITS 會從遠端檔案下載指定的範圍。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: df0ce0bf-dff1-4a48-a16f-fd2f4d5f7189
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4b878b4f48441808bf971c051397d3af9bd975fe
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718462"
---
# <a name="bitsadmin-addfilewithranges"></a>bitsadmin addfilewithranges

將檔案加入至指定的作業。 BITS 會從遠端檔案下載指定的範圍。 此參數只對下載作業有效。

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
| rangelist | 以逗號分隔的位移：長度配對清單。 使用冒號來分隔 [長度] 值的位移值。 例如，的值會`0:100,2000:100,5000:eof`告訴 BITS 要從 offset 0、100個位元組到位移2000，以及從 offset 5000 到檔案結尾的剩餘位元組傳輸100個位元組。 |

## <a name="remarks"></a>備註

- Token **eof**是中位移和長度配對內的有效長度值`<rangelist>`。 它會指示服務讀取至指定檔案的結尾。

- 如果`addfilewithranges`指定了零長度範圍與另一個使用相同位移的範圍，此命令將會失敗，錯誤碼為0x8020002c，例如：

    `c:\bits>bitsadmin /addfilewithranges j2 http://bitsdc/dload/1k.zip c:\1k.zip 100:0,100:5`

    **錯誤訊息：** 無法將檔案新增至作業-0x8020002c。 位元組範圍清單包含一些重迭的範圍，但不受支援。

    因應措施 **：** 請先不要指定零長度的範圍。 例如，使用 `bitsadmin /addfilewithranges j2 http://bitsdc/dload/1k.zip c:\1k.zip 100:5,100:0`

## <a name="examples"></a>範例

若要從位移0傳輸100個位元組，從位移2000的100個位元組，以及從 offset 5000 到檔案結尾的剩餘位元組：

```
bitsadmin /addfilewithranges http://downloadsrv/10mb.zip c:\10mb.zip 0:100,2000:100,5000:eof
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
