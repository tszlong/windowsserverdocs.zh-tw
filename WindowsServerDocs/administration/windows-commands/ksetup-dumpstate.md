---
title: ksetup dumpstate
description: Ksetup dumpstate 命令的參考主題，它會顯示電腦上定義之所有領域的領域設定目前狀態。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3ef2f7b8-97af-4f42-9542-cff324840637
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4ccb75ac143239d97b823fb7030f9a8020b4b4f6
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2020
ms.locfileid: "83817738"
---
# <a name="ksetup-dumpstate"></a>ksetup dumpstate

顯示電腦上定義之所有領域的領域設定目前狀態。 此命令會顯示與**ksetup**命令相同的輸出。

## <a name="syntax"></a>語法

```
ksetup /dumpstate
```

### <a name="remarks"></a>備註

- 此命令的輸出包含預設領域（電腦所屬的網域），以及此電腦上定義的所有領域。 每個領域包含下列各項：

  - 與此領域相關聯的所有金鑰發佈中心（Kdc）。

  - 此領域的所有**設定領域**旗標。

  - KDC 密碼。

- 此命令不會顯示 DNS 偵測或命令所指定的功能變數名稱 `ksetup /domain` 。

- 此命令不會顯示使用命令所設定的電腦密碼 `ksetup /setcomputerpassword` 。

## <a name="examples"></a>範例

若要找出電腦上的 Kerberos 領域設定，請輸入：

```
ksetup /dumpstate
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [ksetup 命令](ksetup.md)