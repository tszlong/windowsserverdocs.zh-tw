---
title: auditpol 清單
description: 適用於 Windows 命令主題**auditpol 清單**-清單稽核原則類別目錄和/或子類別目錄，或定義清單使用者針對每個使用者稽核原則。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 45502abe-3d6e-4e13-94f0-8e6fcb6db860
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 08f524ef0aacd731f709ce7a2e17b3d831da1e5b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858579"
---
# <a name="auditpol-list"></a>auditpol 清單

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

清單的稽核原則類別目錄和/或子類別目錄或列出使用者定義的每個使用者稽核原則。

## <a name="syntax"></a>語法
```
auditpol /list
[/user|/category|subcategory[:<categoryname>|<{guid}>|*]]
[/v] [/r]
```
## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|/user|擷取所有已定義的每個使用者稽核原則的使用者。 如果搭配 /v 參數，也會顯示使用者的安全性識別碼 (SID)。|
|/category|顯示由系統所瞭解的類別目錄的名稱。 如果搭配 /v 參數，也會顯示類別目錄全域唯一識別碼 (GUID)。|
|/subcategory|顯示子類別目錄和其相關聯的 GUID 的名稱。|
|/v|顯示與類別或子類別目錄的 GUID 或 /user 搭配使用時，會顯示每個使用者的 SID。|
|/r|以逗點分隔值 (CSV) 格式的報表中顯示輸出。|
|/?|在命令提示字元顯示說明。|
## <a name="remarks"></a>備註
每位使用者原則的所有 list 作業，您必須擁有都讀取權限的安全性描述元中設定該物件。 您也可以執行清單作業所擁有**管理稽核及安全性記錄**(SeSecurityPrivilege) 使用者權限。 不過，此權限可讓其他不是執行清單作業所需的存取。
## <a name="BKMK_examples"></a>範例
若要列出所有使用者都有定義的稽核原則，請輸入：
```
auditpol /list /user
```
若要列出所有使用者都有定義的稽核原則和其相關聯的 SID，請輸入：
```
auditpol /list /user /v
```
若要列出所有的分類和子類別目錄報表格式，請輸入：
```
auditpol /list /subcategory:* /r
```
若要列出的詳細的追蹤] 與 [DS 存取類別的子類別目錄，請輸入：
```
auditpol /list /subcategory:"detailed Tracking","DS Access"
```
#### <a name="additional-references"></a>其他參考資料
[命令列語法關鍵](command-line-syntax-key.md)
