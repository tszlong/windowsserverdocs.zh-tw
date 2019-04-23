---
title: bitsadmin addfilewithranges
description: 適用於 Windows 命令主題**bitsadmin addfilewithranges** -將檔案加入至指定的作業。 位元會從遠端檔案下載指定的範圍。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 69b402195f90977aa63299c1a2a550ba310a4513
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832339"
---
# <a name="bitsadmin-addfilewithranges"></a>bitsadmin addfilewithranges

將檔案加入至指定的作業。 位元會從遠端檔案下載指定的範圍。 這個參數是僅適用於下載工作。

## <a name="syntax"></a>語法

```
bitsadmin /AddFileWithRanges <Job> <RemoteURL> <LocalName> <RangeList>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|
|RemoteURL|*RemoteURL*是伺服器上檔案的 URL。|
|LocalName|*LocalName*是本機電腦上的檔案名稱。 *LocalName*必須包含檔案的絕對路徑。|
|RangeList|*RangeList*是位移： 長度組的逗號分隔清單。 您可以使用冒號來分開的長度值的位移的值。 例如，值`0:100,2000:100,5000:eof`告訴 BITS 傳送 100 個位元組，從位移 0，100 個位元組的位移 2000，而從剩餘的位元組位移 5000 到檔案結尾。|

## <a name="more-information"></a>更多資訊

-   語彙基元**eof**是有效的長度值中的位移和長度組內 *\<RangeList >*。 它會指示服務，以讀取至指定的檔案結尾。
-   請注意 AddFileWithRanges 將會失敗，錯誤碼 0x8020002c，當這類指定以及另一個範圍，使用相同的位移、 長度為零的範圍：C:\bits > bitsadmin /addfilewithranges j2 http://bitsdc/dload/1k.zip c:\1k.zip 100:0、 100:5

    錯誤訊息：無法將檔案新增至作業-0x8020002c。 位元組範圍清單包含一些重疊的範圍，不支援。

    因應措施： 請勿第一次指定零長度的範圍。 例如： bitsadmin /addfilewithranges j2 http://bitsdc/dload/1k.zip c:\1k.zip 100:5、 100:0。

## <a name="BKMK_examples"></a>範例

下列範例會指示將從位移 0，100 個位元組位移的 2000 中，從 100 個位元組的位元，並從剩餘的位元組位移 5000 到檔案結尾。
```
C:\>bitsadmin /addfilewithranges http://downloadsrv/10mb.zip c:\10mb.zip "0:100,2000:100,5000:eof"
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)