---
title: ftp 類型
description: Ftp type 命令的參考主題，其會設定或顯示檔案傳輸類型。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6e96dcd4-08f8-4e7b-90b7-1e1761fea4c7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 02b6d5b4bd7944c9f4126ba4877360de02586cfb
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820268"
---
# <a name="ftp-type"></a>ftp 類型

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

設定或顯示檔案傳輸類型。 **Ftp**命令同時支援 ASCII （預設）和二進位圖像檔案傳輸類型：

- 我們建議您在傳輸文字檔時使用 ASCII。 在 ASCII 模式中，會執行與網路標準字元集之間的字元轉換。 例如，根據目標作業系統，會視需要轉換行尾字元。

- 我們建議您在傳輸可執行檔時使用 binary。 在二進位模式中，檔案會以一個位元組的單位傳輸。

## <a name="syntax"></a>語法

```
type [<typename>]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| `[<typename>]` | 指定檔案傳輸類型。 如果您未指定此參數，則會顯示目前的類型。|

### <a name="examples"></a>範例

若要將檔案傳輸類型設定為 ASCII，請輸入：

```
type ascii
```

若要將傳輸檔案類型設定為 binary，請輸入：

```
type binary
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [其他 FTP 指引](https://docs.microsoft.com/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
