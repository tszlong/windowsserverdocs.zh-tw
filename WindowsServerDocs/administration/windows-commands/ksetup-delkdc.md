---
title: ksetup delkdc
description: Ksetup delkdc 命令的參考文章，其會刪除 Kerberos 領域的金鑰發佈中心 (KDC) 名稱的實例。
ms.topic: article
ms.assetid: 7d6ec389-094c-4a7b-a78b-605497ddc289
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f57e409afc62b831590f371befee2775f5c9cce9
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87887974"
---
# <a name="ksetup-delkdc"></a>ksetup delkdc

刪除 Kerberos 領域的金鑰發佈中心 (KDC) 名稱的實例。

對應會儲存在登錄中的底下 `HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\LSA\Kerberos\Domains` 。 執行此命令之後，建議您確定 KDC 已移除，且不再出現于清單中。

> [!NOTE]
> 若要從多部電腦移除領域設定資料，請使用**安全性設定範本**嵌入式管理單元搭配原則發佈，而不是在個別電腦上明確使用**ksetup**命令。

## <a name="syntax"></a>語法

```
ksetup /delkdc <realmname> <KDCname>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<realmname>` | 指定大寫 DNS 名稱，例如 CORP。CONTOSO.COM。 這是當您執行**ksetup**命令時顯示的預設領域，而這是您想要從中刪除 KDC 的領域。 |
| `<KDCname>` | 指定區分大小寫的完整功能變數名稱，例如 mitkdc.contoso.com。 |

### <a name="examples"></a>範例

若要在 Windows 領域和非 Windows 領域之間查看所有關聯，並判斷要移除哪些內容，請輸入：

```
ksetup
```

若要移除關聯，請輸入：

```
ksetup /delkdc CORP.CONTOSO.COM mitkdc.contoso.com
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [ksetup 命令](ksetup.md)

- [ksetup addkdc 命令](ksetup-addkdc.md)