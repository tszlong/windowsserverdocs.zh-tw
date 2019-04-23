---
title: auditpol
description: 適用於 Windows 命令主題**auditpol** -顯示相關資訊，並執行各種功能以操控稽核原則。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: d7e8364be977e868ac161704e67c37ec5c400e49
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849219"
---
# <a name="auditpol"></a>auditpol



顯示相關資訊，並執行函式來管理稽核原則。

如需如何使用此命令的範例，請參閱 < 範例 > 一節中的每個主題。

## <a name="syntax"></a>語法

```
Auditpol command [<sub-command><options>]
```

## <a name="parameters"></a>參數

|子命令|描述|
|-----------|-----------|
|/get|顯示目前的稽核原則。</br>請參閱[Auditpol get](auditpol-get.md)取得語法和選項。|
|/set|設定稽核原則。</br>請參閱[Auditpol set](auditpol-set.md)取得語法和選項。|
|/list|顯示選取的原則項目。</br>請參閱[Auditpol list](auditpol-list.md)取得語法和選項。|
|/backup|將稽核原則儲存到檔案。</br>請參閱[Auditpol 備份](auditpol-backup.md)取得語法和選項。|
|/restore|使用 auditpol /backup 先前建立的檔案從還原的稽核原則。</br>請參閱[Auditpol 還原](auditpol-restore.md)取得語法和選項。|
|/ 清除|清除稽核原則。</br>請參閱[Auditpol 清除](auditpol-clear.md)取得語法和選項。|
|/remove|移除所有的每個使用者稽核原則設定，並停用所有的系統稽核原則設定。</br>請參閱[Auditpol 移除](auditpol-remove.md)取得語法和選項。|
|/resourceSACL|設定全域資源系統存取控制清單 (Sacl)。</br>注意：僅適用於 Windows 7 和 Windows Server 2008 R2。</br>請參閱[Auditpol resourceSACL](auditpol-resourcesacl.md)。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

稽核原則的命令列工具可以用來：
-   設定及查詢系統稽核原則。
-   設定和查詢的每個使用者稽核原則。
-   設定和查詢稽核選項。
-   設定和查詢用來委派存取權的稽核原則的安全性描述元。
-   報表或備份至以逗號分隔值 (CSV) 文字檔的稽核原則。
-   從 CSV 文字檔案中載入的稽核原則。
-   設定全域資源 Sacl。

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)