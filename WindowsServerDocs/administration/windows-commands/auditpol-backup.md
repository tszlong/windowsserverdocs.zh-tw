---
title: auditpol backup
description: Auditpol backup 命令的參考文章，它會備份系統稽核原則設定、所有使用者的每個使用者稽核原則設定，以及以逗號分隔值 (CSV) 文字檔的所有審核選項。
ms.topic: reference
ms.assetid: dc84e581-aa0f-4c91-b13b-1d970bad5517
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4c5dc71ef737c3c939d895a0693ff8213105caa1
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89029146"
---
# <a name="auditpol-backup"></a>auditpol backup

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

備份系統稽核原則設定、所有使用者的每個使用者稽核原則設定，以及以逗號分隔值 (CSV) 文字檔的所有審核選項。

若要對*每個使用者*和*系統*策略執行*備份*作業，您必須擁有安全描述項中該物件集的 [**寫入**] 或 [**完全控制**] 許可權。 如果您有 [**管理審核和安全性記錄**檔 (SeSecurityPrivilege) 使用者權限，也可以執行*備份*作業。 不過，此許可權可讓您不需要執行整體 *備份* 作業的額外存取權。

## <a name="syntax"></a>語法

```
auditpol /backup /file:<filename>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|-----------|------------- |
| /file | 指定將備份稽核原則之檔案的名稱。 |
| /? | 在命令提示字元顯示說明。 |

## <a name="examples"></a>範例

若要將所有使用者、系統稽核原則設定和所有審核選項的每個使用者稽核原則設定備份到名為 auditpolicy.csv 的 CSV 格式文字檔中，請輸入：

```
auditpol /backup /file:C:\auditpolicy.csv
```

> [!NOTE]
> 如果未指定磁片磁碟機，則會使用目前的目錄。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [auditpol restore](auditpol-restore.md)

- [auditpol 命令](auditpol.md)
