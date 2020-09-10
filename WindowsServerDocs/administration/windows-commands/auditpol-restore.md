---
title: auditpol restore
description: Auditpol restore 命令的參考文章，此命令會還原系統稽核原則設定、所有使用者的每一使用者稽核原則設定，以及與/backup 選項所使用的逗點分隔值 (CSV) 檔案格式語法一致的檔案中的所有審核選項。
ms.topic: reference
ms.assetid: ad73e520-484f-4cf1-a7f9-ae7488e9edf6
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: d6dc8dde77189cbd1134779bf89f253402f181dc
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89633116"
---
# <a name="auditpol-restore"></a>auditpol restore

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

還原系統稽核原則設定、所有使用者的每一使用者稽核原則設定，以及與/backup 選項所使用的逗點分隔值 (CSV) 檔案格式一致之檔案中的所有審核選項。

若要對*每個使用者*和*系統*策略執行*還原*作業，您必須擁有安全描述項中該物件集的 [**寫入**] 或 [**完全控制**] 許可權。 如果您有 [**管理審核和安全性記錄**檔 (SeSecurityPrivilege) ] 使用者權限，也可以執行*還原*作業，這在發生錯誤或惡意攻擊的情況下還原安全描述項時相當實用。

## <a name="syntax"></a>語法

```
auditpol /restore /file:<filename>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| ------- | -------- |
| /file | 指定應從中還原稽核原則的檔案。 檔案必須使用/backup 選項來建立，或必須與/backup 選項所使用的 CSV 檔案格式一致。 |
| /? |在命令提示字元顯示說明。 |

## <a name="examples"></a>範例

若要還原系統稽核原則設定、所有使用者的每個使用者稽核原則設定，以及使用/backup 命令所建立之檔案中名為 auditpolicy.csv 的所有審核選項，請輸入：

```
auditpol /restore /file:c:\auditpolicy.csv
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [auditpol backup](auditpol-backup.md)

- [auditpol 命令](auditpol.md)
