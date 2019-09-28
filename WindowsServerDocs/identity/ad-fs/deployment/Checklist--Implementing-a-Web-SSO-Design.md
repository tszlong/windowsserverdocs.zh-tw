---
ms.assetid: 30657638-5709-48c5-87aa-98f688e07b4c
title: 檢查清單-執行網頁 SSO 設計
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 8488e0c9195930374aacd959e72d0eff34142ca7
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408464"
---
# <a name="checklist-implementing-a-web-sso-design"></a>檢查清單：實作網頁 SSO 設計

此父檢查清單包含有關 Web Single @ no__t-1Sign @ no__t-2On \(SSO @ no__t-4 設計 Active Directory 同盟服務 \(AD FS @ no__t-6 之重要概念的跨 @ no__t-0reference 連結。 它也包含次檢查清單連結，其中的資訊可協助您完成實作此設計所需執行的工作。  
  
> [!NOTE]  
> 請依序完成此檢查清單中的工作。 當參照連結將您導向概念性主題或次檢查清單時，請在檢閱概念性主題或完成次檢查清單中的工作之後返回此主題，以便您可以繼續執行此檢查清單中的其餘工作。  
  
![web sso @ no__t-1Checklist：實作網頁 SSO 設計**  
  
||工作|參考資料|  
|-|--------|-------------|  
|![網頁 sso](media/icon_checkboxo.gif)|請參閱有關網頁 SSO 設計的重要概念，並判斷您可以使用哪些 AD FS 部署目標來自訂此設計，以符合組織的需求。 **注意：**|@no__t 0web sso](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[網頁 Sso 設計](https://technet.microsoft.com/library/dd807033.aspx)<br /><br />@no__t 0web 的 sso，](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[可識別您的 AD FS 部署目標](https://technet.microsoft.com/library/dd807053.aspx)|  
|![網頁 sso](media/icon_checkboxo.gif)|檢查硬體、軟體、憑證、網域名稱系統 \(DNS @ no__t-1、屬性存放區，以及在您的組織中部署 AD FS 的用戶端需求。|![web sso @ no__t-1Appendix A：檢閱 AD FS 需求](https://technet.microsoft.com/library/ff678034.aspx)|  
|![網頁 sso](media/icon_checkboxo.gif)|根據您的設計計畫，在公司網路或周邊網路中安裝一部或多部同盟伺服器。 **注意：** 網頁 SSO 設計只需要一部同盟伺服器就能順利運作。 單一同盟伺服器會同時在宣告提供者角色和信賴憑證者角色中運作。|![web sso @ no__t-1Checklist：設定同盟伺服器](Checklist--Setting-Up-a-Federation-Server.md)|  
|![網頁 sso](media/icon_checkboxo.gif)|@no__t 0Optional @ no__t-1 判斷您的組織是否需要周邊網路中的同盟伺服器 proxy。|![web sso @ no__t-1Checklist：設定同盟伺服器 Proxy](Checklist--Setting-Up-a-Federation-Server-Proxy.md)|  
|![網頁 sso](media/icon_checkboxo.gif)|根據您的網頁 SSO 設計計畫與您想要的使用方式而定，新增適當的屬性存放區、信賴憑證者信任與宣告規則至 Federation Service。|![web sso @ no__t-1Checklist：設定帳戶夥伴組織 @ no__t-0|  
|![網頁 sso](media/icon_checkboxo.gif)|如果您是資源夥伴組織中的系統管理員，宣告 @ no__t-0enable 您的網頁瀏覽器應用程式、Web 服務應用程式，或使用 WIF 和 WIF SDK 的 Microsoft® Office SharePoint®伺服器應用程式。 **注意：**|@no__t 0web sso](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Windows Identity Foundation](https://go.microsoft.com/fwlink/?LinkId=122266)<br /><br />@no__t 0web sso](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Windows Identity FOUNDATION SDK](https://go.microsoft.com/fwlink/?LinkId=122266)| 
