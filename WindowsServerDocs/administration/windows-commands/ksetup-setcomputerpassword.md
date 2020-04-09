---
title: ksetup： setcomputerpassword
description: '\* * * * 的 Windows 命令主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e307d8f6-3b93-4c24-ac04-f31549f7dc7d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3e65ea6e935d9fde9c23842755c36e418928dec7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80841371"
---
# <a name="ksetupsetcomputerpassword"></a>ksetup： setcomputerpassword



設定本機電腦的密碼。 如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
ksetup /setcomputerpassword <Password>
```

#### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<密碼 >|會使用提供的密碼來設定本機電腦上的電腦帳戶。</br>只能使用具有系統管理許可權的帳戶來設定密碼。 密碼可以是1到156個英數位元或特殊字元。|

## <a name="remarks"></a>備註

此命令只會影響電腦帳戶。

您必須重新開機電腦，密碼變更才會生效。

電腦帳戶密碼不會顯示在登錄中，或做為**ksetup**命令的輸出。

## <a name="examples"></a><a name=BKMK_Examples></a>典型

將本機電腦上的電腦帳戶密碼從 IPops897 變更為 IPop $ 897！。
```
ksetup /setcomputerpassword IPop$897!
```

## <a name="additional-references"></a>其他參考資料

-   [Ksetup](ksetup.md)
-   - [命令列語法關鍵](command-line-syntax-key.md)