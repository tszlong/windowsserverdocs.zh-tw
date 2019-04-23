---
title: BranchCache 託管快取模式部署概觀
description: 本指南提供在執行 Windows Server 2016 和 Windows 10 電腦上的託管快取模式部署 BranchCache 的指示
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: 55686a9c-60dd-47f4-9f1f-fe72c2873a44
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 930a9b4872a7a79351055841a5d716dd99df0fa9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829509"
---
# <a name="branchcache-hosted-cache-mode-deployment-overview"></a>BranchCache 託管快取模式部署概觀

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

您可以使用本指南來部署 BranchCache 託管快取伺服器，分公司辦公室的電腦加入網域的位置。 您可以使用本主題來取得 BranchCache 託管快取模式部署程序的概觀。

本概觀包含針對您的需求以及部署的簡單逐步概觀 BranchCache 基礎結構。

## <a name="bkmk_components"></a>託管快取伺服器部署基礎結構

在此部署中，託管快取伺服器部署在 Active Directory 網域服務中使用服務連接點\(AD DS\)，以及您可以選擇使用 Windows Server 2016、 Windows Server 2012 R2 與 Windows 中的 BranchCacheServer 2012 中，若要在 Web 和檔案共用的內容預先雜湊為基礎的內容伺服器，然後預先載入託管快取伺服器上的內容。

下圖顯示，才能部署 BranchCache 託管快取伺服器的基礎結構。

![BranchCache 託管快取模式概觀](../../../media/BranchCache-Hcm-Overview/Bc-Hcm-Overview.jpg)

> [!IMPORTANT]
> 雖然此部署會說明在雲端資料中心的內容伺服器，您可以使用本指南來部署 BranchCache 託管快取伺服器，不論在總公司或雲端位置，會部署您的內容伺服器 –。

### <a name="hcs1-in-the-branch-office"></a>在分公司 HCS1

您必須設定此電腦做為託管快取伺服器。 如果您選擇預先內容伺服器的資料，以便您可以將內容預先載入託管快取伺服器上，您可以匯入資料套件，其中包含從您的 Web 和檔案伺服器內容。

### <a name="web1-in-the-cloud-data-center"></a>在雲端資料中心的 WEB1

WEB1 是 BranchCache\-啟用內容伺服器。 如果您選擇預先內容伺服器的資料，以便您可以將內容預先載入託管快取伺服器上，您可以在 WEB1 上，共用的內容預先雜湊，然後建立資料套件，複製到 HCS1。

### <a name="file1-in-the-cloud-data-center"></a>在雲端資料中心的 FILE1

FILE1 是 BranchCache\-啟用內容伺服器。 如果您選擇預先內容伺服器的資料，以便您可以將內容預先載入託管快取伺服器上，您可以共用的內容，在 FILE1 上預先雜湊，然後建立資料套件，複製到 HCS1。
  
### <a name="dc1-in-the-main-office"></a>在總公司中的 DC1

DC1 是網域控制站，而且您必須設定 預設網域原則或另一個更適合您的部署，以啟用自動託管快取探索的服務連接點的 BranchCache 群組原則設定的原則。

當在分支中的用戶端電腦已重新整理的群組原則並套用此原則設定時，它們會自動找出並開始使用在分公司的託管快取伺服器。

### <a name="client-computers-in-the-branch-office"></a>在分公司的用戶端電腦

您必須重新整理群組原則，來套用新 BranchCache 群組原則設定，並允許用戶端尋找並使用託管快取伺服器的用戶端電腦上。

## <a name="bkmk_overview"></a>託管快取伺服器部署程序概觀

>[!NOTE]
>一節提供如何執行這些步驟的詳細資訊[BranchCache 託管快取模式部署](4-Bc-Hcm-Deployment.md)。

部署 BranchCache 託管快取伺服器的程序分為下列階段：

>[!NOTE]
>某些步驟是選擇性的這些步驟示範如何預先雜湊和預先載入託管快取伺服器上的內容，例如。 當您部署 BranchCache 託管快取模式時，您不需要 prehash 內容網頁及檔案內容伺服器上，以建立資料套件，並匯入資料套件，以預先載入託管快取伺服器的內容。 在本節中，和一節中步驟會標為選擇性[BranchCache 託管快取模式部署](4-Bc-Hcm-Deployment.md)，讓您可以略過它們要的話。

1. 在 HCS1，請使用 Windows PowerShell 命令，將電腦設定為託管快取伺服器，並在 Active Directory 中登錄服務連接點。

2. \(選擇性\)上 HCS1，BranchCache 預設值不符合您的部署目標伺服器和託管快取中，如果設定您想要為託管快取配置的磁碟空間數量。 也請設定您偏好的託管快取的磁碟位置。

3. \(選擇性\)預先在內容伺服器上的內容，建立資料套件，以及預先載入託管快取伺服器上的內容。

    > [!NOTE]
    > 在您的託管快取伺服器上預先雜湊處理並預先載入內容是選擇性的不過如果您選擇預先雜湊和預先載入，您必須執行的所有步驟，並用於您的部署。 \(例如，如果您還沒有 Web 伺服器，您不需要執行任何預先雜湊處理並預先載入的網頁伺服器內容相關的步驟。\)

    1. 在 WEB1 上，預先雜湊網頁伺服器內容，並建立資料套件。

    2. FILE1 上, 預先雜湊檔案伺服器，並建立資料套件。

    3. 從 WEB1 和 FILE1，資料將封裝複製到託管快取伺服器 HCS1。

    4. 在 HCS1，匯入預先載入資料快取的資料套件。

4. 在 DC1 上，設定 BranchCache 原則設定的群組原則設定已加入網域分支 office 託管快取模式的用戶端電腦。

5. 用戶端電腦上重新整理群組原則。

若要繼續進行本指南，請參閱[BranchCache 託管快取模式部署規劃](3-Bc-Hcm-Plan.md)。