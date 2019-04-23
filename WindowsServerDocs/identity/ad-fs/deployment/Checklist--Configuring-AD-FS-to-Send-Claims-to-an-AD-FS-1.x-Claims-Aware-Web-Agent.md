---
ms.assetid: 551c1a0d-8d30-41b4-9c4a-35a3337dd3bc
title: 部署同盟伺服器
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 32675aa7fb8c7b928bcf80a4d1072fe5eab7cd61
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59864079"
---
# <a name="checklist-configuring-ad-fs-to-send-claims-to-an-ad-fs-1x-claims-aware-web-agent"></a>檢查清單：設定 AD FS 以將宣告傳送至 AD FS 1.x Claims-aware Web 代理程式

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012
  
## <a name="checklist-configuring-ad-fs-to-send-claims-to-an-adfs1x-claims-aware-web-agent"></a>檢查清單：設定 AD FS 以將宣告傳送至 AD FS 1.x 宣告\-感知網路代理程式  
此檢查清單包含設定 Active Directory 同盟服務所需的工作\(AD FS\) Windows Server 2012 中傳送可以理解的應用程式的宣告所裝載的 Federation ServiceWeb 伺服器執行 AD FS 1。*x*宣告\-感知網路代理程式。  
  
> [!NOTE]  
> 請依序完成此檢查清單中的工作。 當某個參考連結帶您進入某個程序時，請在完成該程序中的步驟之後返回本主題，讓您可以繼續進行此檢查清單中其餘的工作。  
  
![設定 AD FS 來傳送宣告](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**檢查清單：設定 AD FS 以將宣告傳送至 AD FS 1.x 宣告\-感知網路代理程式**  
  
||工作|參考資料|  
|-|--------|-------------|  
|![設定 AD FS 來傳送宣告](media/icon_checkboxo.gif)|規劃 Windows Server 2012 中的 AD FS 與 AD FS 的先前版本之間的互通性，並了解名稱識別碼宣告類型。|![設定 AD FS 來傳送宣告](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[規劃互通性與 AD FS 1.x](https://technet.microsoft.com/library/ff678040.aspx)|  
|![設定 AD FS 來傳送宣告](media/icon_checkboxo.gif)|如果您尚未這樣做，使用右側的連結，先建立一個信賴憑證者信任與 Windows Server 2012 中 AD FS Federation Service AD FS 1。*x* Federation Service。|[檢查清單：設定 AD FS 來傳送至 AD FS 1.x Federation Service 的宣告](Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Federation-Service.md)|  
|![設定 AD FS 來傳送宣告](media/icon_checkboxo.gif)|之前，您可以達到互通性與應用程式所裝載的 AD FS 1。*x*宣告\-感知網路代理程式，您必須先建立一個信賴憑證者信任 AD FS Federation Service 中的 AD FS 1 的 Windows Server 2012 中。 *x*宣告\-感知網路代理程式。 **注意：** 建立此信任 AD FS Federation Service 中的已加入新的對等**應用程式**AD fs 1.x Federation Service \( **Federation Service\\信任原則\\我的組織\\應用程式**\)。 此信賴憑證者信任是必要的因為 AD FS 沒有對等**應用程式**自己嵌入式管理單元中的節點\-中。 不過，它仍必須有應用程式的安全通道。<br /><br />當您設定信任使用右邊的連結中的程序時，您必須執行下列中新增信賴憑證者信任精靈 來設定此信任的 AD FS 1 與交互操作。*x*宣告\-感知網路代理程式：<br /><br />1.在 **選取資料來源**頁面上，選取**輸入資料的相關信賴憑證者合作對象信任手動**。<br />2.在 **選擇設定檔**頁面上，選取**AD FS 1.0 和 1.1 設定檔**。<br />3.在 **設定 URL**頁面的  **WS\-同盟被動式 URL**，型別**應用程式 URL** AD FS 1 中所定義。*x*協力廠商同盟服務。<br />4.在 **設定識別碼**頁面的 **信賴憑證者信任組件的識別項**，型別**應用程式 URL** AD FS 1 中所定義。*x*宣告\-感知網路代理程式|![設定 AD FS 來傳送宣告](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[信賴憑證者合作對象信任手動建立](../../ad-fs/operations/Create-a-Relying-Party-Trust.md)|  
|![設定 AD FS 來傳送宣告](media/icon_checkboxo.gif)|請連絡系統管理員的 Web 伺服器執行 AD FS 1。*x*宣告\-了解 Web 代理程式，並編輯 web.config 檔案與宣告相關聯的系統管理員\-感知應用程式\(預設網站中 網際網路資訊服務\(IIS\) \)指向位於 AD FS Federation Service 的 Web 代理程式。<br /><br />比方說，取代*myresourcefederationserver*標記中`<fs> https://myresourcefederationserver/adfs/fs/federationserverservice.asmx</fs>`web.config 檔案，以有效的 AD FS 同盟伺服器名稱。<br /><br />這是必要的應用程式和 AD FS 1.x 宣告\-要能夠使用從 Windows Server 2012 中 AD FS 同盟服務傳送給它的宣告感知網路代理程式。|N\/A|  
|![設定 AD FS 來傳送宣告](media/icon_checkboxo.gif)|您稍早建立的信賴憑證者合作對象信任，您必須先建立宣告規則，會接受連入宣告，從屬性存放區擷取和傳遞、 篩選或轉換成可以了解和使用的名稱識別碼宣告類型AD FS 1。*x*宣告\-感知網路代理程式。 **注意：** 建立此規則之前，請確定您要在其中建立此規則的宣告規則集有第一次擷取輕量型目錄存取通訊協定從前的規則\(LDAP\)從屬性存放區的屬性宣告。 此宣告將用於做為輸入來建立規則，您將 AD FS 1。*x*\-相容宣告。 如需如何建立規則，以擷取 LDAP 屬性的詳細資訊，請參閱[建立規則，以宣告方式傳送 LDAP 屬性](../../ad-fs/operations/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md)。|![設定 AD FS 來傳送宣告](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[建立規則來傳送 AD FS 1.x 相容的宣告](../../ad-fs/operations/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)|  
  

