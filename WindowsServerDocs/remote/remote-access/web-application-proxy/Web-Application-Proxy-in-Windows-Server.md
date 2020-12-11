---
description: 深入瞭解： Windows Server 中的 Web 應用程式 Proxy
ms.assetid: 0b3587b2-219f-43d8-88b4-1254eaa8b910
title: Windows Server 中的 Web 應用程式 Proxy
ms.topic: article
ms.author: kgremban
author: eross-msft
ms.openlocfilehash: c95f47d057008249ca2ebc852e97b68f90d604e8
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97046466"
---
# <a name="web-application-proxy-in-windows-server"></a>Windows Server 中的 Web 應用程式 Proxy

>適用於︰Windows Server&reg; 2016

**此內容與內部部署版本的 Web 應用程式 Proxy 相關。若要透過雲端啟用對內部部署應用程式的安全存取，請參閱 [Azure AD 應用程式 Proxy 內容](/azure/active-directory/manage-apps/application-proxy)。**

本節內容說明 Windows Server 2016 的 Web 應用程式 Proxy 的新功能和變更。 此處所列的新功能和變更，最可能會在您使用預覽版時產生最大影響。

## <a name="web-application-proxy-new-features"></a>Web 應用程式 Proxy 的新功能

- HTTP 基本應用程式發佈的預先驗證

  HTTP Basic 是許多通訊協定（包括 ActiveSync）使用的授權通訊協定，可將豐富的用戶端（包括智慧型手機）連接到您的 Exchange 信箱。 Web 應用程式 Proxy 傳統上會使用在 ActiveSync 用戶端上不支援的重新導向來與 AD FS 互動。 這個新版本的 Web 應用程式 Proxy 可讓 HTTP 應用程式接收應用程式對同盟服務的非宣告信賴憑證者信任，以支援使用 HTTP basic 發佈應用程式。

  如需有關 HTTP 基本發佈的詳細資訊，請參閱 [使用 AD FS 預先驗證發行應用程式](../web-application-proxy/../web-application-proxy/Publishing-Applications-using-AD-FS-Preauthentication.md)

- 應用程式的萬用字元網域發佈

  若要支援 SharePoint 2013 之類的案例，應用程式的外部 URL 現在可以包含萬用字元，讓您可以從特定網域內發佈多個應用程式，例如 HTTPs：//*。 這可簡化 SharePoint 應用程式的發佈。

- HTTP 至 HTTPS 的重新導向

  為了確保您的使用者可以存取您的應用程式，即使他們不想在 URL 中輸入 HTTPS，Web 應用程式 Proxy 現在也支援 HTTP 到 HTTPS 重新導向。

- HTTP 發行

  現在可以使用傳遞預先驗證發佈 HTTP 應用程式

- 遠端桌面閘道應用程式的發佈

  如需在 Web 應用程式 Proxy 中 RDG 的詳細資訊，請參閱 [使用 SharePoint 和 Exchange 和 RDG 發行應用程式](../web-application-proxy/Publishing-Applications-with-SharePoint,-Exchange-and-RDG.md)

- 新的偵錯工具記錄檔，可進行更佳的疑難排解和改進的服務記錄檔，以取得完整的審核記錄和

  如需疑難排解的詳細資訊，請參閱 [Web 應用程式 Proxy 疑難排解](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn770156(v=ws.11))

- 系統管理員主控台 UI 改進

- 將用戶端 IP 位址傳播至後端應用程式

## <a name="see-also"></a>另請參閱

-   [Windows Server 2016 中的新功能](../../../get-started/whats-new-in-windows-server-2016.md)

-   [使用 AD FS 預先驗證發佈應用程式](../web-application-proxy/Publishing-Applications-using-AD-FS-Preauthentication.md)

-   [對 Web 應用程式 Proxy 進行疑難排解](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn770156(v=ws.11))

