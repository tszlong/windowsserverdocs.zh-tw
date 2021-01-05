---
ms.assetid: bbb5b68f-00ad-4715-8176-0c2769b706c4
title: 部署適用于 Windows Server 2012 R2 AD FS 的同盟伺服器陣列
description: 深入瞭解：部署同盟伺服器陣列
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: c7258a31d9fae23f4aa3b691d1fc34535d857d8b
ms.sourcegitcommit: 3247e193d9fe1b57543fff215460a6d9db52f58b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/30/2020
ms.locfileid: "97814927"
---
# <a name="deploying-a-federation-server-farm"></a>部署同盟伺服器陣列

若要部署同盟伺服器陣列，請依序完成此檢查清單中的工作。 當參考連結將您導向概念性主題時，請在檢閱此概念性主題之後傳回此檢查清單，以便繼續執行此檢查清單中的其餘工作。

![部署同盟伺服器陣列的圖示檢查清單。](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**檢查清單：部署同盟伺服器** 陣列

|Task|參考|
|--------|-------------|
|當您準備部署 Active Directory 同盟服務 AD FS 時，請參閱重要的概念和考慮 \( \) 。 **注意：**|![[Windows Server 2012 R2 中的 AD FS 設計指南] 連結的圖示，您可以在參考中使用來部署同盟伺服器陣列。](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Windows Server 2012 R2 中的 AD FS 設計指南](../../ad-fs/design/AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)<p>![瞭解重要 AD FS 概念連結的圖示，您可以在參考中使用這些連結來部署同盟伺服器陣列。](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[瞭解重要 AD FS 概念](../../ad-fs/technical-reference/Understanding-Key-AD-FS-Concepts.md)|
||如果您決定要將 Microsoft SQL Server 用於 AD FS 設定存放區，請確實部署可運作的 SQL Server 執行個體。|[SQL Server](/sql/sql-server/) **警告：** 在 Windows Server 2012 R2 中，如果您想要建立 AD FS 伺服器陣列，並使用 SQL Server 來儲存設定資料，可以使用 SQL Server 2008 和更新版本，包括 SQL Server 2012。|
|將您的電腦加入 Active Directory 網域中。|![將電腦加入網域連結的圖示，您可以在參考中用來部署同盟伺服器陣列。將](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[電腦加入網域](Join-a-Computer-to-a-Domain.md)|
|註冊 AD FS 的安全通訊端層 \( SSL \) 憑證。|![註冊 AD FS 連結之 SSL 憑證的圖示，您可以在參考中用來部署同盟伺服器陣列。](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[註冊 AD FS 的 SSL 憑證](Enroll-an-SSL-Certificate-for-AD-FS.md)|
|安裝 AD FS 角色服務。|![安裝「AD FS 角色服務」連結的圖示，您可以在參考中使用該連結來部署同盟伺服器陣列。](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[安裝 AD FS 角色服務](Install-the-AD-FS-Role-Service.md)|
|設定同盟伺服器。|![[設定同盟伺服器] 連結的圖示，您可以在 [參考] 中使用這些連結來部署同盟伺服器陣列。](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[設定同盟伺服器](Configure-a-Federation-Server.md)|
|選擇性步驟：使用裝置註冊服務 DRS 設定同盟伺服器 \( \) 。|![[使用裝置註冊服務設定同盟伺服器] 連結的圖示，您可以在參考中使用來部署同盟伺服器陣列。](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[使用裝置註冊服務設定同盟伺服器](Configure-a-federation-server-with-Device-Registration-Service.md)|
|將主機 \( a \) 和別名 \( CNAME \) 資源記錄新增至 \( \) 同盟服務和 DRS 的公司網域名稱系統 DNS。|![針對同盟服務和 DRS 連結設定公司 DNS 的圖示，您可以在參考中使用該連結來部署同盟伺服器陣列。](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[設定同盟服務和 DRS 的公司 DNS](Configure-Corporate-DNS-for-the-Federation-Service-and-DRS.md)|
|確認該同盟伺服器運作正常。|![部署同盟伺服器陣列](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[會確認同盟伺服器可運作](Verify-That-a-Federation-Server-Is-Operational.md)|


## <a name="see-also"></a>另請參閱
[AD FS 部署](../../ad-fs/AD-FS-Deployment.md)

[Windows Server 2012 R2 AD FS 部署指南](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)
