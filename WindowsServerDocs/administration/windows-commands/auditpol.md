---
title: auditpol
description: Auditpol 命令的參考文章，它會顯示和執行函式以操作稽核原則的相關資訊。
ms.topic: reference
ms.assetid: a02cfb9d-732f-4e77-aeba-f18265daa3af
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 83798a38f304a9e3ef1e32fe189bf37949e205ec
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89633057"
---
# <a name="auditpol"></a>auditpol

顯示和執行函式以操作稽核原則的相關資訊，包括：

- 設定和查詢系統稽核原則。

- 設定和查詢每一使用者的稽核原則。

- 設定和查詢審核選項。

- 設定和查詢用來將存取權委派給稽核原則的安全描述項。

- 將稽核原則報告或備份到以逗號分隔的值， (CSV) 文字檔。

- 從 CSV 文字檔載入稽核原則。

- 設定全域資源 Sacl。

## <a name="syntax"></a>語法

```
auditpol command [<sub-command><options>]
```

### <a name="parameters"></a>參數

| 子命令 | 描述 |
| ----------- | ----------- |
| /get | 顯示目前的稽核原則。 如需詳細資訊，請參閱 [auditpol get](auditpol-get.md) 以取得語法和選項。 |
| /set | 設定稽核原則。 如需詳細資訊，請參閱「適用于語法和選項的 [auditpol 設定](auditpol-set.md) 」。 |
| /list | 顯示可選取的原則元素。 如需詳細資訊，請參閱適用于語法和選項的 [auditpol 清單](auditpol-list.md) 。 |
| /backup | 將稽核原則儲存至檔案。 如需詳細資訊，請參閱 [auditpol backup](auditpol-backup.md) 以取得語法和選項。 |
| /restore | 從先前使用 auditpol 建立的檔案還原稽核原則/backup。 如需詳細資訊，請參閱 [auditpol restore](auditpol-restore.md) 以取得語法和選項。 |
| /clear | 清除稽核原則。 如需詳細資訊，請參閱 [auditpol clear](auditpol-clear.md) 以取得語法和選項。 |
| /remove | 移除所有的每個使用者稽核原則設定，並停用所有系統稽核原則設定。 如需詳細資訊，請參閱「 [auditpol 移除](auditpol-remove.md) 」以取得語法和選項。 |
| /resourceSACL | 設定全域資源系統存取控制清單 (Sacl) 。 **注意：** 僅適用于 Windows 7 和 Windows Server 2008 R2。 如需詳細資訊，請參閱 [Auditpol resourceSACL](auditpol-resourcesacl.md)。 |
| /?| 在命令提示字元顯示說明。 |

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
