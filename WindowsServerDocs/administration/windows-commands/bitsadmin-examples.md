---
title: bitsadmin 範例
description: 下列範例示範如何使用 bitsadmin 工具來執行常見工作。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cb8f8374-ba6e-4a68-85a1-9a95b8215354
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 05/31/2018
ms.openlocfilehash: a98e1a876c972b0f146ff37aff0a77399b684e99
ms.sourcegitcommit: 8eea7aadbe94f5d4635c4ffedc6a831558733cc0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/29/2019
ms.locfileid: "66308554"
---
# <a name="bitsadmin-examples"></a>bitsadmin 範例

下列範例示範如何使用`bitsadmin`工具執行常見工作。

## <a name="transfer-a-file"></a>將檔案傳輸

**/Transfer**參數是執行下列工作的捷徑。 此參數會建立作業、 將檔案加入工作、 在傳輸佇列中，作業就會啟動和完成的工作。 BITSAdmin 會繼續在 MS-DOS 視窗中顯示進度資訊，直到傳送完成或發生錯誤。

**bitsadmin /transfer myDownloadJob /download /priority 正常 `https://downloadsrv/10mb.zip c:\\10mb.zip`**

## <a name="create-a-download-job"></a>建立下載工作

使用 **/ 建立**參數，以建立名為 myDownloadJob 的下載作業。

**bitsadmin /create myDownloadJob**

BITSAdmin 傳回唯一識別作業的 GUID。 在後續呼叫中使用的 GUID 或作業名稱。 下列文字是範例輸出。

``` syntax
Created job {C775D194-090F-431F-B5FB-8334D00D1CB6}.
```

接下來，使用 **/addfile**將一或多個檔案新增至下載作業的參數。

## <a name="add-files-to-the-download-job"></a>將檔案新增至下載作業

使用 **/addfile**將檔案新增至作業的參數。 針對您想要新增每個檔案重複這個呼叫。 如果有多個作業使用 myDownloadJob 做為其名稱，您必須取代 myDownloadJob 來唯一識別作業的作業的 guid。

**bitsadmin /addfile myDownloadJob https://downloadsrv/10mb.zip c:\\10mb.zip**

若要啟動的傳送佇列中的作業，使用 **/繼續**切換。

## <a name="activate-the-download-job"></a>啟動下載作業

當您建立新的工作時，則 BITS 會暫停工作。 若要啟動的傳送佇列中的作業，使用 **/繼續**切換。 如果有多個作業使用 myDownloadJob 做為其名稱，您必須取代 myDownloadJob 來唯一識別作業的作業的 guid。

**bitsadmin /resume myDownloadJob**

若要判斷作業的進度，請使用 **/list**， **/info**，或 **/監視**切換。

## <a name="determine-the-progress-of-the-download-job"></a>判斷下載作業的進度

使用 **/info**參數來判斷作業的進度。 如果有多個作業使用 myDownloadJob 做為其名稱，您必須取代 myDownloadJob 來唯一識別作業的作業的 guid。

**bitsadmin /info myDownloadJob /verbose**

**/Info** switch 就會傳回工作的狀態和檔案傳輸的位元組數目。 當傳送狀態時，位元已成功轉移作業中的所有檔案。 **/Verbose**引數會提供作業的完整詳細資料。 下列文字是範例輸出。

``` syntax
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

若要在傳輸佇列中收到所有作業的資訊，請使用 **/list**或是 **/監視**切換。

## <a name="completing-the-download-job"></a>完成下載作業

當傳送作業的狀態時，位元已成功轉移作業中的所有檔案。 不過，不會提供檔案直到您使用 **/ 完成**切換。 如果有多個作業使用 myDownloadJob 做為其名稱，您必須取代 myDownloadJob 來唯一識別作業的作業的 guid。

**bitsadmin /complete myDownloadJob**

## <a name="monitoring-jobs-in-the-transfer-queue"></a>傳輸佇列中的監視作業

使用 **/list**， **/監視**，或 **/info**切換到監視的傳輸佇列中的作業。 **/List**交換器提供佇列中的所有作業的資訊。

**bitsadmin /list**

**/List** switch 就會傳回工作的狀態和檔案和傳輸佇列中的所有作業傳送的位元組數目。 下列文字是範例輸出。

``` syntax
{6AF46E48-41D3-453F-B7AF-A694BBC823F7} job1 SUSPENDED 0 / 0 0 / 0
{482FCAF0-74BF-469B-8929-5CCD028C9499} job2 TRANSIENT_ERROR 0 / 1 0 / UNKNOWN

Listed 2 job(s).
```

使用 **/監視**來監視佇列中的所有作業的參數。 **/監視**交換器就重新整理資料，每隔 5 秒。 若要停止重新整理，請輸入 CTRL + C。

**bitsadmin /monitor**

**/監視**switch 就會傳回工作的狀態和檔案和傳輸佇列中的所有作業傳送的位元組數目。 下列文字是範例輸出。

``` syntax
MONITORING BACKGROUND COPY MANAGER(5 second refresh)
{6AF46E48-41D3-453F-B7AF-A694BBC823F7} job1 SUSPENDED 0 / 0 0 / 0
{482FCAF0-74BF-469B-8929-5CCD028C9499} job2 TRANSIENT_ERROR 0 / 1 0 / UNKNOWN
{0B138008-304B-4264-B021-FD04455588FF} job3 TRANSFERRED 1 / 1 100379370 / 100379370
```

## <a name="deleting-jobs-from-the-transfer-queue"></a>刪除來自傳輸佇列的作業

使用**重設/** 切換到從傳輸佇列中移除所有工作。

**bitsadmin /reset**

下列文字是範例輸出。

``` syntax
{DC61A20C-44AB-4768-B175-8000D02545B9} canceled.
{BB6E91F3-6EDA-4BB4-9E01-5C5CBB5411F8} canceled.
2 out of 2 jobs canceled.
```
