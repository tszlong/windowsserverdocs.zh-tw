---
title: Sc 查詢
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ac365f89-4b20-4de6-a582-b204c5e7d0eb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9ed9ee813e3ea41f24ce5c012eecd278ef051138
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879289"
---
# <a name="sc-query"></a>Sc 查詢



取得並顯示指定的服務、 驅動程式、 服務類型或驅動程式類型的相關資訊。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
sc [<ServerName>] query [<ServiceName>] [type= {driver | service | all}] [type= {own | share | interact | kernel | filesys | rec | adapt}] [state= {active | inactive | all}] [bufsize= <BufferSize>] [ri= <ResumeIndex>] [group= <GroupName>]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<ServerName>|指定由服務所在的遠端伺服器的名稱。 名稱必須使用通用命名慣例 (UNC) 格式 (例如\\ \\myserver)。 若要在本機執行 SC.exe，省略這個參數。|
|\<ServiceName>|指定所傳回的服務名稱**getkeyname**作業。 這**查詢**參數不是與其他**查詢**參數 (而非*ServerName*)。|
|type= {driver | 服務 | 所有}|指定要列舉的項目。 第一種類型的預設值是**服務**。</br>-驅動程式：指定驅動程式會列舉。</br>-服務：指定只有服務會列舉。</br>-所有：指定列舉驅動程式和服務。|
|type= {own | 分享 | 互動 | 核心 | filesys | rec | adapt}|指定服務的類型或列舉的驅動程式的類型。 第二個類型的預設值是**自己**。</br>-自己：指定在自己的處理序中執行服務。 它不會與其他服務共用的可執行檔。</br>-共用：指定服務執行為共用的程序。 它與其他服務共用的可執行檔。</br>-互動：指定服務可以互動桌面，並接收來自使用者的輸入。 互動式服務必須在 LocalSystem 帳戶下執行。</br>-核心：指定驅動程式。</br>-filesys:指定檔案系統驅動程式。|
|state= {active | 非使用中 | 所有}|指定要列舉之服務的啟動的狀態。 預設狀態是**active**。</br>-使用中：指定所有作用中服務。</br>為非作用中：指定所有已暫停或停止服務。</br>-所有：指定所有的服務。|
|bufsize= \<BufferSize>|指定的列舉型別緩衝區的大小 （以位元組為單位）。 預設緩衝區大小是 1024 個位元組。 顯示從查詢產生超過 1024 個位元組時，您應該增加列舉緩衝區的大小。|
|ri= \<ResumeIndex>|指定列舉型別是開始或繼續的索引編號。 預設值是**0** （零）。 使用此參數搭配**bufsize =** 參數時的預設緩衝區可以顯示查詢所傳回的詳細資訊。|
|group= \<GroupName>|指定要列舉的服務群組。 根據預設，會列舉所有群組 (**群組 =""**)。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

-   不含參數和其值之間的空格 (也就是**類型 = 自己**，而非**類型 = 自己**)，則作業會失敗。
-   **查詢**作業會顯示服務的下列資訊：WIN32_EXIT_B、 SERVICE_EXIT_B、 檢查點和 WAIT_HINT SERVICE_NAME （服務的登錄子機碼名稱），輸入，狀態 （以及狀態未提供）。
-   **類型 =** 兩次在某些情況下，可以使用參數。 第一個出現**類型 =** 參數指定要查詢的服務、 驅動程式，或兩者 (**所有**)。 第二個的外觀**類型 =** 參數會指定從類型**建立**以進一步縮小查詢範圍的作業。
-   當產生的顯示**查詢**命令超過列舉緩衝區的大小，會顯示類似下面的訊息：  
    ```
    Enum: more data, need 1822 bytes start resume at index 79
    ```  
    要顯示的其餘**查詢**的詳細資訊，請重新執行**查詢**，設定**bufsize =** 位元組以及設定數**ri =** 至指定的索引。 例如，剩餘的輸出會顯示在命令提示字元中輸入下列：  
    ```
    sc query bufsize= 1822 ri= 79
    ```

## <a name="BKMK_examples"></a>範例

若要顯示只有作用中服務的資訊，請輸入下列命令：
```
sc query
sc query type= service
```
若要顯示資訊的作用中服務，並以指定的緩衝區大小為 2,000 個位元組中，輸入：
```
sc query type= all bufsize= 2000
```
若要顯示 WUAUSERV 服務的資訊，請輸入：
```
sc query wuauserv
```
若要顯示所有服務 （作用中和非使用中） 的資訊，請輸入：
```
sc query state= all
```
若要顯示的所有服務 （作用中和非使用中），開始於行 56 的資訊，請輸入：
```
sc query state= all ri= 56
```
若要顯示互動式服務的資訊，請輸入：
```
sc query type= service type= interact
```
若要顯示只使用驅動程式的資訊，請輸入：
```
sc query type= driver
```
若要顯示網路驅動程式介面規格 (NDIS) 群組中的驅動程式的資訊，請輸入：
```
sc query type= driver group= ndis
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)