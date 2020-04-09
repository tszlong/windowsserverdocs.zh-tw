---
ms.assetid: e7f9e518-2d5d-4a0d-9147-34e1304f42ac
title: 檢查清單-設定 AD FS 以使用來自 AD FS 1.x 的宣告
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 0b3b58b158e6443cf4f0e2b9e09960f46d57ebbc
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854161"
---
# <a name="checklist-configuring-ad-fs--to-consume-claims-from-ad-fs-1x"></a>檢查清單：設定 AD FS 從 AD FS 1.x 使用宣告

  
## <a name="checklist-configuring-ad-fs-to-consume-claims-from-adfs1x"></a>檢查清單：設定 AD FS 從 AD FS 1.x 使用宣告  
此檢查清單包含在 Windows Server 2012 中設定 Active Directory 同盟服務 \(AD FS\) 同盟服務，以取用 AD FS 1 所傳送的宣告時，所需執行的工作。*x*同盟服務。  
  
> [!NOTE]  
> 請依序完成此檢查清單中的工作。 當某個參考連結帶您進入某個程序時，請在完成該程序中的步驟之後返回本主題，讓您可以繼續進行此檢查清單中其餘的工作。  
  
![從 AD FS 檢查清單中取用宣告](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**：將 AD FS 設定為使用來自 AD FS 1.x 的宣告**  
  
||工作|參考|  
|-|--------|-------------|  
|![使用來自 AD FS 的宣告](media/icon_checkboxo.gif)|規劃 Windows Server 2012 中 AD FS 與舊版 AD FS 之間的互通性，並深入瞭解名稱識別碼宣告類型。|![從 AD FS](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[規劃與 AD FS 1.x 的互通性](https://technet.microsoft.com/library/ff678040.aspx)使用宣告|  
|![使用來自 AD FS 的宣告](media/icon_checkboxo.gif)|在您可以與舊版 AD FS 互通之前，您必須先在 AD FS 同盟服務中建立宣告提供者信任。 **注意：** 您無法使用 AD FS 1 建立信任。使用同盟中繼資料的*x*同盟服務。<p>當您使用右側連結中的程式來設定信任時，您必須在 [新增宣告提供者信任] 嚮導中執行下列動作，以設定此信任與 AD FS 1 交互操作。*x*同盟服務：<p>1. 在 [**選取資料來源**] 頁面上，選取 **[手動輸入信賴憑證者信任的相關資料**]。<br />2. 在 [**選擇設定檔**] 頁面上，選取 [ **AD FS 1.0 和1.1 設定檔**]。<br />3. 在 [**設定 URL** ] 頁面的 [ **WS\-同盟] [被動 URL**] 底下，輸入 AD FS 1 中所定義的**同盟服務端點 URL** 。合作夥伴的*x*同盟服務。<br />4. 在 [**設定識別碼**] 頁面的 [**宣告提供者信任識別碼**] 底下，輸入 AD FS 1 中所定義的**同盟服務 URI** 。合作夥伴的*x*同盟服務。|![使用來自 AD FS](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[手動建立宣告提供者信任](../../ad-fs/operations/Create-a-Claims-Provider-Trust.md)的宣告|  
|![使用來自 AD FS 的宣告](media/icon_checkboxo.gif)|在您稍早建立的宣告提供者信任上，您必須建立一個宣告規則，以接受從 AD FS 1.x 同盟服務傳入的宣告，並將它們傳遞、篩選或轉換成名稱識別碼宣告類型。<p>當名稱識別碼宣告類型已通過、篩選或轉換時，可以用來做為另一個規則或規則的輸入，以便在 Windows Server 2012 中的 AD FS 同盟服務加以瞭解和使用。|![取用來自 AD FS 的宣告](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[，以建立規則來傳送 AD FS 1.X 相容](../../ad-fs/operations/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)宣告|  
|![使用來自 AD FS 的宣告](media/icon_checkboxo.gif)|請洽詢 AD FS 1 的系統管理員。*x*同盟服務並具有 AD FS 1 的系統管理員。*x*同盟服務設定新的資源夥伴信任。 此外，請在同盟服務屬性\)、同盟服務端點 URL 和匯出的權杖中，為系統管理員提供同盟服務 URI \(\-使用公開金鑰僅 \(簽署憑證檔案。\) 系統管理員將需要這些專案來設定信任。|N\/A|  
  

