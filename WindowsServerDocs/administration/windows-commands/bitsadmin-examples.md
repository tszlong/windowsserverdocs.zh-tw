---
title: bitsadmin 範例
description: 下列範例示範如何使用 bitsadmin 工具來執行最常見的工作。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: cb8f8374-ba6e-4a68-85a1-9a95b8215354
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 05/31/2018
ms.openlocfilehash: 96447410f76e4402c456b5ec402cc730480aedaf
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850801"
---
# <a name="bitsadmin-examples"></a>bitsadmin 範例

下列範例示範如何使用 `bitsadmin` 工具來執行最常見的工作。

## <a name="transfer-a-file"></a>傳輸檔案

**/Transfer**參數是執行下面所列工作的快捷方式。 此參數會建立作業、將檔案新增至作業、在傳送佇列中啟動作業，以及完成作業。 BITSAdmin 會繼續在 MS-DOS 視窗中顯示進度資訊，直到傳輸完成或發生錯誤為止。

`bitsadmin /transfer myDownloadJob /download /priority normal https://downloadsrv/10mb.zip c:\\10mb.zip`

## <a name="create-a-download-job"></a>建立下載作業

使用 **/create**參數來建立名為 myDownloadJob 的下載作業。

### <a name="syntax"></a>語法

```
bitsadmin /create myDownloadJob
```

BITSAdmin 會傳回可唯一識別作業的 GUID。 在後續呼叫中使用 GUID 或作業名稱。 下列文字是範例輸出。

#### <a name="sample-output"></a>範例輸出

`created job {C775D194-090F-431F-B5FB-8334D00D1CB6}`

## <a name="add-files-to-the-download-job"></a>將檔案新增至下載作業

使用 **/addfile**參數將檔案新增至作業。 針對您要新增的每個檔案重複此呼叫。

如果多個作業使用 myDownloadJob 作為其名稱，您必須將 myDownloadJob 取代為作業的 GUID，以唯一識別作業。

### <a name="syntax"></a>語法

```
bitsadmin /addfile myDownloadJob https://downloadsrv/10mb.zip c:\\10mb.zip
```

## <a name="activate-the-download-job"></a>啟動下載作業

當您建立新的作業時，BITS 會暫停工作。 若要在傳送佇列中啟用作業，請使用 **/resume**參數。

如果多個作業使用 myDownloadJob 作為其名稱，您必須將 myDownloadJob 取代為作業的 GUID，以唯一識別作業。

### <a name="syntax"></a>語法

`bitsadmin /resume myDownloadJob`

## <a name="determine-the-progress-of-the-download-job"></a>判斷下載作業的進度

使用 **/info**參數可傳回作業的狀態，以及所傳輸的檔案和位元組數目。 當狀態為「已傳輸」時，BITS 已成功傳送作業中的所有檔案。

- 使用 **/verbose**引數來取得作業的完整詳細資料。

- 使用 **/list**或 **/監視**參數來取得傳送佇列中的所有作業。

如果多個作業使用 myDownloadJob 作為其名稱，您必須將 myDownloadJob 取代為作業的 GUID，以唯一識別作業。

### <a name="syntax"></a>語法

`bitsadmin /info myDownloadJob /verbose`

#### <a name="sample-output"></a>範例輸出

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

## <a name="completing-the-download-job"></a>正在完成下載工作

當作業的狀態為「已傳輸」時，BITS 已成功傳送作業中的所有檔案。 不過，除非您使用 **/complete**參數，否則無法使用這些檔案。

如果多個作業使用 myDownloadJob 作為其名稱，您必須將 myDownloadJob 取代為作業的 GUID，以唯一識別作業。

### <a name="syntax"></a>語法

`bitsadmin /complete myDownloadJob`

## <a name="monitoring-jobs-in-the-transfer-queue"></a>監視傳輸佇列中的工作

使用 **/list**、 **/監視**或 **/info**參數來監視傳送佇列中的作業。 **/List**參數會提供佇列中所有作業的資訊。

## <a name="list-switch"></a>/list 參數

**/List**參數會傳回作業的狀態，以及傳送佇列中所有作業所傳輸的檔案和位元組數目。

### <a name="syntax"></a>語法

`bitsadmin /list`

#### <a name="sample-output-for-the-list-switch"></a>/List 參數的範例輸出

```
{6AF46E48-41D3-453F-B7AF-A694BBC823F7} job1 SUSPENDED 0 / 0 0 / 0
{482FCAF0-74BF-469B-8929-5CCD028C9499} job2 TRANSIENT_ERROR 0 / 1 0 / UNKNOWN

Listed 2 job(s).
```

## <a name="monitor-switch"></a>/監視參數

**/監視**參數會傳回作業的狀態，以及傳送佇列中所有作業的檔案和位元組數，每5秒重新整理一次。 若要停止重新整理，請按 CTRL + C。

### <a name="syntax"></a>語法

`bitsadmin /monitor`

#### <a name="sample-output"></a>範例輸出

```
MONITORING BACKGROUND COPY MANAGER(5 second refresh)
{6AF46E48-41D3-453F-B7AF-A694BBC823F7} job1 SUSPENDED 0 / 0 0 / 0
{482FCAF0-74BF-469B-8929-5CCD028C9499} job2 TRANSIENT_ERROR 0 / 1 0 / UNKNOWN
{0B138008-304B-4264-B021-FD04455588FF} job3 TRANSFERRED 1 / 1 100379370 / 100379370
```

## <a name="deleting-jobs-from-the-transfer-queue"></a>從傳送佇列刪除作業

使用 **/reset**參數從傳輸佇列中移除所有作業。

### <a name="syntax"></a>語法

`bitsadmin /reset`

#### <a name="sample-output"></a>範例輸出

```
{DC61A20C-44AB-4768-B175-8000D02545B9} canceled.
{BB6E91F3-6EDA-4BB4-9E01-5C5CBB5411F8} canceled.
2 out of 2 jobs canceled.
```
