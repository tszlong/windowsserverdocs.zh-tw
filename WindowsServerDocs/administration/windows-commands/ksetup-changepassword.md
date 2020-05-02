---
title: ksetup： changepassword
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 283078e7-a88f-4875-90e6-f8605e6b7ea7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 32c48e896b77043820eea42159e20c089bd69fb8
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724728"
---
# <a name="ksetupchangepassword"></a>ksetup： changepassword



會使用金鑰發佈中心（KDC）密碼（kpasswd）值來變更已登入使用者的密碼。

## <a name="syntax"></a>語法

```
ksetup /changepassword <OldPasswd> <NewPasswd>
```

#### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<OldPasswd>|指出登入使用者的現有密碼。|
|\<NewPasswd>|指出登入使用者的新密碼。|

## <a name="remarks"></a>備註

此命令會使用 KDC 密碼（kpasswd）值來變更已登入使用者的密碼。 如果設定，kpasswd 會藉由執行**ksetup/dumpstate**命令，在輸出中顯示。

使用者的新密碼必須符合在此電腦上設定的所有密碼需求。

如果在目前的網域中找不到使用者帳戶，系統會要求您提供使用者帳戶所在的功能變數名稱。

如果您想要在下次登入時強制變更密碼，此命令允許使用星號（*），因此系統會提示使用者輸入新密碼。

命令的輸出會通知您「成功」或「失敗」狀態。

## <a name="examples"></a>範例

將目前登入此電腦的使用者密碼變更為此網域：
```
ksetup /changepassword Pas$w0rd Pa$$w0rd
```
變更目前在 Contoso 網域中登入之使用者的密碼：
```
ksetup /domain CONTOSO /changepassword Pas$w0rd Pa$$w0rd
```
強制目前登入的使用者在下次登入時變更密碼：
```
ksetup /changepassword Pas$w0rd *
```

## <a name="additional-references"></a>其他參考

-   - [命令列語法關鍵](command-line-syntax-key.md)