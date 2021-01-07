---
title: BranchCache 託管快取模式部署概觀
description: 瞭解如何在分公司部署 BranchCache 託管快取伺服器，該伺服器會將電腦加入網域。
manager: brianlic
ms.topic: article
ms.assetid: 55686a9c-60dd-47f4-9f1f-fe72c2873a44
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 8abb25c7d2b8e3bcf0e5361bbc4fc4da2a2e7567
ms.sourcegitcommit: 605a9b46b74b2c7a9116e631e902467ea02a6e70
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/07/2021
ms.locfileid: "97965590"
---
# <a name="branchcache-hosted-cache-mode-deployment-overview"></a>BranchCache 託管快取模式部署概觀

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

您可以使用本指南，將 BranchCache 託管快取伺服器部署在已加入網域之電腦的分公司。 您可以使用本主題來瞭解 BranchCache 託管快取模式部署程式。

本總覽包含您所需的 BranchCache 基礎結構，以及簡單的部署逐步解說。

## <a name="hosted-cache-server-deployment-infrastructure"></a><a name="bkmk_components"></a>託管快取伺服器部署基礎結構

在此部署中，會使用 Active Directory Domain Services AD DS 中的服務連接點來部署託管快取伺服器 \( \) ，而且您可以選擇在 Windows server 2016、windows Server 2012 R2 和 windows server 2012 中使用 BranchCache，以 prehash Web 和檔案型內容伺服器上的共用內容，然後在託管快取伺服器上預先載入內容。

下圖顯示部署 BranchCache 託管快取伺服器所需的基礎結構。

![BranchCache 託管快取模式總覽](../../../media/BranchCache-Hcm-Overview/Bc-Hcm-Overview.jpg)

> [!IMPORTANT]
> 雖然此部署描述雲端資料中心內的內容伺服器，但您可以使用本指南來部署 BranchCache 託管快取伺服器，不論您將內容伺服器部署在何處（在總公司或雲端位置）。

### <a name="hcs1-in-the-branch-office"></a>分公司的 HCS1

您必須將這部電腦設定為託管快取伺服器。 如果您選擇 prehash 內容伺服器資料，讓您可以預先載入託管快取伺服器上的內容，則可以從您的 Web 和檔案伺服器匯入包含內容的資料套件。

### <a name="web1-in-the-cloud-data-center"></a>雲端資料中心內的 WEB1

WEB1 是啟用 BranchCache 的 \- 內容伺服器。 如果您選擇 prehash 內容伺服器資料，讓您可以預先載入託管快取伺服器上的內容，您可以在 WEB1 上 prehash 共用內容，然後建立您複製到 HCS1 的資料套件。

### <a name="file1-in-the-cloud-data-center"></a>雲端資料中心的 FILE1

FILE1 是啟用 BranchCache 的 \- 內容伺服器。 如果您選擇 prehash 內容伺服器資料，讓您可以預先載入託管快取伺服器上的內容，您可以 prehash FILE1 上的共用內容，然後建立您複製到 HCS1 的資料套件。

### <a name="dc1-in-the-main-office"></a>總公司中的 DC1

DC1 是網域控制站，您必須設定預設網域原則或其他更適合您的部署的原則，並使用 BranchCache 群組原則設定來啟用服務連接點的自動裝載快取探索。

當分支中的用戶端電腦重新整理群組原則並套用此原則設定時，它們會自動尋找並開始使用分公司的託管快取伺服器。

### <a name="client-computers-in-the-branch-office"></a>分公司的用戶端電腦

您必須重新整理用戶端電腦上的群組原則，以套用新的 BranchCache 群組原則設定，並允許用戶端尋找並使用託管快取伺服器。

## <a name="hosted-cache-server-deployment-process-overview"></a><a name="bkmk_overview"></a>託管快取伺服器部署流程總覽

>[!NOTE]
>[BranchCache 託管快取模式部署](4-Bc-Hcm-Deployment.md)一節中會提供如何執行這些步驟的詳細資料。

部署 BranchCache 託管快取伺服器的程式會在下列階段進行：

>[!NOTE]
>下列步驟是選擇性的，例如示範如何在託管快取伺服器上 prehash 和預先載入內容的步驟。 當您以託管快取模式部署 BranchCache 時，不需要在 Web 和檔案內容伺服器上 prehash 內容、建立資料套件，以及匯入資料套件，以便預先載入託管快取伺服器與內容。 在這一節和 [BranchCache 託管快取模式部署](4-Bc-Hcm-Deployment.md) 的區段中，這些步驟都是選擇性的，所以如果您想要的話，可以略過這些步驟。

1. 在 HCS1 上，使用 Windows PowerShell 命令將電腦設定為託管快取伺服器，並在 Active Directory 中註冊服務連接點。

2. \(選擇性 \) 在 HCS1 上，如果 BranchCache 預設值不符合您的伺服器和託管快取的部署目標，請設定您要為託管快取配置的磁碟空間量。 也請設定您想要用於託管快取的磁片位置。

3. \(選擇性 \) Prehash 內容伺服器上的內容、建立資料套件，以及在託管快取伺服器上預先載入內容。

    > [!NOTE]
    > Prehashing 和預先載入託管快取伺服器上的內容是選擇性的，不過，如果您選擇 prehash 和預先載入，則必須執行適用于您部署的所有下列步驟。 \(例如，如果您沒有 Web 服務器，就不需要執行任何與 prehashing 和預先載入 Web 服務器內容相關的步驟。\)

    1. 在 WEB1 上，prehash Web 服務器內容，並建立資料套件。

    2. 在 FILE1 上，prehash 檔案伺服器內容，並建立資料套件。

    3. 從 WEB1 和 FILE1，將資料封裝複製到託管快取伺服器 HCS1。

    4. 在 HCS1 上，匯入資料套件以預先載入資料快取。

4. 在 DC1 上，藉由設定群組原則與 BranchCache 原則設定，為託管快取模式設定已加入網域的分公司用戶端電腦。

5. 在用戶端電腦上，重新整理群組原則。

若要繼續進行本指南，請參閱 [BranchCache 託管快取模式部署計畫](3-Bc-Hcm-Plan.md)。