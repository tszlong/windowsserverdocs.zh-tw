---
title: 針對事件識別碼50錯誤訊息進行疑難排解
description: 說明如何疑難排解事件識別碼50錯誤訊息
author: Deland-Han
manager: dcscontentpm
audience: ITPro
ms.topic: article
ms.author: delhan
ms.date: 12/25/2019
ms.openlocfilehash: 202e0604fc492ff72cd1794bc8197a12c1ab9163
ms.sourcegitcommit: 8cf04db0bc44fd98f4321dca334e38c6573fae6c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/03/2020
ms.locfileid: "75654379"
---
# <a name="troubleshoot-the-event-id-50-error-message"></a>針對事件識別碼50錯誤訊息進行疑難排解

##  <a name="symptoms"></a>問題

當資訊寫入實體磁片時，系統事件記錄檔中可能會記錄下列兩個事件訊息： 

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

這些事件識別碼訊息的意義完全相同，而且會因為相同的原因而產生。 基於本文的目的，只會描述事件識別碼50訊息。

> [!NOTE] 
> 描述中的裝置和路徑和特定的十六進位資料會有所不同。 

##  <a name="more-information"></a>更多資訊

當 Windows 嘗試將資訊寫入磁片時，如果發生一般錯誤，則會記錄事件識別碼50訊息。 當 Windows 嘗試將資料從檔案系統快取管理員（而非硬體層級快取）認可至實體磁片時，就會發生此錯誤。 此行為是 Windows 記憶體管理的一部分。 例如，如果程式傳送寫入要求，則快取管理員會快取寫入要求，而程式會告知寫入已順利完成。 在稍後的時間點，快取管理員會嘗試將資料延遲寫入實體磁片。 當「快取管理員」嘗試將資料認可到磁片時，寫入資料時發生錯誤，而且會從快取中清除資料並予以捨棄。 回寫式快取可改善系統效能，但是遺失延遲寫入失敗，可能會導致資料遺失和磁片區完整性遺失。

請務必記住，快取管理員並非所有的 i/o 都是緩衝處理的 i/o。 程式可以設定會略過快取管理員的 FILE_FLAG_NO_BUFFERING 旗標。 當 SQL 對資料庫執行重要寫入時，會設定此旗標，以保證交易會直接完成至磁片。 例如，記錄檔的非關鍵寫入會執行緩衝處理的 i/o，以改善整體效能。 事件識別碼50訊息永遠不會由非緩衝處理的 i/o 所造成。

事件識別碼50訊息有數個不同的來源。 例如，如果重新導向器發生網路連線問題，則會從 MRxSmb 來源記錄事件識別碼50訊息。 為避免執行不正確的疑難排解步驟，請務必檢查事件識別碼50訊息，以確認它是指磁片 i/o 問題，而且這是適用的文章。

事件識別碼50訊息類似于事件識別碼9和事件識別碼11訊息。 雖然錯誤與事件識別碼9和事件識別碼11訊息所指出的錯誤並不嚴重，但是您可以針對事件識別碼50訊息使用相同的疑難排解技巧，如同您對事件識別碼9和事件識別碼11訊息所做的一樣。 不過，請記住，堆疊中的任何專案都可能造成遺失延遲的寫入，例如篩選驅動程式和迷你埠驅動程式。 

您可以使用與任何伴隨的「磁片」錯誤（以事件識別碼9、11、51錯誤訊息或其他訊息表示）相關聯的二進位資料，協助您找出問題。

###  <a name="how-to-decode-the-data-section-of-an-event-id-50-event-message"></a>如何解碼事件識別碼50事件訊息的資料區段 

當您解碼「摘要」一節所包含的事件識別碼50訊息範例中的資料區段時，您會看到嘗試執行寫入作業失敗，因為裝置忙碌中且資料已遺失。 本節說明如何將此事件識別碼50訊息解碼。 

下表描述此訊息的每個位移代表的意義： 

|OffsetLengthValues|長度|值|
|-----------|------------|---------|
|0x00|2|未使用|
|0x02|2|傾印資料大小 = 0x0004|
|0x04|2|字串數目 = 0x0002|
|0x06|2|字串的位移|
|0x08|2|事件類別|
|0x0c|4|NTSTATUS 錯誤碼 = 0x80040032 = IO_LOST_DELAYED_WRITE|
|0x10|8|未使用|
|0x18|8|未使用|
|0x20|8|未使用|
|0x28|4|NT 狀態錯誤碼|

#### <a name="key-sections-to-decode"></a>要解碼的主要區段

**錯誤碼**

在 [摘要] 區段的範例中，錯誤碼會列在第二行。 這一行的開頭為 "0008："，它包含這一行中的最後四個位元組：0008： 00 00 00 00 32 00 04 80 在此情況下，錯誤碼為0x80040032。 下列程式碼是錯誤50的代碼，而且對於所有事件識別碼50訊息都是相同的： IO_LOST_DELAYED_WRITEWARNINGNote 當您將事件識別碼訊息中的十六進位資料轉換成狀態碼時，請記住這些值會以下列方式表示：位元組由小到大格式。

**目標磁片**

您可以使用事件識別碼訊息「描述」一節中的磁片磁碟機所列的符號連結，識別嘗試寫入的磁片，例如： \Device\HarddiskVolume4。 如需有關如何識別磁片磁碟機的詳細資訊，請按一下下列文章編號，以查看 Microsoft 知識庫中的文章： [159865](/EN-US/help/159865)如何區別實體磁片裝置與事件訊息

**最終狀態代碼**

最後的狀態碼是事件識別碼50訊息中最重要的資訊片段。 這是發出 i/o 要求時傳回的錯誤碼，而這是資訊的金鑰來源。 在「摘要」一節的範例中，最終狀態代碼會列在0x28，第六行以 "0028：" 開頭，並在這一行中包含唯一的四個八位： 

```
0028: 11 00 00 80 
```

在此情況下，最終狀態等於0x80000011。此狀態碼會對應至 STATUS_DEVICE_BUSY，並表示裝置目前忙碌中。

>[!NOTE] 
> 當您將事件識別碼50訊息中的十六進位資料轉換成狀態碼時，請記住，這些值是以位元組由小到大的格式表示。 因為狀態碼是您感興趣的唯一資訊片段，所以以單字格式（而不是位元組）來查看資料可能比較容易。 如果您這樣做，位元組將會是正確的格式，而且資料可能會更容易快速地解讀。

若要這麼做，請按一下 [**事件屬性**] 視窗中的 [**文字**]。 在 [資料字] 視圖中，[徵兆] 區段中的範例會如下所示：資料： 

```
() Bytes (.) 
Words 0000: 00040000 00560002 00000000 80040032 0010: 00000000 00000000 00000000 00000000 0020: 00000000 00000000 80000011
```

若要取得 Windows NT 狀態碼的清單，請參閱 NTSTATUS。H 在 Windows 軟體發展人員套件（SDK）中。