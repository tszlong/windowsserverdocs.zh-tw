---
ms.assetid: c4d83dd3-2846-4658-8b9c-93901ee69766
title: 部署同盟伺服器
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 225e6b52ed46eef6f2ccacb5b0d9c6e6d880f475
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847169"
---
# <a name="deploying-federation-servers"></a>部署同盟伺服器

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

若要部署同盟伺服器在 Active Directory Federation Services \(AD FS\)，完成每一項工作中[檢查清單：設定同盟伺服器](Checklist--Setting-Up-a-Federation-Server.md)。  
  
> [!NOTE]  
> 當您使用這份檢查清單時，我們建議您先閱讀參考同盟伺服器中的 < 規劃[Windows Server 2012 中 AD FS 設計指南](https://technet.microsoft.com/library/dd807036.aspx)開始設定伺服器的程序之前。 下列檢查清單，如此一來，以提供進一步了解同盟伺服器的設計和部署程序。  
  
## <a name="about-federation-servers"></a>有關同盟伺服器  
同盟伺服器是執行 Windows Server 2008 搭配 AD FS 軟體安裝的電腦已設定以做為同盟伺服器角色。 同盟伺服器進行驗證，或從其他組織中的使用者帳戶及可以位於任何地方在網際網路的用戶端電腦的要求路由傳送。  
  
在電腦上安裝 AD FS 軟體，並使用 AD FS 同盟伺服器設定精靈來設定同盟伺服器角色的動作，就會在同盟伺服器。 它也會使 AD FS 管理嵌入式管理單元\-中在該電腦上可用**開始\\系統管理工具\\**功能表，讓您可以指定下列項目：  
  
-   AD FS 主機名稱，其中夥伴組織和應用程式會傳送權杖的要求和回應  
  
-   夥伴組織和應用程式的 AD FS 識別碼會使用來識別唯一的名稱或您組織的位置  
  
-   語彙基元\-簽署問題並登入的權杖會使用伺服器陣列中的所有同盟伺服器的憑證  
  
-   自訂 ASP.NET 網頁的用戶端登入、 登出和加強的用戶端體驗的帳戶夥伴探索的位置  
  
    > [!NOTE]  
    > 大部分的這些核心使用者介面\(UI\)設定包含在每部同盟伺服器上的 web.config 檔案。 Web.config 檔案中未指定的 AD FS 的主機名稱和 AD FS 的識別碼值。  
  
同盟伺服器裝載發行認證為基礎的權杖宣告發行引擎\(例如，使用者名稱和密碼\)，呈現給它。 安全性權杖是密碼編譯簽署的資料單位，表示一個或多個宣告。 宣告是伺服器對陳述式\(，例如名稱、 識別、 金鑰、 群組、 權限或功能\)有關用戶端。 在同盟伺服器上驗證的認證後\(使用者登入程序透過\)，檢查指定的屬性存放區中儲存的使用者屬性收集使用者的宣告。  
  
在 同盟網頁單一\-登\-上\(SSO\)設計\(中所涉及兩個或多個組織的 AD FS 設計\)，宣告就可以修改的特定信賴憑證者的宣告規則合作對象。 內建宣告傳送到資源夥伴組織中的同盟伺服器的語彙基元。 在資源夥伴的同盟伺服器收到的宣告與連入宣告之後，它會執行宣告發行引擎來執行一組宣告規則來篩選、 通過，或將這些宣告轉換。 宣告然後內建傳送到資源夥伴中的 Web 伺服器的新語彙基元。  
  
在網頁 SSO 設計\(牽涉到只在一個組織中的 AD FS 設計\)，在單一同盟伺服器可以使用，讓員工可以登入一次，並仍然存取多個應用程式。  
  
