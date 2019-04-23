---
title: bitsadmin resume
description: 適用於 Windows 命令主題**bitsadmin 繼續**-啟動新的或已暫停的工作，在傳輸佇列。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7c7540a9-a11a-4910-923a-2a2a61cbf11d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 76027ac927f8a9bb2558e3ce6d75e4f6692e56e7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842029"
---
# <a name="bitsadmin-resume"></a>bitsadmin resume



啟動新的或已暫停的工作，在傳輸佇列。

## <a name="syntax"></a>語法

```
bitsadmin /Resume <Job>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|

## <a name="BKMK_examples"></a>範例

下列範例會繼續工作名為*myDownloadJob*。
```
C:\>bitsadmin /Resume myDownloadJob
```
其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)