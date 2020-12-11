---
description: 深入瞭解：檢查清單：設定同盟伺服器
ms.assetid: 8f954004-40d5-4c5e-8e0d-e8700c8ec7b1
title: 檢查清單-設定同盟伺服器
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.author: billmath
ms.openlocfilehash: b968b5a1d573901fa13813e498cbf273e93e6c14
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97050306"
---
# <a name="checklist-setting-up-a-federation-server"></a>檢查清單：設定同盟伺服器

這份檢查清單包含 &reg; 在 Active Directory 同盟服務 AD FS 中為同盟伺服器角色準備執行 Windows server 2012 的伺服器所需的部署 \( 工作 \) 。

> [!NOTE]
> 請依序完成此檢查清單中的工作。 當某個參考連結帶您進入某個程序時，請在完成該程序中的步驟之後返回本主題，讓您可以繼續進行此檢查清單中其餘的工作。

![設定](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**同盟伺服器檢查清單：設定同盟伺服器**

|Task|參考|
|--------|-------------|
|開始部署 AD FS 同盟伺服器之前，請先參閱：1. \) 選擇 Windows 內部資料庫 \( WID \) 或 SQL Server 來儲存 AD FS configuration Database 2 的優點和缺點。 \) AD FS 部署拓撲類型及其相關聯的伺服器放置和網路設定建議。|![設定同盟伺服器會](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[決定您的 AD FS 部署拓撲](../design/determine-your-ad-fs-deployment-topology.md)<p>![設定同盟伺服器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[AD FS 部署拓撲考慮](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/gg982489(v=ws.11))|
|請參閱 AD FS 容量規劃指引，以判斷您應在生產環境中使用的適當同盟伺服器數目。|![設定同盟伺服器容量的](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[同盟伺服器規劃](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/gg749917(v=ws.11))|
|請參閱 AD FS 設計指南中的資訊，以瞭解在組織中放置同盟伺服器的位置|![設定](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[同盟伺服器規劃同盟伺服器放置](../design/planning-federation-server-placement.md)<p>![設定](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[同盟伺服器以放置同盟伺服器](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dd807127(v=ws.11))|
|判斷獨立的 \- 同盟伺服器或同盟伺服器陣列是否更適合您的部署。|![設定同盟伺服器建立](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[同盟伺服器](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dd807101(v=ws.11))的時機<p>![設定同盟伺服器建立](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[同盟伺服器](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dd807062(v=ws.11))陣列的時機|
|判斷是否要在帳戶夥伴組織或資源夥伴組織中建立此新的同盟伺服器。|![設定](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[同盟伺服器審核帳戶夥伴中同盟伺服器的角色](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dd807117(v=ws.11))<p>![設定](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[同盟伺服器審核資源夥伴中的同盟伺服器角色](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dd807065(v=ws.11))|
|查看同盟伺服器如何使用服務通訊憑證和權杖 \- 簽署憑證的相關資訊，安全地驗證用戶端和同盟伺服器 proxy 要求。 **注意：** 雖然使用具有不合格主機名稱（例如 HTTPs： myserver）的憑證是很常見的作法 \/ \/ ，但是這些憑證沒有安全性值，可以讓攻擊者模擬企業用戶端的 AD FS 同盟服務。 因此，建議您使用完整的功能變數名稱 \( FQDN \) （例如 HTTPs： \/ \/ myserver.contoso.com），並且只使用發行給同盟服務 FQDN 的 SSL 憑證。|![設定同盟伺服器的](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[同盟伺服器憑證需求](../design/certificate-requirements-for-federation-servers.md)|
|請參閱有關如何更新公司網路網域名稱系統 DNS 的資訊 \( ， \) 讓同盟伺服器的成功名稱解析可以進行。|![設定同盟伺服器的](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[同盟伺服器名稱解析需求](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dd807041(v=ws.11))|
|將即將成為同盟伺服器的電腦加入至帳戶夥伴樹系或資源夥伴樹系中的網域，以用來驗證該樹系的使用者或信任的樹系。 **注意：** 如果您想要設定帳戶夥伴組織中的同盟伺服器，電腦必須先加入樹系中的任何網域，您的同盟伺服器將用來驗證來自該樹系或信任樹系的使用者。|![設定同盟伺服器將](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[電腦加入網域](Join-a-Computer-to-a-Domain.md)|
|在公司網路 DNS 中建立新的資源記錄，以將同盟伺服器的 DNS 主機名稱指向同盟伺服器的 IP 位址。|![設定同盟伺服器會](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[將主機 &#40;&#41; 資源記錄新增至同盟伺服器的公司 DNS](Add-a-Host--A--Resource-Record-to-Corporate-DNS-for-a-Federation-Server.md)|
|\(選擇性 \) ：如果您要將同盟伺服器新增至同盟伺服器陣列，您可能必須先在 \- 伺服器陣列中的第一部同盟伺服器上匯出現有權杖簽署憑證的私密金鑰，如此一來 \( \) ，當其他同盟伺服器必須匯入相同的憑證時，憑證的檔案格式就會準備就緒。<p>當您所發出的伺服器驗證憑證可由多部電腦重複使用， \( 而不需要匯出 \) 或您將取得伺服器陣列中每部同盟伺服器的唯一伺服器驗證憑證時，就不需要匯出私密金鑰。 **注意：** AD FS 管理嵌入式管理單元會 \- 將同盟伺服器的伺服器驗證憑證視為服務通訊憑證。|![設定同盟伺服器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[匯出伺服器驗證憑證的私用金鑰部分](Export-the-Private-Key-Portion-of-a-Server-Authentication-Certificate.md)|
|當您 \( 從憑證授權單位單位 CA 取得伺服器驗證憑證或私密金鑰之後 \) \( \) ，必須接著將憑證檔案匯入每部同盟伺服器的預設網站。 **注意：** 您必須先在預設的網站上安裝此憑證，才能使用 AD FS 同盟伺服器設定向導。|![設定同盟伺服器將](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[伺服器驗證憑證匯入至預設的網站](Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md)|
|\(選擇性 \) ：若要從 CA 取得伺服器驗證憑證，您可以使用 Internet Information Services \( IIS \) 來建立同盟伺服器的範例憑證。 **注意：** 使用自我 \- 簽署的伺服器驗證憑證，在生產環境中部署同盟伺服器並不是安全性最佳作法。|![設定同盟伺服器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[IIS：建立自我 \- 簽署的伺服器憑證](https://go.microsoft.com/fwlink/?LinkID=108271)，然後完成將[伺服器驗證憑證匯入至預設網站](Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md)的程式|
|如果您要設定帳戶夥伴組織中的同盟伺服器陣列環境，您必須在伺服器陣列所在的 Active Directory Domain Services AD DS 中建立並設定專用服務帳戶，並將伺服器陣列中的每部 \( \) 同盟伺服器設定為使用此帳戶。 藉由執行此程式，您將可讓公司網路上的用戶端使用 Windows 整合式驗證，向伺服器陣列中的任何同盟伺服器進行驗證。|![設定同盟伺服器手動設定](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[同盟伺服器陣列的服務帳戶](Manually-Configure-a-Service-Account-for-a-Federation-Server-Farm.md)|
|在將成為同盟伺服器的電腦上安裝同盟服務的角色服務。|![設定同盟伺服器會](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[安裝同盟服務角色服務](Install-the-Federation-Service-Role-Service.md)|
|使用 AD FS 同盟伺服器設定向導，將電腦上的 AD FS 軟體設定為同盟伺服器角色中的動作。<p>當您想要設定獨立 \- 同盟伺服器、在新的伺服器陣列中建立第一部同盟伺服器，或將電腦加入現有的同盟伺服器陣列時，請遵循此程式。 **注意：** 針對同盟網頁單一登入 \- \( SSO \) 設計，您必須至少在帳戶夥伴組織中至少有一部同盟伺服器，以及在資源夥伴組織中至少有一部同盟伺服器。|![設定同盟伺服器建立](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[Stand-Alone 同盟伺服器](Create-a-Stand-Alone-Federation-Server.md)<p>![設定](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[同盟伺服器在同盟伺服器陣列中建立第一部同盟伺服器](Create-the-First-Federation-Server-in-a-Federation-Server-Farm.md)<p>![設定同盟伺服器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[將同盟伺服器新增至同盟伺服器](Add-a-Federation-Server-to-a-Federation-Server-Farm.md)陣列|
|\(選擇性 \) 使用 AD FS 管理] 嵌入式管理單元 \- 來新增及設定部署設計所需的必要 AD FS 憑證。 如需有關何時使用嵌入式管理單元新增或變更憑證的詳細資訊 \- ，請參閱 [同盟伺服器的憑證需求](../design/certificate-requirements-for-federation-servers.md)。|![設定同盟伺服器](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[新增 Token-Signing 憑證](Add-a-Token-Signing-Certificate.md)<p>![設定同盟伺服器](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[新增 Token-Decrypting 憑證](Add-a-Token-Decrypting-Certificate.md)<p>![設定同盟伺服器](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[設定服務通訊憑證](Set-a-Service-Communications-Certificate.md)|
|如果這是您組織中的第一部同盟伺服器，請設定同盟服務，使其符合您的 AD FS 設計。|![設定同盟伺服器](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[檢查清單：設定帳戶夥伴組織](Checklist--Configuring-the-Account-Partner-Organization.md)<p>![設定同盟伺服器](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[檢查清單：設定資源夥伴組織](Checklist--Configuring-the-Resource-Partner-Organization.md)|
|從用戶端電腦，確認同盟伺服器可運作。|![設定](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[同盟伺服器會確認同盟伺服器是否可運作](Verify-That-a-Federation-Server-Is-Operational.md)|
