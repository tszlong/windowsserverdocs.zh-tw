---
title: bitsadmin addfile
description: 適用於 Windows 命令主題**bitsadmin addfile** -將檔案加入至指定的作業。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1b31aa93-0364-465b-af36-754968825989
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1c3027bdc4f3f8f3e3ca50400b2c5dbf33bf2bc5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861749"
---
# <a name="bitsadmin-addfile"></a>bitsadmin addfile

將檔案加入至指定的作業。

## <a name="syntax"></a>語法

```
bitsadmin /AddFile <Job> <RemoteURL> <LocalName>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|
|RemoteURL|在伺服器上檔案的 URL。|
|LocalName|本機電腦上的檔案名稱。 *LocalName*必須包含檔案的絕對路徑。|

## <a name="BKMK_examples"></a>範例

將檔案新增至作業。 針對您想要新增每個檔案重複這個呼叫。 如果有多個作業使用*myDownloadJob*做為其名稱中，您必須取代*myDownloadJob*來唯一識別作業的作業的 guid。
```
C:\>bitsadmin /addfile myDownloadJob http://downloadsrv/10mb.zip c:\10mb.zip
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)