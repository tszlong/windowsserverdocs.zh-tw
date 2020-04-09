---
title: tcmsetup
description: 瞭解如何設定及停用 TAPI 用戶端。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 15e0c10f-996f-4301-92e5-943f7ee8212d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bbfc9a0238d258f11233b0e48a30048c1d62cef4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80833411"
---
# <a name="tcmsetup"></a>tcmsetup



設定或停用 TAPI 用戶端。

## <a name="syntax"></a>語法

```
tcmsetup [/q] [/x] /c <Server1> [<Server2> …] 
tcmsetup  [/q] /c /d
```

#### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/q|防止顯示訊息方塊。|
|/x|指定連線導向的回撥用於高封包遺失的高流量網路。 省略此參數時，將使用不需連線的回撥。|
|/c|必要。 指定用戶端設定。|
|\<Server1 >|必要。 指定具有用戶端將使用之 TAPI 服務提供者的遠端伺服器名稱。 用戶端將會使用服務提供者的線路與電話。 用戶端必須位於與伺服器相同的網域中，或與包含伺服器的網域具有雙向信任關係的網域中。|
|\<Server2 > 。|指定此用戶端可以使用的任何其他伺服器。 如果要指定伺服器清單，請使用空格來分隔伺服器名稱。|
|/d|清除遠端伺服器清單。 防止 TAPI 用戶端使用遠端伺服器上的 TAPI 服務提供者，來停用 TAPI 用戶端。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

-   若要執行這項程序，您必須是本機電腦的 Administrators 群組成員，或者必須委派有適當的授權。 如果電腦已加入網域中，那麼只有 Domain Admins 群組成員才能執行這項程序。 基於安全性最佳做法，請考慮以 [**執行身分**] 來執行此程序。
-   為了讓 TAPI 正常運作，您必須執行**tcmsetup**來指定 TAPI 用戶端將使用的遠端伺服器。
-   在用戶端使用者可以在 TAPI 伺服器上使用電話或線路之前，電話語音伺服器管理員必須將使用者指派給電話或線路。
-   透過此命令建立的電話語音伺服器清單，會取代用戶端可用的任何現有電話語音伺服器清單。 您無法使用此命令新增到現有清單。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

[命令 shell 總覽](https://technet.microsoft.com/library/cc737438(v=ws.10).aspx)

[指定用戶端電腦上的電話語音伺服器](https://technet.microsoft.com/library/cc759226(v=ws.10).aspx)

[指派電話語音使用者到線路或電話](https://technet.microsoft.com/library/cc736875(v=ws.10).aspx)

