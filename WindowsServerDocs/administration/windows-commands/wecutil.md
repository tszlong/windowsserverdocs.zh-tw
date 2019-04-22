---
title: wecutil
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0c82a6cb-d652-429c-9c3d-0f568c78d54b
author: coreyp-at-msft
ms.author: coreyp
manager: dansimps
ms.openlocfilehash: 50151a322f1ccde2be927de31b0e5c30732b278e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813949"
---
# <a name="wecutil"></a>wecutil



可讓您建立和管理訂用帳戶會轉送的事件，從遠端電腦。 在遠端電腦必須支援 Ws-management 通訊協定。 如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。


## <a name="syntax"></a>語法

```
wecutil  [{es | enum-subscription}] 
[{gs | get-subscription} <Subid> [/f:<Format>] [/uni:<Unicode>]] 
[{gr | get-subscriptionruntimestatus} <Subid> [<Eventsource> …]] 
[{ss | set-subscription} [<Subid> [/e:[<Subenabled>]] [/esa:<Address>] [/ese:[<Srcenabled>]] [/aes] [/res] [/un:<Username>] [/up:<Password>] [/d:<Desc>] [/uri:<Uri>] [/cm:<Configmode>] [/ex:<Expires>] [/q:<Query>] [/dia:<Dialect>] [/tn:<Transportname>] [/tp:<Transportport>] [/dm:<Deliverymode>] [/dmi:<Deliverymax>] [/dmlt:<Deliverytime>] [/hi:<Heartbeat>] [/cf:<Content>] [/l:<Locale>] [/ree:[<Readexist>]] [/lf:<Logfile>] [/pn:<Publishername>] [/essp:<Enableport>] [/hn:<Hostname>] [/ct:<Type>]] [/c:<Configfile> [/cun:<Username> /cup:<Password>]]] 
[{cs | create-subscription} <Configfile> [/cun:<Username> /cup:<Password>]] 
[{ds | delete-subscription} <Subid>] 
[{rs | retry-subscription} <Subid> [<Eventsource>…]] 
[{qc | quick-config} [/q:[<Quiet>]]].
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|{es\|列舉訂用帳戶}|顯示所有存在的遠端事件訂閱的名稱。|
|{gs\|取得訂用帳戶} \<Subid > [/ f:\<格式 >] [/ uni:\<Unicode >]|顯示遠端的訂用帳戶設定資訊。 \<Subid > 是一個字串，可唯一識別訂用帳戶。 \<Subid > 中指定的字串相同\<SubscriptionId > 標記的 XML 組態檔，用來建立訂用帳戶。|
|{gr \| get subscriptionruntimestatus} \<Subid > [\<Eventsource >...]|顯示訂用帳戶的執行階段狀態。 \<Subid > 是一個字串，可唯一識別訂用帳戶。 \<Subid > 中指定的字串相同\<SubscriptionId > 標記的 XML 組態檔，用來建立訂用帳戶。 \<Eventsource > 是一個字串，識別做為事件來源的電腦。 \<Eventsource > 應該是完整的網域名稱、 NetBIOS 名稱或 IP 位址。|
|{ss\|設定訂用帳戶} \<Subid > [/ e: [\<Subenabled >]] [/ esa:\<位址 >] [/ ese: [\<Srcenabled >]] [/aes] [/res] [/ 取消：\<使用者名稱 >] [/ 最多：\<密碼 >] [/ d:\<Desc >] [/ uri:\<Uri >] [/ cm:\<Configmode >] [/ ex:\<Expires >] [/ 問\<查詢 >] [/ dia:\<方言 >] [/ tn:\<Transportname >] [/ tp:\<Transportport >] [/ dm:\<Deliverymode >] [/ dmi:\<Deliverymax >] [/ dmlt:\<Deliverytime >] [/ 大家好：\<活動訊號 >] [/ cf:\<內容 >] [/ l\<地區設定 >] [/ ree: [\<Readexist >]] [/ lf:\<記錄檔 >] [/ pn:\<Publishername >] [/ essp:\<Enableport >] [/ hn:\<主機名稱 >] [/ ct:\<類型 >]</br>或</br>{ss\|設定訂用帳戶包裝\<Configfile > [/ 寸 （中國):\<Comusername >/cup:\<Compassword >]|變更訂用帳戶設定。 您可以指定訂用帳戶 ID 和適當的選項來變更訂用帳戶的參數，或您可以指定 XML 組態檔來變更訂用帳戶的參數。|
|{cs\|建立訂用帳戶} \<Configfile > [/ 寸 （中國):\<使用者名稱 >/cup:\<密碼 >]|建立遠端的訂用帳戶。 \<Configfile > 指定包含訂用帳戶組態的 XML 檔案的路徑。 路徑可以是絕對的或相對於目前的目錄。|
|{ds\|刪除訂用帳戶} \<Subid >|刪除訂用帳戶，並將事件傳遞至事件記錄檔的訂用帳戶的所有事件來源取消都訂閱。 不會刪除已收到並記錄任何事件。 \<Subid > 是一個字串，可唯一識別訂用帳戶。 \<Subid > 中指定的字串相同\<SubscriptionId > 標記的 XML 組態檔，用來建立訂用帳戶。|
|{rs\|重試訂用帳戶} \<Subid > [\<Eventsource >...]|重試建立連線，並將遠端的訂用帳戶要求傳送至非使用中的訂用帳戶。 嘗試重新啟動所有的事件來源，或指定事件來源。 已停用的來源不會重試。 \<Subid > 是一個字串，可唯一識別訂用帳戶。 \<Subid > 中指定的字串相同\<SubscriptionId > 標記的 XML 組態檔，用來建立訂用帳戶。 \<Eventsource > 是一個字串，識別做為事件來源的電腦。 \<Eventsource > 應該是完整的網域名稱、 NetBIOS 名稱或 IP 位址。|
|{qc \| quick-config} [/q:[\<Quiet>]]|設定 Windows Event Collector 服務，以確保訂用帳戶可以建立並持續透過重新開機。 這包括下列步驟：</br>1.如果已停用，請啟用 ForwardedEvents 通道。</br>2.設定 Windows Event Collector 服務延遲開始時間。</br>3.如果未執行，請啟動 Windows Event Collector 服務。|

## <a name="options"></a>選項。

|選項|描述|
|------|-----------|
|/f:\<格式 >|指定的格式顯示的資訊。 \<格式 > 可以是 XML 或 Terse。 如果<Format>為 XML，輸出會顯示在 XML 格式。 如果\<格式 > 是 Terse，輸出會顯示在名稱 / 值組。 預設為 Terse。|
|/c:\<Configfile >|指定包含訂用帳戶組態的 XML 檔案的路徑。 路徑可以是絕對的或相對於目前的目錄。 此選項僅能搭配 **/cun**並 **/cup**選項，並與其他所有選項互斥。|
|/e:[\<Subenabled>]|啟用或停用訂用帳戶。 \<Subenabled > 可以是 true 或 false。 此選項的預設值為 true。|
|/esa:\<位址 >|指定事件來源的位址。 \<地址 > 是一個字串，包含完整的網域名稱、 NetBIOS 名稱或 IP 位址，用來識別可做為事件來源的電腦。 此選項必須搭配 **/ese**， **/aes**， **/res**，或 **/un**並 **/up**選項。|
|/ese:[\<Srcenabled>]|啟用或停用的事件來源。 \<Srcenabled > 可以是 true 或 false。 此選項只允許 **/esa**指定選項。 此選項的預設值為 true。|
|/aes|將所指定的事件來源 **/esa**選項如果尚不屬於訂用帳戶。 如果所指定的地址 **/esa**選項已經訂用帳戶的一部分，則會報告錯誤。 如果此選項只允許 **/esa**指定選項。|
|/res|移除所指定的事件來源 **/esa**選項，如果它已經是訂用帳戶的一部分。 如果所指定的地址 **/esa**選項不屬於訂用帳戶，則會報告錯誤。 如果此選項只允許 **/esa**指定選項。|
|/un:\<使用者名稱 >|指定使用者的認證，使用指定的事件來源 **/esa**選項。 如果此選項只允許 **/esa**指定選項。|
|向上 /:\<密碼 >|指定對應至的使用者認證的密碼。 如果此選項只允許 **/un**指定選項。|
|/d:\<Desc >|提供的訂用帳戶的描述。|
|/uri:\<Uri >|指定所使用的訂用帳戶的事件類型。 \<Uri > 包含 URI 的字串，並結合了事件的來源電腦，以唯一識別事件的來源位址。 URI 字串用於訂用帳戶中的所有事件來源位址。|
|/cm:\<Configmode>|設定組態模式。 \<Configmode > 可以是下列字串之一：Normal、 自訂、 MinLatency 或 MinBandwidth。 標準、 MinLatency 和 MinBandwidth 模式設定傳遞模式，傳遞最大項目、 活動訊號間隔時間和傳遞最大延遲時間。 **/Dm**， **/dmi**， **/hi**或是 **/dmlt**選項只能指定設定模式，是否要設定為 Custom。|
|/ ex:\<到期 >|設定訂用帳戶的到期的時間。 \<到期 > 應定義於標準的 XML 或 ISO8601 日期時間格式： yyyy-MM-Yyyy-mm-ddthh [.sss] [Z]，其中 T 是時間分隔符號，而 Z 表示 UTC 時間。|
|/q:\<查詢 >|指定訂用帳戶的查詢字串。 格式\<查詢 > 可能會不同，不同的 URI 值，並套用至訂用帳戶中的所有來源。|
|/dia:\<方言 >|定義查詢字串使用的方言。|
|/tn:\<Transportname >|指定傳輸用來連接到遠端事件來源的名稱。|
|/tp:\<Transportport >|設定連接到遠端事件來源時，會將傳輸所使用的連接埠號碼。|
|/dm:\<Deliverymode >|指定的傳遞模式。 \<Deliverymode > 可以是 pull 或 push。 此選項只適用於如果 **/cm**選項設定為 Custom。|
|/dmi:\<Deliverymax >|設定批次傳遞的項目數目上限。 此選項只適用於如果 **/cm**設定為 Custom。|
|/dmlt:\<Deliverytime >|在傳遞的事件批次中設定的最大延遲。 \<Deliverytime > 是的毫秒數。 此選項只適用於如果 **/cm**設定為 Custom。|
|/ 大家好：\<活動訊號 >|定義活動訊號間隔時間。 \<活動訊號 > 是的毫秒數。 此選項只適用於如果 **/cm**設定為 Custom。|
|/cf:\<內容 >|指定傳回的事件格式。 \<內容 > 可以是事件或 RenderedText。 RenderedText 值時，事件會傳回與附加至事件的當地語系化字串 （例如事件描述）。 預設值是 RenderedText。|
|/l:\<地區設定 >|RenderedText 格式指定當地語系化的字串傳遞的地區設定。 \<地區設定 > 是語言和國家/地區的識別碼，例如 「 EN-US-我們"。 此選項只適用於如果 **/cf**選項設定為 RenderedText。|
|/ree:[\<Readexist>]|識別訂用帳戶傳遞的事件。 \<Readexist > 可以為 true 或 false。 當<Readexist>為 true，從訂用帳戶事件來源會讀取所有現有的事件。 當<Readexist>是 false，未來的 （抵達） 事件會傳遞。 預設值是適用於 **/ree**不含值的選項。 如果沒有 **/ree**指定選項時，預設值為 false。|
|/lf:\<記錄檔 >|指定用來儲存接收自事件來源的事件的本機事件記錄檔。|
|/pn:\<Publishername>|指定發行者名稱。 它必須是擁有或匯入所指定的記錄檔的發行者 **/lf**選項。|
|/essp:\<Enableport >|指定的連接埠號碼必須附加至遠端服務的服務主體名稱。 \<Enableport > 可以是 true 或 false。 連接埠編號附加時<Enableport>為 true。 附加連接埠號碼，某些設定可能必須防止從遭拒的存取權的事件來源中。|
|/hn:\<主機名稱 >|指定本機電腦的 DNS 名稱。 此名稱可由遠端事件來源來推送回事件，而且必須僅用於發送訂閱。|
|/ct:\<類型 >|設定遠端來源存取的認證類型。 \<型別 > 應該是下列值之一： 預設、 negotiate、 digest、 basic 或 localmachine。 預設值是預設值。|
|/cun:\<Comusername >|設定共用的使用者認證，以便用於沒有自己的使用者認證的事件來源。 如果指定此選項，但 **/c**選項，則使用者名稱和 UserPassword 設定的組態檔中的個別事件來源會被忽略。 如果您想要使用特定的事件來源的不同的認證，您應該指定覆寫此值 **/un**並 **/up**選項特定的事件來源在命令列上的另一個**ss**命令。|
|/ cup:\<Compassword >|設定對於共用的使用者認證的使用者密碼。 當\<Compassword > 設為 * （星號），從主控台讀取密碼。 此選項時才有效 **/cun**指定選項。|
|/q:[\<Quiet>]|指定是否設定程序會提示您確認。 \<無訊息 > 可以是 true 或 false。 如果<Quiet>為 true，組態程序不會提示確認。 此選項的預設值為 false。|

## <a name="remarks"></a>備註

> [!IMPORTANT]
> 如果您收到的訊息，「 RPC 伺服器無法使用？當您嘗試執行 wecutil，您需要啟動 Windows Event Collector 服務 (wecsvc)。 若要啟動 wecsvc，請在提升權限的命令提示字元，輸入 net 啟動 wecsvc。

-   下列範例顯示組態檔的內容：  
    ```
    <Subscription xmlns="https://schemas.microsoft.com/2006/03/windows/events/subscription">
    <Uri>https://schemas.microsoft.com/wbem/wsman/1/windows/EventLog</Uri>
    <!-- Use Normal (default), Custom, MinLatency, MinBandwidth -->
    <ConfigurationMode>Normal</ConfigurationMode>
    <Description>Forward Sample Subscription</Description>
    <SubscriptionId>SampleSubscription</SubscriptionId>
    <Query><![CDATA[
    <QueryList>
    <Query Path="Application">
    <Select>*</Select>
    </Query>
    </QueryList>
    ]]></Query>
    <EventSources>
    <EventSource Enabled="true">
    <Address>mySource.myDomain.com</Address>
    <UserName>myUserName</UserName>
    <Password>*</Password>
    </EventSource>
    </EventSources>
    <CredentialsType>Default</CredentialsType>
    <Locale Language="EN-US"></Locale>
    </Subscription>
    ```

## <a name="BKMK_examples"></a>範例

輸出 sub1 訂用帳戶的組態資訊：
```
wecutil gs sub1
```
輸出範例：
```
EventSource[0]:
Address: localhost
Enabled: true
Description: Subscription 1
Uri: wsman:microsoft/logrecord/sel
DeliveryMode: pull
DeliveryMaxSize: 16000
DeliveryMaxItems: 15
DeliveryMaxLatencyTime: 1000
HeartbeatInterval: 10000
Locale:
ContentFormat: renderedtext
LogFile: HardwareEvents
```
顯示 sub1 訂用帳戶的執行階段狀態：
```
wecutil gr sub1
```
更新名為從新的 XML 檔案，稱為 WsSelRg2.xml sub1 訂用帳戶設定：
```
wecutil ss sub1 /c:%Windir%\system32\WsSelRg2.xml
```
更新訂用帳戶設定名為 sub2 有多個參數：
```
wecutil ss sub2 /esa:myComputer /ese /un:uname /up:* /cm:Normal
```
建立訂用帳戶從 Windows Vista 應用程式事件記錄檔，在 mySource.myDomain.com 的遠端電腦的事件轉送至 ForwardedEvents 記錄檔 （請參閱 < 備註 >，如需組態檔的範例）：
```
wecutil cs subscription.xml
```
刪除名為 sub1 訂用帳戶：
```
wecutil ds sub1
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
