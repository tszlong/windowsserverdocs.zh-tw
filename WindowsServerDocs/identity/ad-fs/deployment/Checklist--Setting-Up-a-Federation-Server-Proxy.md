---
ms.assetid: 38c9bcd3-c6f8-4153-8e42-5fd31568c65a
title: "檢查清單-聯盟 Proxy 伺服器設定"
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 997ee901172eb2d873ad02fa8ce39da09c5171c7
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="checklist-setting-up-a-federation-server-proxy"></a>檢查清單︰ 聯盟 Proxy 伺服器設定

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

檢查此清單包含部署工作備妥聯盟伺服器 proxy 中的角色 Active Directory 同盟服務 \(AD FS\) 執行 Windows Server® 2012 年的伺服器。  
  
> [!NOTE]  
> 完成此訂單中的檢查清單中的工作。 當參考連結可讓您的程序時，返回本主題之後在您完成該程序中的步驟操作，以便您可以繼續檢查清單中的其餘的工作。  
  
![聯盟的 proxy 伺服器設定](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**檢查清單︰ 設定聯盟 proxy 伺服器**  
  
||工作|參考資料|  
|-|--------|-------------|  
|![聯盟的 proxy 伺服器設定](media/icon_checkboxo.gif)|部署您 AD FS 聯盟伺服器 proxy 您開始之前，請檢查 AD FS 部署拓撲類型及其相關聯的伺服器位置和網路配置建議。|![聯盟的 proxy 伺服器設定](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[判斷您 AD FS 部署拓撲](https://technet.microsoft.com/library/gg982491.aspx)<br /><br />![聯盟的 proxy 伺服器設定](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[規劃聯盟伺服器 Proxy 位置](https://technet.microsoft.com/library/dd807130.aspx)<br /><br />![聯盟的 proxy 伺服器設定](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[放置聯盟 Proxy 伺服器](https://technet.microsoft.com/library/dd807048.aspx)|  
|![聯盟的 proxy 伺服器設定](media/icon_checkboxo.gif)|檢查 AD FS 容量規劃指導方針，以判斷正確的聯盟伺服器 proxy，您應該 production 環境中使用數。|![聯盟的 proxy 伺服器設定](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[聯盟 Proxy 伺服器的容量的計劃](https://technet.microsoft.com/library/gg749898.aspx)|  
|![聯盟的 proxy 伺服器設定](media/icon_checkboxo.gif)|單一聯盟 proxy 伺服器或聯盟 proxy 伺服器陣列適合您的部署。 **注意：**聯盟伺服器也執行聯盟 proxy 伺服器的責任。|![聯盟的 proxy 伺服器設定](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[建立聯盟 Proxy 伺服器的時機](https://technet.microsoft.com/library/dd807032.aspx)<br /><br />![聯盟的 proxy 伺服器設定](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[當建立聯盟 Proxy 伺服器陣列](https://technet.microsoft.com/library/dd807082.aspx)|  
|![聯盟的 proxy 伺服器設定](media/icon_checkboxo.gif)|判斷是否要建立這個新的聯盟伺服器 proxy account 合作夥伴公司或組織資源合作夥伴周邊網路中。|![聯盟的 proxy 伺服器設定](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[檢視聯盟伺服器 Proxy Account 合作夥伴中的角色](https://technet.microsoft.com/library/dd807109.aspx)<br /><br />![聯盟的 proxy 伺服器設定](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[檢視聯盟伺服器 Proxy 資源夥伴中的角色](https://technet.microsoft.com/en-us/library/dd807052.aspx)|  
|![聯盟的 proxy 伺服器設定](media/icon_checkboxo.gif)|您的電腦將會變成聯盟 proxy 伺服器上安裝 AD FS 之前，請了解取得伺服器驗證憑證的名稱，聯盟伺服器 proxy 陣列 — 新增或共用發電廠中的所有伺服器的憑證。|![聯盟的 proxy 伺服器設定](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[聯盟的 Proxy 伺服器的憑證需求](https://technet.microsoft.com/library/dd807054.aspx)|  
|![聯盟的 proxy 伺服器設定](media/icon_checkboxo.gif)|檢查 AD FS 設計節目表中了解如何更新周邊網路中的網域名稱系統 \(DNS\) 發生成功的名稱解析為聯盟伺服器和聯盟的 proxy 伺服器的資訊。|![聯盟的 proxy 伺服器設定](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[聯盟的 Proxy 伺服器的名稱解析需求](https://technet.microsoft.com/library/dd807055.aspx)|  
|![聯盟的 proxy 伺服器設定](media/icon_checkboxo.gif)|判斷聯盟 proxy 伺服器是否必須加入網域。 雖然不需要聯盟伺服器 proxy 加入網域，有更容易管理的遠端管理和群組原則的功能時的加入網域。|![聯盟的 proxy 伺服器設定](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[加入網域的電腦](Join-a-Computer-to-a-Domain.md)|  
|![聯盟的 proxy 伺服器設定](media/icon_checkboxo.gif)|根據您周邊網路的 DNS 基礎結構的設定方式完成主題中的程序的其中一個上直接前您在組織中部署聯盟 proxy 伺服器。 **注意：**未執行兩個程序。 朗讀[聯盟的 Proxy 伺服器的名稱解析需求](https://technet.microsoft.com/library/dd807055.aspx)若要判斷的程序最適合您的組織的需求。|![聯盟的 proxy 伺服器設定](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[設定為聯盟伺服器 Proxy 做周邊網路 DNS 區域中的名稱解析](Configure-Name-Resolution-for-a-Federation-Server-Proxy-in-a-DNS-Zone-That-Serves-Only-the-Perimeter-Network.md)<br /><br />![聯盟的 proxy 伺服器設定](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[的 DNS 區域，提供同時周邊網路和網際網路戶端聯盟伺服器 Proxy 設定名稱解析](Configure-Name-Resolution-for-a-Federation-Server-Proxy-in-a-DNS-Zone-That-Serves-Both-the-Perimeter-Network-and-Internet-Clients.md)|  
|![聯盟的 proxy 伺服器設定](media/icon_checkboxo.gif)|取得伺服器驗證憑證之後，您必須在網際網路資訊服務 \(IIS\) 預設聯盟 proxy 伺服器的網站上安裝它。|![聯盟的 proxy 伺服器設定](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[匯入伺服器驗證憑證的預設網站](Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md)|  
|![聯盟的 proxy 伺服器設定](media/icon_checkboxo.gif)|取得伺服器驗證憑證的憑證授權單位另一種 \(Optional\) \(CA\)，您可以使用 IIS 您聯盟伺服器 proxy 取得範例憑證。<br /><br />因為 IIS 產生 self\ 簽署憑證並非來自受信任的來源，請使用它來只能在下列案例中建立 self\ 簽署的憑證：<br /><br />-當您有建立有限、已知群組中的使用者與您的伺服器安全通訊端層 \(SSL\) 通道<br />-當您有 third\ 廠商憑證問題進行疑難排解**注意：**並不安全的最佳做法部署聯盟伺服器 proxy production 環境中使用 self\ 簽章，伺服器驗證憑證。|![聯盟的 proxy 伺服器設定](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[IIS：建立 Self\-Signed 伺服器的憑證](https://go.microsoft.com/fwlink/?LinkID=108271)|  
|![聯盟的 proxy 伺服器設定](media/icon_checkboxo.gif)|將會變成聯盟 proxy 伺服器的電腦上安裝同盟服務 Proxy 角色服務。|![聯盟的 proxy 伺服器設定](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[安裝同盟服務 Proxy 角色服務](Install-the-Federation-Service-Proxy-Role-Service.md)|  
|![聯盟的 proxy 伺服器設定](media/icon_checkboxo.gif)|AD FS 軟體的電腦上設定，請使用 AD FSFederation Proxy 伺服器設定精靈做在聯盟 proxy 伺服器的角色。|![聯盟的 proxy 伺服器設定](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[聯盟伺服器 Proxy 角色設定電腦](Configure-a-Computer-for-the-Federation-Server-Proxy-Role.md)|  
|![聯盟的 proxy 伺服器設定](media/icon_checkboxo.gif)|使用事件檢視器，請確認聯盟 proxy 伺服器開始。|![聯盟的 proxy 伺服器設定](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[確認聯盟伺服器 Proxy 是操作](Verify-That-a-Federation-Server-Proxy-Is-Operational.md)|  
