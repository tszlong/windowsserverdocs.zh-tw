---
title: bitsadmin getproxybypasslist
description: 適用於 Windows 命令主題**bitsadmin getproxybypasslist** -擷取 proxy 略過清單，指定作業。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 50959be3-7014-4bc9-9a7b-68f1ff94a94a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 020b8fc0019eb103a0e469258be8705b80dd45de
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854109"
---
# <a name="bitsadmin-getproxybypasslist"></a>bitsadmin getproxybypasslist

擷取指定作業的 proxy 略過清單。

## <a name="syntax"></a>語法

```
bitsadmin /GetProxyBypassList <Job>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|

## <a name="remarks"></a>備註

略過清單包含主機名稱或 IP 位址，或兩者不是要透過 proxy 路由傳送。 清單可以包含 「\<本機 >"來參照相同的區域網路上的所有伺服器。 清單可以是以分號或者逗號分隔。

## <a name="BKMK_examples"></a>範例

下列範例會擷取名為作業的 proxy 略過清單*myDownloadJob*。
```
C:\>bitsadmin /GetProxyBypassList myDownloadJob
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)