---
title: bitsadmin setaclflag
description: Bitsadmin setaclflag 命令的參考文章，其會設定存取控制清單（ACL）傳播旗標。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6e3bcda0-827d-4dfd-8384-d1da018f3e10
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dcf07f944813c0c8d7a4ff4c4f52c598c0f3bf47
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85927956"
---
# <a name="bitsadmin-setaclflag"></a>bitsadmin setaclflag

設定作業的存取控制清單（ACL）傳播旗標。 旗標指出您想要使用所要下載的檔案來維護擁有者和 ACL 資訊。 例如，若要使用檔案來維護擁有者和群組，請將 **flags**參數設為 `og` 。

## <a name="syntax"></a>語法

```
bitsadmin /setaclflag <job> <flags>
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| 作業 | 作業的顯示名稱或 GUID。 |
| flags | 指定一或多個值，包括：<ul><li>**o** -使用 file 複製擁有者資訊。</li><li>**g** -使用 file 複製群組資訊。</li><li>**d** -使用檔案複製任意存取控制清單（DACL）資訊。</li><li>**s** -使用檔案複製系統存取控制清單（SACL）資訊。</li></ul> |

## <a name="examples"></a>範例

若要為名為*myDownloadJob*的作業設定存取控制清單傳播旗標，因此它會使用下載的檔案來維護擁有者和群組資訊。

```
bitsadmin /setaclflags myDownloadJob og
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
