---
title: bitsadmin addfilewithranges
description: '**Bitsadmin addfilewithranges**的 Windows 命令主題-將檔案新增至指定的作業。 BITS 會從遠端檔案下載指定的範圍。'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: df0ce0bf-dff1-4a48-a16f-fd2f4d5f7189
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 557f19f6e106e5fb73b3a229090eecf0fc048758
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382016"
---
# <a name="bitsadmin-addfilewithranges"></a>bitsadmin addfilewithranges

將檔案加入至指定的作業。 BITS 會從遠端檔案下載指定的範圍。 此參數只對下載作業有效。

## <a name="syntax"></a>語法

```
bitsadmin /AddFileWithRanges <Job> <RemoteURL> <LocalName> <RangeList>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|
|RemoteURL|*RemoteURL*是伺服器上檔案的 URL。|
|LocalName|*LocalName*是本機電腦上的檔案名。 *LocalName*必須包含檔案的絕對路徑。|
|RangeList|*RangeList*是以逗號分隔的位移：長度配對清單。 使用冒號來分隔 [長度] 值的位移值。 例如，`0:100,2000:100,5000:eof` 的值會告訴 BITS 要從位移0、100位元組到位移2000，以及從 offset 5000 到檔案結尾的剩餘位元組傳輸100個位元組。|

## <a name="more-information"></a>更多資訊

-   Token **eof**是 *@no__t 2RangeList >* 的位移和長度配對內的有效長度值。 它會指示服務讀取至指定檔案的結尾。
-   請注意，當指定零長度範圍和另一個具有相同位移的範圍時，AddFileWithRanges 將會失敗，錯誤碼為0x8020002c，例如：C:\bits > bitsadmin/addfilewithranges j2 http://bitsdc/dload/1k.zip c:\1k.zip 100：0100：5

    錯誤訊息：無法將檔案新增至作業-0x8020002c。 位元組範圍清單包含一些重迭的範圍，但不受支援。

    因應措施：請先不要指定零長度的範圍。 例如： bitsadmin/addfilewithranges j2 http://bitsdc/dload/1k.zip c:\1k.zip 100：5100：0。

## <a name="examples"></a>範例

下列範例會告訴 BITS 要從位移0、100位元組到位移2000，以及從位移5000到檔案結尾的剩餘位元組傳輸100個位元組。

```
C:\>bitsadmin /addfilewithranges http://downloadsrv/10mb.zip c:\10mb.zip "0:100,2000:100,5000:eof"
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)