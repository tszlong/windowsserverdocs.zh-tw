---
title: ksetup:setcomputerpassword
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e307d8f6-3b93-4c24-ac04-f31549f7dc7d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0679bb9ee429e05c7679411c5493bd21b530ef8e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831539"
---
# <a name="ksetupsetcomputerpassword"></a>ksetup:setcomputerpassword



設定本機電腦的密碼。 如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
ksetup /setcomputerpassword <Password>
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<密碼 >|若要在本機電腦上設定的電腦帳戶，會使用所提供的密碼。</br>只可以藉由使用具有系統管理權限的帳戶設定密碼。 密碼可以從 1 到 156 英數或特殊字元。|

## <a name="remarks"></a>備註

此命令會影響電腦帳戶。

您必須重新啟動電腦，密碼變更，才會生效。

電腦帳戶密碼不會顯示在登錄中或做為從輸出**ksetup**命令。

## <a name="BKMK_Examples"></a>範例

將變更本機電腦上的電腦帳戶密碼從 IPops897 為 IPop$ 897 ！。
```
ksetup /setcomputerpassword IPop$897!
```

#### <a name="additional-references"></a>其他參考資料

-   [Ksetup](ksetup.md)
-   [命令列語法關鍵](command-line-syntax-key.md)