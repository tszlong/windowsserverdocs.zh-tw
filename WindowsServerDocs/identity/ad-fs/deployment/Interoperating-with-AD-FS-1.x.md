---
ms.assetid: 97999892-29c6-4076-be19-5e5259d8ada6
title: 部署同盟伺服器
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: f80425b6f062040c51357353038fd07ff0a79ae6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812169"
---
# <a name="interoperating-with-ad-fs-1x"></a>與 AD FS 1.x 互通

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

Active Directory Federation Services 之間的互通性\(AD FS\)在 Windows Server® 2012年和 AD FS 1。*x*，完成一或多個下列的工作，視您組織的需求而定：  
  
-   規劃 Windows Server 2012 中的 AD FS 與舊版的 AD FS 之間的互通性，並了解名稱識別碼宣告類型。 如需詳細資訊，請參閱 <<c0> [ 規劃互通性與 AD FS 1.x](https://technet.microsoft.com/library/ff678040.aspx)。  
  
-   如果您將會從可供 AD FS 1 的 Windows Server 2012 中 AD FS 同盟服務傳送宣告。*x* Federation Service，請參閱[檢查清單：設定 AD FS 來傳送至 AD FS 1.x Federation Service 的宣告](Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Federation-Service.md)。  
  
-   如果您將會從可供執行 AD FS 1 之 Web 伺服器所裝載的應用程式的 Windows Server 2012 中 AD FS 同盟服務傳送宣告。*x*宣告\-感知網路代理程式，請參閱[檢查清單：設定 AD FS 以將宣告傳送至 AD FS 1.x Claims-aware Web 代理程式](Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Claims-Aware-Web-Agent.md)。  
  
-   如果您將從 AD FS 1 傳送宣告。*x* Federation Service 到由 AD FS 同盟服務在 Windows Server 2012，請參閱[檢查清單：設定 AD FS 使用宣告從 AD FS 1.x](Checklist--Configuring-AD-FS--to-Consume-Claims-from-AD-FS-1.x.md)。  
  
## <a name="differences-between-federation-service-settings"></a>Federation Service 設定之間的差異  
雖然大部分的 AD FS 1。*x* Federation Service AD FS 同盟服務，在 Windows Server 2012 設定類似的方式設定工作，有些設定名稱已變更。 下表列出設定 AD FS 1 的名稱。*x* Federation Service 和其對等名稱使用於 Windows Server 2012 中 AD FS 同盟服務。  
  
|AD FS 1.x Federation Service 設定|對等設定 Windows Server 2012 中 AD FS 同盟服務  
|----------------------------------------|---------------------------------------------------------------------------------------------------------- 
|帳戶夥伴|宣告提供者信任  
|資源夥伴|信賴憑證者信任 
|應用程式|信賴憑證者信任  
|應用程式屬性|信賴憑證者的合作對象信任屬性  
|應用程式 URL|信賴憑證者的合作對象識別項和 WS\-同盟被動端點 URL  
|Federation Service URI|Federation Service 識別碼  
|Federation Service 端點 URL|WS\-同盟被動端點 URL  
  
## <a name="see-also"></a>另請參閱  
[AD FS 和 AD FS 1.x 互通性](https://go.microsoft.com/fwlink/?LinkId=200776)  
  

