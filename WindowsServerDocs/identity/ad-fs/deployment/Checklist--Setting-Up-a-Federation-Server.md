---
ms.assetid: 8f954004-40d5-4c5e-8e0d-e8700c8ec7b1
title: 檢查清單-設定同盟伺服器
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 8fed51679ecabd883cfdcdf01523e123a1ca5243
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408515"
---
# <a name="checklist-setting-up-a-federation-server"></a>檢查清單：設定同盟伺服器

此檢查清單包含為 Active Directory 同盟服務 @no__t 0AD FS @ no__t-1 中的同盟伺服器角色準備執行 Windows Server®2012的伺服器所需的部署工作。  
  
> [!NOTE]  
> 請依序完成此檢查清單中的工作。 當某個參考連結帶您進入某個程序時，請在完成該程序中的步驟之後返回本主題，讓您可以繼續進行此檢查清單中其餘的工作。  
  
![setting up a federation server @ no__t-1Checklist：設定同盟伺服器 @ no__t-0  
  
||工作|參考資料|  
|-|--------|-------------|  
|![設定同盟伺服器](media/icon_checkboxo.gif)|開始部署 AD FS 同盟伺服器之前，請先參閱;1. \) 選擇 Windows Internal Database \(WID @ no__t-2 或 SQL Server 儲存 AD FS 設定資料庫2的優點和缺點。 \)AD FS 部署拓撲類型及其相關聯的伺服器位置和網路設定建議。|@no__t-向上0setting 同盟伺服器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[決定您的 AD FS 部署拓撲](https://technet.microsoft.com/library/gg982491.aspx)<br /><br />![setting 同盟伺服器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[AD FS 部署拓撲考慮](https://technet.microsoft.com/library/gg982489.aspx)|  
|![設定同盟伺服器](media/icon_checkboxo.gif)|請參閱 AD FS 容量規劃指南，以判斷您應該在生產環境中使用的適當同盟伺服器數目。|![setting](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[同盟伺服器容量規劃](https://technet.microsoft.com/library/gg749917.aspx)|  
|![設定同盟伺服器](media/icon_checkboxo.gif)|請參閱 AD FS 設計指南中的資訊，瞭解如何將同盟伺服器放置在您的組織中|![setting](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[同盟伺服器規劃同盟伺服器位置](https://technet.microsoft.com/library/dd807069.aspx)<br /><br />![setting](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[要放置同盟伺服器](https://technet.microsoft.com/library/dd807127.aspx)的聯盟伺服器|  
|![設定同盟伺服器](media/icon_checkboxo.gif)|判斷待命 @ no__t-0alone 同盟伺服器或同盟伺服器陣列是否較適合您的部署。|@no__t-](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[在建立同盟伺服器時](https://technet.microsoft.com/library/dd807101.aspx)0setting 同盟伺服器<br /><br />@no__t-](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[在建立同盟伺服器陣列時](https://technet.microsoft.com/library/dd807062.aspx)，0setting|  
|![設定同盟伺服器](media/icon_checkboxo.gif)|判斷是否要在帳戶夥伴組織中或資源夥伴組織中建立此新的同盟伺服器。|@no__t-向上 0setting](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[同盟伺服器檢查帳戶夥伴中的同盟伺服器角色](https://technet.microsoft.com/library/dd807117.aspx)<br /><br />@no__t-向上 0setting](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[同盟伺服器檢查資源夥伴中的同盟伺服器角色](https://technet.microsoft.com/library/dd807065.aspx)|  
|![設定同盟伺服器](media/icon_checkboxo.gif)|請參閱同盟伺服器如何使用服務通訊憑證和權杖 @ no__t-0signing 憑證的相關資訊，以安全地驗證用戶端和同盟伺服器 proxy 要求。 **注意**：雖然使用具有不合格主機名稱的憑證（例如 HTTPs： \/ @ no__t-1myserver）很長，但這些憑證沒有安全性價值，而且可以讓攻擊者模擬企業用戶端的 AD FS 同盟服務. 因此，建議您使用完整功能變數名稱 \(FQDN @ no__t-1，例如 HTTPs： \/\/myserver.contoso.com，而且只會使用發行至同盟服務 FQDN 的 SSL 憑證。|![setting 同盟伺服器的聯盟伺服器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[證書需求](https://technet.microsoft.com/library/dd807040.aspx)|  
|![設定同盟伺服器](media/icon_checkboxo.gif)|請參閱有關如何更新公司網路網域名稱系統 \(DNS @ no__t-1 的資訊，以便能夠成功地解析同盟伺服器的名稱。|![setting 同盟伺服器的聯盟伺服器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[名稱解析需求](https://technet.microsoft.com/library/dd807041.aspx)|  
|![設定同盟伺服器](media/icon_checkboxo.gif)|將即將成為同盟伺服器的電腦加入至帳戶夥伴樹系或資源夥伴樹系中的網域，以便用來驗證該樹系的使用者或信任的樹系。 **注意：** 如果您想要在帳戶夥伴組織中設定同盟伺服器，電腦必須先聯結至樹系中的任何網域，而您的同盟伺服器將用來驗證來自該樹系的使用者，或來自信任的樹系。|@no__t-向上0setting 同盟伺服器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[將電腦加入網域](Join-a-Computer-to-a-Domain.md)|  
|![設定同盟伺服器](media/icon_checkboxo.gif)|在公司網路 DNS 中建立新的資源記錄，將同盟伺服器的 DNS 主機名稱指向同盟伺服器的 IP 位址。|@no__t-向上0setting 同盟伺服器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[將&#40;主機 a&#41;資源記錄新增至同盟伺服器的公司 DNS](Add-a-Host--A--Resource-Record-to-Corporate-DNS-for-a-Federation-Server.md)|  
|![設定同盟伺服器](media/icon_checkboxo.gif)|\(Optional @ no__t-1 如果要將同盟伺服器新增至同盟伺服器陣列，您可能必須先匯出現有 token @ no__t-2signing 憑證的私密金鑰，\(on 伺服器陣列 @ no__t-4 中的第一部同盟伺服器，讓您當其他同盟伺服器必須匯入相同的憑證時，請備妥憑證的檔案格式。<br /><br />當您所發出的伺服器驗證憑證可以由多部電腦重複使用時，不需要匯出私密金鑰 \(without 需要匯出 @ no__t-1，或當您將取得每個的唯一伺服器驗證憑證時伺服器陣列中的同盟伺服器。 **注意：** AD FS Management snap @ no__t-0in 指的是同盟伺服器的伺服器驗證憑證，做為服務通訊憑證。|@no__t-向上0setting 同盟伺服器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[匯出伺服器驗證憑證的私密金鑰部分](Export-the-Private-Key-Portion-of-a-Server-Authentication-Certificate.md)|  
|![設定同盟伺服器](media/icon_checkboxo.gif)|在您從憑證授權單位單位 \(CA @ no__t-3 取得伺服器驗證憑證 \(or 私密金鑰 @ no__t-1 之後，您必須將憑證檔案匯入每部同盟伺服器的預設網站。 **注意：** 您必須先在預設的網站上安裝此憑證，才能使用 AD FS 同盟伺服器設定向導。|@no__t-向上0setting 同盟伺服器將](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[伺服器驗證憑證匯入到預設的網站](Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md)|  
|![設定同盟伺服器](media/icon_checkboxo.gif)|\(Optional @ no__t-1 做為從 CA 取得伺服器驗證憑證的替代方法，您可以使用 Internet Information Services \(IIS @ no__t-3 來建立同盟伺服器的範例憑證。 **注意**：使用自我 @ no__t-0signed 伺服器驗證憑證，在生產環境中部署同盟伺服器並不是安全性最佳作法。|![setting up a federation server @ no__t-1IIS：建立自我 @ no__t-0Signed 伺服器憑證 @ no__t-1，然後完成將[伺服器驗證憑證匯入至預設網站](Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md)的程式|  
|![設定同盟伺服器](media/icon_checkboxo.gif)|如果您要在帳戶夥伴組織中設定同盟伺服器陣列環境，您必須在伺服器陣列所在的 Active Directory Domain Services \(AD DS @ no__t-1 中建立並設定專屬的服務帳戶，並加以設定伺服器陣列中的同盟伺服器，以使用此帳戶。 藉由執行此程式，您將可讓公司網路上的用戶端使用 Windows 整合式驗證，向伺服器陣列中的任何同盟伺服器進行驗證。|![setting](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[同盟伺服器手動設定同盟伺服器陣列的服務帳戶](Manually-Configure-a-Service-Account-for-a-Federation-Server-Farm.md)|  
|![設定同盟伺服器](media/icon_checkboxo.gif)|在即將成為同盟伺服器的電腦上安裝同盟服務角色服務。|@no__t-向上0setting 同盟伺服器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[安裝同盟服務角色服務](Install-the-Federation-Service-Role-Service.md)|  
|![設定同盟伺服器](media/icon_checkboxo.gif)|使用 [AD FS 同盟伺服器設定] 嚮導，將電腦上的 AD FS 軟體設定為同盟伺服器角色。<br /><br />當您想要設定待命 @ no__t-0alone 同盟伺服器時，請遵循此程式，在新的伺服器陣列中建立第一部同盟伺服器，或將電腦加入現有的同盟伺服器陣列。 **注意：** 針對同盟 Web 單一登入 @ no__t-0On \(SSO @ no__t-2 設計，您必須在帳戶夥伴組織中至少有一部同盟伺服器，並在資源夥伴組織中至少要有一部同盟伺服器。|@no__t-向上 0setting](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[同盟伺服器建立獨立同盟伺服器](Create-a-Stand-Alone-Federation-Server.md)<br /><br />@no__t-向上 0setting](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[同盟伺服器在同盟伺服器陣列中建立第一部同盟伺服器](Create-the-First-Federation-Server-in-a-Federation-Server-Farm.md)<br /><br />@no__t-向上 0setting](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[同盟伺服器將同盟伺服器新增至同盟伺服器](Add-a-Federation-Server-to-a-Federation-Server-Farm.md)陣列|  
|![設定同盟伺服器](media/icon_checkboxo.gif)|\(Optional @ no__t-1 使用 AD FS 管理 snap @ no__t-2in 來新增和設定部署設計所需的必要 AD FS 憑證。 如需有關如何使用 snap @ no__t-0in 新增或變更憑證的詳細資訊，請參閱[同盟伺服器的憑證需求](https://technet.microsoft.com/library/dd807040.aspx)。|@no__t-向上0setting 同盟伺服器](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[新增權杖簽署憑證](Add-a-Token-Signing-Certificate.md)<br /><br />@no__t-向上0setting 同盟伺服器](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[新增權杖解密憑證](Add-a-Token-Decrypting-Certificate.md)<br /><br />@no__t-向上0setting 同盟伺服器](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[設定服務通訊憑證](Set-a-Service-Communications-Certificate.md)|  
|![設定同盟伺服器](media/icon_checkboxo.gif)|如果這是您組織中的第一部同盟伺服器，請設定同盟服務，使其符合您的 AD FS 設計。|![setting up a federation server @ no__t-1Checklist：設定帳戶夥伴組織 @ no__t-0<br /><br />![setting up a federation server @ no__t-1Checklist：設定資源夥伴組織 @ no__t-0|  
|![設定同盟伺服器](media/icon_checkboxo.gif)|從用戶端電腦，確認同盟伺服器可運作。|@no__t-向上 0setting](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[同盟伺服器，確認同盟伺服器可運作](Verify-That-a-Federation-Server-Is-Operational.md)| 
