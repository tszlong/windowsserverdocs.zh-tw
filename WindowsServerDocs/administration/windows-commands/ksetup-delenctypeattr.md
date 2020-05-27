---
title: ksetup delenctypeattr
description: Ksetup delenctypeattr 的參考主題，它會移除網域的加密類型屬性。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4fc25ef3-e271-4229-a712-72c507df55aa
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b3076a25b619615402a599bd8aaa6ce9d10d4fe0
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2020
ms.locfileid: "83817938"
---
# <a name="ksetup-delenctypeattr"></a>ksetup delenctypeattr

移除網域的加密類型屬性。 成功或失敗完成時，會顯示狀態訊息。

您可以藉由執行**klist**命令並查看輸出，來查看 Kerberos 票證授權票證（TGT）和工作階段金鑰的加密類型。 您可以藉由執行命令來設定要連接及使用的網域 `ksetup /domain <domainname>` 。

## <a name="syntax"></a>語法

```
ksetup /delenctypeattr <domainname>
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| ----------| ----------- |
| `<domainname>` | 您想要建立連接的網功能變數名稱稱。 您可以使用完整功能變數名稱或簡單格式的名稱，例如 corp.contoso.com 或 contoso。 |

### <a name="examples"></a>範例

若要判斷這部電腦上設定的目前加密類型，請輸入：

```
klist
```

若要將網域設定為 mit.contoso.com，請輸入：

```
ksetup /domain mit.contoso.com
```

若要確認網域的 [加密類型] 屬性為何，請輸入：

```
ksetup /getenctypeattr mit.contoso.com
```

若要移除網域 mit.contoso.com 的 [設定加密類型] 屬性，請輸入：

```
ksetup /delenctypeattr mit.contoso.com
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [klist 命令](klist.md)

- [ksetup 命令](ksetup.md)

- [ksetup 網域命令](ksetup-domain.md)

- [ksetup addenctypeattr 命令](ksetup-addenctypeattr.md)

- [ksetup setenctypeattr 命令](ksetup-setenctypeattr.md)
