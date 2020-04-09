---
ms.assetid: 8f954004-40d5-4c5e-8e0d-e8700c8ec7b1
title: 檢查清單-設定同盟伺服器
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: f035baaccc5aae7218c5c524ed876e2b45eba252
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854961"
---
# <a name="checklist-setting-up-a-federation-server"></a>檢查清單：設定同盟伺服器

此檢查清單包含在 Active Directory 同盟服務 \(AD FS\)中，為同盟伺服器角色準備執行 Windows Server&reg; 2012 的伺服器所需的部署工作。  
  
> [!NOTE]  
> 請依序完成此檢查清單中的工作。 當某個參考連結帶您進入某個程序時，請在完成該程序中的步驟之後返回本主題，讓您可以繼續進行此檢查清單中其餘的工作。  
  
![設定](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**同盟伺服器檢查清單：設定同盟伺服器**  
  
||工作|參考|  
|-|--------|-------------|  
|![設定同盟伺服器](media/icon_checkboxo.gif)|開始部署 AD FS 同盟伺服器之前，請先參閱;1.\) 選擇 Windows 內部資料庫 \(WID\) 或 SQL Server 儲存 AD FS 設定資料庫2的優點和缺點。\) AD FS 部署拓撲類型及其相關聯的伺服器位置和網路設定建議。|![設定同盟伺服器時，會](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[決定您的 AD FS 部署拓撲](https://technet.microsoft.com/library/gg982491.aspx)<p>![設定同盟伺服器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[AD FS 部署拓撲考慮](https://technet.microsoft.com/library/gg982489.aspx)|  
|![設定同盟伺服器](media/icon_checkboxo.gif)|請參閱 AD FS 容量規劃指南，以判斷您應該在生產環境中使用的適當同盟伺服器數目。|![設定](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[同盟伺服器容量的同盟伺服器規劃](https://technet.microsoft.com/library/gg749917.aspx)|  
|![設定同盟伺服器](media/icon_checkboxo.gif)|請參閱 AD FS 設計指南中的資訊，瞭解如何將同盟伺服器放置在您的組織中|![設定同盟伺服器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[規劃同盟伺服器位置](https://technet.microsoft.com/library/dd807069.aspx)<p>![設定](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[同盟伺服器以放置同盟伺服器](https://technet.microsoft.com/library/dd807127.aspx)|  
|![設定同盟伺服器](media/icon_checkboxo.gif)|判斷獨立\-獨立的同盟伺服器或同盟伺服器陣列是否較適合您的部署。|![](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[在建立同盟伺服器時設定同盟](https://technet.microsoft.com/library/dd807101.aspx)伺服器<p>![](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[在建立同盟伺服器陣列時設定同盟](https://technet.microsoft.com/library/dd807062.aspx)伺服器|  
|![設定同盟伺服器](media/icon_checkboxo.gif)|判斷是否要在帳戶夥伴組織中或資源夥伴組織中建立此新的同盟伺服器。|![設定同盟伺服器時，請](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[檢查帳戶夥伴中的同盟伺服器角色](https://technet.microsoft.com/library/dd807117.aspx)<p>![設定同盟伺服器時，請](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[參閱資源夥伴中的同盟伺服器角色](https://technet.microsoft.com/library/dd807065.aspx)|  
|![設定同盟伺服器](media/icon_checkboxo.gif)|請參閱同盟伺服器如何使用服務通訊憑證和權杖\-簽署憑證，以安全地驗證用戶端和同盟伺服器 proxy 要求的相關資訊。 **注意：** 雖然使用具有不合格主機名稱的憑證（例如 HTTPs：\/\/myserver）很長，但這些憑證沒有安全性價值，而且可以讓攻擊者模擬 AD FS 同盟服務給企業用戶端。 因此，建議您使用 \(FQDN\) 的完整功能變數名稱，例如 HTTPs：\/\/myserver.contoso.com，而且只會使用發行至您同盟服務 FQDN 的 SSL 憑證。|![設定同盟伺服器的聯盟伺服器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[證書需求](https://technet.microsoft.com/library/dd807040.aspx)|  
|![設定同盟伺服器](media/icon_checkboxo.gif)|請參閱如何更新公司網路網域名稱系統 \(DNS\) 的相關資訊，以便對同盟伺服器進行成功的名稱解析。|![設定同盟伺服器的聯盟伺服器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[名稱解析需求](https://technet.microsoft.com/library/dd807041.aspx)|  
|![設定同盟伺服器](media/icon_checkboxo.gif)|將即將成為同盟伺服器的電腦加入至帳戶夥伴樹系或資源夥伴樹系中的網域，以便用來驗證該樹系的使用者或信任的樹系。 **注意：** 如果您想要在帳戶夥伴組織中設定同盟伺服器，電腦必須先聯結至樹系中的任何網域，而您的同盟伺服器將用來驗證來自該樹系的使用者，或來自信任的樹系。|![設定同盟伺服器將](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[電腦加入網域](Join-a-Computer-to-a-Domain.md)|  
|![設定同盟伺服器](media/icon_checkboxo.gif)|在公司網路 DNS 中建立新的資源記錄，將同盟伺服器的 DNS 主機名稱指向同盟伺服器的 IP 位址。|![設定同盟伺服器時，請](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[將主機&#40;a&#41;資源記錄新增至同盟伺服器的公司 DNS](Add-a-Host--A--Resource-Record-to-Corporate-DNS-for-a-Federation-Server.md)|  
|![設定同盟伺服器](media/icon_checkboxo.gif)|\(選擇性\) 如果您要將同盟伺服器新增至同盟伺服器陣列，您可能必須先匯出\) 伺服器陣列中第一部同盟伺服器上現有權杖\-簽署憑證 \(的私密金鑰，如此當其他同盟伺服器必須匯入相同的憑證時，您就可以使用憑證的檔案格式。<p>當您所發出的伺服器驗證憑證可以由多部 \(電腦重複使用時，不需要匯出私密金鑰\) 或當您要為伺服器陣列中的每部同盟伺服器取得唯一的伺服器驗證憑證時，就不需要匯出私密金鑰。 **注意：** 中的 [AD FS 管理] 嵌入式\-管理單元是指做為服務通訊憑證之同盟伺服器的伺服器驗證憑證。|![設定同盟伺服器時](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[，請匯出伺服器驗證憑證的私密金鑰部分](Export-the-Private-Key-Portion-of-a-Server-Authentication-Certificate.md)|  
|![設定同盟伺服器](media/icon_checkboxo.gif)|從證書 \(頒發機構單位\)取得伺服器驗證憑證 \(或私密金鑰\) 之後，您必須將憑證檔案匯入每部同盟伺服器的預設網站。 **注意：** 您必須先在預設的網站上安裝此憑證，才能使用 AD FS 同盟伺服器設定向導。|![設定同盟伺服器將](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[伺服器驗證憑證匯入到預設的網站](Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md)|  
|![設定同盟伺服器](media/icon_checkboxo.gif)|\(選擇性\) 作為從 CA 取得伺服器驗證憑證的替代方法，您可以使用 Internet Information Services \(IIS\) 來建立同盟伺服器的範例憑證。 **注意：** 使用自我\-簽署的伺服器驗證憑證，在生產環境中部署同盟伺服器並不是安全性最佳作法。|![設定同盟伺服器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[IIS：建立自我\-簽署的伺服器憑證](https://go.microsoft.com/fwlink/?LinkID=108271)，然後完成將[伺服器驗證憑證匯入至預設網站](Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md)的程式|  
|![設定同盟伺服器](media/icon_checkboxo.gif)|如果您要在帳戶夥伴組織中設定同盟伺服器陣列環境，您必須在伺服器陣列所在的 Active Directory Domain Services \(AD DS\) 中建立並設定專屬的服務帳戶，並在伺服器陣列中設定每部同盟伺服器以使用此帳戶。 藉由執行此程式，您將可讓公司網路上的用戶端使用 Windows 整合式驗證，向伺服器陣列中的任何同盟伺服器進行驗證。|![設定同盟伺服器時，請](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[手動設定同盟伺服器陣列的服務帳戶](Manually-Configure-a-Service-Account-for-a-Federation-Server-Farm.md)|  
|![設定同盟伺服器](media/icon_checkboxo.gif)|在即將成為同盟伺服器的電腦上安裝同盟服務角色服務。|![設定同盟伺服器時](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[，請安裝同盟服務角色服務](Install-the-Federation-Service-Role-Service.md)|  
|![設定同盟伺服器](media/icon_checkboxo.gif)|使用 [AD FS 同盟伺服器設定] 嚮導，將電腦上的 AD FS 軟體設定為同盟伺服器角色。<p>當您想要設定獨立的\-獨立同盟伺服器時，請遵循此程式，在新的伺服器陣列中建立第一部同盟伺服器，或將電腦加入現有的同盟伺服器陣列。 **注意：** 如需 \(SSO\) 設計上的同盟 Web 單一登入\-，您至少必須在帳戶夥伴組織中擁有一部同盟伺服器，並在資源夥伴組織中至少要有一部同盟伺服器。|![設定同盟伺服器時，請](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[建立獨立同盟伺服器](Create-a-Stand-Alone-Federation-Server.md)<p>![設定同盟伺服器時](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[，請在同盟伺服器陣列中建立第一部同盟伺服器](Create-the-First-Federation-Server-in-a-Federation-Server-Farm.md)<p>![設定](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[同盟伺服器將同盟伺服器新增至同盟伺服器](Add-a-Federation-Server-to-a-Federation-Server-Farm.md)陣列|  
|![設定同盟伺服器](media/icon_checkboxo.gif)|\(選擇性\) 使用中的 [AD FS 管理] 嵌入式管理單元\-來新增和設定部署設計所需的必要 AD FS 憑證。 如需有關使用中的嵌入式管理\-單元新增或變更憑證之時機的詳細資訊，請參閱[同盟伺服器的憑證需求](https://technet.microsoft.com/library/dd807040.aspx)。|![設定同盟伺服器時](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[，請新增權杖簽署憑證](Add-a-Token-Signing-Certificate.md)<p>![設定同盟伺服器時，請](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[新增權杖解密憑證](Add-a-Token-Decrypting-Certificate.md)<p>![設定同盟伺服器](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[設定服務通訊憑證](Set-a-Service-Communications-Certificate.md)|  
|![設定同盟伺服器](media/icon_checkboxo.gif)|如果這是您組織中的第一部同盟伺服器，請設定同盟服務，使其符合您的 AD FS 設計。|![設定同盟伺服器](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[檢查清單：設定帳戶夥伴組織](Checklist--Configuring-the-Account-Partner-Organization.md)<p>![設定同盟伺服器](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[檢查清單：設定資源夥伴組織](Checklist--Configuring-the-Resource-Partner-Organization.md)|  
|![設定同盟伺服器](media/icon_checkboxo.gif)|從用戶端電腦，確認同盟伺服器可運作。|![設定](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[同盟伺服器時，請確認同盟伺服器可運作](Verify-That-a-Federation-Server-Is-Operational.md)| 
