---
title: ftp type
description: Ftp type 命令的參考文章，可設定或顯示檔案傳輸類型。
ms.topic: reference
ms.assetid: 6e96dcd4-08f8-4e7b-90b7-1e1761fea4c7
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 943ac3ca85c1e99118ea772338ea427d758927cb
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89639396"
---
# <a name="ftp-type"></a>ftp type

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

設定或顯示檔案傳輸類型。 **Ftp**命令支援 ASCII (預設) 和二進位影像檔案傳輸類型：

- 我們建議您在傳輸文字檔時使用 ASCII。 在 ASCII 模式中，會執行從網路標準字元集到和轉換的字元。 例如，系統會根據目標作業系統，視需要轉換行尾字元。

- 我們建議您在傳輸可執行檔時使用二進位檔。 在二進位模式中，檔案會以單一位元組單位傳輸。

## <a name="syntax"></a>語法

```
type [<typename>]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `[<typename>]` | 指定檔案傳輸類型。 如果您未指定此參數，則會顯示目前的型別。|

### <a name="examples"></a>範例

若要將檔案傳輸類型設定為 ASCII，請輸入：

```
type ascii
```

若要將傳輸檔案類型設定為 [二進位]，請輸入：

```
type binary
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [其他 FTP 指導方針](/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
