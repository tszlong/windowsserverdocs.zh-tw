---
title: bitsadmin setaclflag
description: '**Bitsadmin setaclflag**的 Windows 命令主題-設定存取控制清單傳播旗標。'
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: fbdb12c29af7b4db8b25846d43ee1c93b2454ff2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380763"
---
# <a name="bitsadmin-setaclflag"></a>bitsadmin setaclflag

設定作業的存取控制清單（ACL）傳播旗標。 旗標指出您想要使用所要下載的檔案來維護擁有者和 ACL 資訊。 例如，若要使用檔案來維護擁有者和群組，請  to `OG` 中設定 **旗標**。

## <a name="syntax"></a>語法

```
bitsadmin /SetAclFlags <Job> <Flags>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|
|Flags|指定下列一個或多個旗標值：</br>I/O使用檔案複製擁有者資訊。</br>G使用 file 複製群組資訊。</br>D使用 file 複製 DACL 資訊。</br>-S：使用 file 複製 SACL 資訊。|

## <a name="remarks"></a>備註

當作業從 Windows （SMB）共用下載資料時，會使用 SetAclFlags 參數來維護擁有者和存取控制清單資訊。

## <a name="BKMK_examples"></a>典型

下列範例會針對名為*myDownloadJob*的作業設定存取控制清單傳播旗標，以使用下載的檔案來維護擁有者和群組資訊。
```
C:\>bitsadmin /setaclflags myDownloadJob OG
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)