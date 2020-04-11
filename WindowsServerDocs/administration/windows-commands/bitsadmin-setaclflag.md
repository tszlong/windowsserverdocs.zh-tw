---
title: bitsadmin setaclflag
description: 適用于**bitsadmin setaclflag**的 Windows 命令主題，其會設定存取控制清單（ACL）傳播旗標。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6e3bcda0-827d-4dfd-8384-d1da018f3e10
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f0aae550e94d04db518edccafb1d6bcf46d0320b
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/11/2020
ms.locfileid: "81123068"
---
# <a name="bitsadmin-setaclflag"></a>bitsadmin setaclflag

設定作業的存取控制清單（ACL）傳播旗標。 旗標指出您想要使用所要下載的檔案來維護擁有者和 ACL 資訊。 例如，若要使用檔案來維護擁有者和群組，請將 **flags**參數設為 `og`。

## <a name="syntax"></a>語法

```
bitsadmin /setaclflag <job> <flags>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| 工作 | 作業的顯示名稱或 GUID。 |
| flags | 指定一或多個值，包括：<ul><li>**o** -使用 file 複製擁有者資訊。</li><li>**g** -使用 file 複製群組資訊。</li><li>**d** -使用檔案複製任意存取控制清單（DACL）資訊。</li><li>**s** -使用檔案複製系統存取控制清單（SACL）資訊。</li></ul> |

## <a name="remarks"></a>備註

當作業從 Windows （SMB）共用下載資料時，會使用/setaclflag 參數來維護擁有者和存取控制清單資訊。

## <a name="examples"></a>範例

下列範例會針對名為*myDownloadJob*的作業設定存取控制清單傳播旗標，以使用下載的檔案來維護擁有者和群組資訊。

```
C:\>bitsadmin /setaclflags myDownloadJob og
```

## <a name="additional-references"></a>其他參考資料

[命令列語法索引鍵](command-line-syntax-key.md)&reg;'    