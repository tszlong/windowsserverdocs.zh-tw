---
title: wbadmin get status
description: Wbadmin get status 命令的參考文章，它會報告目前正在執行之備份或復原作業的狀態。
ms.topic: reference
ms.assetid: 2911b944-7b95-46aa-8c1e-1d55a2fcc94c
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 764c5d3dc808b12488e36af5808acf4c895c5af1
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2020
ms.locfileid: "92155948"
---
# <a name="wbadmin-get-status"></a>wbadmin get status

報告目前正在執行之備份或復原作業的狀態。

若要使用此命令取得目前正在執行之備份或復原作業的狀態，您必須是 **Backup Operators** 群組或 **Administrators** 群組的成員，或者必須已被委派適當的許可權。 此外，您必須從提升許可權的命令提示字元中，以滑鼠右鍵按一下 [**命令提示**字元]，然後選取 [以**系統管理員身分執行**]，以執行**wbadmin** 。

> [!IMPORTANT]
> 此命令不會停止，直到備份或復原作業完成為止。 即使您關閉命令視窗，也會繼續執行命令。 若要停止目前的備份或復原操作，請執行 [wbadmin stop job](wbadmin-stop-job.md) 命令。

## <a name="syntax"></a>Syntax

```
wbadmin get status
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [wbadmin 命令](wbadmin.md)

- [wbadmin stop job 命令](wbadmin-stop-job.md)

- [Get-wbjob](/powershell/module/windowserverbackup/Get-WBJob)
