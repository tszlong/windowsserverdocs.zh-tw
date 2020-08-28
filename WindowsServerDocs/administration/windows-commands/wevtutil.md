---
title: wevtutil
description: Wevtutil 的參考文章，可讓您取得事件記錄和發行者的相關資訊。
ms.topic: reference
ms.assetid: d4c791e0-7e59-45c5-aa55-0223b77a4822
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8b36619def02434a253f0e6e97ef47cb78190b8a
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89031726"
---
# <a name="wevtutil"></a>wevtutil



可讓您擷取事件記錄檔和發行者的相關資訊。 您也可以使用此命令來安裝和解除安裝事件資訊清單、執行查詢以及匯出、封存和清除記錄檔。

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
|{el \| 列舉-記錄}|顯示所有記錄檔的名稱。|
|{gl \| 取得記錄} \<Logname> [/f： \<Format> ]|顯示指定記錄檔的設定資訊，包括是否啟用記錄檔、記錄檔目前的大小上限，以及儲存記錄檔之檔案的路徑。|
|{sl \| 組-log} \<Logname> [/e： \<Enabled> ][/i： \<Isolation> ][/lfn： \<Logpath> ][/rt： \<Retention> ][/ab： \<Auto> ][/ms： \<MaxSize> ][/l： \<Level> ][/k： \<Keywords> ][/ca： \<Channel> ][/c： \<Config> ]|修改指定之記錄檔的設定。|
|{ep \| 列舉-發行者}|顯示本機電腦上的事件發行者。|
|{gp \| get-發行者} \<Publishername> [/ge： \<Metadata> ][/gm： \<Message> ][/f： \<Format> ]]|顯示指定之事件發行者的設定資訊。|
|{im \| 安裝-資訊清單} \<Manifest>|從資訊清單安裝事件發行者和記錄。 如需事件資訊清單和使用此參數的詳細資訊，請參閱 Microsoft 開發人員網路上的 Windows 事件記錄檔 SDK (MSDN) 網站 ([https://msdn.microsoft.com](../../index.yml)) 。|
|{um \| 卸載-資訊清單} \<Manifest>|從資訊清單中卸載所有發行者和記錄。 如需事件資訊清單和使用此參數的詳細資訊，請參閱 Microsoft 開發人員網路上的 Windows 事件記錄檔 SDK (MSDN) 網站 ([https://msdn.microsoft.com](../../index.yml)) 。|
|{qe \| query-events} \<Path> [/lf： \<Logfile> ][/sq： \<Structquery> ][/q： \<Query> ][/bm： \<Bookmark> ][/sbm： \<Savebm> ][/rd： \<Direction> ][/f： \<Format> ][/l： \<Locale> ][/c： \<Count> ][/e： \<Element> ]|從事件記錄檔、記錄檔或使用結構化查詢讀取事件。 根據預設，您會提供的記錄檔名稱 \<Path> 。 但是，如果您使用 **/lf** 選項，則 \<Path> 必須是記錄檔的路徑。 如果您使用 **/sq** 參數， \<Path> 必須是包含結構化查詢之檔案的路徑。|
|{gli \| get-loginfo} \<Logname> [/lf： \<Logfile> ]|顯示事件記錄檔或記錄檔的狀態資訊。 如果使用 **/lf** 選項， \<Logname> 就是記錄檔的路徑。 您可以執行 **wevtutil el** 來取得記錄檔名稱的清單。|
|{epl \| export-log} \<Path> \<Exportfile> [/lf： \<Logfile> ][/sq： \<Structquery> ][/q： \<Query> ][/ow： \<Overwrite> ]|從事件記錄檔、記錄檔或使用結構化查詢將事件匯出至指定的檔案。 根據預設，您會提供的記錄檔名稱 \<Path> 。 但是，如果您使用 **/lf** 選項，則 \<Path> 必須是記錄檔的路徑。 如果您使用 **/sq** 選項，則 \<Path> 必須是包含結構化查詢之檔案的路徑。 \<Exportfile> 這是將儲存匯出事件的檔案路徑。|
|{al 封存 \| -記錄} \<Logpath> [/l： \<Locale> ]|以自主格式封存指定的記錄檔。 會建立具有地區設定名稱的子目錄，並在該子目錄中儲存所有地區設定特定的資訊。 藉由執行 **wevtutil al**來建立目錄和記錄檔之後，無論是否安裝發行者，都可以讀取檔案中的事件。|
|{cl \| clear-log} \<Logname> [/bu： \<Backup> ]|從指定的事件記錄檔清除事件。 **/Bu**選項可以用來備份已清除的事件。|

## <a name="options"></a>選項

|       選項       |                                                                                                                                                                                                                                                                 描述                                                                                                                                                                                                                                                                  |
|--------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /f\<Format>    |                                                                                                                                                               指定輸出應該是 XML 或文字格式。 如果 \<Format> 是 xml，則輸出會以 xml 格式顯示。 如果 \<Format> 是文字，則會顯示不含 XML 標記的輸出。 預設為 Text。                                                                                                                                                                |
|   /e:\<Enabled>    |                                                                                                                                                                                                                                         啟用或停用記錄檔。 \<Enabled> 可以是 true 或 false。                                                                                                                                                                                                                                          |
|  /i\<Isolation>   | 設定記錄檔隔離模式。 \<Isolation> 可以是系統、應用程式或自訂的。 記錄檔的隔離模式會判斷記錄檔是否與相同隔離類別中的其他記錄共用會話。 如果您指定系統隔離，目標記錄檔將至少與系統記錄檔共用寫入權限。 如果您指定應用程式隔離，目標記錄檔將至少與應用程式記錄檔共用寫入權限。 如果您指定自訂隔離，則也必須使用 **/ca** 選項來提供安全描述項。 |
|  /lfn:\<Logpath>   |                                                                                                                                                                                                           定義記錄檔名稱。 \<Logpath> 是檔案的完整路徑，其中事件記錄檔服務會儲存此記錄檔的事件。                                                                                                                                                                                                           |
|  rt\<Retention>  |                                                            設定記錄檔保留模式。 \<Retention> 可以是 true 或 false。 當記錄檔到達大小上限時，記錄保留模式會決定事件記錄檔服務的行為。 如果事件記錄檔達到其大小上限，而且記錄檔保留模式為 true，則會保留現有的事件，並捨棄傳入的事件。 如果記錄保留模式為 false，則連入事件會覆寫記錄中最舊的事件。                                                             |
|    ab\<Auto>     |                                                                                                                                   指定記錄自動備份原則。 \<Auto> 可以是 true 或 false。 如果這個值為 true，就會在記錄檔達到大小上限時自動備份記錄檔。 如果這個值為 true， **/rt** 選項) 指定的保留 (也必須設定為 true。                                                                                                                                    |
|   /ms\<MaxSize>   |                                                                                                                                                                        設定記錄檔的大小上限（以位元組為單位）。 最小記錄大小為1048576個位元組 (1024KB) 而記錄檔一律是64KB 的倍數，因此您輸入的值會據以四捨五入。                                                                                                                                                                         |
|    /l:\<Level>     |                                                                                                                                                                     定義記錄的層級篩選。 \<Level> 可以是任何有效的層級值。 此選項只適用于具有專用會話的記錄。 您可以藉由將設定為0來移除層級篩選 <Level> 。                                                                                                                                                                      |
|   /k\<Keywords>   |                                                                                                                                                                                         指定記錄檔的關鍵字篩選。 \<Keywords> 可以是任何有效的64位關鍵字遮罩。 此選項只適用于具有專用會話的記錄。                                                                                                                                                                                         |
|   /ca\<Channel>   |                                                                                                                   設定事件記錄檔的存取權限。 \<Channel> 這是使用安全描述項定義語言 (SDDL) 的安全描述項。 如需有關 SDDL 格式的詳細資訊，請參閱 Microsoft 開發人員網路 (MSDN) 網站 ([https://msdn.microsoft.com](../../index.yml)) 。                                                                                                                    |
|    /c\<Config>    |                                                                                                                                  指定設定檔案的路徑。 此選項會導致從中定義的設定檔讀取記錄檔屬性 \<Config> 。 如果您使用此選項，則不能指定 <Logname> 參數。 記錄檔名稱將會從設定檔案中讀取。                                                                                                                                   |
|  /ge\<Metadata>   |                                                                                                                                                                                                                 取得此發行者可引發之事件的中繼資料資訊。 \<Metadata> 可以是 true 或 false。                                                                                                                                                                                                                 |
|   /gm\<Message>   |                                                                                                                                                                                                                       顯示實際的訊息，而非數值訊息識別碼。 \<Message> 可以是 true 或 false。                                                                                                                                                                                                                        |
|   lf\<Logfile>   |                                                                                                                                                                                  指定應從記錄檔或記錄檔讀取事件。 \<Logfile> 可以是 true 或 false。 若為 true，則命令的參數是記錄檔的路徑。                                                                                                                                                                                   |
| /sq\<Structquery> |                                                                                                                                                                                指定應以結構化查詢取得事件。 \<Structquery> 可以是 true 或 false。 若為 true， <Path> 則為包含結構化查詢之檔案的路徑。                                                                                                                                                                                |
|    一起\<Query>     |                                                                                                                                                                     定義 XPath 查詢，以篩選讀取或匯出的事件。 如果未指定此選項，則會傳回或匯出所有事件。 當 **/sq** 為 true 時，無法使用此選項。                                                                                                                                                                     |
|  bm\<Bookmark>   |                                                                                                                                                                                                                                 指定檔案的路徑，該檔案包含上一個查詢的書簽。                                                                                                                                                                                                                                 |
|   /sbm:\<Savebm>   |                                                                                                                                                                                                             指定檔案的路徑，該檔案是用來儲存此查詢的書簽。 副檔名應該是 .xml。                                                                                                                                                                                                              |
|  /rd\<Direction>  |                                                                                                                                                                                                   指定讀取事件的方向。 \<Direction> 可以是 true 或 false。 若為 true，則會先傳回最新的事件。                                                                                                                                                                                                   |
|    /l:\<Locale>    |                                                                                                                                                                                          定義用來列印特定地區設定中之事件文字的地區設定字串。 只有在使用 **/f** 選項列印文字格式的事件時才可使用。                                                                                                                                                                                          |
|    /c\<Count>     |                                                                                                                                                                                                                                                  設定要讀取的事件數目上限。                                                                                                                                                                                                                                                  |
|   /e:\<Element>    |                                                                                                                                                           在 XML 中顯示事件時包含根項目。 \<Element> 是您想要在根項目內的字串。 例如， **/e： root**會產生包含根項目配對的 XML \<root> </root> 。                                                                                                                                                            |
|  允許\<Overwrite>  |                                                                                                                                                                 指定應覆寫匯出檔案。 \<Overwrite> 可以是 true 或 false。 若為 true，且中指定的匯出檔案 <Exportfile> 已存在，則會覆寫，而不進行確認。                                                                                                                                                                 |
|   /bu\<Backup>    |                                                                                                                                                                                                      指定將儲存已清除之事件的檔案路徑。 將 .evtx 副檔名包含在備份檔案名稱中。                                                                                                                                                                                                       |
|    /r\<Remote>    |                                                                                                                                                                                            在遠端電腦上執行命令。 \<Remote> 這是遠端電腦的名稱。 **Im**和**um**參數不支援遠端操作。                                                                                                                                                                                            |
|   u\<Username>   |                                                                                                                                                                          指定不同的使用者登入遠端電腦。 \<Username> 是 domain\user 或 user 格式的使用者名稱。 只有在指定 **/r** 選項時，才適用此選項。                                                                                                                                                                          |
|   /p\<Password>   |                                                                                                                                               指定使用者的密碼。 如果使用 **/u**選項，且未指定此選項 \<Password> ，或為 *，則會提示使用者輸入密碼。只有在指定 \* \* /u 選項時，才適用此選項* \* 。                                                                                                                                                |
|     /a\<Auth>     |                                                                                                                                                                                             定義用來連接到遠端電腦的驗證類型。 \<Auth> 可以是 Default、Negotiate、Kerberos 或 NTLM。 預設值為 Negotiate。                                                                                                                                                                                              |
|  通道\<Unicode>   |                                                                                                                                                                                                             以 Unicode 顯示輸出。 \<Unicode> 可以是 true 或 false。 如果 <Unicode> 是 true，則輸出會是 Unicode。                                                                                                                                                                                                             |

## <a name="remarks"></a>備註

-   使用具有 sl 參數的設定檔

    設定檔是一個 XML 檔案，其格式與 wevtutil gl \<Logname> /f： XML 的輸出相同。 若要顯示啟用保留的配置檔案格式，請啟用 autobackup，並設定應用程式記錄檔的記錄檔大小上限：
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

## <a name="examples"></a>範例

列出所有記錄檔的名稱：
```
wevtutil el
```
以 XML 格式顯示本機電腦上系統記錄檔的設定資訊：
```
wevtutil gl System /f:xml
```
使用設定檔來設定事件記錄檔屬性 (如需設定檔的範例，請參閱備註) ：
```
wevtutil sl /c:config.xml
```
顯示有關 Microsoft Windows-Eventlog 事件發行者的資訊，包括發行者可引發之事件的相關中繼資料：
```
wevtutil gp Microsoft-Windows-Eventlog /ge:true
```
從 myManifest.xml 資訊清單檔案安裝發行者和記錄：
```
wevtutil im myManifest.xml
```
從 myManifest.xml 資訊清單檔案中卸載發行者和記錄：
```
wevtutil um myManifest.xml
```
以文字格式從應用程式記錄檔中顯示三個最新的事件：
```
wevtutil qe Application /c:3 /rd:true /f:text
```
顯示應用程式記錄檔的狀態：
```
wevtutil gli Application
```
將事件從系統記錄匯出至 C:\backup\system0506.evtx：
```
wevtutil epl System C:\backup\system0506.evtx
```
將應用程式記錄檔中的所有事件儲存至 C:\admin\backups\a10306.evtx 之後，請將其清除：
```
wevtutil cl Application /bu:C:\admin\backups\a10306.evtx
```

#### <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
