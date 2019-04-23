---
title: tracert
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9032a032-2e5e-49d4-9e86-f821600e4ba6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6a97c7656e646a22892eee5caa13d0163d05293d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837179"
---
# <a name="tracert"></a>tracert

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

判斷目的地所將網際網路控制訊息通訊協定 (ICMP) 回應要求或 ICMPv6 訊息傳送至含有以累加方式增加時間 (TTL) 欄位值的目的地路徑。 顯示的路徑是接近/側路由器路由器介面的來源主機與目的地之間路徑中的清單。 接近/側介面是路由器傳送的主機路徑中最接近的介面。 Tracert 會使用任何參數，顯示說明。   

## <a name="syntax"></a>語法  
```  
tracert [/d] [/h <MaximumHops>] [/j <Hostlist>] [/w <timeout>] [/R] [/S <Srcaddr>] [/4][/6] <TargetName>  
```  
### <a name="parameters"></a>參數  
|參數|描述|  
|-------|--------|  
|/d|防止**tracert**嘗試解析中繼路由器的 IP 位址，其名稱。 這可以加快的顯示器**tracert**結果。|  
|/h \<MaximumHops>|搜尋目標 （目的地） 的路徑中指定躍點的數目上限。 預設值為 30 的躍點。|  
|/j \<Hostlist>|指定會 echo 要求訊息中所指定的中繼目的地設定 IP 標頭中使用鬆散來源路由選項*Hostlist*。 鬆散來源路由中，連續的中繼目的地可以分隔一或多個路由器。 位址或主機清單中的名稱數目上限是 9。 *Hostlist*是以空格分隔的一系列的 IP 位址 （小數點十進位表示法）。 只有時追蹤 IPv4 位址，請使用此參數。|  
|/w \<timeout>|以毫秒為單位來等候 ICMP 時間已超過或回應對應至給定的 echo 要求訊息至接收回覆訊息中指定時間的量。 如果在逾時內未接收，則會顯示星號 （*）。 預設逾時值為 4000 （4 秒）。|  
|/R|指定的 IPv6 路由延伸模組標頭用來將 echo 要求訊息傳送至本機主機，做為中繼目的地使用目的地和測試的反向路由。|  
|/S \<Srcaddr>|指定要用於 「 回應要求訊息的來源位址。 追蹤 IPv6 位址時，才，請使用此參數。|  
|/4|指定這個 tracert.exe 可用於此追蹤的僅 IPv4。|  
|/6|指定這個 tracert.exe 可用於此追蹤的僅 IPv6。|  
|\<TargetName>|指定的目的地，識別由 IP 位址或主機名稱。|  
|/?|在命令提示字元顯示說明。|  

## <a name="remarks"></a>備註  
-   這項診斷工具判斷目的地所傳送 ICMP 回應要求訊息具有不同的時間 (TTL) 值到目的地的路徑。 沿著路徑的每個路由器所要遞減的 TTL，在 IP 封包中至少為 1 然後再將它轉送。 實際上，TTL 是最大連結的計數器。 封包上的 TTL 到達 0 時，路由器應該傳回 ICMP 時間已超過訊息的來源電腦。 tracert 會判斷傳送第一個回應要求訊息 ttl 為 1，並遞增在每個後續的傳輸，直到目標回應或躍點數目上限，以 1 TTL 為止的路徑。 躍點數目上限是預設值為 30，並使用指定 **/h**參數。 路徑是檢查中繼路由器和 「 回應回覆訊息傳回目的地所傳回的 ICMP 時間已超過訊息來判斷。 不過，有些路由器不會傳回具有過期的 TTL 值的封包的時間已超過訊息，而且是 invisile tracert 命令。 在此情況下，星號 （*） 的資料列會顯示該躍點。  
-   沿著路徑，並提供網路延遲和封包遺失，針對每個路由器和路徑中的連結，請使用**pathping**命令。  
-   此命令才會提供網際網路通訊協定 (TCP/IP) 通訊協定安裝為在 網路連線的網路介面卡的內容中的元件。  

## <a name="BKMK_Examples"></a>範例  
若要追蹤到名為 corp7.microsoft.com 主機路徑，請輸入：  
```  
tracert corp7.microsoft.com  
```  
若要追蹤到名為 corp7.microsoft.com 主機路徑，並防止其名稱的解析的每個 IP 位址，請輸入：  
```  
tracert /d corp7.microsoft.com  
```  
若要追蹤到名為 corp7.microsoft.com 主機路徑，並使用鬆散來源路由 10.12.0.1/10.29.3.1/10.1.44.1，請輸入：  
```  
tracert /j 10.12.0.1 10.29.3.1 10.1.44.1 corp7.microsoft.com  
```  
## <a name="additional-references"></a>其他參考資料  
-   [命令列語法關鍵](command-line-syntax-key.md)  
