---
title: wevtutil
description: 適用于 wevtutil 的 Windows 命令主題，可讓您取得事件記錄檔和發行者的相關資訊。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d4c791e0-7e59-45c5-aa55-0223b77a4822 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 62be2b14373457a3b114e8d067e1c7aa32b2182d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80829351"
---
# <a name="wevtutil"></a>wevtutil



可讓您擷取事件記錄檔和發行者的相關資訊。 您也可以使用此命令來安裝和解除安裝事件資訊清單、執行查詢以及匯出、封存和清除記錄檔。 如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
wevtutil [{el | enum-logs}] [{gl | get-log} <Logname> [/f:<Format>]]
[{sl | set-log} <Logname> [/e:<Enabled>] [/i:<Isolation>] [/lfn:<Logpath>] [/rt:<Retention>] [/ab:<Auto>] [/ms:<MaxSize>] [/l:<Level>] [/k:<Keywords>] [/ca:<Channel>] [/c:<Config>]] 
[{ep | enum-publishers}] 
[{gp | get-publisher} <Publishername> [/ge:<Metadata>] [/gm:<Message>] [/f:<Format>]] [{im | install-manifest} <Manifest>] 
[{um | uninstall-manifest} <Manifest>] [{qe | query-events} <Path> [/lf:<Logfile>] [/sq:<Structquery>] [/q:<Query>] [/bm:<Bookmark>] [/sbm:<Savebm>] [/rd:<Direction>] [/f:<Format>] [/l:<Locale>] [/c:<Count>] [/e:<Element>]] 
[{gli | get-loginfo} <Logname> [/lf:<Logfile>]] 
[{epl | export-log} <Path> <Exportfile> [/lf:<Logfile>] [/sq:<Structquery>] [/q:<Query>] [/ow:<Overwrite>]] 
[{al | archive-log} <Logpath> [/l:<Locale>]] 
[{cl | clear-log} <Logname> [/bu:<Backup>]] [/r:<Remote>] [/u:<Username>] [/p:<Password>] [/a:<Auth>] [/uni:<Unicode>]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|{el \| enum-logs}|顯示所有記錄檔的名稱。|
|{gl \| 取得記錄檔} \<Logname > [/f：\<格式 >]|顯示指定記錄檔的設定資訊，其中包括記錄是否已啟用、記錄檔的目前大小上限，以及儲存記錄檔之檔案的路徑。|
|{sl \| set-log} \<Logname > [/e：\<啟用 >] [/i：\<隔離 >] [/lfn：\<Logpath >] [/rt：\<保留 >] [/ab：\<自動 >] [/ms：\<MaxSize >] [/l：\<Level >] [/k：\<關鍵字 >] [/c：\<Config >]\<|修改指定之記錄檔的設定。|
|{ep \| 列舉-發行者}|顯示本機電腦上的事件發行者。|
|{gp \| get 發行者} \<Publishername > [/ge：\<中繼資料 >] [/gm：\<訊息 >] [/f：\<格式 >]]|顯示指定之事件發行者的設定資訊。|
|{im \| 安裝-資訊清單} \<資訊清單 >|從資訊清單安裝事件發行者和記錄。 如需事件資訊清單和使用此參數的詳細資訊，請參閱 Microsoft 開發人員網路（MSDN）網站上的 Windows 事件記錄檔 SDK （[https://msdn.microsoft.com](https://msdn.microsoft.com)）。|
|{um \| 卸載-資訊清單} \<資訊清單 >|從資訊清單中卸載所有發行者和記錄。 如需事件資訊清單和使用此參數的詳細資訊，請參閱 Microsoft 開發人員網路（MSDN）網站上的 Windows 事件記錄檔 SDK （[https://msdn.microsoft.com](https://msdn.microsoft.com)）。|
|{qe \| 查詢事件} \<路徑 > [/lf：\<Logfile >] [/sq：\<Structquery >] [/q：\<查詢 >] [/bm：\<書簽 >] [/sbm：\<Savebm >] [/rd：\<方向 >] [/f：\<格式 >] [/l：\<地區設定 >] [/c：\<計數 >] [/e：\<元素 >]|從事件記錄檔、記錄檔或使用結構化查詢讀取事件。 根據預設，您會提供 \<路徑 > 的記錄檔名稱。 不過，如果您使用 **/lf**選項，則 \<路徑 > 必須是記錄檔的路徑。 如果您使用 **/sq**參數，\<路徑 > 必須是包含結構化查詢之檔案的路徑。|
|{gli \| loginfo} \<Logname > [/lf：\<記錄檔 >]|顯示事件記錄檔或記錄檔的狀態資訊。 如果使用 **/lf**選項，\<Logname > 是記錄檔的路徑。 您可以執行**wevtutil el**來取得記錄檔名稱的清單。|
|{epl \| 匯出記錄檔} \<路徑 > \<Exportfile > [/lf：\<記錄檔 >] [/sq：\<Structquery >] [/q：\<查詢 >] [/ow：\<覆寫 >]|從事件記錄檔、記錄檔或使用結構化查詢，將事件匯出至指定的檔案。 根據預設，您會提供 \<路徑 > 的記錄檔名稱。 不過，如果您使用 **/lf**選項，則 \<路徑 > 必須是記錄檔的路徑。 如果您使用 **/sq**選項，\<路徑 > 必須是包含結構化查詢之檔案的路徑。 \<Exportfile > 是將儲存所匯出事件之檔案的路徑。|
|{al \| archive-log} \<Logpath > [/l：\<地區設定 >]|以獨立格式封存指定的記錄檔。 會建立具有地區設定名稱的子目錄，並將所有地區設定特定資訊儲存在該子目錄中。 藉由執行**wevtutil al**建立目錄和記錄檔之後，不論是否已安裝發行者，都可以讀取檔案中的事件。|
|{cl \| 清除記錄} \<Logname > [/bu：\<備份 >]|清除指定事件記錄檔中的事件。 您可以使用 **/bu**選項來備份已清除的事件。|

## <a name="options"></a>選項

|       選項       |                                                                                                                                                                                                                                                                 描述                                                                                                                                                                                                                                                                  |
|--------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /f：\<格式 >    |                                                                                                                                                               指定輸出應該是 XML 或文字格式。 如果 \<格式 > 是 XML，則輸出會以 XML 格式顯示。 如果 \<格式 > 為文字，則會顯示不含 XML 標記的輸出。 預設值是 Text。                                                                                                                                                                |
|   /e：\<啟用 >    |                                                                                                                                                                                                                                         啟用或停用記錄檔。 \<啟用 > 可以是 true 或 false。                                                                                                                                                                                                                                          |
|  /i：\<隔離 >   | 設定記錄隔離模式。 \<隔離 > 可以是 [系統]、[應用程式] 或 [自訂]。 記錄檔的隔離模式會決定記錄檔是否與相同隔離類別中的其他記錄共用會話。 如果您指定系統隔離，目標記錄檔將至少與系統記錄共用寫入權限。 如果您指定應用程式隔離，目標記錄檔將至少與應用程式記錄檔共用寫入權限。 如果您指定自訂隔離，則也必須使用 **/ca**選項提供安全描述項。 |
|  /lfn：\<Logpath >   |                                                                                                                                                                                                           定義記錄檔名稱。 \<Logpath > 是檔案的完整路徑，事件記錄檔服務會在此檔案中儲存此記錄檔的事件。                                                                                                                                                                                                           |
|  /rt：\<保留 >  |                                                            設定記錄保留模式。 \<保留 > 可以是 true 或 false。 記錄保留模式會決定當記錄達到其大小上限時，事件記錄檔服務的行為。 如果事件記錄檔達到其大小上限，而且記錄保留模式為 true，則會保留現有的事件並捨棄傳入事件。 如果記錄保留模式為 false，傳入事件會覆寫記錄檔中最舊的事件。                                                             |
|    /ab：\<Auto >     |                                                                                                                                   指定記錄自動備份原則。 \<Auto > 可以是 true 或 false。 如果此值為 true，則記錄會在達到大小上限時自動備份。 如果此值為 true，則保留（使用 **/rt**選項指定）也必須設定為 true。                                                                                                                                    |
|   /ms：\<MaxSize >   |                                                                                                                                                                        設定記錄檔的大小上限（以位元組為單位）。 記錄大小的最小值是1048576個位元組（1024KB），而記錄檔一律是64KB 的倍數，因此您輸入的值將會相應地四捨五入。                                                                                                                                                                         |
|    /l：\<層級 >     |                                                                                                                                                                     定義記錄檔的層級篩選。 \<層級 > 可以是任何有效的層級值。 此選項只適用于具有專用會話的記錄。 您可以藉由將 <Level> 設定為0來移除層級篩選。                                                                                                                                                                      |
|   /k：\<關鍵字 >   |                                                                                                                                                                                         指定記錄檔的關鍵字篩選準則。 \<關鍵字 > 可以是任何有效的64位關鍵字遮罩。 此選項只適用于具有專用會話的記錄。                                                                                                                                                                                         |
|   /ca：\<通道 >   |                                                                                                                   設定事件記錄檔的存取權限。 \<通道 > 是使用安全描述項定義語言（SDDL）的安全描述項。 如需 SDDL 格式的詳細資訊，請參閱 Microsoft 開發人員網路（MSDN）網站（[https://msdn.microsoft.com](https://msdn.microsoft.com)）。                                                                                                                    |
|    /c：\<Config >    |                                                                                                                                  指定設定檔案的路徑。 此選項將會從 \<Config > 中定義的設定檔中讀取記錄檔屬性。 如果您使用此選項，則不能指定 <Logname> 參數。 記錄檔名稱會從設定檔案中讀取。                                                                                                                                   |
|  /ge：\<中繼資料 >   |                                                                                                                                                                                                                 取得此發行者可以引發之事件的中繼資料資訊。 \<中繼資料 > 可以是 true 或 false。                                                                                                                                                                                                                 |
|   /gm：\<訊息 >   |                                                                                                                                                                                                                       顯示實際的訊息，而不是數值訊息識別碼。 \<訊息 > 可以是 true 或 false。                                                                                                                                                                                                                        |
|   /lf：\<Logfile >   |                                                                                                                                                                                  指定應該從記錄檔或記錄檔讀取事件。 \<記錄檔 > 可以是 true 或 false。 若為 true，則命令的參數是記錄檔的路徑。                                                                                                                                                                                   |
| /sq：\<Structquery > |                                                                                                                                                                                指定應該使用結構化查詢來取得事件。 \<Structquery > 可以是 true 或 false。 若為 true，<Path> 是包含結構化查詢之檔案的路徑。                                                                                                                                                                                |
|    /q：\<查詢 >     |                                                                                                                                                                     定義用來篩選讀取或匯出事件的 XPath 查詢。 如果未指定此選項，則會傳回或匯出所有事件。 當 **/sq**為 true 時，無法使用此選項。                                                                                                                                                                     |
|  /bm：\<書簽 >   |                                                                                                                                                                                                                                 指定包含前一個查詢之書簽的檔案路徑。                                                                                                                                                                                                                                 |
|   /sbm：\<Savebm >   |                                                                                                                                                                                                             指定檔案的路徑，這個檔案是用來儲存此查詢的書簽。 副檔名應該是 .xml。                                                                                                                                                                                                              |
|  /rd：\<方向 >  |                                                                                                                                                                                                   指定事件的讀取方向。 \<方向 > 可以是 true 或 false。 若為 true，則會先傳回最新的事件。                                                                                                                                                                                                   |
|    /l：\<地區設定 >    |                                                                                                                                                                                          定義用來以特定地區設定列印事件文字的地區設定字串。 僅適用于使用 **/f**選項來列印文字格式的事件時。                                                                                                                                                                                          |
|    /c：\<計數 >     |                                                                                                                                                                                                                                                  設定要讀取的最大事件數目。                                                                                                                                                                                                                                                  |
|   /e：\<元素 >    |                                                                                                                                                           在 XML 中顯示事件時包含根項目。 \<元素 > 是您想要在根項目內的字串。 例如， **/e： root**會產生 XML，其中包含根項目組，\<根 ></root>。                                                                                                                                                            |
|  /ow：\<覆寫 >  |                                                                                                                                                                 指定應該覆寫匯出檔案。 \<覆寫 > 可以是 true 或 false。 若為 true，且 <Exportfile> 中指定的匯出檔案已經存在，則會覆寫該檔案而不進行確認。                                                                                                                                                                 |
|   /bu：\<備份 >    |                                                                                                                                                                                                      指定將儲存已清除之事件的檔案路徑。 在備份檔案的名稱中包含 .evtx 副檔名。                                                                                                                                                                                                       |
|    /r：\<遠端 >    |                                                                                                                                                                                            在遠端電腦上執行命令。 \<遠端 > 是遠端電腦的名稱。 **Im**和**um**參數不支援遠端操作。                                                                                                                                                                                            |
|   /u：\<Username >   |                                                                                                                                                                          指定不同的使用者登入遠端電腦。 \<Username > 是 domain\user 或 user 格式的使用者名稱。 只有在指定 **/r**選項時，才適用此選項。                                                                                                                                                                          |
|   /p：\<密碼 >   |                                                                                                                                               指定使用者的密碼。 如果使用 **/u**選項，但未指定此選項，或 \<密碼 > 為 *，則會提示使用者輸入密碼。只有在指定 \*\*/u\* 選項時，才適用此選項*。                                                                                                                                                |
|     /a：\<Auth >     |                                                                                                                                                                                             定義用來連接到遠端電腦的驗證類型。 \<Auth > 可以是 [預設]、[Negotiate]、[Kerberos] 或 [NTLM]。 預設值為 Negotiate。                                                                                                                                                                                              |
|  /uni：\<Unicode >   |                                                                                                                                                                                                             以 Unicode 顯示輸出。 \<Unicode > 可以是 true 或 false。 如果 <Unicode> 為 true，則輸出會是 Unicode。                                                                                                                                                                                                             |

## <a name="remarks"></a>備註

-   使用具有 sl 參數的設定檔

    設定檔是一個 XML 檔案，其格式與 wevtutil gl 的輸出 \<Logname >/f： xml 相同。 下列範例會顯示啟用保留的配置檔案格式、啟用 autobackup，以及設定應用程式記錄檔的記錄大小上限：  
    ```
    <?xml version=1.0 encoding=UTF-8?>
    <channel name=Application isolation=Application
    xmlns=https://schemas.microsoft.com/win/2004/08/events>
    <logging>
    <retention>true</retention>
    <autoBackup>true</autoBackup>
    <maxSize>9000000</maxSize>
    </logging>
    <publishing>
    </publishing>
    </channel>
    ```

## <a name="examples"></a><a name=BKMK_examples></a>典型

列出所有記錄檔的名稱：
```
wevtutil el
```
以 XML 格式顯示本機電腦上系統記錄檔的設定資訊：
```
wevtutil gl System /f:xml
```
使用設定檔來設定事件記錄檔屬性（如需設定檔範例，請參閱備註）：
```
wevtutil sl /c:config.xml
```
顯示 Microsoft-Windows-Eventlog 事件發行者的相關資訊，包括發行者可以引發之事件的相關中繼資料：
```
wevtutil gp Microsoft-Windows-Eventlog /ge:true
```
從 myManifest 資訊清單檔案安裝發行者和記錄檔：
```
wevtutil im myManifest.xml
```
從 myManifest 資訊清單檔案卸載發行者和記錄檔：
```
wevtutil um myManifest.xml
```
以文字格式顯示應用程式記錄檔中的三個最新事件：
```
wevtutil qe Application /c:3 /rd:true /f:text
```
顯示應用程式記錄檔的狀態：
```
wevtutil gli Application 
```
將事件從系統記錄檔匯出至 C:\backup\system0506.evtx：
```
wevtutil epl System C:\backup\system0506.evtx
```
將應用程式記錄檔中的所有事件儲存至 C:\admin\backups\a10306.evtx 之後，將其清除：
```
wevtutil cl Application /bu:C:\admin\backups\a10306.evtx
```

#### <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
