---
title: ksetup dumpstate
description: '>ksetup dumpstate 命令的參考文章，其中顯示電腦上定義之所有領域的領域設定的目前狀態。'
ms.topic: reference
ms.assetid: 3ef2f7b8-97af-4f42-9542-cff324840637
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8723eac23cc2444bf4c16a661b13cc8cef4798dc
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89025522"
---
# <a name="ksetup-dumpstate"></a>ksetup dumpstate

顯示電腦上定義的所有領域之領域設定的目前狀態。 此命令會顯示與 **>ksetup** 命令相同的輸出。

## <a name="syntax"></a>語法

```
ksetup /dumpstate
```

### <a name="remarks"></a>備註

- 此命令的輸出包含預設領域 (電腦所屬的網域) 和此電腦上定義的所有領域。 每個領域包含下列各項：

  - 所有的金鑰發佈中心 (與此領域相關聯的 Kdc) 。

  - 此領域的所有 **設定領域** 旗標。

  - KDC 密碼。

- 此命令不會顯示 DNS 偵測或命令所指定的功能變數名稱 `ksetup /domain` 。

- 此命令不會顯示使用命令設定的電腦密碼 `ksetup /setcomputerpassword` 。

## <a name="examples"></a>範例

若要找出電腦上的 Kerberos 領域設定，請輸入：

```
ksetup /dumpstate
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [>ksetup 命令](ksetup.md)