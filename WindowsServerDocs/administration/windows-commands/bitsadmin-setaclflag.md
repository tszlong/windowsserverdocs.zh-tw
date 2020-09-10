---
title: bitsadmin setaclflag
description: Bitsadmin setaclflag 命令的參考文章，此命令會將存取控制清單 (ACL) 傳播旗標。
ms.topic: reference
ms.assetid: 6e3bcda0-827d-4dfd-8384-d1da018f3e10
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 0cc0dac9027bd76735592620f89118318548dbdb
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89631052"
---
# <a name="bitsadmin-setaclflag"></a>bitsadmin setaclflag

設定作業 (ACL) 傳播旗標的存取控制清單。 旗標表示您想要使用所下載的檔案來維護擁有者和 ACL 資訊。 例如，若要維護檔案的擁有者和群組，請將 **旗標** 參數設定為 `og` 。

## <a name="syntax"></a>語法

```
bitsadmin /setaclflag <job> <flags>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| 作業 | 作業的顯示名稱或 GUID。 |
| flags | 指定一或多個值，包括：<ul><li>**o** -複製具有檔案的擁有者資訊。</li><li>**g** -使用 file 複製群組資訊。</li><li>**d** -複製任意存取控制清單 (DACL) 資訊與檔案。</li><li>**s** -複製系統存取控制清單 (SACL) 具有檔案的資訊。</li></ul> |

## <a name="examples"></a>範例

若要設定名為 *myDownloadJob*之作業的存取控制清單傳播旗標，請使用下載的檔案來維護擁有者和群組資訊。

```
bitsadmin /setaclflags myDownloadJob og
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
