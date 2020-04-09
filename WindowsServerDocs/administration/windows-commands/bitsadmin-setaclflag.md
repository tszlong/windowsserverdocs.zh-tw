---
title: bitsadmin setaclflag
description: 適用于 bitsadmin setaclflag 的 Windows 命令主題，其會設定存取控制清單傳播旗標。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6e3bcda0-827d-4dfd-8384-d1da018f3e10
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4ac47e554dde6a555e891d89668cd12fec3179d4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849671"
---
# <a name="bitsadmin-setaclflag"></a>bitsadmin setaclflag

設定作業的存取控制清單（ACL）傳播旗標。 旗標指出您想要使用所要下載的檔案來維護擁有者和 ACL 資訊。 例如，若要以檔案維護擁有者和群組，請將 **旗標** 設定為 `OG`。

## <a name="syntax"></a>語法

```
bitsadmin /SetAclFlags <Job> <Flags>
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|
|Flags|指定下列一個或多個旗標值：</br>-O：使用檔案複製擁有者資訊。</br>-G：使用 file 複製群組資訊。</br>-D：使用 file 複製 DACL 資訊。</br>-S：使用 file 複製 SACL 資訊。|

## <a name="remarks"></a>備註

當作業從 Windows （SMB）共用下載資料時，會使用 SetAclFlags 參數來維護擁有者和存取控制清單資訊。

## <a name="examples"></a><a name=BKMK_examples></a>典型

下列範例會針對名為*myDownloadJob*的作業設定存取控制清單傳播旗標，以使用下載的檔案來維護擁有者和群組資訊。
```
C:\>bitsadmin /setaclflags myDownloadJob OG
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)