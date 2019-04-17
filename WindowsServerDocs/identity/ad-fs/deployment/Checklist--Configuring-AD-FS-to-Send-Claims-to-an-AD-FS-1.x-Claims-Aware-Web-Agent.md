---
ms.assetid: 551c1a0d-8d30-41b4-9c4a-35a3337dd3bc
title: "部署聯盟伺服器"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 32675aa7fb8c7b928bcf80a4d1072fe5eab7cd61
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="checklist-configuring-ad-fs-to-send-claims-to-an-ad-fs-1x-claims-aware-web-agent"></a>檢查清單︰ 設定 AD FS 傳送給 AD FS 1.x 宣告感知 Web 代理程式宣告

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012
  
## <a name="checklist-configuring-ad-fs-to-send-claims-to-an-ad-fs-1x-claims-aware-web-agent"></a>檢查清單︰ 設定 AD FS 傳送給 AD FS 1.x claims\ 感知 Web 代理程式宣告  
此檢查清單會包含所需的設定您的 Active Directory 同盟服務 \(AD FS\) 同盟服務，傳送宣告可以了解應用程式執行 AD FS 1 的網頁伺服器主控 Windows Server 2012 中的工作。*x* claims\ 感知 Web 代理程式。  
  
> [!NOTE]  
> 完成此訂單中的檢查清單中的工作。 當參考連結可讓您的程序時，返回本主題之後在您完成該程序中的步驟操作，以便您可以繼續檢查清單中的其餘的工作。  
  
![設定宣告傳送給 AD FS](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**檢查清單︰ 設定 AD FS 傳送給 AD FS 1.x claims\ 感知 Web 代理程式宣告**  
  
||工作|參考資料|  
|-|--------|-------------|  
|![設定宣告傳送給 AD FS](media/icon_checkboxo.gif)|跨平台與 Windows Server 2012 中的 AD FS 舊版 AD FS 計劃以及了解更多有關名稱 ID 宣告類型。|![設定宣告傳送給 AD FS](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[規劃 AD FS 使用的跨平台 1.x](https://technet.microsoft.com/library/ff678040.aspx)|  
|![設定宣告傳送給 AD FS](media/icon_checkboxo.gif)|如果已經執行此動作，使用右側連結第一次建立信賴廠商信任 AD FS 1 與 Windows Server 2012 中 AD FS 同盟服務。*x*同盟服務。|[檢查清單︰ 設定 AD FS 傳送給 AD FS 1.x 同盟服務宣告](Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Federation-Service.md)|  
|![設定宣告傳送給 AD FS](media/icon_checkboxo.gif)|您可以在前達到相互 AD FS 1 裝載的應用程式。*x* claims\ 感知網路代理程式，您必須先建立信賴廠商信任 AD FS 同盟服務中的 AD FS 1 到 Windows Server 2012 中。 *x* claims\ 感知 Web 代理程式。 **注意：**建立 AD FS 同盟服務此信任相當新增新的**應用程式**以 AD FS 1.x 同盟服務 \ (**聯盟 Service\\Trust Policy\\My Organization\\Application**\)。 因為 AD FS 不會有相當依賴廠商信任是必要**的應用程式**節點，其 snap\ 中的。 不過，您仍必須先應用程式安全的通道。<br /><br />當您設定程序使用中的直接連結信任時，您必須完成以下新增可以廠商信任精靈中交互操作 AD FS 1 信任此設定。*x* claims\ 感知 Web 代理程式：<br /><br />1.在**選取資料來源**頁面上，選取 [**輸入資料可以手動廠商信任**。<br />2.在**選擇設定檔**頁面上，選取**的設定檔 AD FS 1.0 和 1.1**。<br />3.在**設定的 URL**頁面上，在**WS\-聯盟被動式 URL**，輸入**應用程式 URL** AD FS 1 中所定義。*x*的合作夥伴同盟服務。<br />4.在**設定識別碼**頁面上，在**Relying 部分信任識別碼**，輸入**應用程式 URL** AD FS 1 中所定義。*x* claims\ 感知 Web 代理程式|![設定宣告傳送給 AD FS](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[可以廠商信任手動建立](../../ad-fs/operations/Create-a-Relying-Party-Trust.md)|  
|![設定宣告傳送給 AD FS](media/icon_checkboxo.gif)|請洽詢系統管理員身分執行 AD FS 1 的網頁伺服器。*x* claims\ 感知網頁代理程式和編輯 web.config 與 claims\ 感知應用程式相關聯的系統管理員 \（在 [網際網路資訊服務 \(IIS\)\) 指向網路代理程式 AD FS 同盟服務，預設網站。<br /><br />例如，更換*myresourcefederationserver*在標記`<fs>https://myresourcefederationserver/adfs/fs/federationserverservice.asmx</fs>`的 web.config 有效 AD FS 聯盟伺服器名稱。<br /><br />這是必要無法使用從 Windows Server 2012 中 AD FS 同盟服務會傳送至該宣告應用程式和 AD FS 1.x claims\ 感知網路代理程式。|A N\ 日|  
|![設定宣告傳送給 AD FS](media/icon_checkboxo.gif)|在您先前建立的依賴廠商信任，您必須建立理賠要求將需要連入宣告擷取自屬性存放區與通過、篩選或轉換成名稱 ID 規則宣告類型，可以了解，並由 AD FS 1。*x* claims\ 感知 Web 代理程式。 **注意：**您建立本規則之前，請確定您建立此規則宣告規則集合有第一次從屬性存放區擷取輕量型 Directory 存取通訊協定 \(LDAP\) 屬性理賠要求前出現的規則。 做為您建立傳送給 AD FS 1 規則輸入，將會使用此理賠要求。*x*\-compatible 理賠要求。 如需如何建立規則解壓縮 LDAP 屬性，請查看[建立規則為宣告傳送 LDAP 屬性，](../../ad-fs/operations/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md)。|![設定宣告傳送給 AD FS](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[建立傳送給 AD FS 規則 1.x 相容宣告](../../ad-fs/operations/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)|  
  

