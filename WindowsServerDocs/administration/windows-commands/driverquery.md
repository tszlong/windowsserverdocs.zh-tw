---
title: driverquery
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 92ca4b84-e4e2-405b-9f31-bf6db9f66839
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 88a59f9da9927bb923418695bc760303c0fb00b0
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439484"
---
# <a name="driverquery"></a>driverquery



可讓系統管理員可以顯示一份已安裝的裝置驅動程式和其屬性。 如果未指定參數，使用**必須先**本機電腦上執行。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
driverquery [/s <System> [/u [<Domain>\]<Username> [/p <Password>]]] [/fo {table | list | csv}] [/nh] [/v | /si]
```

## <a name="parameters"></a>參數

|         參數         |                                                                                                                                         描述                                                                                                                                          |
|---------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       /s \<system >        |                                                                                      指定的名稱或遠端電腦的 IP 位址。 請勿使用反斜線。 預設是本機電腦。                                                                                       |
| /u [\<網域 >\]<Username> | 所指定的使用者帳戶的認證執行命令*使用者*或是*網域*\*使用者<em>。根據預設， \* \*/s</em> \*會使用目前登入的電腦，會發出命令之使用者的認證。 **/u**無法使用，除非 **/s**指定。 |
|      /p \<Password>       |                                                                           指定在指定的使用者帳戶的密碼 **/u**參數。 **/p**無法使用，除非 **/u**指定。                                                                            |
|        /fo {table         |                                                                                                                                             list                                                                                                                                             |
|            /nh            |                                                                                      省略的顯示驅動程式資訊的標頭資料列。 不是有效的 if **/fo**參數設為**清單**。                                                                                      |
|            /v             |                                                                                                               顯示詳細資訊輸出。 **/v**不適用於簽署的驅動程式。                                                                                                               |
|            /si            |                                                                                                                          提供已簽署的驅動程式的相關資訊。                                                                                                                          |
|            /?             |                                                                                                                             在命令提示字元顯示說明。                                                                                                                             |

## <a name="BKMK_examples"></a>範例

若要在本機電腦上顯示一份已安裝的裝置驅動程式，請輸入：
```
driverquery 
```
若要以逗號分隔值 (CSV) 格式顯示輸出，請輸入：
```
driverquery /fo csv 
```
若要隱藏在輸出中的標頭資料列，請輸入：
```
driverquery /nh 
```
若要使用**必須先**遠端伺服器上的命令**server1**使用您目前的認證，在本機電腦上，輸入：
```
driverquery /s server1
```
若要使用**必須先**遠端伺服器上的命令**server1**使用的認證**user1**網域上**maindom**，型別：
```
driverquery /s server1 /u maindom\user1 /p p@ssw3d
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)