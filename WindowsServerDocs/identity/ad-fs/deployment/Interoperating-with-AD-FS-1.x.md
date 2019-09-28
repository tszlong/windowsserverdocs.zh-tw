---
ms.assetid: 97999892-29c6-4076-be19-5e5259d8ada6
title: 部署同盟伺服器
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: f2aaca5ffc846c41af82c276750c564db38b5020
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359506"
---
# <a name="interoperating-with-ad-fs-1x"></a>與 AD FS 1.x 互通

如需 Active Directory 同盟服務 @no__t-Windows Server®2012和 AD FS 1 中的 0AD FS @ no__t-1 之間的互通性。*x*，視您組織的需求而定，完成下列一或多項工作：  
  
-   規劃 Windows Server 2012 中 AD FS 與舊版 AD FS 之間的互通性，並深入瞭解名稱識別碼宣告類型。 如需詳細資訊，請參閱[規劃與 AD FS 1.x 的互通性](https://technet.microsoft.com/library/ff678040.aspx)。  
  
-   如果您要從 Windows Server 2012 中的 AD FS 同盟服務傳送宣告，可由 AD FS 1 使用。*x*同盟服務，請參閱 [Checklist：設定 AD FS 將宣告傳送至 AD FS 1.x 同盟服務 @ no__t-0。  
  
-   如果您要從 Windows Server 2012 中的 AD FS 同盟服務傳送宣告，而該應用程式是由執行 AD FS 1 的 Web 服務器所主控。*x*宣告 @ no__t-1aware Web 代理程式，請參閱 [Checklist：設定 AD FS 將宣告傳送至 AD FS 1.x 宣告感知 Web 代理程式 @ no__t-0。  
  
-   如果您將從 AD FS 1 傳送宣告。*x*同盟服務若要供 Windows Server 2012 中的 AD FS 同盟服務使用，請參閱 [Checklist：設定 AD FS 以取用來自 AD FS 1.x @ no__t-0 的宣告。  
  
## <a name="differences-between-federation-service-settings"></a>同盟服務設定之間的差異  
雖然大部分 AD FS 1。*x*同盟服務設定的工作方式類似于 Windows Server 2012 設定中的 AD FS 同盟服務，某些設定名稱已變更。 下表列出 AD FS 1 的設定名稱。*x*同盟服務和其對等名稱，適用于 Windows Server 2012 中的 AD FS 同盟服務。  
  
|AD FS 1.x 同盟服務設定|Windows Server 2012 設定中的對等 AD FS 同盟服務  
|----------------------------------------|---------------------------------------------------------------------------------------------------------- 
|帳戶夥伴|宣告提供者信任  
|資源夥伴|信賴憑證者信任 
|應用程式|信賴憑證者信任  
|應用程式屬性|信賴憑證者信任屬性  
|應用程式 URL|信賴憑證者識別碼和 WS @ no__t-0Federation 被動端點 URL  
|同盟服務 URI|Federation Service 識別碼  
|同盟服務端點 URL|WS @ no__t-0Federation 被動端點 URL  
  
## <a name="see-also"></a>另請參閱  
[AD FS 和 AD FS 1.x 互通性](https://go.microsoft.com/fwlink/?LinkId=200776)  
  

