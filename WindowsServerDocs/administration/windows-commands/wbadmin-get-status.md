---
title: wbadmin get 狀態
description: Wbadmin get status 的參考主題，它會報告目前正在執行之備份或復原作業的狀態。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2911b944-7b95-46aa-8c1e-1d55a2fcc94c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0e41c54b9f916f0032a4976cdfa6d3ca101fb744
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2020
ms.locfileid: "83821188"
---
# <a name="wbadmin-get-status"></a>wbadmin get 狀態



報告目前正在執行之備份或復原作業的狀態。

若要使用這個子命令，您必須是**Backup Operators**群組或**Administrators**群組的成員，或者必須已被委派適當的許可權。 此外，您必須從提升許可權的命令提示字元執行**wbadmin** 。 （若要開啟提升許可權的命令提示字元，請以滑鼠右鍵按一下**命令提示**字元，然後按一下 [以**系統管理員身分執行**]）。

## <a name="syntax"></a>語法

```
wbadmin get status
```

### <a name="parameters"></a>參數

這個子命令沒有任何參數。

## <a name="remarks"></a>備註

-   在目前的備份或復原作業完成之前，這個子命令不會停止，即使您關閉命令視窗，子命令仍會繼續執行。
-   如果您想要停止目前的備份或復原作業，請使用**wbadmin stop job**子命令。

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)
-   [Restore](wbadmin.md)
-   [Get-wbjob](https://technet.microsoft.com/library/jj902426.aspx) Cmdlet