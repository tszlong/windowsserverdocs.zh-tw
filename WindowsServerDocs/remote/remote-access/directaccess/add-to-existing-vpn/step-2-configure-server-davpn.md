---
title: 步驟 2 設定 DirectAccess VPN 伺服器
description: 本主題是本指南新增 DirectAccess 加入現有的遠端存取 (VPN) 部署適用於 Windows Server 2016 的一部分
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fe221fc9-c7d9-4508-b8a1-000d2515283c
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f8a6448661861fdc9f97c66fb130bfc03d0ce72c
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446971"
---
#  <a name="step-2-configure-the-directaccess-vpn-server"></a>步驟 2 設定 DirectAccess VPN 伺服器

>適用於：Windows Server （半年通道），Windows Server 2016

本主題說明如何使用啟用 DirectAccess 精靈，設定基本的遠端存取部署所需的用戶端與伺服器設定。

下表提供您可以使用本主題來完成步驟的概觀。

|工作       |描述|
|-----------|-----------|
|設定 DirectAccess 用戶端|設定遠端存取伺服器包含 DirectAccess 用戶端的安全性群組。|
|設定網路拓撲|設定遠端存取伺服器設定。|
|設定 DNS 尾碼搜尋清單|視需要修改尾碼搜尋清單。|
|GPO 設定|視需要修改 GPO。|

## <a name="to-start-the-enable-directacces-wizard"></a>啟動啟用 DirectAcces 精靈

1. 在 [伺服器管理員] 中，按一下**工具**，然後按一下**遠端存取**。除非您已選取，啟用 DirectAccess 精靈自動啟動**不要再顯示此畫面**。 

2. 如果精靈沒有自動啟動，以滑鼠右鍵按一下 路由及遠端存取樹狀目錄中的伺服器節點，然後按一下**啟用 DirectAccess**。

3. 按一下 [下一步]  。

## <a name="configure-directaccess-clients"></a>設定 DirectAccess 用戶端

要佈建來使用 DirectAccess 的用戶端電腦，必須屬於選取的安全性群組。 設定 DirectAccess 之後，會佈建安全性群組中的用戶端電腦以接收 DirectAccess 群組原則。

1. 在 [選取群組]  頁面中，按一下 [新增]  。

2. 在 [選取群組]  對話方塊中，選取包含 DirectAccess 用戶端電腦的安全性群組。

3. 選取 [僅針對攜帶型電腦啟用 DirectAccess]  核取方塊，只允許攜帶型電腦存取內部網路。

4. 選取 [使用強制通道]  核取方塊，透過遠端存取伺服器路由所有用戶端流量 (到內部網路和到網際網路)。

5. 按一下 [下一步]  。

## <a name="configure-the-network-topology"></a>設定網路拓撲

若要部署遠端存取，您必須設定遠端存取伺服器，包括正確的網路介面卡、用戶端電腦可以連線到遠端存取伺服器的公用 URL (ConnectTo 位址)，以及其主體符合 ConnectTo 位址的 IP-HTTPS 憑證。

1. 在 [網路拓撲]  頁面上，按一下要用於您組織的部署拓撲。 在 [輸入用戶端用於連線至遠端存取伺服器的公用名稱或 IPv4 位址]  、輸入部署的公用名稱 (此名稱符合 IP-HTTPS 憑證的主體名稱，例如，edge1.contoso.com)，然後按 [下一步]  。

## <a name="configure-the-dns-suffix-search-list"></a>設定 DNS 尾碼搜尋清單

對於 DNS 用戶端，您可以設定 DNS 網域尾碼搜尋清單來擴充或修訂其 DNS 搜尋能力。 您可以在清單中加入其他尾碼，搜尋多個指定的 DNS 網域中簡短、不完整的電腦名稱。 然後，如果 DNS 查詢失敗，DNS 用戶端服務可以使用這份清單到原來名稱附加其他名稱尾碼行尾結束符號，並針對這些替代 Fqdn 重複 DNS 查詢轉送到 DNS 伺服器。

1. 選取 [以 DNS 用戶端尾碼搜尋清單設定 DirectAccess 用戶端]  指定其他尾碼用於用戶端名稱搜尋。

2. 輸入新的尾碼名稱，在**新的後置詞**，然後按一下**新增**。 此外，您可以在 變更搜尋順序，並移除後的置詞從**若要使用的網域尾碼**。

>[注意]在不相鄰的命名空間案例\(其中一或多個網域電腦具有不符合電腦所屬 Active Directory 網域的 DNS 尾碼\)，您應該確保搜尋清單已自訂為包含所有必要的尾碼。 遠端存取精靈預設會將 Active Directory DNS 名稱設定為用戶端上的主要 DNS 尾碼。 系統管理員必須確定新增用戶端使用的 DNS 尾碼進行名稱解析。

對於電腦及伺服器，下列的預設 DNS 搜尋行為是預先決定的用於完成和解析簡短、 不完整的名稱。當尾碼搜尋清單是空的或未指定時，在電腦的主要 DNS 尾碼會附加至簡短的非限定的名稱，以及 DNS 查詢用來解析 FQDN 結果。 

如果此查詢失敗時，電腦附加的網路連線設定任何連線特定 DNS 尾碼，可以嘗試其他查詢以尋找替代的 Fqdn。如果不設定任何連線特定尾碼，或查詢的這些結果的特定連線的 Fqdn 無法通過，則用戶端可以接著開始將重試逐次減少主要尾碼 （也稱為轉移） 為基礎的查詢。

比方說，如果主要尾碼是"example.microsoft.com"，轉移程序可以重試查詢簡短名稱藉由在"microsoft.com"與"com"網域中搜尋它。

當尾碼搜尋清單不是空的至少有一個指定 DNS 尾碼，嘗試檢查和解析簡短的 DNS 名稱僅限於搜尋只在指定的尾碼清單即可達成的 Fqdn。 

如果查詢附加以及嘗試清單中每個尾碼的結果所組成的所有 FQDN 都無法解析，查詢程序會失敗，並產生「 找不到名稱 」的結果。 

> [!WARNING]
> 如果使用網域尾碼清單，則用戶端就可以在查詢沒有回應或無法解析查詢時，根據不同的 DNS 網域名稱繼續傳送其他替代查詢。 一旦使用尾碼清單中的項目解析名稱，就不會嘗試未使用的清單項目。 因此，最有效率的方式是優先排列最常使用的網域尾碼清單。
> 
> 只有當 DNS 名稱項目不完整時，才使用網域名稱尾碼搜尋。 要讓 DNS 名稱完全合格，需在名稱結尾輸入結尾的句點 (.)。

## <a name="gpo-configuration"></a>GPO 設定

當您設定遠端存取時，DirectAccess 設定會收集到群組原則物件 (GPO)。 

在  **GPO 設定**，列出 DirectAccess 伺服器 GPO 名稱與用戶端 GPO 名稱。 此外，您可以修改 GPO 選擇設定。

兩個 Gpo 會自動填入 DirectAccess 設定，並以這種方式分配：

1. **DirectAccess 用戶端 GPO**。 這個 GPO 包含用戶端設定，包括 IPv6 轉換技術設定、NRPT 項目，以及「具有進階安全性的 Windows 防火牆」連線安全性規則。 這個 GPO 會套用到為用戶端電腦指定的安全性群組。

2. **DirectAccess 伺服器 GPO**。 這個 GPO 包含 DirectAccess 組態設定會套用到設定為遠端存取伺服器部署中的任何伺服器。 它也包含「具有進階安全性的 Windows 防火牆」連線安全性規則。

## <a name="summary"></a>總結

遠端存取設定完成後**摘要**隨即出現。 您可以變更設定的設定，或按一下**完成**套用設定。
