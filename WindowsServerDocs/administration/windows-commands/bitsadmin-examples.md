---
title: bitsadmin examples
description: 範例顯示如何使用 bitsadmin 工具來執行最常見的工作。
ms.topic: reference
ms.assetid: cb8f8374-ba6e-4a68-85a1-9a95b8215354
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 05/31/2018
ms.openlocfilehash: e1fcf545efee765b5130c616ddee53b25f03d0d3
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89030486"
---
# <a name="bitsadmin-examples"></a>bitsadmin examples

下列範例示範如何使用 `bitsadmin` 工具來執行最常見的工作。

## <a name="transfer-a-file"></a>傳送檔案

若要建立作業，請新增檔案、在傳送佇列中啟動作業，以及完成作業：

`bitsadmin /transfer myDownloadJob /download /priority normal https://downloadsrv/10mb.zip c:\\10mb.zip`

BITSAdmin 會繼續在 MS-DOS 視窗中顯示進度資訊，直到傳送完成或發生錯誤為止。

## <a name="create-a-download-job"></a>建立下載工作

若要建立名為 *myDownloadJob*的下載作業：

```
bitsadmin /create myDownloadJob
```

BITSAdmin 會傳回可唯一識別作業的 GUID。 在後續的呼叫中使用 GUID 或作業名稱。 以下文字是範例輸出。

### <a name="sample-output"></a>範例輸出

`created job {C775D194-090F-431F-B5FB-8334D00D1CB6}`

## <a name="add-files-to-the-download-job"></a>將檔案新增至下載作業

若要將檔案新增至作業：

```
bitsadmin /addfile myDownloadJob https://downloadsrv/10mb.zip c:\\10mb.zip
```

針對您要新增的每個檔案重複此呼叫。 如果有多個作業使用 *myDownloadJob* 做為其名稱，您必須使用作業的 GUID 來唯一識別它是否完成。

## <a name="activate-the-download-job"></a>啟動下載作業

當您建立新的作業之後，BITS 會自動暫停工作。 若要在傳送佇列中啟用作業：

```
bitsadmin /resume myDownloadJob
```

如果有多個作業使用 *myDownloadJob* 做為其名稱，您必須使用作業的 GUID 來唯一識別它是否完成。

## <a name="determine-the-progress-of-the-download-job"></a>判斷下載作業的進度

**/Info**參數會傳回作業的狀態，以及傳輸的檔案和位元組數目。 當狀態顯示為時 `TRANSFERRED` ，表示 BITS 已成功傳輸作業中的所有檔案。 您也可以新增 **/verbose** 引數以取得作業的完整詳細資料，以及 **/list** 或 **/monitor** 來取得傳送佇列中的所有作業。

若要傳回作業的狀態：

```
bitsadmin /info myDownloadJob /verbose
```

如果有多個作業使用 *myDownloadJob* 做為其名稱，您必須使用作業的 GUID 來唯一識別它是否完成。

## <a name="complete-the-download-job"></a>完成下載作業

若要在狀態變更為之後完成工作 `TRANSFERRED` ：

```
bitsadmin /complete myDownloadJob
```

您必須在 `/complete` 作業中的檔案變成可用之前，先執行參數。 如果有多個作業使用 *myDownloadJob* 做為其名稱，您必須使用作業的 GUID 來唯一識別它是否完成。

## <a name="monitor-jobs-in-the-transfer-queue-using-the-list-switch"></a>使用/list 參數監視傳送佇列中的作業

傳回作業的狀態，以及傳送佇列中所有作業的檔案和位元組數目。

```
bitsadmin /list
```

### <a name="sample-output"></a>範例輸出

```
{6AF46E48-41D3-453F-B7AF-A694BBC823F7} job1 SUSPENDED 0 / 0 0 / 0
{482FCAF0-74BF-469B-8929-5CCD028C9499} job2 TRANSIENT_ERROR 0 / 1 0 / UNKNOWN

Listed 2 job(s).
```

## <a name="monitor-jobs-in-the-transfer-queue-using-the-monitor-switch"></a>使用/monitor 參數監視傳送佇列中的作業

若要傳回作業的狀態，以及傳送佇列中所有工作所傳送的檔案和位元組數，每隔5秒重新整理一次資料：

```
bitsadmin /monitor
```

> [!NOTE]
> 若要停止重新整理，請按 CTRL + C。

### <a name="sample-output"></a>範例輸出

```
MONITORING BACKGROUND COPY MANAGER(5 second refresh)
{6AF46E48-41D3-453F-B7AF-A694BBC823F7} job1 SUSPENDED 0 / 0 0 / 0
{482FCAF0-74BF-469B-8929-5CCD028C9499} job2 TRANSIENT_ERROR 0 / 1 0 / UNKNOWN
{0B138008-304B-4264-B021-FD04455588FF} job3 TRANSFERRED 1 / 1 100379370 / 100379370
```

## <a name="monitor-jobs-in-the-transfer-queue-using-the-info-switch"></a>使用/info 參數監視傳送佇列中的作業

傳回作業的狀態以及傳送的檔案和位元組數目：

```
bitsadmin /info
```

### <a name="sample-output"></a>範例輸出

```
GUID: {482FCAF0-74BF-469B-8929-5CCD028C9499} DISPLAY: myDownloadJob
TYPE: DOWNLOAD STATE: TRANSIENT_ERROR OWNER: domain\user
PRIORITY: NORMAL FILES: 0 / 1 BYTES: 0 / UNKNOWN
CREATION TIME: 12/17/2002 1:21:17 PM MODIFICATION TIME: 12/17/2002 1:21:30 PM
COMPLETION TIME: UNKNOWN
NOTIFY INTERFACE: UNREGISTERED NOTIFICATION FLAGS: 3
RETRY DELAY: 600 NO PROGRESS TIMEOUT: 1209600 ERROR COUNT: 0
PROXY USAGE: PRECONFIG PROXY LIST: NULL PROXY BYPASS LIST: NULL
ERROR FILE:    https://downloadsrv/10mb.zip -> c:\10mb.zip
ERROR CODE:    0x80072ee7 - The server name or address could not be resolved
ERROR CONTEXT: 0x00000005 - The error occurred while the remote file was being
processed.
DESCRIPTION:
JOB FILES:
0 / UNKNOWN WORKING https://downloadsrv/10mb.zip -> c:\10mb.zip
NOTIFICATION COMMAND LINE: none
```

## <a name="delete-jobs-from-the-transfer-queue"></a>從傳送佇列中刪除作業

若要從傳輸佇列中移除所有工作，請使用/reset 參數：

```
bitsadmin /reset
```

### <a name="sample-output"></a>範例輸出

```
{DC61A20C-44AB-4768-B175-8000D02545B9} canceled.
{BB6E91F3-6EDA-4BB4-9E01-5C5CBB5411F8} canceled.
2 out of 2 jobs canceled.
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
