---
ms.assetid: c4d83dd3-2846-4658-8b9c-93901ee69766
title: 部署同盟伺服器
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 9d04cfa134d99a03e8823366d17932296cdfdfb8
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86964200"
---
# <a name="deploying-federation-servers"></a>部署同盟伺服器

若要在 Active Directory 同盟服務 AD FS 中部署同盟伺服器 \( \) ，請完成[檢查清單：設定同盟伺服器](Checklist--Setting-Up-a-Federation-Server.md)中的每個工作。  
  
> [!NOTE]  
> 當您使用此檢查清單時，我們建議您先閱讀[Windows server 2012 中 AD FS 設計指南中](../design/ad-fs-design-guide-in-windows-server-2012.md)的同盟伺服器規劃參考，然後再開始設定伺服器的程式。 以這種方式遵循檢查清單，可讓您更瞭解同盟伺服器的設計和部署程式。  
  
## <a name="about-federation-servers"></a>關於同盟伺服器  
同盟伺服器是執行 Windows Server 2008 的電腦，其中安裝的 AD FS 軟體已設定為可在同盟伺服器角色中運作。 同盟伺服器會從其他組織中的使用者帳戶，以及可位於網際網路任何位置的用戶端電腦，進行驗證或路由傳送要求。  
  
在電腦上安裝 AD FS 軟體，並使用 [AD FS 同盟伺服器設定] Wizard 針對同盟伺服器角色進行設定的動作，會使該電腦成為同盟伺服器。 此外，也會 \- 在 [**啟動系統 \\ 管理工具 \\ ** ] 功能表中，讓該電腦上的 [AD FS 管理] 嵌入式管理單元可供您指定下列各項：  
  
-   夥伴組織和應用程式將傳送權杖要求和回應的 AD FS 主機名稱  
  
-   夥伴組織和應用程式將用來識別貴組織唯一名稱或位置的 AD FS 識別碼  
  
-   伺服器陣列 \- 中所有同盟伺服器將用來發出和簽署權杖的權杖簽署憑證  
  
-   適用于用戶端登入、登出和帳戶夥伴探索的自訂 ASP.NET 網頁的位置，可增強用戶端體驗  
  
    > [!NOTE]  
    > 大部分的核心使用者介面 \( UI \) 設定都包含在每部同盟伺服器上的 web.config 檔案中。 web.config 檔案中未指定 AD FS 主機名稱和 AD FS 識別碼值。  
  
同盟伺服器裝載宣告發行引擎，會根據認證發出權杖，例如 \( ，提供給它的使用者名稱和密碼 \) 。 安全性權杖是以密碼編譯方式簽署的資料單位，可表示一或多個宣告。 宣告是伺服器建立的語句，例如 \( 名稱、身分識別、金鑰、群組、許可權或 \) 用戶端的功能。 透過使用者登入程式在同盟伺服器上驗證認證之後 \( \) ，系統會透過檢查儲存在指定之屬性存放區中的使用者屬性來收集使用者的宣告。  
  
在同盟 Web 單一 \- 登錄 \- \( SSO \) 設計中 \( ，AD FS 會牽涉到兩個或多個組織的設計 \) ，您可以透過特定信賴憑證者的宣告規則來修改宣告。 宣告會內建到權杖中，並傳送至資源夥伴組織中的同盟伺服器。 在資源夥伴中的同盟伺服器接收宣告做為傳入宣告之後，它會執行宣告發行引擎來執行一組宣告規則，以篩選、傳遞或轉換這些宣告。 然後，宣告會內建到新的權杖中，並傳送至資源夥伴中的 Web 服務器。  
  
在網頁 SSO 設計中 \( ，只涉及一個組織的 AD FS 設計 \) ，可以使用單一同盟伺服器，讓員工能夠登入一次，而且仍然可以存取多個應用程式。  
  
