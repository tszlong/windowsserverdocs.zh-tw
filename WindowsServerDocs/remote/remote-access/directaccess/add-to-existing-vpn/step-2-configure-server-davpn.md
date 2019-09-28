---
title: 步驟2：設定 DirectAccess-VPN 伺服器
description: 本主題是將 DirectAccess 新增至 Windows Server 2016 的現有遠端存取（VPN）部署指南的一部分
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fe221fc9-c7d9-4508-b8a1-000d2515283c
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7ee691a02df385e29bdac9656d50bc2c6d3af087
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388744"
---
#  <a name="step-2-configure-the-directaccess-vpn-server"></a>步驟2：設定 DirectAccess-VPN 伺服器

>適用於：Windows Server (半年度管道)、Windows Server 2016

本主題說明如何使用啟用 DirectAccess 精靈，設定基本的遠端存取部署所需的用戶端與伺服器設定。

下表提供您可以使用本主題完成之步驟的總覽。

|工作       |描述|
|-----------|-----------|
|設定 DirectAccess 用戶端|設定遠端存取伺服器包含 DirectAccess 用戶端的安全性群組。|
|設定網路拓撲|設定遠端存取伺服器設定。|
|設定 DNS 尾碼搜尋清單|視需要修改尾碼搜尋清單。|
|GPO 設定|視需要修改 GPO。|

## <a name="to-start-the-enable-directacces-wizard"></a>啟動啟用 DirectAcces 精靈

1. 在伺服器管理員中，按一下 [**工具**]，然後按一下 [**遠端存取**]。除非您已選取 [不要**再顯示此畫面**]，否則會自動啟動 [啟用 DirectAccess Wizard]。 

2. 如果 wizard 不會自動啟動，請以滑鼠右鍵按一下 [路由及遠端存取] 樹狀目錄中的伺服器節點，然後按一下 [**啟用 DirectAccess**]。

3. 按一下 [下一步]。

## <a name="configure-directaccess-clients"></a>設定 DirectAccess 用戶端

要佈建來使用 DirectAccess 的用戶端電腦，必須屬於選取的安全性群組。 設定 DirectAccess 之後，會佈建安全性群組中的用戶端電腦以接收 DirectAccess 群組原則。

1. 在 [選取群組] 頁面中，按一下 [新增]。

2. 在 [選取群組] 對話方塊中，選取包含 DirectAccess 用戶端電腦的安全性群組。

3. 選取 [僅針對攜帶型電腦啟用 DirectAccess] 核取方塊，只允許攜帶型電腦存取內部網路。

4. 選取 [使用強制通道] 核取方塊，透過遠端存取伺服器路由所有用戶端流量 (到內部網路和到網際網路)。

5. 按一下 [下一步]。

## <a name="configure-the-network-topology"></a>設定網路拓撲

若要部署遠端存取，您必須設定遠端存取伺服器，包括正確的網路介面卡、用戶端電腦可以連線到遠端存取伺服器的公用 URL (ConnectTo 位址)，以及其主體符合 ConnectTo 位址的 IP-HTTPS 憑證。

1. 在 [網路拓撲] 頁面上，按一下要用於您組織的部署拓撲。 在 [輸入用戶端用於連線至遠端存取伺服器的公用名稱或 IPv4 位址]、輸入部署的公用名稱 (此名稱符合 IP-HTTPS 憑證的主體名稱，例如，edge1.contoso.com)，然後按 [下一步]。

## <a name="configure-the-dns-suffix-search-list"></a>設定 DNS 尾碼搜尋清單

對於 DNS 用戶端，您可以設定 DNS 網域尾碼搜尋清單來擴充或修訂其 DNS 搜尋能力。 您可以在清單中加入其他尾碼，搜尋多個指定的 DNS 網域中簡短、不完整的電腦名稱。 然後，如果 DNS 查詢失敗，DNS 用戶端服務可以使用此清單，將其他名稱尾碼附加至原始名稱，並對 DNS 伺服器重複執行這些替代 Fqdn 的 DNS 查詢。

1. 選取 [以 DNS 用戶端尾碼搜尋清單設定 DirectAccess 用戶端] 指定其他尾碼用於用戶端名稱搜尋。

2. 在 [**新尾碼**] 中輸入新的尾碼名稱，然後按一下 [**加入**]。 此外，您也可以變更搜尋順序，並從**要使用的網域尾碼**移除尾碼。

>下在脫離的命名空間案例中 \(where 一或多個網域電腦的 DNS 尾碼不符合電腦所屬的 Active Directory 網域 @ no__t-1，您應該確保搜尋清單已自訂為包含所有必要的一直. 遠端存取精靈預設會將 Active Directory DNS 名稱設定為用戶端上的主要 DNS 尾碼。 系統管理員必須確定新增用戶端使用的 DNS 尾碼進行名稱解析。

針對電腦和伺服器，會預先決定下列預設 DNS 搜尋行為，並在完成和解析簡短、不完整的名稱時使用。當尾碼搜尋清單是空的或未指定時，會將電腦的主要 DNS 尾碼附加至簡短的不完整名稱，並使用 DNS 查詢來解析結果 FQDN。 

如果此查詢失敗，電腦可以附加針對網路連線所設定的任何連線特定 DNS 尾碼，以嘗試其他查詢替代 Fqdn。如果未設定任何連線特定的尾碼，或這些結果連接特定的 Fqdn 的查詢失敗，則用戶端就可以開始根據系統化的主要尾碼（也稱為轉移）來重試查詢。

例如，如果主要尾碼為 "example.microsoft.com"，則轉移程式可以在 "microsoft.com" 和 "com" 定義域中搜尋，以重試簡短名稱的查詢。

當尾碼搜尋清單不是空的，而且至少指定了一個 DNS 尾碼時，就只會搜尋指定尾碼清單中所提供的 Fqdn，以嘗試限定及解析簡短 DNS 名稱。 

如果查詢附加以及嘗試清單中每個尾碼的結果所組成的所有 FQDN 都無法解析，查詢程序會失敗，並產生「 找不到名稱 」的結果。 

> [!WARNING]
> 如果使用網域尾碼清單，則用戶端就可以在查詢沒有回應或無法解析查詢時，根據不同的 DNS 網域名稱繼續傳送其他替代查詢。 一旦使用尾碼清單中的項目解析名稱，就不會嘗試未使用的清單項目。 因此，最有效率的方式是優先排列最常使用的網域尾碼清單。
> 
> 只有當 DNS 名稱項目不完整時，才使用網域名稱尾碼搜尋。 要讓 DNS 名稱完全合格，需在名稱結尾輸入結尾的句點 (.)。

## <a name="gpo-configuration"></a>GPO 設定

當您設定「遠端存取」時，DirectAccess 設定會收集到群組原則物件（GPO）中。 

在 [ **GPO 設定**] 中，會列出 DIRECTACCESS 伺服器 GPO 名稱和用戶端 GPO 名稱。 此外，您可以修改 GPO 選擇設定。

系統會使用 DirectAccess 設定自動填入兩個 Gpo，並以這種方式散發：

1. **DirectAccess 用戶端 GPO**。 這個 GPO 包含用戶端設定，包括 IPv6 轉換技術設定、NRPT 項目，以及「具有進階安全性的 Windows 防火牆」連線安全性規則。 這個 GPO 會套用到為用戶端電腦指定的安全性群組。

2. **DirectAccess 伺服器 GPO**。 此 GPO 包含 DirectAccess 設定，可套用至在部署中設定為遠端存取服務器的任何伺服器。 它也包含「具有進階安全性的 Windows 防火牆」連線安全性規則。

## <a name="summary"></a>總結

完成「遠端存取」設定之後，就會顯示 [**摘要**]。 您可以變更已設定的設定，或按一下 **[完成]** 套用設定。
