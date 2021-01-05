---
description: 深入瞭解：檢查清單：執行同盟網頁 SSO 設計
ms.assetid: 6b49cde3-d2cb-4ece-b9b7-dc600e037495
title: 檢查清單-執行同盟網頁 SSO 設計
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.author: billmath
ms.openlocfilehash: b6bec4d2923e69c80e32585a14d8781a6f705ae5
ms.sourcegitcommit: 3247e193d9fe1b57543fff215460a6d9db52f58b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/30/2020
ms.locfileid: "97814949"
---
# <a name="checklist-implementing-a-federated-web-sso-design"></a>檢查清單：實作同盟網頁 SSO 設計

此父檢查清單包含 \- 有關 Active Directory 同盟服務 AD FS 之同盟 Web 單一 \- 登錄 \- \( SSO \) 設計的 \( 重要概念的交互參照連結 \) 。 它也包含次檢查清單連結，其中的資訊可協助您完成實作此設計所需執行的工作。

> [!NOTE]
> 請依序完成此檢查清單中的工作。 當參照連結將您導向概念性主題或次檢查清單時，請在檢閱概念性主題或完成次檢查清單中的工作之後返回此主題，以便您可以繼續執行此檢查清單中的其餘工作。

![執行同盟網頁 SSO 設計檢查清單的圖示。](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**檢查清單：執行同盟網頁 SSO 設計**

|Task|參考|
|--------|-------------|
|複習有關同盟網頁 SSO 設計的重要概念，並判斷您可以使用哪些 AD FS 部署目標來自訂此設計，以符合組織的需求。|![同盟網頁 SSO 設計連結的圖示，您可以在參考中使用該連結來執行同盟網頁 SSO 設計。同盟](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[網頁 SSO 設計](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dd807050(v=ws.11))<p>![識別您的 AD FS 部署目標連結的圖示，您可以在參考中使用該連結來執行同盟網頁 SSO 設計。](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[識別您的 AD FS 部署目標](../design/identifying-your-ad-fs-deployment-goals.md)<p>![規劃部署連結的圖示，您可以在參考中使用它來執行同盟網頁 SSO 設計。](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[規劃您的部署](../design/planning-your-deployment.md)|
|請參閱在組織中部署 AD FS 的硬體、軟體、憑證、網域名稱系統 \( DNS \) 、屬性存放區和用戶端需求。|![附錄 A：審核 AD FS 需求連結的圖示，您可以在參考中使用來執行同盟網頁 SSO 設計。](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[附錄 A：審查 AD FS 需求](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/ff678034(v=ws.11))|
|在兩個夥伴組織中部署 AD FS 之前，請先參閱有關宣告、宣告規則、屬性存放區和 AD FS 設定資料庫的重要概念。|![瞭解主要 AD FS 概念連結的圖示，您可以在參考中使用這些連結來執行同盟網頁 SSO 設計。](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[瞭解重要 AD FS 概念](../../ad-fs/technical-reference/Understanding-Key-AD-FS-Concepts.md)|
|根據您的設計計畫，在每個夥伴組織中安裝一部或多部同盟伺服器。 **注意：** 針對同盟網頁 SSO 設計，至少需要帳戶夥伴組織中至少一部同盟伺服器，以及在資源夥伴組織中至少有一部同盟伺服器。|![檢查清單的圖示：設定同盟伺服器連結，您可以在參考中用來執行同盟網頁 SSO 設計。](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[檢查清單：設定同盟伺服器](Checklist--Setting-Up-a-Federation-Server.md)|
|\(選擇性 \) ：決定您的組織是否需要同盟伺服器 proxy。 如果您的設計計畫呼叫 proxy，您可以在每個夥伴組織中安裝一或多個同盟伺服器 proxy。|![檢查清單的圖示：設定同盟伺服器 Proxy 連結您可以在參考中用來執行同盟網頁 SSO 設計。](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[檢查清單：設定同盟伺服器 Proxy](Checklist--Setting-Up-a-Federation-Server-Proxy.md)|
|根據您的設計計畫來共用憑證、設定用戶端及設定兩個夥伴組織之間的信任關係，讓他們可以透過同盟信任來通訊。|![檢查清單的圖示：設定帳戶夥伴組織連結您可以在參考中用來執行同盟網頁 SSO 設計。](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[檢查清單：設定帳戶夥伴組織](Checklist--Configuring-the-Account-Partner-Organization.md)<p>![檢查清單的圖示：設定您可以在參考中用來執行同盟網頁 SSO 設計的「資源夥伴組織」連結。](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[檢查清單：設定資源夥伴組織](Checklist--Configuring-the-Resource-Partner-Organization.md)|
|如果您是資源夥伴組織的系統管理員，宣告會 \- 使用 WIF 和 WIF SDK 來啟用您的網頁瀏覽器應用程式、web 服務應用程式或 Microsoft &reg; Office SharePoint &reg; Server 應用程式。|![您可以在參考中用來執行同盟網頁 SSO 設計的 Windows Identity Foundation 連結圖示。](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Windows Identity Foundation](https://go.microsoft.com/fwlink/?LinkId=122266)<p>![同盟網頁 sso](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Windows Identity FOUNDATION SDK](https://go.microsoft.com/fwlink/?LinkId=122266)|
