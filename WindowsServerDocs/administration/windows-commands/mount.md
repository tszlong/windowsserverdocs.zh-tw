---
title: 掛上 - mount
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: dd9d7ecb-ef00-4aaa-bcd0-423fa636e34a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f9d13f5eddef80d99c11fe59c6bd5e589fa67546
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858369"
---
# <a name="mount"></a>掛上 - mount



您可以使用**掛接**以掛接網路檔案系統 (NFS) 網路共用。

## <a name="syntax"></a>語法

```
mount [-o <Option>[...]] [-u:<UserName>] [-p:{<Password> | *}] {\\<ComputerName>\<ShareName> | <ComputerName>:/<ShareName>} {<DeviceName> | *}
```

## <a name="description"></a>描述

**掛接**命令列公用程式掛接檔案系統所識別*ShareName*所識別之 NFS 伺服器匯出*ComputerName*並將它與相關聯所指定的磁碟機代號*DeviceName*或者，如果星號 (**&#42;**) 使用時，依第一個可用的驅動程式字母。 使用者接著可以存取匯出的檔案系統，如同它已在本機電腦上的磁碟機。 未選項或引數，搭配使用時**掛接**顯示所有已掛接的 NFS 檔案系統的相關資訊。

**掛接**公用程式是安裝 NFS 用戶端時，才提供使用。

下列選項和引數可以搭配**掛接**公用程式。

|詞彙|定義|
|----|----------|
|-o rsize=\<buffersize>|設定以 kb 為單位的讀取緩衝區的大小。 可接受的值為 1、 2、 4、 8、 16 及 32。預設值為 32 KB。|
|-o wsize=\<buffersize>|設定以 kb 為單位的寫入緩衝區的大小。 可接受的值為 1、 2、 4、 8、 16 及 32。預設值為 32 KB。|
|-o timeout=\<seconds>|設定逾時值 （秒） 的遠端程序呼叫 (RPC)。 可接受的值為 0.8、 0.9 及 1-60; 範圍中的任何整數預設值為 0.8。|
|-o retry=\<number>|設定軟掛接重試的次數。 可接受的值是 1-10; 範圍內的整數預設值為 1。|
|-o mtype={soft | 硬}|設定掛接類型 (預設值是**虛**)。 不論掛接類型為何**掛接**會傳回如果無法立即掛接共用。 一旦共用已成功掛接，不過，如果掛接類型為**硬**，NFS 用戶端會繼續嘗試存取共用，直到成功為止。 如此一來，如果無法使用 NFS 伺服器，任何嘗試存取共用的 Windows 程式會出現停止回應，或 「 擱置 」，如果掛接類型為**硬**。|
|-o 匿名驗證|以匿名使用者身分掛接。|
|-o nolock|停用鎖定 (預設值是**啟用**)。|
|-o casesensitive|強制執行不區分大小寫的伺服器上的檔案查閱。|
|-o fileaccess=\<mode>|指定 NFS 共用上建立的新檔案的預設權限模式。 指定*模式*表單中的三位數數字*ogw*，其中*o*， *g*，以及*w*皆是數字表示存取權授與檔案的擁有者、 群組和世界中，分別。 數字必須在範圍 0-7，具有下列意義：</br>-   0:不允許存取</br>-1: x （執行存取權）</br>-2: w （寫入存取權）</br>-3: wx</br>-4: r （讀取）</br>-   5: rx</br>-6: rw</br>-7: rwx|
|-o lang={euc-jp|euc-tw|euc-kr|shift jis|big5|ksc5601|gb2312-80|ansi}|指定預設的編碼方式用於檔案和目錄名稱，是否使用，必須設為下列其中一項：</br>-   **ansi**</br>-   **big5** （繁體中文）</br>-   **euc jp** （日文）</br>-   **euc 韓國**（韓文）</br>-   **euc tw** （繁體中文）</br>-   **gb2312 80** （簡體中文）</br>-   **ksc5601** （韓文）</br>-   **shift jis** （日文）</br>如果此選項設定為**ansi**系統針對非英文地區設定設定的編碼配置會設定為地區設定的預設編碼配置。 以下是針對指定的地區設定的預設編碼配置：</br>-日文：SHIFT-JIS</br>-韓文：KS_C_5601-1987</br>-中文簡體：GB2312-80</br>-繁體中文：BIG5|
|-u:\<使用者名稱 >|指定要使用掛接共用的使用者名稱。 如果*使用者名稱*未加上反斜線 (**\**)，它會被視為 UNIX 使用者名稱。|
|-p:\<密碼 >|若要使用掛接共用密碼。 如果您使用星號 (**&#42;**)，您將會提示輸入密碼。|

> [!NOTE]
