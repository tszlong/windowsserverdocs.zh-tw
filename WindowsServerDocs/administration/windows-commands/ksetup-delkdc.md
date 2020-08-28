---
title: ksetup delkdc
description: '>ksetup delkdc 命令的參考文章，此命令會刪除 Kerberos 領域金鑰發佈中心 (KDC) 名稱的實例。'
ms.topic: reference
ms.assetid: 7d6ec389-094c-4a7b-a78b-605497ddc289
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0ecc6048fb9c2b916603f2b30313dc21521fd821
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89025602"
---
# <a name="ksetup-delkdc"></a>ksetup delkdc

刪除 Kerberos 領域金鑰發佈中心 (KDC) 名稱的實例。

對應會儲存在登錄中的底下 `HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\LSA\Kerberos\Domains` 。 執行此命令之後，建議您確定 KDC 已移除，且不會再出現在清單中。

> [!NOTE]
> 若要從多部電腦移除領域設定資料，請使用 [ **安全性設定範本** ] 嵌入式管理單元搭配 [原則發佈]，而不是在個別電腦上明確使用 **>ksetup** 命令。

## <a name="syntax"></a>語法

```
ksetup /delkdc <realmname> <KDCname>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<realmname>` | 指定大寫 DNS 名稱，例如 CORP。CONTOSO.COM。 這是您執行 **>ksetup** 命令時所顯示的預設領域，而且是您想要從中刪除 KDC 的領域。 |
| `<KDCname>` | 指定區分大小寫的完整功能變數名稱，例如 mitkdc.contoso.com。 |

### <a name="examples"></a>範例

若要查看 Windows 領域和非 Windows 領域之間的所有關聯，以及判斷要移除哪些關聯，請輸入：

```
ksetup
```

若要移除關聯，請輸入：

```
ksetup /delkdc CORP.CONTOSO.COM mitkdc.contoso.com
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [>ksetup 命令](ksetup.md)

- [>ksetup addkdc 命令](ksetup-addkdc.md)