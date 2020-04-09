---
title: auditpol
description: '**Auditpol**的 Windows 命令主題，它會顯示和執行函式以操作稽核原則的相關資訊。'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a02cfb9d-732f-4e77-aeba-f18265daa3af
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 00365b0e46b8bff761cf991dbdbd09d8f5e9c687
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851131"
---
# <a name="auditpol"></a>auditpol

顯示和執行函式的相關資訊，以操作稽核原則。

如需如何使用此命令的範例，請參閱每個主題中的範例一節。

## <a name="syntax"></a>語法

```
auditpol command [<sub-command><options>]
```

#### <a name="parameters"></a>參數

| 子命令 | 描述 |
| ----------- | ----------- |
| /get | 顯示目前的稽核原則。 如需詳細資訊，請參閱[auditpol get](auditpol-get.md) for 語法和選項。 |
| /set | 設定稽核原則。 如需詳細資訊，請參閱適用于語法和選項的[auditpol set](auditpol-set.md) 。 |
| /list | 顯示可選取的原則元素。 如需詳細資訊，請參閱[auditpol list](auditpol-list.md)中的語法和選項。 |
| /備份 | 將稽核原則儲存至檔案。 如需詳細資訊，請參閱[auditpol backup](auditpol-backup.md)以取得語法和選項。 |
| /restore | 從先前使用 auditpol/backup. 所建立的檔案還原稽核原則 如需詳細資訊，請參閱適用于語法和選項的[auditpol restore](auditpol-restore.md) 。 |
| /clear | 清除稽核原則。 如需詳細資訊，請參閱[auditpol clear](auditpol-clear.md)以取得語法和選項。 |
| /remove | 移除所有的每一使用者稽核原則設定，並停用所有系統稽核原則設定。 如需詳細資訊，請參閱[auditpol remove](auditpol-remove.md)中的語法和選項。 |
| /resourceSACL | 設定全域資源系統存取控制清單（Sacl）。 **注意：** 僅適用于 Windows 7 和 Windows Server 2008 R2。 如需詳細資訊，請參閱[Auditpol resourceSACL](auditpol-resourcesacl.md)。 |
| /?| 在命令提示字元顯示說明。 |

## <a name="remarks"></a>備註

稽核原則命令列工具可以用來：

- 設定並查詢系統稽核原則。

- 設定並查詢每個使用者的稽核原則。

- 設定和查詢的審核選項。

- 設定並查詢用來委派稽核原則存取權的安全描述項。

- 將稽核原則報告或備份至逗號分隔值（CSV）文字檔。

- 從 CSV 文字檔載入稽核原則。

- 設定全域資源 Sacl。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)