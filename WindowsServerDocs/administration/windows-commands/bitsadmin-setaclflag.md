---
title: bitsadmin setaclflag
description: 適用於 Windows 命令主題**bitsadmin setaclflag** -設定存取控制清單傳用旗標。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6e3bcda0-827d-4dfd-8384-d1da018f3e10
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 89d825a4bc4512022fed98a3188537d3977fa3c3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867399"
---
# <a name="bitsadmin-setaclflag"></a>bitsadmin setaclflag

設定存取控制清單 (ACL) 傳用旗標的作業。 旗標會指出您想要維護所下載之檔案的擁有者和 ACL 資訊。 例如，為了維持與檔案的擁有者和群組，設定 **旗標** 至`OG`。

## <a name="syntax"></a>語法

```
bitsadmin /SetAclFlags <Job> <Flags>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|
|Flags|指定一或多個下列的旗標值：</br>-O:複製檔案的擁有者資訊。</br>-G:複製檔案群組資訊。</br>-D:複製檔案的 DACL 資訊。</br>-S： 複製 SACL 檔案的資訊。|

## <a name="remarks"></a>備註

SetAclFlags 參數用來維護工作下載從 Windows (SMB) 共用的資料時的擁有者及存取控制清單資訊。

## <a name="BKMK_examples"></a>範例

下列範例會設定存取控制清單傳用旗標，名為作業*myDownloadJob*維護的擁有者和群組資訊與下載的檔案。
```
C:\>bitsadmin /setaclflags myDownloadJob OG
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)