---
title: auditpol restore
description: Auditpol restore 命令的參考主題，會還原系統稽核原則設定、所有使用者的每個使用者稽核原則設定，以及與/備份選項所使用之逗號分隔值（CSV）檔案格式一致的檔案中的所有審核選項。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ad73e520-484f-4cf1-a7f9-ae7488e9edf6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 64605a985c1cff13b842a99ae4ea52485bfc8220
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719052"
---
# <a name="auditpol-restore"></a>auditpol restore

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

針對所有使用者，還原系統稽核原則設定、每個使用者的稽核原則設定，以及與/備份選項所使用之逗號分隔值（CSV）檔案格式一致的檔案中所有的審核選項。

若要對*每個使用者*和*系統*策略執行*還原*作業，您必須擁有安全描述項中該物件集的 [**寫入**] 或 [**完全控制**] 許可權。 如果您擁有「**管理審核和安全性記錄檔**」（SeSecurityPrivilege）使用者權限，也可以執行*還原*作業，這在發生錯誤或惡意攻擊時還原安全描述項時非常有用。

## <a name="syntax"></a>語法

```
auditpol /restore /file:<filename>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| ------- | -------- |
| /file | 指定應該從中還原稽核原則的檔案。 檔案必須已經使用/備份選項建立，或必須與/備份選項所使用的 CSV 檔案格式一致。 |
| /? |在命令提示字元顯示說明。 |

## <a name="examples"></a>範例

若要針對所有使用者還原系統稽核原則設定、每個使用者的稽核原則設定，以及使用/備份命令所建立之名為 auditpolicy 之檔案中的所有審核選項，請輸入：

```
auditpol /restore /file:c:\auditpolicy.csv
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [auditpol 備份](auditpol-backup.md)

- [auditpol 命令](auditpol.md)
