---
title: wevtutil
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d4c791e0-7e59-45c5-aa55-0223b77a4822 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e6d57f95379fce80bec9cb5e8445b28f887123c8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826729"
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

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|{el\|列舉記錄}|顯示所有記錄檔的名稱。|
|{gl\|取得記錄檔} \<Logname > [/ f:\<格式 >]|要儲存記錄檔的檔案，會顯示指定的記錄檔，其中包括是否已啟用記錄檔、 記錄檔之路徑的目前大小上限的設定資訊。|
|{sl\|組記錄} \<Logname > [/ e:\<已啟用 >] [/ i:\<隔離 >] [/ 助您在使用：\<Logpath >] [/ rt:\<保留 >] [/ ab:\<自動 >] [/ ms:\<MaxSize >] [/ l\<層級 >] [/ k\<關鍵字 >] [/ ca:\<通道 >] [/ c:\<組態 >]|修改指定的記錄檔的組態。|
|{ep\|列舉發行者}|顯示本機電腦上的事件發佈者。|
|{gp\|取得發行者} \<Publishername > [/ ge:\<中繼資料 >] [/ gm:\<訊息 >] [/ f:\<格式 >]]|顯示指定的事件發行者的組態資訊。|
|{im\|安裝資訊清單}\<資訊清單 >|從資訊清單，會安裝事件發佈者與記錄檔。 如需有關事件資訊清單和使用此參數的詳細資訊，請參閱 Microsoft Developers Network (MSDN) 網站上的 Windows 事件記錄檔 SDK (https://msdn.microsoft.com)。|
|{um\|解除安裝資訊清單}\<資訊清單 >|從資訊清單，解除安裝所有的發行者和記錄檔。 如需有關事件資訊清單和使用此參數的詳細資訊，請參閱 Microsoft Developers Network (MSDN) 網站上的 Windows 事件記錄檔 SDK (https://msdn.microsoft.com)。|
|{qe\|查詢事件}\<路徑 > [/ lf:\<記錄檔 >] [/ sq:\<Structquery >] [/ 問\<查詢 >] [/ bm:\<書籤 >] [/ sbm:\<Savebm >] [/ rd:\<方向 >] [/ f\<格式 >] [/ 高度：\<地區設定 >] [/ c:\<計數 >] [/ e:\<項目 >]|讀取事件記錄檔，從記錄檔，或使用結構化的查詢中的事件。 根據預設，您提供的記錄檔名稱\<路徑 >。 不過，如果您使用 **/lf**選項，然後\<路徑 > 必須是記錄檔的路徑。 如果您使用 **/sq**參數，\<路徑 > 必須是包含結構化的查詢的檔案路徑。|
|{gli \| get loginfo} \<Logname > [/ lf:\<記錄檔 >]|顯示相關的事件記錄檔或記錄檔的狀態資訊。 如果 **/lf**使用選項時， \<Logname > 是一個記錄檔的路徑。 您可以執行**wevtutil el**取得記錄檔名稱的清單。|
|{epl\|匯出記錄}\<路徑 > \<Exportfile > [/ lf:\<記錄檔 >] [/ sq:\<Structquery >] [/ 問：\<查詢 >] [/ ow:\<覆寫 >]|匯出事件從事件記錄檔，從記錄檔，或使用結構化的查詢，來指定的檔案。 根據預設，您提供的記錄檔名稱\<路徑 >。 不過，如果您使用 **/lf**選項，然後\<路徑 > 必須是記錄檔的路徑。 如果您使用 **/sq**選項，\<路徑 > 必須是包含結構化的查詢的檔案路徑。 \<Exportfile > 是要儲存匯出的事件檔案的路徑。|
|{al\|封存記錄檔} \<Logpath > [/ 高度：\<地區設定 >]|封存的自封式格式中指定的記錄檔。 建立地區設定名稱的子目錄和所有的地區設定特定資訊會儲存在該子目錄中。 目錄和記錄檔會建立執行之後**wevtutil al**，與否，是否已安裝 「 發行者 」 可以讀取檔案中的事件。|
|{cl\|清除記錄檔} \<Logname > [/ bu:\<備份 >]|從指定的事件記錄檔清除事件。 **/Bu**選項可用來備份已清除的事件。|

## <a name="options"></a>選項。

|選項|描述|
|------|-----------|
|/f:\<格式 >|指定的輸出應該是 XML 或文字格式。 如果\<格式 > 為 XML，輸出會顯示在 XML 格式。 如果\<格式 > 是文字，而不需要 XML 標記會顯示輸出。 預設值是文字。|
|/e\<已啟用 >|啟用或停用記錄檔。 \<啟用 > 可以是 true 或 false。|
|/i:\<隔離 >|將記錄檔隔離模式。 \<隔離 > 可以是應用程式或自訂的系統。 記錄檔隔離模式會決定記錄檔是否共用相同的隔離類別中的其他記錄檔的工作階段。 如果您指定系統隔離時，會共用目標記錄至少寫入與系統記錄檔的權限。 如果您指定應用程式隔離時，會共用目標記錄至少寫入應用程式記錄檔的權限。 如果您指定自訂的隔離，則您也必須使用提供的安全性描述元 **/ca**選項。|
|/lfn:\<Logpath >|定義記錄檔名稱。 \<Logpath > 是的事件記錄服務儲存此記錄檔的事件檔案的完整路徑。|
|/rt:\<保留 >|設定記錄保留模式。 \<保留 > 可以是 true 或 false。 記錄保留模式會決定事件記錄檔服務的行為，當記錄檔到達大小上限。 如果事件記錄檔達到大小上限，而且記錄保留模式，則為 true，會保留現有的事件，並捨棄的內送事件。 如果記錄保留模式為 false，則內送事件覆寫最舊的事件記錄檔中。|
|/ab:\<自動 >|指定記錄檔的自動備份原則。 \<自動 > 可以是 true 或 false。 如果此值為 true，記錄檔將會自動備份到達大小上限時。 如果此值為 true，保留期 (與指定 **/rt**選項) 也必須設定為 true。|
|/ms:\<MaxSize>|設定記錄檔的大小上限，以位元組為單位。 記錄大小下限為 1048576 個位元組 (1024 KB) 和記錄檔一律是 64 kb 的倍數，讓您輸入的值會隨之四捨五入。|
|/l:\<層級 >|定義記錄層級篩選。 \<層級 > 可以是任何有效的層級值。 此選項只適用於使用專用的工作階段記錄檔。 您可以移除層級篩選，藉由設定<Level>設為 0。|
|/k:\<關鍵字 >|指定記錄檔的關鍵字篩選器。 \<關鍵字 > 可以是任何有效的 64 位元的關鍵字遮罩。 此選項只適用於使用專用的工作階段記錄檔。|
|/ca:\<通道 >|設定事件記錄檔的存取權限。 \<通道 > 已使用 Security Descriptor Definition Language (SDDL) 的安全性描述元。 如需 SDDL 格式的詳細資訊，請參閱 Microsoft Developers Network (MSDN) 網站 (https://msdn.microsoft.com)。|
|/c:\<設定 >|指定組態檔的路徑。 這個選項就會讀取組態檔中定義的記錄檔屬性\<組態 >。 如果您使用此選項，您必須指定<Logname>參數。 記錄檔名稱會讀取組態檔。|
|/ge:\<中繼資料 >|取得可以由這個發行者引發事件的中繼資料資訊。 \<中繼資料 > 可以是 true 或 false。|
|/gm:\<訊息 >|顯示實際的訊息，而不是數字的訊息識別碼。 \<訊息 > 可以是 true 或 false。|
|/lf:\<記錄檔 >|指定從記錄檔，或從記錄檔應該讀取的事件。 \<記錄檔 > 可以是 true 或 false。 如果為 true，則命令參數會是記錄檔的路徑。|
|/ sq:\<Structquery >|指定應該取得與結構化查詢的事件。 \<Structquery > 可以是 true 或 false。 如果為 true，<Path>是包含結構化的查詢的檔案的路徑。|
|/q:\<查詢 >|定義 XPath 查詢來篩選所讀取或匯出的事件。 如果未指定此選項，將會傳回所有事件，或匯出。 此選項時，不提供 **/sq**為 true。|
|/bm:\<書籤 >|指定要包含一個查詢的書籤的檔案路徑。|
|/sbm:\<Savebm >|指定用來儲存此查詢的書籤檔案的路徑。 檔案名稱的副檔名應該是.xml。|
|/rd:\<方向 >|指定事件已讀取的方向。 \<方向 > 可以是 true 或 false。 如果為 true，會先傳回最新的事件。|
|/l:\<地區設定 >|定義用來列印事件文字中的特定地區設定的地區設定字串。 僅適用於列印中使用的文字格式的事件時 **/f**選項。|
|/c:\<計數 >|設定要讀取的事件數目上限。|
|/e\<項目 >|顯示在 XML 中的事件時，請包含根項目。 \<項目 > 是您想在根項目中的字串。 例如， **/e:root**會導致 XML，其中包含根項目配對\<根 ></root>。|
|/ ow:\<覆寫 >|指定應該覆寫的匯出檔案。 \<覆寫 > 可以是 true 或 false。 如果為 true，並將匯出檔案中指定<Exportfile>已經存在，它將會覆寫而不進行確認。|
|/bu:\<備份 >|指定要儲存已清除的事件檔案的路徑。 包含備份的檔案名稱.evtx 副檔名。|
|/r:\<遠端 >|在遠端電腦上執行命令。 \<遠端 > 是遠端電腦的名稱。 **Im**並**um**參數不支援遠端作業。|
|您\<使用者名稱 >|指定不同的使用者登入遠端電腦。 \<使用者名稱 > 是在表單網域 \ 使用者或使用者的使用者名稱。 此選項才適用的時機 **/r**指定選項。|
|/p:\<密碼 >|指定使用者的密碼。 如果 **/u**選項使用，而且未指定此選項或\<密碼 > 是 「 * 」，將會提示使用者輸入密碼。 此選項才適用的時機 **/u**指定選項。|
|/a:\<驗證 >|定義連線到遠端電腦的驗證類型。 \<驗證 > 可以是預設值、 Negotiate、 Kerberos 或 NTLM。 預設值是 「 交涉 」。|
|/uni:\<Unicode >|以 Unicode，會顯示輸出。 \<Unicode > 可以是 true 或 false。 如果<Unicode>為 true，則輸出為 unicode 格式。|

## <a name="remarks"></a>備註

-   使用組態檔搭配 sl 參數

    組態檔是 XML 檔案的 wevtutil gl 輸出相同的格式與\<Logname > /f:xml。 下列範例顯示組態檔，可讓保留，可讓自動備份，並在應用程式記錄檔上設定最大記錄檔大小的格式：  
    ```
    <?xml version="1.0" encoding="UTF-8"?>
    <channel name="Application" isolation="Application"
    xmlns="https://schemas.microsoft.com/win/2004/08/events">
    <logging>
    <retention>true</retention>
    <autoBackup>true</autoBackup>
    <maxSize>9000000</maxSize>
    </logging>
    <publishing>
    </publishing>
    </channel>
    ```

## <a name="BKMK_examples"></a>範例

列出所有記錄檔的名稱：
```
wevtutil el
```
顯示在 XML 格式的本機電腦上的系統記錄檔的組態資訊：
```
wevtutil gl System /f:xml
```
使用組態檔來設定事件記錄檔屬性 （請參閱 < 備註 >，如需組態檔的範例）：
```
wevtutil sl /c:config.xml
```
顯示 Microsoft-Windows 事件記錄檔事件發行者，包括 「 發行者 」 可以引發的事件的相關中繼資料的相關資訊：
```
wevtutil gp Microsoft-Windows-Eventlog /ge:true
```
安裝從 myManifest.xml 資訊清單檔案的發行者和記錄檔：
```
wevtutil im myManifest.xml
```
從 myManifest.xml 資訊清單檔，解除安裝發行者和記錄檔：
```
wevtutil um myManifest.xml
```
以文字格式顯示三個最新的事件，應用程式記錄檔：
```
wevtutil qe Application /c:3 /rd:true /f:text
```
顯示應用程式記錄檔的狀態：
```
wevtutil gli Application 
```
從系統記錄檔匯出事件，以 C:\backup\system0506.evtx:
```
wevtutil epl System C:\backup\system0506.evtx
```
清除所有應用程式記錄檔事件之後將它們儲存至 C:\admin\backups\a10306.evtx:
```
wevtutil cl Application /bu:C:\admin\backups\a10306.evtx
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
