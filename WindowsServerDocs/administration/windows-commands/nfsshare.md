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
ms.openlocfilehash: 50df88a3fddbceb1595f328bdd4e3c6f526e3a2c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881689"
---
# <a name="nfsshare"></a>nfsshare



您可以使用**nfsshare**網路檔案系統 (NFS) 共用的控制項。

## <a name="syntax"></a>語法

```
nfsshare <ShareName>=<Drive:Path> [-o <Option=value>...]
nfsshare {<ShareName> | <Drive>:<Path> | * } /delete
```

## <a name="description"></a>描述

不含引數， **nfsshare**命令列公用程式列出所有的 NFS 伺服器匯出的網路檔案系統 (NFS) 共用。 具有*ShareName*做為唯一的引數， **nfsshare**列出的屬性所識別的 NFS 共用*ShareName*。 當*ShareName*並*磁碟機 ***:*** 路徑*提供， **nfsshare**匯出所識別的資料夾*磁碟機 ***:*** 路徑*作為*ShareName*。 當 **/刪除**選項時，指定的資料夾已不再開放給 NFS 用戶端。

## <a name="options"></a>選項。

**Nfsshare**命令可接受下列選項和引數：

|詞彙|定義|
|----|----------|
|-o anon = {yes | no}|指定匿名 （未對應的） 的使用者是否可存取共用的目錄。 預設值是**沒有**。|
|-o rw [=\<主機 > [:<Host>]...]|提供共用目錄的主機或用戶端的讀寫存取所指定的群組*主機*。 以冒號分隔主機和群組的名稱 (**:**)。 如果*主機*未指定，則所有主機和用戶端群組 (但不包括指定**ro**選項) 具有讀寫存取。 如果沒有**ro**也**rw**選項設定，所有用戶端共用目錄的讀寫存取。|
|-o w [=\<主機 > [:<Host>]...]|提供唯讀存取共用目錄的主機或用戶端所指定的群組*主機*。 以冒號分隔主機和群組的名稱 (**:**)。 如果*主機*未指定，則所有用戶端 (但不包括指定**rw**選項) 有唯讀存取權。 如果**ro**選項設定為一或多個用戶端，但**rw**未設定選項，以指定的用戶端**ro**選項有共用目錄的存取權。|
|-o encoding={big5|euc-jp|euc-kr|euc-tw|gb2312-80|ksc5601|shift-jis}|指定預設的編碼方式用於檔案和目錄名稱，是否使用，必須設為下列其中一項：</br>-   **big5** （繁體中文）</br>-   **euc jp** （日文）</br>-   **euc 韓國**（韓文）</br>-   **euc tw** （繁體中文）</br>-   **gb2312 80** （簡體中文）</br>-   **ksc5601** （韓文）</br>-   **shift jis** （日文）</br>若此選項未設定，預設的編碼配置為 ANSI 或系統上非英文地區設定中，使用預設編碼方式的地區設定的配置。 以下是針對指定的地區設定的預設編碼配置：</br>-日文：SHIFT-JIS</br>-韓文：KS_C_5601-1987</br>-中文簡體：GB2312-80</br>繁體中文：BIG5|
|-o anongid=\<gid>|指定匿名 （未對應的） 的使用者會存取共用目錄使用*gid*做為其群組識別碼 (GID)。 預設值為-2。 報告未對應的使用者所擁有檔案的擁有者，即使已停用匿名存取時，會使用匿名 GID。|
|-o  anonuid=\<uid>|指定匿名 （未對應的） 的使用者會存取共用目錄使用*uid*做為其使用者識別碼 (UID)。 預設值為-2。 報告未對應的使用者所擁有檔案的擁有者，即使已停用匿名存取時，會使用匿名 UID。|
|-o 根 [=\<主機 > [:<Host>]...]|提供的主機或用戶端的共用目錄的根目錄存取所指定的群組*主機*。 以冒號分隔主機和群組的名稱 (**:**)。 如果*主機*未指定，所有用戶端具有根存取權。 如果**根**未設定此選項，沒有用戶端有共用目錄的根權限。|
|/delete|如果*ShareName*或是*磁碟機 ***:*** 路徑*指定時，會刪除指定的共用。 如果 * 指定時，會刪除所有的 NFS 共用。|

> [!NOTE]
> 若要檢視此命令的完整語法，請在命令提示字元中輸入：</br>> **nfsshare /?**

##

[Services for Network File System 命令參考](services-for-network-file-system-command-reference.md)另請參閱