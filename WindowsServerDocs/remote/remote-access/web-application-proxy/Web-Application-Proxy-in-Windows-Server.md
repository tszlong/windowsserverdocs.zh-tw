---
ms.assetid: 0b3587b2-219f-43d8-88b4-1254eaa8b910
title: Windows Server 中的 Web 應用程式 Proxy
ms.topic: article
ms.author: kgremban
author: eross-msft
ms.openlocfilehash: 433ab4825ff23526b656949c0df94befd892e29e
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87939739"
---
# <a name="web-application-proxy-in-windows-server"></a>Windows Server 中的 Web 應用程式 Proxy

>適用於︰Windows Server&reg; 2016

**此內容與內部部署版本的 Web 應用程式 Proxy 相關。若要在雲端上啟用內部部署應用程式的安全存取，請參閱[Azure AD 應用程式 Proxy 內容](/azure/active-directory/manage-apps/application-proxy)。**

本節中的內容說明 Windows Server 2016 Web 應用程式 Proxy 的新功能和變更。 此處所列的新功能和變更，是當您使用預覽時，最可能有最大的影響。

## <a name="web-application-proxy-new-features"></a>Web 應用程式 Proxy 的新功能

- HTTP 基本應用程式發佈的預先驗證

  HTTP Basic 是由許多通訊協定所使用的授權通訊協定（包括 ActiveSync），可透過您的 Exchange 信箱連接豐富型用戶端（包括 smartphone）。 Web 應用程式 Proxy 傳統上會與使用重新導向的 AD FS 互動，而 ActiveSync 用戶端不支援這種方式。 這個新版本的 Web 應用程式 Proxy 支援使用 HTTP basic 來發行應用程式，方法是啟用 HTTP 應用程式，以接收應用程式對同盟服務的非宣告信賴憑證者信任。

  如需有關 HTTP 基本發行的詳細資訊，請參閱[使用 AD FS 預先驗證發佈應用程式](../web-application-proxy/../web-application-proxy/Publishing-Applications-using-AD-FS-Preauthentication.md)

- 應用程式的萬用字元網域發佈

  為了支援 SharePoint 2013 這類案例，應用程式的外部 URL 現在可以包含萬用字元，讓您可以從特定網域內發佈多個應用程式，例如 HTTPs：//*. sp-apps. .com。 這可簡化 SharePoint 應用程式的發佈。

- HTTP 至 HTTPS 的重新導向

  為了確保您的使用者可以存取您的應用程式，即使他們不想在 URL 中輸入 HTTPS，Web 應用程式 Proxy 現在也支援 HTTP 至 HTTPS 重新導向。

- HTTP 發佈

  現在可以使用傳遞預先驗證發行 HTTP 應用程式

- 發行遠端桌面閘道應用程式

  如需有關在 Web 應用程式 Proxy 中 RDG 的詳細資訊，請參閱[使用 SharePoint、Exchange 和 RDG 發行應用程式](../web-application-proxy/Publishing-Applications-with-SharePoint,-Exchange-and-RDG.md)

- 新的偵錯工具記錄檔，以取得完整的審核記錄及改良的錯誤處理，以進行更好的疑難排解和改良

  如需有關疑難排解的詳細資訊，請參閱[疑難排解 Web 應用程式 Proxy](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn770156(v=ws.11))

- 系統管理員主控台的 UI 改良功能

- 將用戶端 IP 位址傳播至後端應用程式

## <a name="see-also"></a>另請參閱

-   [Windows Server 2016 中的新功能](../../../get-started/whats-new-in-windows-server-2016.md)

-   [使用 AD FS 預先驗證發佈應用程式](../web-application-proxy/Publishing-Applications-using-AD-FS-Preauthentication.md)

-   [對 Web 應用程式 Proxy 進行疑難排解](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn770156(v=ws.11))

