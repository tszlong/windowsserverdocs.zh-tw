---
title: BranchCache 託管快取模式部署概觀
description: 本指南提供的指示說明如何在執行 Windows Server 2016 和 Windows 10 的電腦上，以託管快取模式部署 BranchCache。
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: article
ms.assetid: 55686a9c-60dd-47f4-9f1f-fe72c2873a44
ms.author: pashort
author: shortpatti
ms.openlocfilehash: dc6ade92eb5fe04271033973911ccb98e871d236
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406374"
---
# <a name="branchcache-hosted-cache-mode-deployment-overview"></a>BranchCache 託管快取模式部署概觀

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

您可以使用本指南，在將電腦加入網域的分公司中部署 BranchCache 託管快取伺服器。 您可以使用本主題來深入瞭解 BranchCache 託管快取模式部署程式。

此總覽包含您需要的 BranchCache 基礎結構，以及部署的簡單逐步總覽。

## <a name="bkmk_components"></a>託管快取伺服器部署基礎結構

在此部署中，會使用 Active Directory Domain Services \(AD DS\)中的服務連接點來部署託管快取伺服器，而且您可以選擇 Windows Server 2016、Windows Server 2012 R2 和 Windows Server 2012 中的 BranchCache 來預先雜湊 Web 和檔案型內容伺服器上的共用內容，然後在託管快取伺服器上預先載入內容。

下圖顯示部署 BranchCache 託管快取伺服器所需的基礎結構。

![BranchCache 託管快取模式總覽](../../../media/BranchCache-Hcm-Overview/Bc-Hcm-Overview.jpg)

> [!IMPORTANT]
> 雖然此部署描述的是雲端資料中心的內容伺服器，但您可以使用本指南來部署 BranchCache 託管快取伺服器，無論您在總公司或雲端位置部署內容伺服器。

### <a name="hcs1-in-the-branch-office"></a>分公司中的 HCS1

您必須將這部電腦設定為託管快取伺服器。 如果您選擇預先雜湊內容伺服器資料，讓您可以預先載入託管快取伺服器上的內容，您可以從 Web 和檔案伺服器匯入包含內容的資料套件。

### <a name="web1-in-the-cloud-data-center"></a>雲端資料中心內的 WEB1

WEB1 是啟用的內容伺服器\-BranchCache。 如果您選擇預先雜湊內容伺服器資料，讓您可以預先載入託管快取伺服器上的內容，您可以在 WEB1 上預先雜湊共用內容，然後建立您複製到 HCS1 的資料封裝。

### <a name="file1-in-the-cloud-data-center"></a>雲端資料中心的 FILE1

FILE1 是啟用內容伺服器\-BranchCache。 如果您選擇預先雜湊內容伺服器資料，讓您可以預先載入託管快取伺服器上的內容，您可以在 FILE1 上預先雜湊共用內容，然後建立您複製到 HCS1 的資料封裝。
  
### <a name="dc1-in-the-main-office"></a>總公司中的 DC1

DC1 是網域控制站，而且您必須設定預設網域原則，或其他更適合您部署的原則，並使用 BranchCache 群組原則設定來啟用服務連接點的自動託管快取探索。

當分支中的用戶端電腦重新整理群組原則並套用此原則設定時，會自動尋找並開始使用分公司中的託管快取伺服器。

### <a name="client-computers-in-the-branch-office"></a>分公司的用戶端電腦

您必須重新整理用戶端電腦上的群組原則，才能套用新的 BranchCache 群組原則設定，並允許用戶端找出並使用託管快取伺服器。

## <a name="bkmk_overview"></a>託管快取伺服器部署程式總覽

>[!NOTE]
>[BranchCache 託管快取模式部署](4-Bc-Hcm-Deployment.md)一節中會提供如何執行這些步驟的詳細資料。

部署 BranchCache 託管快取伺服器的程式會在下列階段中發生：

>[!NOTE]
>下列幾個步驟是選擇性的，例如示範如何在託管快取伺服器上預先雜湊和預先載入內容的步驟。 當您以託管快取模式部署 BranchCache 時，您不需要在 Web 和檔案內容伺服器上預先雜湊內容、建立資料封裝，以及匯入資料封裝，以便預先載入託管快取伺服器的內容。 在本節和[BranchCache 託管快取模式部署](4-Bc-Hcm-Deployment.md)一節中，會將這些步驟標示為選擇性，讓您可以視需要略過它們。

1. 在 HCS1 上，使用 Windows PowerShell 命令將電腦設定為託管快取伺服器，並在 Active Directory 中註冊服務連接點。

2. \(選擇性\) 在 HCS1 上，如果 BranchCache 預設值不符合伺服器和託管快取的部署目標，請設定您要為託管快取配置的磁碟空間量。 此外，也請設定您偏好用於託管快取的磁片位置。

3. \(選擇性\) 在內容伺服器上預先雜湊內容、建立資料套件，以及在託管快取伺服器上預先載入內容。

    > [!NOTE]
    > 在託管快取伺服器上 Prehashing 並預先載入內容是選擇性的，不過，如果您選擇預先雜湊和預先載入，則必須執行下列適用于您部署的所有步驟。 \(例如，如果您沒有 Web 服務器，就不需要執行任何與 prehashing 和預先載入網頁伺服器內容相關的步驟。\)

    1. 在 WEB1 上，預先雜湊網頁伺服器內容並建立資料封裝。

    2. 在 FILE1 上，預先雜湊檔案伺服器內容並建立資料封裝。

    3. 從 WEB1 和 FILE1，將資料封裝複製到託管快取伺服器 HCS1。

    4. 在 HCS1 上，匯入資料封裝以預先載入資料快取。

4. 在 DC1 上，藉由設定群組原則與 BranchCache 原則設定，將已加入網域的分公司用戶端電腦設定為託管快取模式。

5. 在用戶端電腦上，重新整理群組原則。

若要繼續進行本指南，請參閱[BranchCache 託管快取模式部署規劃](3-Bc-Hcm-Plan.md)。