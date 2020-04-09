---
title: wbadmin 停止作業
description: Wbadmin stop job 的 Windows 命令主題，它會取消目前正在執行的備份或復原操作。 無法重新開機已取消的作業-您必須從頭重新執行已取消的備份或復原操作。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3b83b398-39c7-4410-bf17-5c1fb1a4f46d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4a00b4a93e0aaa954f8f07adae825a4f582c5581
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80829481"
---
# <a name="wbadmin-stop-job"></a>wbadmin 停止作業



取消目前正在執行的備份或復原作業。 無法重新開機已取消的作業-您必須從頭重新執行已取消的備份或復原操作。

若要使用此子命令停止備份或復原作業，您必須是**Backup Operators**群組或**Administrators**群組的成員，或者必須已被委派適當的許可權。 此外，您必須從提升許可權的命令提示字元執行**wbadmin** 。 （若要開啟提升許可權的命令提示字元，以滑鼠右鍵按一下**命令提示**字元，然後按一下 [以**系統管理員身分執行**]）

## <a name="syntax"></a>語法

```
wbadmin stop job
[-quiet]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|-quiet|執行子命令，而不提示使用者。|

## <a name="additional-references"></a>其他參考資料

-   - [命令列語法關鍵](command-line-syntax-key.md)
-   [Restore](wbadmin.md)