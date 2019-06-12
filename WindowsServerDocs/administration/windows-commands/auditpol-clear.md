---
title: auditpol 清除
description: 適用於 Windows 命令主題**auditpol 清除**-刪除每個使用者稽核原則，針對所有使用者、 系統稽核原則，所有的子類別，並將所有稽核選項為停用設定重設 （停用）。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 05bfa218-2434-4ad1-b33c-e6fcfb2b4f67
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 86b56386ba9bed2486cdf8cdbb4486fcec6c6265
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66435157"
---
# <a name="auditpol-clear"></a>auditpol 清除

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

刪除每個使用者稽核原則，針對所有使用者、 重設 （停用） 系統稽核原則，所有的子類別，並將所有稽核選項為停用設定。

## <a name="syntax"></a>語法
```
auditpol /clear [/y]
```
## <a name="parameters"></a>參數

| 參數 |                                   描述                                    |
|-----------|----------------------------------------------------------------------------------|
|    /y     | 隱藏確認是否應該清除所有的稽核原則設定的提示。 |
|    /?     |                       在命令提示字元顯示說明。                       |

## <a name="remarks"></a>備註
針對每個使用者原則] 及 [系統原則的清除作業，您必須撰寫或該物件上的完全控制權限設定中的安全性描述元。 您也可以執行清除作業擁有**管理稽核及安全性記錄**(SeSecurityPrivilege) 使用者權限。 不過，此權限可讓其他不需要執行清除作業的存取。
## <a name="BKMK_examples"></a>範例
若要刪除所有使用者、 重設 （停用） 系統稽核原則，所有的子類別目錄，以及設定所有的每個使用者稽核原則稽核原則設定為 停用，在確認提示中輸入：
```
auditpol /clear
```
若要刪除所有使用者的每個使用者稽核原則，請重設系統稽核原則設定，所有的子類別目錄，並設定所有的稽核原則設定為 停用，而不需要確認的提示字元下輸入：
```
auditpol /clear /y
```
> [!NOTE]
> 若要執行這項作業使用指令碼時，則上述範例中會很有用。
> #### <a name="additional-references"></a>其他參考資料
> [命令列語法關鍵](command-line-syntax-key.md)
