---
description: 深入瞭解：何時建立同盟伺服器 Proxy 伺服器陣列
ms.assetid: ad0bf21d-2ace-4565-b1f5-ce57c8eb2689
title: 建立同盟伺服器 Proxy 伺服器陣列的時機
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 370c4c0b30357fb9216a8c89be96351e5054ad5c
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97049326"
---
# <a name="when-to-create-a-federation-server-proxy-farm"></a>建立同盟伺服器 Proxy 伺服器陣列的時機

當您有大型的 Active Directory 同盟服務 \( AD FS \) 部署，而您想要為 proxy 部署提供容錯、負載平衡和擴充能力時，請考慮安裝額外的同盟伺服器 proxy \- 。 在相同的周邊網路中建立兩個或多個同盟伺服器 proxy，並將其設定為保護相同 AD FS 同盟服務建立同盟伺服器 proxy 伺服器陣列的動作。

您可以使用 AD FS 同盟伺服器 Proxy 設定向導]，建立同盟伺服器 proxy 伺服器陣列或安裝其他同盟伺服器 proxy 至現有的伺服器陣列。 如需詳細資訊，請參閱 [When to Create a Federation Server Proxy](When-to-Create-a-Federation-Server-Proxy.md)。

在所有同盟伺服器 proxy 都能以伺服器陣列形式一起運作之前，您必須先在一個 IP 位址和一個網域名稱系統 \( DNS \) 完整功能變數名稱 FQDN 下進行叢集處理 \( \) 。 您可以藉由在 \( 周邊網路內部署 Microsoft 網路負載平衡 NLB 來叢集伺服器 \) 。 下表中的工作需要適當設定 NLB，以叢集伺服器陣列中的同盟伺服器 proxy。

如需有關如何使用 Microsoft NLB 技術來設定叢集 FQDN 的詳細資訊，請參閱 [指定叢集參數](https://go.microsoft.com/fwlink/?linkid=74651)。

## <a name="configuring-federation-server-proxies-for-a-farm"></a>設定伺服器陣列的同盟伺服器 Proxy
下表說明必須完成才能讓每個同盟伺服器 proxy 參與伺服器陣列的工作。

|Task|描述|
|--------|---------------|
|將伺服器陣列中的所有 proxy 指向相同的 AD FS 同盟服務名稱|當您建立同盟伺服器 Proxy 時，必須針對將參與陣列的所有同盟伺服器 Proxy，在 [AD FS 同盟伺服器 Proxy 設定] 中輸入相同的同盟服務名稱。 同盟伺服器 proxy 會使用組成此 DNS 主機名稱的 URL 來判斷它所聯絡的 AD FS 同盟服務實例。<p>如需詳細資訊，請參閱 [Configure a Computer for the Federation Server Proxy Role](../../ad-fs/deployment/Configure-a-Computer-for-the-Federation-Server-Proxy-Role.md)。|
|取得及共用憑證|您可以從公開憑證授權單位單位（例如 VeriSign）取得伺服器驗證憑證， \( \) 然後設定憑證，讓所有同盟伺服器 proxy 都能在每個同盟伺服器 proxy 的預設網站上共用相同憑證的相同私密金鑰部分。 若要共用憑證，您必須在每個同盟伺服器 proxy 的預設網站上安裝相同的伺服器驗證憑證。 如需詳細資訊，請參閱將 [伺服器驗證憑證匯入至預設的網站](../../ad-fs/deployment/Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md)。<p>如需詳細資訊，請參閱 [Certificate Requirements for Federation Server Proxies](Certificate-Requirements-for-Federation-Server-Proxies.md)。|

如需新增同盟伺服器 proxy 以建立同盟伺服器 proxy 伺服器陣列的詳細資訊，請參閱 [檢查清單：設定同盟伺服器 proxy](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server-Proxy.md)。

## <a name="see-also"></a>另請參閱
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
