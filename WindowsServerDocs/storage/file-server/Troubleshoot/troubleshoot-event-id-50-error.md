---
title: 針對事件識別碼50錯誤訊息進行疑難排解
description: 說明如何針對事件識別碼50錯誤訊息進行疑難排解
author: Deland-Han
manager: dcscontentpm
ms.topic: article
ms.author: delhan
ms.date: 12/25/2019
ms.openlocfilehash: 930a503f756bf2005e26ac227565b42b627ee11a
ms.sourcegitcommit: 39d55b5a0006aceac6281e8cdb61fc79a209ce1b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/04/2020
ms.locfileid: "93328128"
---
# <a name="troubleshoot-the-event-id-50-error-message"></a>針對事件識別碼50錯誤訊息進行疑難排解

##  <a name="symptoms"></a>徵兆

當資訊寫入實體磁片時，下列兩個事件訊息可能會記錄在系統事件記錄檔中： 

```
Event ID: 50 
Event Type: Warning 
Event Source: Ftdisk 
Description: {Lost Delayed-Write Data} The system was attempting to transfer file data from buffers to \Device\HarddiskVolume4. The write operation failed, and only some of the data may have been written to the file.
Data: 
0000: 00 00 04 00 02 00 56 00 
0008: 00 00 00 00 32 00 04 80 
0010: 00 00 00 00 00 00 00 00 
0018: 00 00 00 00 00 00 00 00 
0020: 00 00 00 00 00 00 00 00 
0028: 11 00 00 80 
```

```
Event ID: 26 
Event Type: Information
Event Source: Application Popup
Description: Windows - Delayed Write Failed : Windows was unable to save all the data for the file \Device\HarddiskVolume4\Program Files\Microsoft SQL Server\MSSQL$INSTANCETWO\LOG\ERRORLOG. The data has been lost. This error may be caused by a failure of your computer hardware or network connection.

Please try to save this file elsewhere.
```

這些事件識別碼訊息表示完全相同的內容，而且會基於相同的原因產生。 基於本文的目的，只會描述事件識別碼50訊息。

> [!NOTE] 
> 描述中的裝置和路徑和特定的十六進位資料會有所不同。 

##  <a name="more-information"></a>相關資訊

如果 Windows 嘗試將資訊寫入磁片時發生一般錯誤，則會記錄事件識別碼50訊息。 當 Windows 嘗試從檔案系統快取管理員認可資料時，會發生此錯誤， (不是硬體層級快取) 至實體磁片。 此行為是 Windows 記憶體管理的一部分。 比方說，如果某個程式傳送寫入要求，快取管理員就會快取寫入要求，而程式會通知寫入已順利完成。 在稍後的時間點，快取管理員會嘗試將資料延遲寫入至實體磁片。 當快取管理員嘗試將資料認可到磁片時，寫入資料時就會發生錯誤，而且資料會從快取中清除並捨棄。 回寫式快取可改善系統效能，但遺失延遲寫入失敗可能會導致資料遺失和磁片區完整性遺失。

請務必記住，快取管理員並非所有的 i/o 都是緩衝處理的 i/o。 程式可以設定略過快取管理員的 FILE_FLAG_NO_BUFFERING 旗標。 當 SQL 執行資料庫的重大寫入時，會設定此旗標，以保證交易會直接完成至磁片。 例如，對記錄檔進行非關鍵性寫入，會執行緩衝的 i/o 來改善整體效能。 事件識別碼50訊息絕對不會從非緩衝 i/o 產生。

事件識別碼50訊息有幾個不同的來源。 例如，如果重新導向程式發生網路連線問題，就會發生 MRxSmb 來源所記錄的事件識別碼50訊息。 若要避免執行不正確的疑難排解步驟，請務必檢查事件識別碼50訊息，確認它是指磁片 i/o 問題，以及本文是否適用。

事件識別碼50訊息類似于事件識別碼9和事件識別碼11訊息。 雖然錯誤不像事件識別碼9所指出的錯誤和事件識別碼11訊息一樣嚴重，但您可以使用與事件識別碼50訊息相同的疑難排解技術，就像您在事件識別碼9和事件識別碼11訊息中所做的一樣。 不過，請記住，堆疊中的任何事物可能會導致寫入遺失延遲，例如篩選器驅動程式和迷你埠驅動程式。 

您可以使用與任何伴隨的「磁片」錯誤相關聯的二進位資料 (由事件識別碼9、11、51錯誤訊息或其他訊息所表示，) 協助您找出問題。

###  <a name="how-to-decode-the-data-section-of-an-event-id-50-event-message"></a>如何解碼事件識別碼50事件訊息的資料區段 

當您將 [摘要] 區段中所包含之事件識別碼50訊息的範例中的資料區段解碼時，您會看到執行寫入作業的嘗試失敗，因為裝置忙碌中且資料遺失。 本節說明如何將此事件識別碼50訊息解碼。 

下表說明此訊息的每個位移代表什麼： 

|OffsetLengthValues|長度|值|
|-----------|------------|---------|
|0x00|2|未使用|
|0x02|2|傾印資料大小 = 0x0004|
|0x04|2|字串數目 = 0x0002|
|0x06|2|字串的位移|
|0x08|2|事件類別目錄|
|0x0c|4|NTSTATUS 錯誤碼 = 0x80040032 = IO_LOST_DELAYED_WRITE|
|0x10|8|未使用|
|0x18|8|未使用|
|0x20|8|未使用|
|0x28|4|NT 狀態錯誤碼|

#### <a name="key-sections-to-decode"></a>要解碼的重要區段

**錯誤碼**

在 [摘要] 區段的範例中，錯誤碼列在第二行中。 這一行開頭為 "0008："，其中包含最後四個位元組：在此案例中為0008： 00 00 00 00 32 00 04 80，錯誤碼為0x80040032。 下列程式碼是錯誤50的程式碼，而且所有事件識別碼50訊息都相同： IO_LOST_DELAYED_WRITEWARNINGNote 當您將事件識別碼訊息中的十六進位資料轉換成狀態碼時，請記住這些值是以位元組由小到大的格式來表示。

**目標磁片**

您可以使用事件識別碼訊息的 [描述] 區段中的磁片磁碟機所列的符號連結，找出寫入嘗試的磁片，例如： \Device\HarddiskVolume4。

**最終狀態代碼**

最後的狀態碼是事件識別碼50訊息中最重要的資訊片段。 這是發出 i/o 要求時傳回的錯誤碼，而且是資訊的主要來源。 在 [摘要] 區段的範例中，最後的狀態碼會列在0x28，第六行的開頭為 "0028："，且在這一行中只包含四個八位： 

```
0028: 11 00 00 80 
```

在此情況下，最終狀態等於0x80000011。此狀態碼會對應至 STATUS_DEVICE_BUSY，表示裝置目前忙碌中。

> [!NOTE] 
> 當您將事件識別碼50訊息中的十六進位資料轉換成狀態碼時，請記住這些值是以位元組由小到大的格式來表示。 因為狀態碼是您感興趣的唯一資訊片段，所以以單字格式（而非位元組）來查看資料可能比較容易。 如果您這樣做，位元組將會是正確的格式，而且資料可能會比較容易解讀。

若要這樣做，請按一下 [ **事件屬性** ] 視窗中的 [ **文字** ]。 在 [資料文字] 視圖中，[徵兆] 區段中的範例會如下所示閱讀：資料： 

```
() Bytes (.) 
Words 0000: 00040000 00560002 00000000 80040032 0010: 00000000 00000000 00000000 00000000 0020: 00000000 00000000 80000011
```

若要取得 Windows NT 狀態碼的清單，請參閱「NTSTATUS」。H Windows 軟體發展人員套件中的 H (SDK) 。
