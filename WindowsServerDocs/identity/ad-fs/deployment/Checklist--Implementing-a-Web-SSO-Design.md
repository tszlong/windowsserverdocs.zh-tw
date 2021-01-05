---
description: 深入瞭解：檢查清單：執行網頁 SSO 設計
ms.assetid: 30657638-5709-48c5-87aa-98f688e07b4c
title: 檢查清單-執行網頁 SSO 設計
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.author: billmath
ms.openlocfilehash: de48195031fd1d413a2751c554fd1020ff59093b
ms.sourcegitcommit: 3247e193d9fe1b57543fff215460a6d9db52f58b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/30/2020
ms.locfileid: "97814987"
---
# <a name="checklist-implementing-a-web-sso-design"></a>檢查清單：實作網頁 SSO 設計

此父檢查清單包含 \- 有關 Active Directory 同盟服務 AD FS 的 Web 單一 \- 登錄 \- \( SSO \) 設計之 \( 重要概念的交互參照連結 \) 。 它也包含次檢查清單連結，其中的資訊可協助您完成實作此設計所需執行的工作。

> [!NOTE]
> 請依序完成此檢查清單中的工作。 當參照連結將您導向概念性主題或次檢查清單時，請在檢閱概念性主題或完成次檢查清單中的工作之後返回此主題，以便您可以繼續執行此檢查清單中的其餘工作。

![執行網頁 SSO 設計檢查清單的圖示。](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**檢查清單：執行網頁 SSO 設計**

|Task|參考|
|--------|-------------|
|請參閱關於網頁 SSO 設計的重要概念，並判斷您可以使用哪些 AD FS 部署目標來自訂此設計，以符合組織的需求。 **注意：**|![識別您的 AD FS 部署目標] 連結的圖示，您可以在參考中使用該連結來執行網頁 SSO 設計。](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[網頁 SSO 設計](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dd807033(v=ws.11))<p>![Web SSO 設計連結的圖示，您可以在參考中使用該連結來執行網頁 SSO 設計。](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[識別您的 AD FS 部署目標](../design/identifying-your-ad-fs-deployment-goals.md)|
|請參閱在組織中部署 AD FS 的硬體、軟體、憑證、網域名稱系統 \( DNS \) 、屬性存放區和用戶端需求。|![附錄 A：審核 AD FS 需求連結的圖示，您可以在參考中用來執行網頁 SSO 設計。](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[附錄 A：審查 AD FS 需求](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/ff678034(v=ws.11))|
|根據您的設計計畫，在公司網路或周邊網路中安裝一部或多部同盟伺服器。 **注意：** 網頁 SSO 設計只需要一部同盟伺服器才能順利運作。 單一同盟伺服器在宣告提供者角色和信賴憑證者角色中都有作用。|![檢查清單的圖示：設定同盟伺服器連結，您可以在參考中用來執行網頁 SSO 設計。](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[檢查清單：設定同盟伺服器](Checklist--Setting-Up-a-Federation-Server.md)|
|\(選擇性 \) ：決定您的組織是否需要周邊網路中的同盟伺服器 proxy。|![檢查清單的圖示：設定同盟伺服器 Proxy 連結，可供您在參考中用來執行網頁 SSO 設計。](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[檢查清單：設定同盟伺服器 Proxy](Checklist--Setting-Up-a-Federation-Server-Proxy.md)|
|根據您的網頁 SSO 設計計畫與您想要的使用方式而定，新增適當的屬性存放區、信賴憑證者信任與宣告規則至 Federation Service。|![檢查清單的圖示：設定帳戶夥伴組織連結，讓您可以在參考中用來執行網頁 SSO 設計。](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[檢查清單：設定帳戶夥伴組織](Checklist--Configuring-the-Account-Partner-Organization.md)|
|如果您是資源夥伴組織的系統管理員，宣告會 \- 使用 WIF 和 WIF SDK 來啟用您的網頁瀏覽器應用程式、web 服務應用程式或 Microsoft &reg; Office SharePoint &reg; Server 應用程式。 **注意：**|![Windows Identity Foundation 連結的圖示，您可以在參考中用來執行網頁 SSO 設計。](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Windows Identity Foundation](https://go.microsoft.com/fwlink/?LinkId=122266)<p>![web sso](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Windows Identity FOUNDATION SDK](https://go.microsoft.com/fwlink/?LinkId=122266)|
