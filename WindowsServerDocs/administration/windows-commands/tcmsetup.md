---
title: tcmsetup
description: Tcmsetup 命令的參考文章，此命令會設定和停用 TAPI 用戶端。
ms.topic: reference
ms.assetid: 15e0c10f-996f-4301-92e5-943f7ee8212d
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 315c13310d8059d5553dff9c780fbdd656867a57
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/05/2020
ms.locfileid: "91718065"
---
# <a name="tcmsetup"></a>tcmsetup

設定或停用 TAPI 用戶端。 若要讓 TAPI 正常運作，您必須執行此命令來指定 TAPI 用戶端將使用的遠端伺服器。

> [!IMPORTANT]
> 若要使用此命令，您必須是本機電腦上 **Administrators** 群組的成員，或者必須已被委派適當的許可權。 如果電腦已加入網域，則 **Domain Admins** 群組的成員可能可以執行此程式。 基於安全性最佳做法，請考慮以 [**執行身分**] 來執行此程序。

## <a name="syntax"></a>語法

```
tcmsetup [/q] [/x] /c <server1> [<server2> …]
tcmsetup  [/q] /c /d
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| /q | 防止顯示訊息方塊。 |
| /x | 指定連線導向的回撥用於高封包遺失的高流量網路。 省略此參數時，將使用不需連線的回撥。 |
| /C | 必要。 指定用戶端設定。 |
| `<server1>` | 必要。 指定擁有用戶端將使用之 TAPI 服務提供者的遠端伺服器名稱。 用戶端會使用服務提供者的線路與電話。 用戶端必須位於與伺服器相同的網域，或與包含伺服器的網域具有雙向信任關係的網域中。 |
| `<server2>…` | 指定此用戶端可以使用的任何其他伺服器。 如果要指定伺服器清單，請使用空格來分隔伺服器名稱。 |
| /d | 清除遠端伺服器清單。 防止 TAPI 用戶端使用遠端伺服器上的 TAPI 服務提供者，來停用 TAPI 用戶端。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 在用戶端使用者可以使用 TAPI 伺服器上的電話或線路之前，電話語音伺服器管理員必須將使用者指派至電話或線路。

- 透過此命令建立的電話語音伺服器清單，會取代用戶端可用的任何現有電話語音伺服器清單。 您無法使用此命令來新增至現有清單。

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [命令 shell 總覽](/previous-versions/windows/it-pro/windows-server-2003/cc737438(v=ws.10))

- [指定用戶端電腦上的電話語音伺服器](/previous-versions/windows/it-pro/windows-server-2003/cc759226(v=ws.10))

- [指派電話語音使用者到線路或電話](/previous-versions/windows/it-pro/windows-server-2003/cc736875(v=ws.10))
