---
title: nfsshare
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 437a2615-335a-442f-9713-d50d5f3983a3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dc77825d63875861839ecdb22bee5a62375aaa13
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437046"
---
# <a name="nfsshare"></a>nfsshare



您可以使用**nfsshare**網路檔案系統 (NFS) 共用的控制項。

## <a name="syntax"></a>語法

```
nfsshare <ShareName>=<Drive:Path> [-o <Option=value>...]
nfsshare {<ShareName> | <Drive>:<Path> | * } /delete
```

## <a name="description"></a>描述

不含引數， **nfsshare**命令列公用程式列出所有的 NFS 伺服器匯出的網路檔案系統 (NFS) 共用。 具有*ShareName*做為唯一的引數， **nfsshare**列出的屬性所識別的 NFS 共用*ShareName*。 當*ShareName*並<em>磁碟機</em> **:** <em>路徑</em>會提供**nfsshare**匯出資料夾由識別<em>磁碟機</em> **:** <em>路徑</em>作為*ShareName*。 當 **/刪除**選項時，指定的資料夾已不再開放給 NFS 用戶端。

## <a name="options"></a>選項。

**Nfsshare**命令可接受下列選項和引數：


|             詞彙              |                                                                                                                                                                                                                      定義                                                                                                                                                                                                                       |
|-------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         -o anon = {yes          |                                                                                                                                                                                                                          no}                                                                                                                                                                                                                          |
|  -o rw [=\<主機 > [:<Host>]...]  |                       提供共用目錄的主機或用戶端的讀寫存取所指定的群組*主機*。 以冒號分隔主機和群組的名稱 ( **:** )。 如果*主機*未指定，則所有主機和用戶端群組 (但不包括指定**ro**選項) 具有讀寫存取。 如果沒有**ro**也**rw**選項設定，所有用戶端共用目錄的讀寫存取。                       |
|  -o w [=\<主機 > [:<Host>]...]  | 提供唯讀存取共用目錄的主機或用戶端所指定的群組*主機*。 以冒號分隔主機和群組的名稱 ( **:** )。 如果*主機*未指定，則所有用戶端 (但不包括指定**rw**選項) 有唯讀存取權。 如果**ro**選項設定為一或多個用戶端，但**rw**未設定選項，以指定的用戶端**ro**選項有共用目錄的存取權。 |
|       -o encoding={big5       |                                                                                                                                                                                                                        euc-jp                                                                                                                                                                                                                         |
|       -o anongid=\<gid>       |                                                                                     指定匿名 （未對應的） 的使用者會存取共用目錄使用*gid*做為其群組識別碼 (GID)。 預設值為-2。 報告未對應的使用者所擁有檔案的擁有者，即使已停用匿名存取時，會使用匿名 GID。                                                                                      |
|      -o  anonuid=\<uid>       |                                                                                      指定匿名 （未對應的） 的使用者會存取共用目錄使用*uid*做為其使用者識別碼 (UID)。 預設值為-2。 報告未對應的使用者所擁有檔案的擁有者，即使已停用匿名存取時，會使用匿名 UID。                                                                                      |
| -o 根 [=\<主機 > [:<Host>]...] |                                                                         提供的主機或用戶端的共用目錄的根目錄存取所指定的群組*主機*。 以冒號分隔主機和群組的名稱 ( **:** )。 如果*主機*未指定，所有用戶端具有根存取權。 如果**根**未設定此選項，沒有用戶端有共用目錄的根權限。                                                                         |
|            /delete            |                                                                                                                                                       如果*ShareName*或是<em>磁碟機</em> **:** <em>路徑</em>指定時，會刪除指定的共用。 如果\*指定時，會刪除所有的 NFS 共用。                                                                                                                                                       |

> [!NOTE]
> 若要檢視此命令的完整語法，請在命令提示字元中輸入：</br>> **nfsshare /?**

# #

[Services for Network File System 命令參考](services-for-network-file-system-command-reference.md)另請參閱