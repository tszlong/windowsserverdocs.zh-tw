---
title: bitsadmin getdisplayname
description: 適用於 Windows 命令主題**bitsadmin getdisplayname** -擷取指定工作的顯示名稱。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e5c0e76c-4cc6-42d8-ac30-30bf3dc11b9b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c1ef16f54b7b825e4293a3870d8181985b83843b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857599"
---
# <a name="bitsadmin-getdisplayname"></a>bitsadmin getdisplayname



擷取指定工作的顯示名稱。

## <a name="syntax"></a>語法

```
bitsadmin /GetDisplayName <Job>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|

## <a name="BKMK_examples"></a>範例

下列範例會擷取名為工作的顯示名稱*myDownloadJob*。
```
C:\>bitsadmin /GetDisplayName myDownloadJob
```
其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)