---
ms.assetid: 97999892-29c6-4076-be19-5e5259d8ada6
title: 部署同盟伺服器
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 800c7fc23c9b126a17e54311fc6df0d3dcf36b4c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855361"
---
# <a name="interoperating-with-ad-fs-1x"></a>與 AD FS 1.x 互通

在 Windows Server&reg; 2012 和 AD FS 1 中，Active Directory 同盟服務 \(AD FS\) 之間的互通性。*x*，視您組織的需求而定，完成下列一或多項工作：  
  
-   規劃 Windows Server 2012 中 AD FS 與舊版 AD FS 之間的互通性，並深入瞭解名稱識別碼宣告類型。 如需詳細資訊，請參閱[規劃與 AD FS 1.x 的互通性](https://technet.microsoft.com/library/ff678040.aspx)。  
  
-   如果您要從 Windows Server 2012 中的 AD FS 同盟服務傳送宣告，可由 AD FS 1 使用。*x*同盟服務，請參閱[檢查清單：設定 AD FS 以將宣告傳送至 AD FS 1.x 同盟服務](Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Federation-Service.md)。  
  
-   如果您要從 Windows Server 2012 中的 AD FS 同盟服務傳送宣告，而該應用程式是由執行 AD FS 1 的 Web 服務器所主控。*x*宣告\-感知 Web 代理程式，請參閱[檢查清單：設定 AD FS 以將宣告傳送至 AD FS 1.X 宣告感知 Web 代理程式](Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Claims-Aware-Web-Agent.md)。  
  
-   如果您將從 AD FS 1 傳送宣告。*x*同盟服務若要供 Windows Server 2012 中的 AD FS 同盟服務使用，請參閱[檢查清單：設定 AD FS 以取用來自 AD FS](Checklist--Configuring-AD-FS--to-Consume-Claims-from-AD-FS-1.x.md)1.x 的宣告。  
  
## <a name="differences-between-federation-service-settings"></a>同盟服務設定之間的差異  
雖然大部分 AD FS 1。*x*同盟服務設定的工作方式類似于 Windows Server 2012 設定中的 AD FS 同盟服務，某些設定名稱已變更。 下表列出 AD FS 1 的設定名稱。*x*同盟服務和其對等名稱，適用于 Windows Server 2012 中的 AD FS 同盟服務。  
  
|AD FS 1.x 同盟服務設定|Windows Server 2012 設定中的對等 AD FS 同盟服務  
|----------------------------------------|---------------------------------------------------------------------------------------------------------- 
|帳戶夥伴|宣告提供者信任  
|資源夥伴|信賴憑證者信任 
|應用程式|信賴憑證者信任  
|應用程式屬性|信賴憑證者信任屬性  
|應用程式 URL|信賴憑證者識別碼和 WS\-同盟被動端點 URL  
|同盟服務 URI|Federation Service 識別碼  
|同盟服務端點 URL|WS\-同盟被動端點 URL  
  
## <a name="see-also"></a>另請參閱  
[AD FS 和 AD FS 1.x 互通性](https://go.microsoft.com/fwlink/?LinkId=200776)  
  

