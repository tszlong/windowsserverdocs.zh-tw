---
title: driverquery
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 5d44a1be300b7178bc2271187344c2fc4ab8815e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377650"
---
# <a name="driverquery"></a>driverquery



可讓系統管理員顯示已安裝的設備磁碟機及其內容的清單。 如果在沒有參數的情況下使用， **driverquery**會在本機電腦上執行。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
driverquery [/s <System> [/u [<Domain>\]<Username> [/p <Password>]]] [/fo {table | list | csv}] [/nh] [/v | /si]
```

## <a name="parameters"></a>參數

|         參數         |                                                                                                                                         描述                                                                                                                                          |
|---------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       /s \<System >        |                                                                                      指定遠端電腦的名稱或 IP 位址。 請勿使用反斜線。 預設是本機電腦。                                                                                       |
| /u [\<Domain > \] @ no__t-2 | 使用使用者或*網域*\*User @ no__t-3 所指定的使用者帳戶*認證來執行*命令。根據預設，\* @ no__t-1/s @ no__t-2 @ no__t-3 會使用目前登入發出命令之電腦的使用者認證。 除非指定了 **/s** ，否則無法使用 **/u** 。 |
|      /p \<密碼 >       |                                                                           指定 **/u**參數中指定之使用者帳戶的密碼。 除非指定 **/u** ，否則無法使用 **/p** 。                                                                            |
|        /fo {資料表         |                                                                                                                                             list                                                                                                                                             |
|            /nh            |                                                                                      省略顯示的驅動程式資訊中的標頭資料列。 如果 **/fo**參數設定為**list**，則無效。                                                                                      |
|            /v             |                                                                                                               顯示詳細資訊輸出。 **/v**對已簽署的驅動程式無效。                                                                                                               |
|            /si            |                                                                                                                          提供已簽署驅動程式的相關資訊。                                                                                                                          |
|            /?             |                                                                                                                             在命令提示字元顯示說明。                                                                                                                             |

## <a name="BKMK_examples"></a>典型

若要顯示本機電腦上已安裝的設備磁碟機清單，請輸入：
```
driverquery 
```
若要以逗號分隔值（CSV）格式顯示輸出，請輸入：
```
driverquery /fo csv 
```
若要隱藏輸出中的標頭資料列，請輸入：
```
driverquery /nh 
```
若要在本機電腦上使用目前的認證，在名為**server1**的遠端伺服器上使用**driverquery**命令，請輸入：
```
driverquery /s server1
```
若要在名為**server1**的遠端伺服器上使用**driverquery**命令，並使用 domain **maindom**上**user1**的認證，請輸入：
```
driverquery /s server1 /u maindom\user1 /p p@ssw3d
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)