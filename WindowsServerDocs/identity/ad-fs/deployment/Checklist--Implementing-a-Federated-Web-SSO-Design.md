---
ms.assetid: 6b49cde3-d2cb-4ece-b9b7-dc600e037495
title: "檢查清單-實作聯盟的網路 SSO 設計"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 1122ee3814f656a7229dc2946d7441d525de5ae2
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="checklist-implementing-a-federated-web-sso-design"></a>檢查清單︰ 實作的聯盟的網路 SSO 設計

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

此家長檢查清單包括重要的概念有關 Active Directory 同盟服務 \(AD FS\) 的聯盟網路 Single\-Sign\-On \(SSO\) 設計 cross\ 參考連結。 它也包含附屬檢查清單可協助您完成工作，才能執行這項設計的連結。  
  
> [!NOTE]  
> 完成此訂單中的檢查清單中的工作。 當參考連結可讓您的概念主題或附屬檢查清單時，返回本主題之後您檢視的概念主題，或在您完成附屬檢查清單中的工作，讓您可以繼續檢查清單中的其餘的工作。  
  
![聯盟網路 sso](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**檢查清單︰ 實作聯盟網路 SSO 設計**  
  
||工作|參考資料|  
|-|--------|-------------|  
|![聯盟的網路 sso](media/icon_checkboxo.gif)|檢視的聯盟網路 SSO 設計的相關重要的概念，並判斷來自訂此需求貴組織的設計，您可以使用哪些 AD FS 部署目標。|![聯盟網路 sso](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[的聯盟網路 SSO 設計](https://technet.microsoft.com/library/dd807050.aspx)<br /><br />![聯盟網路 sso](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[找出您 AD FS 部署務目標](https://technet.microsoft.com/library/dd807053.aspx)<br /><br />![聯盟網路 sso](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[規劃您部署](https://technet.microsoft.com/library/dd807083.aspx)|  
|![聯盟的網路 sso](media/icon_checkboxo.gif)|檢視的硬體、軟體、憑證，網域名稱系統 \(DNS\)、屬性市集中和 client 需求部署您在組織中的 AD FS。|![聯盟網路 sso](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[附錄 a：審查 AD FS 需求](https://technet.microsoft.com/library/ff678034.aspx)|  
|![聯盟的網路 sso](media/icon_checkboxo.gif)|檢視宣告有關的重要概念，取得規則、屬性儲存與 AD FS 設定資料庫之前部署這兩個合作夥伴公司 AD FS。|![聯盟網路 sso](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[了解主要 AD FS 概念](../../ad-fs/technical-reference/Understanding-Key-AD-FS-Concepts.md)|  
|![聯盟的網路 sso](media/icon_checkboxo.gif)|根據您的設計計劃，每個協力廠商組織安裝一或多個聯盟伺服器。 **注意：**的聯盟網路 SSO 設計，您需要至少一個聯盟伺服器 account 合作夥伴組織和至少一個聯盟伺服器資源合作夥伴組織中的。|![聯盟網路 sso](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[檢查清單︰ 設定好聯盟伺服器](Checklist--Setting-Up-a-Federation-Server.md)|  
|![聯盟的網路 sso](media/icon_checkboxo.gif)|\(Optional\) 判斷您的組織是否需要聯盟 proxy 伺服器。 如果您設計計劃的 proxy，您可以安裝一或多個聯盟伺服器 proxy 每個協力廠商組織。|![聯盟網路 sso](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[檢查清單︰ 設定好聯盟伺服器 Proxy](Checklist--Setting-Up-a-Federation-Server-Proxy.md)|  
|![聯盟的網路 sso](media/icon_checkboxo.gif)|根據您的設計計劃，分享憑證，設定戶端，並設定信任關係這兩個合作夥伴中，讓他們可以透過聯盟信任進行通訊。|![聯盟網路 sso](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[檢查清單︰ 設定 Account 合作夥伴公司](Checklist--Configuring-the-Account-Partner-Organization.md)<br /><br />![聯盟網路 sso](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[檢查清單︰ 設定資源合作夥伴公司](Checklist--Configuring-the-Resource-Partner-Organization.md)|  
|![聯盟的網路 sso](media/icon_checkboxo.gif)|如果您是系統管理員可以在資源合作夥伴組織，讓 claims\ 您網頁瀏覽器應用程式、Web 服務應用程式或 Microsoft® Office SharePoint® 伺服器應用程式使用 WIF 和 WIF 的 SDK。|![聯盟網路 sso](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Windows 身分基本知識](https://go.microsoft.com/fwlink/?LinkId=122266)<br /><br />![聯盟網路 sso](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Windows 身分基本知識 SDK](https://go.microsoft.com/fwlink/?LinkId=122266)|  
