---
title: auditpol clear
description: '**Auditpol clear**的 Windows 命令主題會針對所有使用者刪除每個使用者的稽核原則、重設（停用）所有子類別的系統稽核原則，並將所有的審核選項設定為 [停用]。'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 05bfa218-2434-4ad1-b33c-e6fcfb2b4f67
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 971f4ba54d787be29cb9e7d710f556c50c69a8dc
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851201"
---
# <a name="auditpol-clear"></a>auditpol clear

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

刪除所有使用者的每個使用者稽核原則、重設（停用）所有子類別的系統稽核原則，並將所有的審核選項設定為 [停用]。

## <a name="syntax"></a>語法

```
auditpol /clear [/y]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| ----------- | --------------- |
| /y | 抑制確認是否應清除所有稽核原則設定的提示。 |
| /? | 在命令提示字元顯示說明。 |

## <a name="remarks"></a>備註

對於每個使用者原則和系統原則的清除作業，您必須在安全描述項中設定該物件的「寫入」或「完全控制」許可權。 您也可以透過擁有 [**管理審核及安全性記錄檔**] （SeSecurityPrivilege）使用者權限來執行清除作業。 不過，此許可權允許執行清除作業所不需要的其他存取權。

## <a name="examples"></a><a name=BKMK_examples></a>典型

若要為所有使用者刪除每個使用者的稽核原則，請重設（停用）所有子類別的系統稽核原則，並將所有稽核原則設定設為 [停用]，在確認提示中輸入：

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
