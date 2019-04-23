---
title: Wbadmin get 磁碟
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 320edef1-df11-446b-a183-9f81811ef938
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b7d30567285fa0e295758c8b4f4c7cdb45ae621c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858869"
---
# <a name="wbadmin-get-disks"></a>Wbadmin get 磁碟



列出目前上線供本機電腦的內部和外部磁碟。

若要列出的磁碟，都在線上使用此子命令，您必須隸屬**Backup Operators**群組或有**系統管理員**群組，或者您必須已經被委派適當的權限。 此外，您必須執行**wbadmin**從提升權限的命令提示字元。 (若要開啟提升權限的命令提示字元上按一下滑鼠右鍵**命令提示字元**，然後按一下**系統管理員身分執行**。)

## <a name="syntax"></a>語法

```
wbadmin get disks
```

## <a name="parameters"></a>參數

此子命令沒有任何參數。

#### <a name="additional-references"></a>其他參考資料

-   [命令列語法關鍵](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   [取得 WBDisk](https://technet.microsoft.com/library/jj902446.aspx) cmdlet