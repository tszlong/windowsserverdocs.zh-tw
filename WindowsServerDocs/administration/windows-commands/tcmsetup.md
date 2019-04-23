---
title: tcmsetup
description: 了解如何設定和停用 TAPI 用戶端。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 15e0c10f-996f-4301-92e5-943f7ee8212d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ac92c7b793274227bd20e6fa90a4106a32ea0446
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861999"
---
# <a name="tcmsetup"></a>tcmsetup



設定或停用 TAPI 用戶端。

## <a name="syntax"></a>語法

```
tcmsetup [/q] [/x] /c <Server1> [<Server2> …] 
tcmsetup  [/q] /c /d
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/q|會防止顯示訊息方塊。|
|/x|指定將連線導向的回撥會用於高流量網路封包遺失的高。 當省略這個參數時，會使用無連線的回撥。|
|/c|必要。 指定用戶端安裝程式。|
|\<Server1>|必要。 指定用戶端會使用 TAPI 服務提供者的遠端伺服器的名稱。 用戶端會使用服務提供者的線路與電話。 用戶端必須在與伺服器位於相同網域或網域具有雙向信任關係，包含伺服器的網域中。|
|\<Server2>…|指定任何其他伺服器或可使用此用戶端的伺服器。 如果您指定的伺服器清單，請使用空格來分隔伺服器名稱。|
|/d|清除遠端伺服器的清單。 停用 TAPI 用戶端，藉由防止它使用遠端伺服器上的 TAPI 服務提供者。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

-   若要執行此程序，您必須是本機電腦上的 Administrators 群組成員或是已經委派您適當的權限。 如果該電腦已加入網域，則 Domain Admins 群組的成員便可執行此程序。 基於安全性最佳做法，請考慮以 [**執行身分**] 來執行此程序。
-   為了讓 TAPI 才能正確運作，您必須執行**tcmsetup**來指定將供 TAPI 用戶端的遠端伺服器。
-   用戶端使用者可以使用電話或線路上的 TAPI 伺服器之前，電話語音伺服器系統管理員必須將使用者指派到電話或線路。
-   此命令會建立電話語音伺服器清單會取代任何現有的用戶端可用的電話語音伺服器清單。 您無法使用此命令將新增到現有的清單。

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)

[命令殼層概觀](https://technet.microsoft.com/library/cc737438(v=ws.10).aspx)

[指定用戶端電腦上的電話語音伺服器](https://technet.microsoft.com/library/cc759226(v=ws.10).aspx)

[指派電話語音使用者到線路或電話](https://technet.microsoft.com/library/cc736875(v=ws.10).aspx)

