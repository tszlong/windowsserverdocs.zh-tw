---
title: schtasks 命令
description: 適用于 schtasks.exe 命令的參考文章，可排程命令和程式定期執行或在特定時間執行、在排程中加入和移除工作、啟動和停止隨選工作，以及顯示和變更排定的工作。
ms.topic: reference
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 09/16/2020
ms.openlocfilehash: 50b0bd7f7a55a7aa5889c39e4bf9f4e582af7f3b
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/26/2020
ms.locfileid: "91388634"
---
# <a name="schtasks-commands"></a>schtasks 命令

排程命令和程式定期執行或在特定時間執行、新增和移除排程中的工作、啟動和停止隨選工作，以及顯示和變更排定的工作。

> [!NOTE]
> **schtasks.exe**工具會執行與**主控台**中的**排程**工作相同的作業。 您可以搭配使用這些工具，也可以交替使用。

## <a name="required-permissions"></a>所需的權限

- 若要排程、查看及變更本機電腦上的所有工作，您必須是 Administrators 群組的成員。

- 若要排程、查看及變更遠端電腦上的所有工作，您必須是遠端電腦上 Administrators 群組的成員，或者您必須使用 **/u** 參數提供遠端電腦的系統管理員認證。

- 如果本機和遠端電腦位於相同網域中，或本機電腦位於遠端電腦網域信任的網域中，您可以在 **/create**或 **/change**操作中使用 **/u**參數。 否則，遠端電腦無法驗證指定的使用者帳戶，也無法確認該帳戶是否為系統管理員群組的成員。

- 您打算執行的工作必須具有適當的許可權;這些許可權因工作而異。 根據預設，工作會以本機電腦目前使用者的許可權執行，或以 **/u** 參數所指定之使用者的許可權執行（如果包含的話）。 o 執行具有不同使用者帳戶許可權或系統許可權的工作，請使用 **/ru** 參數。

## <a name="syntax"></a>語法

```
schtasks change
schtasks create
schtasks delete
schtasks end
schtasks query
schtasks run
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
|--|--|
| [schtasks 變更](schtasks-change.md) | 變更工作的下列一或多個屬性：<ul><li>工作執行的程式 (/tr) </li><li>用來執行工作的使用者帳戶 ( 的/ru) </li><li>使用者帳戶的密碼 (/rp) </li><li>將僅限互動式屬性新增至工作 (/it) </li></ul> |
| [schtasks 建立](schtasks-create.md) | 排定新的工作。|
| [schtasks 刪除](schtasks-delete.md) | 刪除已排程的工作。 |
| [schtasks end](schtasks-end.md) | 停止工作所啟動的程式。 |
| [schtasks 查詢](schtasks-query.md) | 顯示排程要在電腦上執行的工作。 |
| [schtasks 執行](schtasks-run.md) | 立即啟動排程工作。 **執行**作業會忽略排程，但會使用在工作中儲存的程式檔位置、使用者帳戶和密碼來立即執行工作。 |

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)
