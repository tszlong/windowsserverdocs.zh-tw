---
title: ftp cd
description: Ftp cd 命令的參考主題，它會變更遠端電腦上的工作目錄。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a574855a-31b4-45c6-bce2-581c7231c99b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dbeff9c2fca654120347f0f616325b700f5c9be3
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2020
ms.locfileid: "83819998"
---
# <a name="ftp-cd"></a>ftp cd

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

變更遠端電腦上的工作目錄。

## <a name="syntax"></a>語法

```
cd <remotedirectory>
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| <remotedirectory> | 指定您要變更的遠端電腦上的目錄。 |

### <a name="examples"></a>範例

若要將遠端電腦上的目錄變更為*檔，請*輸入：

```
cd Docs
```

若要將遠端電腦上的目錄變更為 [*可能*的影片]，請輸入：

```
cd  May Videos
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [其他 FTP 指引](https://docs.microsoft.com/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
