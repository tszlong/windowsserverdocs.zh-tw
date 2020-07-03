---
title: ksetup getenctypeattr
description: Ksetup getenctypeattr 命令的參考文章，它會抓取網域的加密類型屬性。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6c7ec002-355e-474d-bc27-27215049f1a8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1fa86e8f9a9f2a2e552c7b968c447707b09e7e86
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85929142"
---
# <a name="ksetup-getenctypeattr"></a>ksetup getenctypeattr

抓取網域的加密類型屬性。 成功或失敗完成時，會顯示狀態訊息。

您可以藉由執行**klist**命令並查看輸出，來查看 Kerberos 票證授權票證（TGT）和工作階段金鑰的加密類型。 您可以藉由執行命令來設定要連接及使用的網域 `ksetup /domain <domainname>` 。

## <a name="syntax"></a>語法

```
ksetup /getenctypeattr <domainname>
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| `<domainname>` | 您想要建立連接的網功能變數名稱稱。 使用完整功能變數名稱或簡單格式的名稱，例如 corp.contoso.com 或 contoso。 |

### <a name="examples"></a>範例

若要驗證網域的 [加密類型] 屬性，請輸入：

```
ksetup /getenctypeattr mit.contoso.com
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [klist 命令](klist.md)

- [ksetup 命令](ksetup.md)

- [ksetup 網域命令](ksetup-domain.md)

- [ksetup addenctypeattr 命令](ksetup-addenctypeattr.md)

- [ksetup setenctypeattr 命令](ksetup-setenctypeattr.md)

- [ksetup delenctypeattr 命令](ksetup-delenctypeattr.md)
