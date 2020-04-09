---
title: bitsadmin takeownership
description: 適用于 bitsadmin takeownership 的 Windows 命令主題，可讓具有系統管理許可權的使用者取得指定工作的擁有權。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ea0ce7cb-440a-498f-a3ef-8368fa43e399
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4a2c0bfc1fcb1606102aece76129c49aad701ead
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849021"
---
# <a name="bitsadmin-takeownership"></a>bitsadmin takeownership

讓具有系統管理許可權的使用者取得指定工作的擁有權。

## <a name="syntax"></a>語法

```
bitsadmin /TakeOwnership <Job>
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|

## <a name="examples"></a><a name=BKMK_examples></a>典型

下列範例會取得名為*myDownloadJob*之作業的擁有權。
```
C:\>bitsadmin /TakeOwnership myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)