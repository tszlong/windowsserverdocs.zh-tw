---
title: wecutil
description: '>wecutil 命令的參考文章，可讓您建立和管理從遠端電腦轉送之事件的訂閱。'
ms.topic: reference
ms.assetid: 0c82a6cb-d652-429c-9c3d-0f568c78d54b
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 12/04/2020
ms.openlocfilehash: b6068135d8a097e6e849ec2e2234a76accd50bbf
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97947284"
---
# <a name="wecutil"></a>wecutil

可讓您建立和管理從遠端電腦轉送之事件的訂閱。 遠端電腦必須支援 WS-Management 的通訊協定。

> [!IMPORTANT]
> 如果您收到訊息「RPC 伺服器無法使用嗎？當您嘗試執行 >wecutil 時，您需要啟動 Windows 事件收集器服務 (wecsvc) 。 若要開始 wecsvc，請在提升許可權的命令提示字元中輸入 `net start wecsvc` 。

## <a name="syntax"></a>語法

```command
wecutil [{es | enum-subscription}] [{gs | get-subscription} <Subid> [/f:<Format>] [/uni:<Unicode>]] [{gr | get-subscriptionruntimestatus} <Subid> [<Eventsource> …]] [{ss | set-subscription} [<Subid> [/e:[<Subenabled>]] [/esa:<Address>] [/ese:[<Srcenabled>]] [/aes] [/res] [/un:<Username>] [/up:<Password>] [/d:<Desc>] [/uri:<Uri>] [/cm:<Configmode>] [/ex:<Expires>] [/q:<Query>] [/dia:<Dialect>] [/tn:<Transportname>] [/tp:<Transportport>] [/dm:<Deliverymode>] [/dmi:<Deliverymax>] [/dmlt:<Deliverytime>] [/hi:<Heartbeat>] [/cf:<Content>] [/l:<Locale>] [/ree:[<Readexist>]] [/lf:<Logfile>] [/pn:<Publishername>] [/essp:<Enableport>] [/hn:<Hostname>] [/ct:<Type>]] [/c:<Configfile> [/cun:<Username> /cup:<Password>]]] [{cs | create-subscription} <Configfile> [/cun:<Username> /cup:<Password>]] [{ds | delete-subscription} <Subid>] [{rs | retry-subscription} <Subid> [<Eventsource>…]] [{qc | quick-config} [/q:[<quiet>]]]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| `{es | enum-subscription}` | 顯示存在的所有遠端事件訂閱的名稱。 |
| `{gs | get-subscription} <Subid> [/f:<Format>] [/uni:<Unicode>]` | 顯示遠端訂用帳戶設定資訊。 `<Subid>` 這是可唯一識別訂閱的字串。 這與 `<SubscriptionId>` 用來建立訂閱之 XML 設定檔的標記中所指定的字串相同。 |
| `{gr | get-subscriptionruntimestatus} <Subid> [<Eventsource> …]` | 顯示訂用帳戶的執行時間狀態。 `<Subid>` 這是可唯一識別訂閱的字串。 這與 `<SubscriptionId>` 用來建立訂閱之 XML 設定檔的標記中所指定的字串相同。 `<Eventsource>` 這是識別作為事件來源之電腦的字串。 它應該是完整功能變數名稱、NetBIOS 名稱或 IP 位址。 |
| `{ss | set-subscription} <Subid> [/e:[<Subenabled>]] [/esa:<Address>] [/ese:[<Srcenabled>]] [/aes] [/res] [/un:<Username>] [/up:<Password>] [/d:<Desc>] [/uri:<Uri>] [/cm:<Configmode>] [/ex:<Expires>] [/q:<Query>] [/dia:<Dialect>] [/tn:<Transportname>] [/tp:<Transportport>] [/dm:<Deliverymode>] [/dmi:<Deliverymax>] [/dmlt:<Deliverytime>] [/hi:<Heartbeat>] [/cf:<Content>] [/l:<Locale>] [/ree:[<Readexist>]] [/lf:<Logfile>] [/pn:<Publishername>] [/essp:<Enableport>] [/hn:<Hostname>] [/ct:<Type>]` <br>**OR**<br>`{ss | set-subscription /c:<Configfile> [/cun:<Comusername> /cup:<Compassword>]` | 變更訂用帳戶設定。 您可以指定訂用帳戶識別碼和適當的選項來變更訂用帳戶參數，也可以指定 XML 設定檔來變更訂用帳戶參數。 |
| `{cs | create-subscription} <Configfile> [/cun:<Username> /cup:<Password>]` | 建立遠端訂用帳戶。 `<Configfile>` 指定包含訂用帳戶設定之 XML 檔案的路徑。 路徑可以是目前目錄的絕對路徑或相對路徑。 |
| `{ds | delete-subscription} <Subid>` | 從傳遞事件至訂用帳戶事件記錄檔的所有事件來源刪除訂閱和取消訂閱。 系統不會刪除已接收和記錄的任何事件。 `<Subid>` 這是可唯一識別訂閱的字串。 這與 `<SubscriptionId>` 用來建立訂閱之 XML 設定檔的標記中所指定的字串相同。 |
| `{rs | retry-subscription} <Subid> [<Eventsource>…]` | 重試建立連線，並將遠端訂閱要求傳送至非使用中的訂用帳戶。 嘗試重新開機所有事件來源或指定的事件來源。 停用的來源不會重試。 `<Subid>` 這是可唯一識別訂閱的字串。 這與 `<SubscriptionId>` 用來建立訂閱之 XML 設定檔的標記中所指定的字串相同。 `<Eventsource>` 這是識別作為事件來源之電腦的字串。 它應該是完整功能變數名稱、NetBIOS 名稱或 IP 位址。 |
| `{qc | quick-config} [/q:[<Quiet>]]` | 設定 Windows 事件收集器服務，以確保可以在重新開機時建立和持續訂用帳戶。 這包括下列步驟：<ol><li>如果已停用，請啟用 ForwardedEvents 通道。</li><li>設定 Windows 事件收集器服務延遲啟動。</li><li>如果 Windows 事件收集器服務未執行，請加以啟動。</li></ol> |

#### <a name="options"></a>選項。

| 選項 | 描述 |
|--|--|
| /f`<Format>` | 指定所顯示之資訊的格式。 `<Format>` 可以是 XML 或簡易。 如果是 **xml**，則輸出會以 xml 格式顯示。 如果 **簡易**，輸出就會以名稱/值組顯示。 預設值為 **簡易**。 |
| /c`<Configfile>` | 指定包含訂用帳戶設定之 XML 檔案的路徑。 路徑可以是目前目錄的絕對路徑或相對路徑。 此選項只可搭配 **/cun** 和 **/cup** 選項使用，並與其他所有選項互斥。 |
| /e： [ `<Subenabled>` ] | 啟用或停用訂用帳戶。 `<Subenabled>` 可以是 true 或 false。 此選項的預設值為 **true**。 |
| /esa:`<Address>` | 指定事件來源的位址。 `<Address>` 這是一個字串，其中包含完整功能變數名稱、NetBIOS 名稱或 IP 位址，以識別作為事件來源的電腦。 此選項應與 **/ese**、 **/aes**、 **/res** 或 **/un** 和 **/up** 選項搭配使用。 |
| /ese： [ `<Srcenabled>` ] | 啟用或停用事件來源。 `<Srcenabled>` 可以是 true 或 false。 只有在指定 **/esa** 選項時，才允許這個選項。 此選項的預設值為 **true**。 |
| /aes | 新增 **/esa** 選項指定的事件來源（如果它還不是訂用帳戶的一部分）。 如果 **/esa** 選項指定的位址已是訂用帳戶的一部分，則會報告錯誤。 只有在指定 **/esa** 選項時，才允許這個選項。 |
| /res | 如果 **/esa** 選項已是訂用帳戶的一部分，則移除它所指定的事件來源。 如果 **/esa** 選項指定的位址不是訂用帳戶的一部分，則會報告錯誤。 只有在指定 **/esa** 選項時，才允許這個選項。 |
| 消除`<Username>` | 指定要搭配 **/esa** 選項所指定之事件來源使用的使用者認證。 只有在指定 **/esa** 選項時，才允許這個選項。 |
| /up`<Password>` | 指定對應至使用者認證的密碼。 只有在指定 **/un** 選項時，才允許這個選項。 |
| /d`<Desc>` | 提供訂用帳戶的描述。 |
| url`<Uri>` | 指定訂用帳戶所使用的事件種類。 `<Uri>` 包含 URI 字串，該字串會與事件來源電腦的位址合併，以唯一識別事件的來源。 URI 字串會用於訂用帳戶中的所有事件來源位址。 |
| /cm`<Configmode>` | 設定配置模式。 `<Configmode>` 可以是下列其中一個字串： **Normal**、 **Custom**、 **MinLatency** 或 **MinBandwidth**。 **Normal**、 **MinLatency** 和 **MinBandwidth** 模式會設定傳遞模式、傳遞最大專案數、心跳間隔時間和傳遞最大延遲時間。 只有在設定模式設定為 [**自訂**] 時，才能指定 **/dm**、 **/dmi**、 **/hi** 或 **/dmlt** 選項。 |
| 例如`<Expires>` | 設定訂閱到期的時間。 `<Expires>` 應以標準 XML 或 ISO8601 日期時間格式來定義： `yyyy-MM-ddThh:mm:ss[.sss][Z]` ，其中 *T* 是時間分隔符號，而 *Z* 表示 UTC 時間。 |
| 一起`<Query>` | 指定訂閱的查詢字串。 `<Query>`不同的 URI 值可能會有不同的格式，並套用至訂用帳戶中的所有來源。 |
| dia`<Dialect>` | 定義查詢字串使用的方言。 |
| /tn`<Transportname>` | 指定用來連接到遠端事件來源的傳輸名稱。 |
| /tp`<Transportport>` | 設定連接到遠端事件來源時，傳輸所使用的通訊埠編號。 |
| 等`<Deliverymode>` | 指定傳遞模式。 `<Deliverymode>` 可以是 pull 或 push。 只有在 **/cm** 選項設定為 [ **自訂**] 時，此選項才有效。 |
| dmi`<Deliverymax>` | 設定批次傳遞的最大專案數。 只有當 **/cm** 設定為 **Custom** 時，此選項才有效。 |
| /dmlt:`<Deliverytime>` | 設定傳遞事件批次的最大延遲。 `<Deliverytime>` 這是毫秒數。 只有當 **/cm** 設定為 Custom 時，此選項才有效。 |
| 你好`<Heartbeat>` | 定義信號間隔。 `<Heartbeat>` 這是毫秒數。 只有當 **/cm** 設定為 **Custom** 時，此選項才有效。 |
| cf`<Content>` | 指定傳回的事件格式。 `<Content>` 可以是事件或 RenderedText。 **RenderedText** 此值時，會以當地語系化 (字串傳回事件，例如事件描述) 附加至事件。 預設值為 **RenderedText**。 |
| /l:`<Locale>` | 指定用於傳遞 RenderedText 格式之當地語系化字串的地區設定。 `<Locale>` 是語言和國家/地區識別碼，例如 EN-US。 只有在 **/cf** 選項設定為 **RenderedText** 時，此選項才有效。 |
| /ree： [ `<Readexist>` ] | 識別針對訂用帳戶傳遞的事件。 `<Readexist>` 可以是 true 或 false。 當 `<Readexist>` 為 true 時，會從訂用帳戶事件來源讀取所有現有的事件。 當 `<Readexist>` 為 false 時，只會傳遞未來的 (抵達) 事件。 沒有值的 **/ree** 選項預設值為 **true** 。 如果未指定 **/ree** 選項，則預設值為 **false**。 |
| lf`<Logfile>` | 指定本機事件記錄檔，用來儲存從事件來源接收的事件。 |
| pn`<Publishername>` | 指定發行者名稱。 它必須是擁有或匯入 **/lf** 選項所指定之記錄檔的發行者。 |
| /essp:`<Enableport>` | 指定埠號碼必須附加至遠端服務的服務主體名稱。 `<Enableport>` 可以是 true 或 false。 當為 true 時，會附加埠號碼 `<Enableport>` 。 附加埠號碼時，可能需要進行某些設定，以防止存取事件來源遭到拒絕。 |
| hn`<Hostname>` | 指定本機電腦的 DNS 名稱。 遠端事件來源會使用這個名稱來回傳事件，而且必須只用于發送訂閱。 |
| /ct`<Type>` | 設定遠端存取來源的認證類型。 `<Type>` 應為下列其中一個值： **default**、 **negotiate**、 **digest**、 **basic** 或 **localmachine**。 預設值為 [ **預設** 值]。 |
| /cun:`<Comusername>` | 針對沒有自己的使用者認證的事件來源，設定要使用的共用使用者認證。 如果使用 **/c** 選項指定這個選項，則會忽略設定檔案中個別事件來源的使用者名稱和 UserPassword 設定。 如果您想要針對特定的事件來源使用不同的認證，您應該在另一個 **ss** 命令的命令列上指定特定事件來源的 **/un** 和 **/up** 選項，以覆寫此值。 |
| cup`<Compassword>` | 設定共用使用者認證的使用者密碼。 當 `<Compassword>` 設為 * (星號) 時，會從主控台讀取密碼。 只有在指定 **/cun** 選項時，這個選項才有效。 |
| /q： [ `<Quiet>` ] | 指定設定程式是否提示確認。 `<Quiet>` 可以是 true 或 false。 若 `<Quiet>` 為 true，則設定程式不會提示您進行確認。 此選項的預設值為 **false**。 |

## <a name="examples"></a>範例

若要顯示設定檔的內容，請輸入：

```XML
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
    </QueryList>]]
  </Query>
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

若要查看名為 *sub1* 之訂用帳戶的輸出設定資訊，請輸入：

```command
wecutil gs sub1
```

範例輸出︰

```output
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

若要顯示名為 sub1 之訂用帳戶的執行時間狀態，請輸入：

```command
wecutil gr sub1
```

若要從名為 *WsSelRg2.xml* 的新 XML 檔案更新名為 *sub1* 的訂用帳戶設定，請輸入：

```command
wecutil ss sub1 /c:%Windir%system32WsSelRg2.xml
```

若要使用多個參數更新名為 *sub2* 的訂用帳戶設定，請輸入：

```command
wecutil ss sub2 /esa:myComputer /ese /un:uname /up:* /cm:Normal
```

若要刪除名為 *sub1* 的訂用帳戶，請輸入：
```
wecutil ds sub1
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
