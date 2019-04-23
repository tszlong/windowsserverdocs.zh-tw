---
title: 使用中斷連線用戶端命令
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 876bbe6c-76ab-4de5-879b-d2066e700326
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b89f6c2ff6d41230afd0a2b251ad6982dfa235b1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841749"
---
# <a name="using-the-disconnect-client-command"></a>使用中斷連線用戶端命令



中斷用戶端連接的多點傳送的傳輸或命名空間。 除非您指定 **/force**，用戶端會切換回另一個的傳輸方法 （如果它支援用戶端）。

## <a name="syntax"></a>語法

```
WDSUTIL /Disconnect-Client /ClientId:<Client ID> [/Server:<Server name>] [/Force]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/ ClientId:\<用戶端識別碼 >|指定要中斷連線的用戶端識別碼。 若要檢視用戶端識別碼，請輸入**WDSUTIL /get-multicasttransmission /show: clients**。|
|[/ 伺服器：\<伺服器名稱 >]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整的網域名稱 (FQDN)。 如果沒有指定伺服器名稱時，會使用本機伺服器。|
|[/Force]|完全停止安裝，並不會使用後援方法。 請注意，Wdsmcast.exe 不支援任何後援機制。 如果您不使用此選項，預設的行為如下所示：</br>-如果您使用 Windows 部署服務用戶端，用戶端會使用單點傳播的繼續安裝。</br>-如果您未使用的 Windows 部署服務用戶端，則安裝將會失敗。</br>重要事項：您應該謹慎使用此選項，因為安裝將會失敗，而且電腦可能處於不穩定的狀態。|

## <a name="BKMK_examples"></a>範例

若要中斷連線的用戶端，請輸入：
```
WDSUTIL /Disconnect-Client /ClientId:1
```
若要中斷連線的用戶端，而且強制安裝失敗，請輸入：
```
WDSUTIL /Disconnect-Client /Server:MyWDSServer /ClientId:1 /Force
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)