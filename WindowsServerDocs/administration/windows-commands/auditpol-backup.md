---
title: auditpol 備份
description: Auditpol backup 命令的參考主題，會備份系統稽核原則設定、所有使用者的每一使用者稽核原則設定，以及逗號分隔值（CSV）文字檔的所有審核選項。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: dc84e581-aa0f-4c91-b13b-1d970bad5517
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ddc6bbbc379453c86df27674b57f29f7c0960772
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719164"
---
# <a name="auditpol-backup"></a>auditpol 備份

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

備份系統稽核原則設定、所有使用者的每一使用者稽核原則設定，以及逗號分隔值（CSV）文字檔的所有 [審核選項]。

若要對*每個使用者*和*系統*策略執行*備份*作業，您必須擁有安全描述項中該物件集的**寫入**或**完全控制**許可權。 如果您有 [**管理審核和安全性記錄檔**] （SeSecurityPrivilege）使用者權限，也可以執行*備份*作業。 不過，此許可權允許執行整體*備份*作業所需的其他存取權。

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

若要將所有使用者、系統稽核原則設定和所有審核選項的每個使用者稽核原則設定備份到名為 auditpolicy 的 CSV 格式文字檔，請輸入：

```
auditpol /backup /file:C:\auditpolicy.csv
```

> [!NOTE]
> 如果未指定磁片磁碟機，則會使用目前的目錄。

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [auditpol restore](auditpol-restore.md)

- [auditpol 命令](auditpol.md)
