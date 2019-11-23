---
title: bitsadmin getproxybypasslist
description: '**Bitsadmin getproxybypasslist**的 Windows 命令主題-抓取指定工作的 proxy 略過清單。'
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 87cc131402707eac40329750e98218ec52083b94
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381424"
---
# <a name="bitsadmin-getproxybypasslist"></a>bitsadmin getproxybypasslist

抓取指定之作業的 proxy 略過清單。

## <a name="syntax"></a>語法

```
bitsadmin /GetProxyBypassList <Job>
```

## <a name="parameters"></a>Parameters

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|

## <a name="remarks"></a>備註

「略過清單」包含不會透過 proxy 路由傳送的主機名稱或 IP 位址（或兩者）。 此清單可包含「\<本機 >」，以參照相同 LAN 上的所有伺服器。 清單可以是分號或空格分隔。

## <a name="BKMK_examples"></a>典型

下列範例會針對名為*myDownloadJob*的作業，抓取 proxy 略過清單。
```
C:\>bitsadmin /GetProxyBypassList myDownloadJob
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)