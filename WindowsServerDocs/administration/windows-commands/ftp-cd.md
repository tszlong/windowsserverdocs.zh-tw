---
title: ftp cd
description: Ftp cd 命令的參考文章，它會變更遠端電腦上的工作目錄。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a574855a-31b4-45c6-bce2-581c7231c99b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0442e2ad6070b84e84632bc6d8646b979d7f948b
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85933288"
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

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [其他 FTP 指引](https://docs.microsoft.com/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
