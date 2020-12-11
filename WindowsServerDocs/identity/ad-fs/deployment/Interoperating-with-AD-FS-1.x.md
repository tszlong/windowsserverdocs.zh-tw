---
description: 深入瞭解：與 AD FS 1.x 交互操作
ms.assetid: 97999892-29c6-4076-be19-5e5259d8ada6
title: 與 AD FS 1.x 互通
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.author: billmath
ms.openlocfilehash: 03bc60daaa456dba97aa1b0e210aed223a0c24a7
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97043356"
---
# <a name="interoperating-with-ad-fs-1x"></a>與 AD FS 1.x 互通

適用于 \( \) Windows Server 2012 中 Active Directory 同盟服務 AD FS &reg; 和 AD FS 1 之間的互通性。*x*，視組織的需求而定，完成下列一或多項工作：

-   規劃 Windows Server 2012 與舊版 AD FS AD FS 之間的互通性，並深入瞭解名稱識別碼宣告類型。 如需詳細資訊，請參閱 [規劃 AD FS 1.x 的互通性](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/ff678040(v=ws.11))。

-   如果您要從 Windows Server 2012 中 AD FS 同盟服務傳送宣告，AD FS 1 可以取用。*x* 同盟服務，請參閱 [檢查清單：設定 AD FS 將宣告傳送至 AD FS 1.x 同盟服務](Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Federation-Service.md)。

-   如果您要從 Windows Server 2012 中的 AD FS 同盟服務傳送宣告，而該應用程式是由執行 AD FS 1 的 Web 服務器所裝載的應用程式所使用。*x* 宣告 \- 感知的 web 代理程式，請參閱 [檢查清單：設定 AD FS，將宣告傳送至 AD FS 1.X 宣告感知的 web 代理程式](Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Claims-Aware-Web-Agent.md)。

-   如果您將從 AD FS 1 傳送宣告。*x* 同盟服務可供 Windows Server 2012 中的 AD FS 同盟服務使用，請參閱 [檢查清單：設定 AD FS 以從 AD FS 1.x 使用宣告](Checklist--Configuring-AD-FS--to-Consume-Claims-from-AD-FS-1.x.md)。

## <a name="differences-between-federation-service-settings"></a>同盟服務設定之間的差異
雖然大部分的 AD FS 1。*x* 同盟服務設定的運作方式與 Windows Server 2012 設定中的 AD FS 同盟服務類似，某些設定名稱已變更。 下表列出 AD FS 1 的設定名稱。*x* 同盟服務及其對等名稱，適用于 Windows Server 2012 中的 AD FS 同盟服務。

|AD FS 1.x 同盟服務設定|Windows Server 2012 設定中的對等 AD FS 同盟服務
|----------------------------------------|----------------------------------------------------------------------------------------------------------
|帳戶夥伴|宣告提供者信任
|資源夥伴|信賴憑證者信任
|應用程式|信賴憑證者信任
|Application Properties|信賴憑證者信任屬性
|應用程式 URL|信賴憑證者識別碼和 WS \- 同盟被動端點 URL
|同盟服務 URI|Federation Service 識別碼
|同盟服務端點 URL|WS \- 同盟被動端點 URL

## <a name="see-also"></a>另請參閱
[AD FS 和 AD FS 1.x 互通性](https://go.microsoft.com/fwlink/?LinkId=200776)

