---
ms.assetid: 30657638-5709-48c5-87aa-98f688e07b4c
title: "檢查清單-實作 Web SSO 設計"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 265daf3acb9632aa92f85962abc44a6a9ea8dfed
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="checklist-implementing-a-web-sso-design"></a>檢查清單︰ 實作 Web SSO 設計

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

此家長檢查清單包含 cross\ 參考重要的概念有關 Active Directory 同盟服務 \(AD FS\) Single\ Sign\ 上 \(SSO\) 設計網頁連結。 它也包含附屬檢查清單可協助您完成工作，才能執行這項設計的連結。  
  
> [!NOTE]  
> 完成此訂單中的檢查清單中的工作。 當參考連結可讓您的概念主題或附屬檢查清單時，返回本主題之後您檢視的概念主題或完成附屬檢查清單中的工作，讓您可以繼續檢查清單中的其餘的工作。  
  
![Web sso](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**檢查清單︰ 實作 Web SSO 設計**  
  
||工作|參考資料|  
|-|--------|-------------|  
|![Web sso](media/icon_checkboxo.gif)|檢視的網頁 SSO 設計的相關重要的概念，並判斷來自訂此需求貴組織的設計，您可以使用哪些 AD FS 部署目標。 **注意：**|![Web sso](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[網站 SSO 設計](https://technet.microsoft.com/library/dd807033.aspx)<br /><br />![Web sso](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[找出您 AD FS 部署務目標](https://technet.microsoft.com/library/dd807053.aspx)|  
|![Web sso](media/icon_checkboxo.gif)|檢視的硬體、軟體、憑證，網域名稱系統 \(DNS\)、屬性市集中和 client 需求部署您在組織中的 AD FS。|![Web sso](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[附錄 a：審查 AD FS 需求](https://technet.microsoft.com/library/ff678034.aspx)|  
|![Web sso](media/icon_checkboxo.gif)|根據您的設計計劃，安裝一或多個聯盟伺服器，或周邊網路中的企業網路。 **注意：**網站 SSO 設計需要只有一個聯盟伺服器成功功能。 單一聯盟伺服器的作用中宣告提供者角色信賴的派對角色。|![Web sso](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[檢查清單︰ 設定好聯盟伺服器](Checklist--Setting-Up-a-Federation-Server.md)|  
|![Web sso](media/icon_checkboxo.gif)|\(Optional\) 判斷您的組織是否需要聯盟伺服器 proxy 周邊網路中的。|![Web sso](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[檢查清單︰ 設定好聯盟伺服器 Proxy](Checklist--Setting-Up-a-Federation-Server-Proxy.md)|  
|![Web sso](media/icon_checkboxo.gif)|根據您的網頁 SSO 設計方案和您想要使用的方式，新增適當屬性市集中，可以廠商信任宣告，並取得規則同盟服務。|![Web sso](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[檢查清單︰ 設定 Account 合作夥伴公司](Checklist--Configuring-the-Account-Partner-Organization.md)|  
|![Web sso](media/icon_checkboxo.gif)|如果您是系統管理員可以在資源合作夥伴組織，讓 claims\ 您網頁瀏覽器應用程式、Web 服務應用程式或 Microsoft® Office SharePoint® 伺服器應用程式使用 WIF 和 WIF 的 SDK。 **注意：**|![Web sso](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Windows 身分基本知識](https://go.microsoft.com/fwlink/?LinkId=122266)<br /><br />![Web sso](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Windows 身分基本知識 SDK](https://go.microsoft.com/fwlink/?LinkId=122266)| 
