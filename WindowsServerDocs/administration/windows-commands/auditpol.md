---
title: auditpol
description: '**Auditpol**的 Windows 命令主題-顯示相關資訊，並執行操作稽核原則的函式。'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a02cfb9d-732f-4e77-aeba-f18265daa3af
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5e249a9e2a07505f052b774208c514b4d16879b8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382376"
---
# <a name="auditpol"></a>auditpol



顯示和執行函式的相關資訊，以操作稽核原則。

如需如何使用此命令的範例，請參閱每個主題中的範例一節。

## <a name="syntax"></a>語法

```
Auditpol command [<sub-command><options>]
```

## <a name="parameters"></a>參數

|子命令|描述|
|-----------|-----------|
|/get|顯示目前的稽核原則。</br>如需語法和選項，請參閱[Auditpol get](auditpol-get.md) 。|
|/set|設定稽核原則。</br>如需語法和選項，請參閱[Auditpol set](auditpol-set.md) 。|
|/list|顯示可選取的原則元素。</br>如需語法和選項，請參閱[Auditpol list](auditpol-list.md) 。|
|/備份|將稽核原則儲存至檔案。</br>如需語法和選項，請參閱[Auditpol backup](auditpol-backup.md) 。|
|/restore|從先前使用 auditpol/backup. 所建立的檔案還原稽核原則</br>如需語法和選項，請參閱[Auditpol restore](auditpol-restore.md) 。|
|/clear|清除稽核原則。</br>如需語法和選項，請參閱[Auditpol clear](auditpol-clear.md) 。|
|/remove|移除所有的每一使用者稽核原則設定，並停用所有系統稽核原則設定。</br>如需語法和選項，請參閱[Auditpol remove](auditpol-remove.md) 。|
|/resourceSACL|設定全域資源系統存取控制清單（Sacl）。</br>注意：僅適用于 Windows 7 和 Windows Server 2008 R2。</br>請參閱[Auditpol resourceSACL](auditpol-resourcesacl.md)。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

稽核原則命令列工具可以用來：
-   設定並查詢系統稽核原則。
-   設定並查詢每個使用者的稽核原則。
-   設定和查詢的審核選項。
-   設定並查詢用來委派稽核原則存取權的安全描述項。
-   將稽核原則報告或備份至逗號分隔值（CSV）文字檔。
-   從 CSV 文字檔載入稽核原則。
-   設定全域資源 Sacl。

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)