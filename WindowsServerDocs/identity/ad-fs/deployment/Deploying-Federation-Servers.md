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
ms.openlocfilehash: 9e66c513c34658c40152cd7b622a1818e7198e47
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855561"
---
# <a name="deploying-federation-servers"></a>部署同盟伺服器

若要在 Active Directory 同盟服務 \(AD FS\)中部署同盟伺服器，請完成[檢查清單：設定同盟伺服器](Checklist--Setting-Up-a-Federation-Server.md)中的每個工作。  
  
> [!NOTE]  
> 當您使用此檢查清單時，我們建議您先閱讀[Windows server 2012 中 AD FS 設計指南中](https://technet.microsoft.com/library/dd807036.aspx)的同盟伺服器規劃參考，然後再開始設定伺服器的程式。 以這種方式遵循檢查清單，可讓您更瞭解同盟伺服器的設計和部署程式。  
  
## <a name="about-federation-servers"></a>關於同盟伺服器  
同盟伺服器是執行 Windows Server 2008 的電腦，其中安裝的 AD FS 軟體已設定為可在同盟伺服器角色中運作。 同盟伺服器會從其他組織中的使用者帳戶，以及可位於網際網路任何位置的用戶端電腦，進行驗證或路由傳送要求。  
  
在電腦上安裝 AD FS 軟體，並使用 [AD FS 同盟伺服器設定] Wizard 針對同盟伺服器角色進行設定的動作，會使該電腦成為同盟伺服器。 此外，也會在 [**啟動\\系統管理工具]\\** 功能表中，讓該電腦上的 [AD FS 管理] 嵌入式管理單元\-可用，讓您可以指定下列各項：  
  
-   夥伴組織和應用程式將傳送權杖要求和回應的 AD FS 主機名稱  
  
-   夥伴組織和應用程式將用來識別貴組織唯一名稱或位置的 AD FS 識別碼  
  
-   伺服器陣列中所有同盟伺服器將用來發出和簽署權杖的權杖\-簽署憑證  
  
-   適用于用戶端登入、登出和帳戶夥伴探索的自訂 ASP.NET 網頁的位置，可增強用戶端體驗  
  
    > [!NOTE]  
    > 大部分的核心使用者介面 \(UI\) 設定都包含在每部同盟伺服器的 web.config 檔案中。 Web.config 檔案中未指定 AD FS 主機名稱和 AD FS 識別碼值。  
  
同盟伺服器裝載宣告發行引擎，會根據認證發出權杖，\(例如，提供給它的使用者名稱和密碼\)。 安全性權杖是以密碼編譯方式簽署的資料單位，可表示一或多個宣告。 宣告是伺服器 \(的語句，例如名稱、身分識別、金鑰、群組、許可權或與用戶端相關的功能\)。 在同盟伺服器上驗證認證之後 \(透過使用者登入程式\)，會透過檢查儲存在指定之屬性存放區中的使用者屬性來收集使用者的宣告。  
  
在同盟 Web 單一\-中，\-在 \(SSO\) 設計 \(AD FS 設計（其中涉及兩個或多個組織）\)時，可由特定信賴憑證者的宣告規則來修改宣告。 宣告會內建到權杖中，並傳送至資源夥伴組織中的同盟伺服器。 在資源夥伴中的同盟伺服器接收宣告做為傳入宣告之後，它會執行宣告發行引擎來執行一組宣告規則，以篩選、傳遞或轉換這些宣告。 然後，宣告會內建到新的權杖中，並傳送至資源夥伴中的 Web 服務器。  
  
在網頁 SSO 設計中 \(一個 AD FS 的設計，其中僅包含一個組織\)，可以使用單一同盟伺服器，讓員工能夠登入一次，而且仍然可以存取多個應用程式。  
  
