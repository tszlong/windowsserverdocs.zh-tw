---
title: bitsadmin setdisplayname
description: 適用于 bitsadmin setdisplayname 的 Windows 命令主題，其會設定指定之作業的顯示名稱。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 13706c53-fb5f-4879-b5ca-82531361d6e1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 601c5b406132e70fb7d4facb97329f7456002bb4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849541"
---
# <a name="bitsadmin-setdisplayname"></a>bitsadmin setdisplayname

設定指定之作業的顯示名稱。

## <a name="syntax"></a>語法

```
bitsadmin /SetDisplayName <Job> <DisplayName>
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|
|DisplayName|用於指定工作之顯示名稱的文字。|

## <a name="examples"></a><a name=BKMK_examples></a>典型

下列範例會將名為*myDownloadJob*之作業的顯示名稱設定為*myDownloadJob2*。
```
C:\>bitsadmin /SetDisplayName myDownloadJob Download Music Job
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)