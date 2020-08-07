---
title: wecutil
description: Wecutil 的參考文章，可讓您建立及管理從遠端電腦轉送之事件的訂閱。
ms.topic: article
ms.assetid: 0c82a6cb-d652-429c-9c3d-0f568c78d54b
author: coreyp-at-msft
ms.author: coreyp
manager: dansimps
ms.openlocfilehash: be3c05bcb8122db0dddd1eea8823222786d58008
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87896524"
---
# <a name="wecutil"></a>wecutil



可讓您建立和管理從遠端電腦轉送之事件的訂閱。 遠端電腦必須支援 WS-MANAGEMENT 通訊協定。


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

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|{es \| 列舉-訂用帳戶}|顯示存在之所有遠端事件訂閱的名稱。|
|{gs \| 取得訂用帳戶} \<Subid>[/f： \<Format> ][/uni： \<Unicode> ]|顯示遠端訂閱設定資訊。 \<Subid>這是可唯一識別訂用帳戶的字串。 \<Subid>與 \<SubscriptionId> 用來建立訂閱的 XML 設定檔標記中所指定的字串相同。|
|{gr \| get-subscriptionruntimestatus} \<Subid>[ \<Eventsource> ...]|顯示訂用帳戶的執行時間狀態。 \<Subid>這是可唯一識別訂用帳戶的字串。 \<Subid>與 \<SubscriptionId> 用來建立訂閱的 XML 設定檔標記中所指定的字串相同。 \<Eventsource>這是一個字串，可識別做為事件來源的電腦。 \<Eventsource>應該是完整功能變數名稱、NetBIOS 名稱或 IP 位址。|
|{ss \| 設定-訂用帳戶} \<Subid>[/e： [ \<Subenabled> ]] [/esa： \<Address> ] [/ese： [ \<Srcenabled> ]] [/aes] [/res] [/un： \<Username> ] [/up： \<Password> ] [/d： \<Desc> ] [/uri： \<Uri> ] [/cm： \<Configmode> ] [/ex： \<Expires> ] [/q： \<Query> ] [/dia： \<Dialect> ] [/tn： \<Transportname> ] [/tp： \<Transportport> ] [/dm： \<Deliverymode> ] [/dmi：] [/dmlt： \<Deliverymax> \<Deliverytime> ] \<Heartbeat> \<Content> \<Locale> \<Readexist> \<Logfile> \<Publishername> \<Enableport> \<Hostname> \<Type> [/hi：] [/cf：] [：] [/ree： []] [/lf：] [/pn：]</br>或</br>{ss \| 設定-訂用帳戶/c： \<Configfile> [/cun： \<Comusername> /cup： \<Compassword> ]|變更訂用帳戶設定。 您可以指定訂用帳戶識別碼和適當的選項來變更訂用帳戶參數，或者您可以指定 XML 設定檔案來變更訂用帳戶參數。|
|{cs \| 建立訂閱} \<Configfile>[/cun： \<Username> /cup： \<Password> ]|建立遠端訂閱。 \<Configfile>指定包含訂用帳戶設定之 XML 檔案的路徑。 路徑可以是目前目錄的絕對或相對路徑。|
|{ds \| 刪除-訂用帳戶}\<Subid>|刪除訂用帳戶，並從將事件傳遞至該訂用帳戶的事件記錄檔的所有事件來源取消訂閱。 已收到並記錄的任何事件都不會刪除。 \<Subid>這是可唯一識別訂用帳戶的字串。 \<Subid>與 \<SubscriptionId> 用來建立訂閱的 XML 設定檔標記中所指定的字串相同。|
|{rs \| 重試-訂用帳戶} \<Subid>[ \<Eventsource> ...]|重試建立連線，並將遠端訂閱要求傳送至非作用中的訂用帳戶。 嘗試重新開機所有事件來源或指定的事件來源。 不會重試已停用的來源。 \<Subid>這是可唯一識別訂用帳戶的字串。 \<Subid>與 \<SubscriptionId> 用來建立訂閱的 XML 設定檔標記中所指定的字串相同。 \<Eventsource>這是一個字串，可識別做為事件來源的電腦。 \<Eventsource>應該是完整功能變數名稱、NetBIOS 名稱或 IP 位址。|
|{qc \| 快速 config} [/q： [ \<Quiet> ]]|設定 Windows 事件收集器服務，以確保可以透過重新開機來建立和持續訂閱。 這包括下列步驟：</br>1. 啟用 ForwardedEvents 通道（如果已停用）。</br>2. 將 Windows 事件收集器服務設定為延遲啟動。</br>3. 啟動 Windows 事件收集器服務（如果未執行）。|

## <a name="options"></a>選項。

|選項|描述|
|------|-----------|
|/f\<Format>|指定所顯示資訊的格式。 \<Format>可以是 XML 或簡易。 如果 <Format> 是 xml，則輸出會以 xml 格式顯示。 如果 \<Format> 為簡易，則輸出會以名稱/值配對顯示。 預設值為簡易。|
|/c\<Configfile>|指定包含訂用帳戶設定之 XML 檔案的路徑。 路徑可以是目前目錄的絕對或相對路徑。 這個選項只能搭配 **/cun**和 **/cup**選項使用，而且與其他所有選項互斥。|
|/e： [ \<Subenabled> ]|啟用或停用訂用帳戶。 \<Subenabled>可以是 true 或 false。 此選項的預設值為 true。|
|esa\<Address>|指定事件來源的位址。 \<Address>這是包含完整功能變數名稱、NetBIOS 名稱或 IP 位址的字串，其可識別作為事件來源的電腦。 此選項應該與 **/ese**、 **/aes**、 **/res**或 **/un**和 **/up**選項搭配使用。|
|/ese： [ \<Srcenabled> ]|啟用或停用事件來源。 \<Srcenabled>可以是 true 或 false。 只有在指定 **/esa**選項時，才能使用此選項。 此選項的預設值為 true。|
|/aes|加入 **/esa**選項指定的事件來源（如果它還不是訂用帳戶的一部分）。 如果 **/esa**選項指定的位址已經是訂用帳戶的一部分，則會報告錯誤。 只有在指定 **/esa**選項時，才允許使用這個選項。|
|/res|移除 **/esa**選項指定的事件來源（如果它已經是訂用帳戶的一部分）。 如果 **/esa**選項指定的位址不是訂用帳戶的一部分，則會報告錯誤。 只有在指定 **/esa**選項時，才允許使用此選項。|
|取消\<Username>|指定要搭配 **/esa**選項指定的事件來源使用的使用者認證。 只有在指定 **/esa**選項時，才允許使用這個選項。|
|/up\<Password>|指定對應于使用者認證的密碼。 只有在指定 **/un**選項時，才允許使用這個選項。|
|/d\<Desc>|提供訂用帳戶的描述。|
|uri\<Uri>|指定訂用帳戶所使用的事件種類。 \<Uri>包含 URI 字串，其結合了事件來源電腦的位址，以唯一識別事件的來源。 URI 字串會用於訂用帳戶中的所有事件來源位址。|
|/cm\<Configmode>|設定設定模式。 \<Configmode>可以是下列其中一個字串： Normal、Custom、MinLatency 或 MinBandwidth。 [一般]、[MinLatency] 和 [MinBandwidth] 模式會設定傳遞模式、傳遞最大專案數、[心跳間隔] 和 [最大延遲時間]。 只有在設定模式設為 [自訂] 時，才可以指定 **/dm**、 **/dmi**、 **/hi**或 **/dmlt**選項。|
|ex\<Expires>|設定訂閱到期的時間。 \<Expires>應該以標準 XML 或 ISO8601 日期-時間格式定義： yyyy-mm-dd-ddThh： MM： ss [. sss] [Z]，其中 T 是時間分隔符號，而 Z 則表示 UTC 時間。|
|一起\<Query>|指定訂用帳戶的查詢字串。 \<Query>不同 URI 值的格式可能會不同，而且會套用至訂用帳戶中的所有來源。|
|dia\<Dialect>|定義查詢字串所使用的方言。|
|/tn\<Transportname>|指定用來連接到遠端事件來源的傳輸名稱。|
|/tp\<Transportport>|設定連接到遠端事件來源時，傳輸所使用的通訊埠編號。|
|等\<Deliverymode>|指定傳遞模式。 \<Deliverymode>可以是提取或推送。 只有當 **/cm**選項設定為 [自訂] 時，這個選項才有效。|
|dmi\<Deliverymax>|設定批次傳遞的最大專案數。 只有當 **/cm**設定為 Custom 時，這個選項才有效。|
|/dmlt:\<Deliverytime>|設定傳遞事件批次的最大延遲。 \<Deliverytime>這是毫秒數。 只有當 **/cm**設定為 Custom 時，這個選項才有效。|
|hi\<Heartbeat>|定義 [心跳間隔]。 \<Heartbeat>這是毫秒數。 只有當 **/cm**設定為 Custom 時，這個選項才有效。|
|cf\<Content>|指定所傳回事件的格式。 \<Content>可以是事件或 RenderedText。 當此值為 RenderedText 時，會以當地語系化的 (字串傳回事件，例如事件描述) 附加至事件。 預設值為 RenderedText。|
|/l:\<Locale>|指定以 RenderedText 格式傳遞當地語系化字串的地區設定。 \<Locale>是語言和國家/地區識別碼，例如 EN-US。 只有當 **/cf**選項設定為 RenderedText 時，這個選項才有效。|
|/ree： [ \<Readexist> ]|識別針對訂用帳戶傳遞的事件。 \<Readexist>可以是 true 或 false。 當 <Readexist> 為 true 時，就會從訂用帳戶事件來源讀取所有現有的事件。 當 <Readexist> 為 false 時，只會傳遞未來 (到達) 事件的時間。 如果沒有值， **/ree**選項的預設值為 true。 如果未指定 **/ree**選項，則預設值為 false。|
|分行符號\<Logfile>|指定用來儲存從事件來源接收之事件的本機事件記錄檔。|
|pn\<Publishername>|指定發行者名稱。 它必須是擁有或匯入 **/lf**選項所指定之記錄檔的發行者。|
|/essp:\<Enableport>|指定必須將通訊埠編號附加至遠端服務的服務主體名稱。 \<Enableport>可以是 true 或 false。 當為 true 時，會附加埠號碼 <Enableport> 。 附加通訊埠編號時，可能需要進行一些設定，以防止存取事件來源遭到拒絕。|
|hn\<Hostname>|指定本機電腦的 DNS 名稱。 這個名稱是由遠端事件來源用來推送事件，而且必須僅用於發送訂閱。|
|/ct\<Type>|設定遠端來源存取的認證類型。 \<Type>應該是下列其中一個值： default、negotiate、digest、basic 或 localmachine。 預設值為 default。|
|/cun:\<Comusername>|設定共用使用者認證，以用於沒有自己的使用者認證的事件來源。 如果使用 **/c**選項指定此選項，則會忽略設定檔中個別事件來源的 [使用者名稱] 和 [UserPassword] 設定。 如果您想要針對特定事件來源使用不同的認證，您應該在另一個**ss**命令的命令列上指定特定事件來源的 **/un**和 **/up**選項，以覆寫此值。|
|杯\<Compassword>|設定共用使用者認證的使用者密碼。 當 \<Compassword> 設定為 * (星號) 時，就會從主控台讀取密碼。 只有在指定 **/cun**選項時，這個選項才有效。|
|/q： [ \<Quiet> ]|指定設定程式是否會提示您確認。 \<Quiet>可以是 true 或 false。 如果 <Quiet> 為 true，則設定程式不會提示您進行確認。 此選項的預設值為 false。|

## <a name="remarks"></a>備註

> [!IMPORTANT]
> 如果您收到訊息「RPC 伺服器無法使用嗎？當您嘗試執行 wecutil 時，您必須啟動 Windows 事件收集器服務， (wecsvc) 。 若要開始 wecsvc，請在提升許可權的命令提示字元中輸入 net start wecsvc。

- 若要顯示設定檔的內容：
  ```
  <Subscription xmlns=https://schemas.microsoft.com/2006/03/windows/events/subscription>
  <Uri>https://schemas.microsoft.com/wbem/wsman/1/windows/EventLog</Uri>
  <!-- Use Normal (default), Custom, MinLatency, MinBandwidth -->
  <ConfigurationMode>Normal</ConfigurationMode>
  <Description>Forward Sample Subscription</Description>
  <SubscriptionId>SampleSubscription</SubscriptionId>
  <Query><![CDATA[
  <QueryList>
  <Query Path=Application>
  <Select>*</Select>
  </Query>
  </QueryList>
  ]]></Query>
  <EventSources>
  <EventSource Enabled=true>
  <Address>mySource.myDomain.com</Address>
  <UserName>myUserName</UserName>
  <Password>*</Password>
  </EventSource>
  </EventSources>
  <CredentialsType>Default</CredentialsType>
  <Locale Language=EN-US></Locale>
  </Subscription>
  ```

## <a name="examples"></a>範例

名為 sub1 之訂用帳戶的輸出設定資訊：
```
wecutil gs sub1
```
範例輸出︰
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
顯示名為 sub1 之訂用帳戶的執行時間狀態：
```
wecutil gr sub1
```
從稱為 WsSelRg2.xml 的新 XML 檔案，更新名為 sub1 的訂用帳戶設定：
```
wecutil ss sub1 /c:%Windir%\system32\WsSelRg2.xml
```
使用多個參數來更新名為 sub2 的訂用帳戶設定：
```
wecutil ss sub2 /esa:myComputer /ese /un:uname /up:* /cm:Normal
```
建立訂用帳戶，以便從遠端電腦的 Windows Vista 應用程式事件記錄檔將事件轉寄至 ForwardedEvents 記錄 (請參閱備註以取得設定檔案) 的範例：
```
wecutil cs subscription.xml
```
刪除名為 sub1 的訂用帳戶：
```
wecutil ds sub1
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
