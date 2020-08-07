---
title: auditpol clear
description: Auditpol clear 命令的參考文章，它會刪除所有使用者的每個使用者稽核原則、重設 (停用所有子類別的系統稽核原則) ，並將所有的審核選項設定為 [停用]。
ms.topic: article
ms.assetid: 05bfa218-2434-4ad1-b33c-e6fcfb2b4f67
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2cdb55633e15484f8ca98432fbe8bc28b5f59fd0
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87895460"
---
# <a name="auditpol-clear"></a>auditpol clear

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

刪除所有使用者的每個使用者稽核原則，重設 (停用所有子類別的系統稽核原則) ，並將所有的審核選項設定為 [停用]。

若要對*每個使用者*和*系統*策略執行*清除*作業，您必須擁有安全描述項中該物件集的 [**寫入**] 或 [**完全控制**] 許可權。 如果您的 [**管理審核和安全性記錄**檔 (SeSecurityPrivilege) 使用者權限，也可以執行*清除*作業。 不過，此許可權允許執行整體*清除*作業所不需要的其他存取權。

## <a name="syntax"></a>語法

```
auditpol /clear [/y]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| ----------- | --------------- |
| /y | 抑制確認是否應清除所有稽核原則設定的提示。 |
| /? | 在命令提示字元顯示說明。 |

## <a name="examples"></a>範例

若要為所有使用者刪除每個使用者的稽核原則，請重設 (停用) 所有子類別的系統稽核原則，並將所有稽核原則設定設為 [停用]，在確認提示中輸入：

```
auditpol /clear
```

若要為所有使用者刪除每個使用者的稽核原則，請重設所有子類別的系統稽核原則設定，並將所有稽核原則設定設為 [停用]，而不需確認提示，請輸入：

```
auditpol /clear /y
```

> [!NOTE]
> 使用腳本來執行此作業時，上述範例會很有用。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [auditpol 命令](auditpol.md)
