---
title: Wbadmin get 狀態
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2911b944-7b95-46aa-8c1e-1d55a2fcc94c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 35fd640aa56bca7c5f5d6f3901fe095d0b8a73cc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863409"
---
# <a name="wbadmin-get-status"></a>Wbadmin get 狀態



報告目前正在執行備份或復原作業的狀態。

若要使用此子命令，您必須隸屬**Backup Operators**群組或有**系統管理員**群組，或者您必須已經被委派適當的權限。 此外，您必須執行**wbadmin**從提升權限的命令提示字元。 (若要開啟提升權限的命令提示字元上按一下滑鼠右鍵**命令提示字元**，然後按一下**系統管理員身分執行**。)

## <a name="syntax"></a>語法

```
wbadmin get status
```

## <a name="parameters"></a>參數

此子命令沒有任何參數。

## <a name="remarks"></a>備註

-   此子命令將不會停止直到目前的備份或復原作業已完成，子命令將會繼續執行，即使您關閉 [命令] 視窗。
-   如果您想要停止目前的備份或復原作業，使用**wbadmin 停止作業**子命令。

#### <a name="additional-references"></a>其他參考資料

-   [命令列語法關鍵](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   [Get-WBJob](https://technet.microsoft.com/library/jj902426.aspx) cmdlet