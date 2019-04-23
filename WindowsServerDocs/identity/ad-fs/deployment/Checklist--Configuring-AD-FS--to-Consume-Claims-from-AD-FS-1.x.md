---
ms.assetid: e7f9e518-2d5d-4a0d-9147-34e1304f42ac
title: 檢查清單-設定使用 AD fs 的宣告的 AD FS 1.x
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: fe99487d3a770547af36f69722b442d0e2cbb8b1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59828289"
---
# <a name="checklist-configuring-ad-fs--to-consume-claims-from-ad-fs-1x"></a>檢查清單：設定 AD FS 使用宣告從 AD FS 1.x

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012
  
## <a name="checklist-configuring-ad-fs-to-consume-claims-from-adfs1x"></a>檢查清單：設定 AD FS 使用宣告從 AD FS 1.x  
此檢查清單包含設定 Active Directory 同盟服務所需的工作\(AD FS\) Windows Server 2012 使用 AD FS 1 所傳送的宣告中的 Federation Service。*x* Federation Service。  
  
> [!NOTE]  
> 請依序完成此檢查清單中的工作。 當某個參考連結帶您進入某個程序時，請在完成該程序中的步驟之後返回本主題，讓您可以繼續進行此檢查清單中其餘的工作。  
  
![使用從 AD FS 的宣告](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**檢查清單：設定 AD FS 使用宣告從 AD FS 1.x**  
  
||工作|參考資料|  
|-|--------|-------------|  
|![使用從 AD FS 的宣告](media/icon_checkboxo.gif)|規劃 Windows Server 2012 中的 AD FS 與舊版的 AD FS 之間的互通性，並了解名稱識別碼宣告類型。|![使用從 AD FS 的宣告](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[規劃互通性與 AD FS 1.x](https://technet.microsoft.com/library/ff678040.aspx)|  
|![使用從 AD FS 的宣告](media/icon_checkboxo.gif)|您可以與舊版 AD FS 的交互操作之前，您必須先建立 AD FS Federation Service 中的宣告提供者信任。 **注意：** 您無法建立信任關係，與 AD FS 1。*x* Federation Service 使用同盟中繼資料。<br /><br />當您設定信任使用右邊的連結中的程序時，您必須執行下列中新增宣告提供者信任精靈 來設定此信任的 AD FS 1 與交互操作。*x* Federation Service:<br /><br />1.在 **選取資料來源**頁面上，選取**輸入資料的相關信賴憑證者合作對象信任手動**。<br />2.在 **選擇設定檔**頁面上，選取**AD FS 1.0 和 1.1 設定檔**。<br />3.在 **設定 URL**頁面的  **WS\-同盟被動式 URL**，型別**Federation Service 端點 URL** AD FS 1 中所定義。*x*協力廠商同盟服務。<br />4.在 **設定識別碼**頁面的 **宣告提供者信任識別碼**，型別**Federation Service URI** AD FS 1 中所定義。*x*協力廠商同盟服務。|![使用從 AD FS 的宣告](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[宣告提供者信任手動建立](../../ad-fs/operations/Create-a-Claims-Provider-Trust.md)|  
|![使用從 AD FS 的宣告](media/icon_checkboxo.gif)|在您稍早建立的宣告提供者信任，您必須建立宣告規則，會從 AD FS 的連入宣告 1.x Federation Service 和通過，篩選，或將它們轉換成名稱識別碼宣告類型。<br /><br />當名稱識別碼宣告類型已傳遞、 經過篩選，或轉換，它可用來當做輸入到另一個規則或規則，以便了解並由 Windows Server 2012 中 AD FS Federation Service。|![使用從 AD FS 的宣告](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[建立規則來傳送 AD FS 1.x 相容的宣告](../../ad-fs/operations/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)|  
|![使用從 AD FS 的宣告](media/icon_checkboxo.gif)|請連絡系統管理員的 AD FS 1。*x* Federation Service 與 AD FS 1 的系統管理員。*x* Federation Service 設定新的資源夥伴信任。 此外，將該系統管理員提供 Federation Service URI\(中的同盟服務屬性\)，Federation Service 端點 URL，以及匯出的語彙基元\-簽署憑證檔案\(與僅限公開金鑰\)。 系統管理員必須設定信任這些項目。|N\/A|  
  

