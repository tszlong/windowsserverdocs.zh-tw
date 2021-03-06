---
description: 深入瞭解：部署同盟伺服器
ms.assetid: c4d83dd3-2846-4658-8b9c-93901ee69766
title: 部署同盟伺服器
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.author: billmath
ms.openlocfilehash: d8342fe7e884c856bf687a0deda6b174b94355b9
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97044936"
---
# <a name="deploying-federation-servers"></a>部署同盟伺服器

若要在 Active Directory 同盟服務 AD FS 中部署同盟伺服器 \( \) ，請完成檢查清單中的每個工作 [：設定同盟伺服器](Checklist--Setting-Up-a-Federation-Server.md)。

> [!NOTE]
> 當您使用此檢查清單時，建議您先閱讀 [Windows server 2012 中 AD FS 設計指南](../design/ad-fs-design-guide-in-windows-server-2012.md) 中的同盟伺服器規劃參考，再開始設定伺服器的程式。 以這種方式遵循檢查清單，可讓您更瞭解同盟伺服器的設計和部署流程。

## <a name="about-federation-servers"></a>關於同盟伺服器
同盟伺服器是執行 Windows Server 2008 的電腦，其安裝的 AD FS 軟體已設定為在同盟伺服器角色中運作。 同盟伺服器會從其他組織中的使用者帳戶，以及可以位於網際網路任何位置的用戶端電腦，驗證或路由傳送要求。

將 AD FS 軟體安裝在電腦上，並使用 AD FS 同盟伺服器設定向導為同盟伺服器角色進行設定的動作，會讓該電腦成為同盟伺服器。 它也會 \- 在 [**開始系統 \\ 管理工具 \\** ] 功能表中，讓該電腦上的 AD FS 管理嵌入式管理單元可供使用，讓您可以指定下列各項：

-   AD FS 的主機名稱，夥伴組織和應用程式會在其中傳送權杖要求和回應

-   夥伴組織和應用程式將用來識別組織唯一名稱或位置的 AD FS 識別碼

-   伺服器陣列 \- 中所有同盟伺服器將用來發出和簽署權杖的權杖簽署憑證

-   用戶端登入、登出及帳戶夥伴探索的自訂 ASP.NET 網頁位置，可增強用戶端體驗

    > [!NOTE]
    > 這些核心使用者介面 UI 的大部分 \( \) 設定都包含在每部同盟伺服器上的 web.config 檔案中。 web.config 檔案中未指定 AD FS 主機名稱和 AD FS 識別碼值。

同盟伺服器會裝載宣告發行引擎，此引擎會根據認證來發出權杖 \( ，例如，向它提供的使用者名稱和密碼 \) 。 安全性權杖是以密碼編譯方式簽署的資料單位，可表示一或多個宣告。 宣告是伺服器所建立的語句 \( ，例如，名稱、身分識別、金鑰、群組、許可權或 \) 用戶端的功能。 透過使用者登入程式在同盟伺服器上驗證認證之後 \( \) ，系統會透過檢查儲存在指定屬性存放區中的使用者屬性來收集使用者的宣告。

在同盟網頁單一 \- 登錄 \- \( （SSO \) 設計 \( ）中，有兩個或多個組織參與的 AD FS 設計 \) ，可讓特定信賴憑證者的宣告規則修改宣告。 宣告內建于權杖中，會傳送至資源夥伴組織中的同盟伺服器。 在資源夥伴中的同盟伺服器收到宣告做為傳入宣告之後，它會執行宣告發行引擎來執行一組宣告規則，以篩選、傳遞或轉換這些宣告。 然後會將宣告內建至新的權杖，以傳送至資源夥伴中的網頁伺服器。

在網頁 SSO 設計 \( 中，僅牽涉到一個組織的 AD FS 設計 \) ，可以使用單一同盟伺服器，讓員工可以一次登入，但仍然可以存取多個應用程式。

