---
title: ksetup:changepassword
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 283078e7-a88f-4875-90e6-f8605e6b7ea7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f629c6c7930777583df38f5af900ed380ec60f9c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878529"
---
# <a name="ksetupchangepassword"></a>ksetup:changepassword



若要變更登入使用者的密碼，會使用金鑰發佈中心 (KDC) 密碼 (kpasswd) 值。 如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
ksetup /changepassword <OldPasswd> <NewPasswd>
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<OldPasswd>|指出登入使用者的現有密碼。|
|\<NewPasswd>|會指出登入使用者的新密碼。|

## <a name="remarks"></a>備註

此命令會使用 KDC (kpasswd) 的密碼值，若要變更登入使用者的密碼。 Kpasswd，如果設定，會顯示在輸出中，執行**ksetup /dumpstate**命令。

使用者的新密碼必須符合此電腦上的所有密碼需求。

如果目前的網域中找不到使用者帳戶，系統會要求您提供的使用者帳戶所在的網域名稱。

如果您想要強制在下次登入時變更密碼，此命令會允許使用星號 （*），因此將會提示使用者輸入新密碼。

命令的輸出會通知您的成功或失敗狀態。

## <a name="BKMK_Examples"></a>範例

變更目前登入在這個網域中的這台電腦之使用者的密碼：
```
ksetup /changepassword Pas$w0rd Pa$$w0rd
```
變更目前登入 Contoso 網域中之使用者的密碼：
```
ksetup /domain CONTOSO /changepassword Pas$w0rd Pa$$w0rd
```
強制目前登入的使用者在下次登入時變更密碼：
```
ksetup /changepassword Pas$w0rd *
```

#### <a name="additional-references"></a>其他參考資料

-   [命令列語法關鍵](command-line-syntax-key.md)