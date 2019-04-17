---
title: BranchCache 裝載快取模式部署概觀
description: 本指南部署 BranchCache 裝載快取模式執行的 Windows Server 2016 和 Windows 10 電腦上提供指示
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: 55686a9c-60dd-47f4-9f1f-fe72c2873a44
ms.author: pashort
author: shortpatti
ms.openlocfilehash: feab3156b89637f64d1af0250df459533b1662c7
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="branchcache-hosted-cache-mode-deployment-overview"></a>BranchCache 裝載快取模式部署概觀

>適用於：Windows Server（以每年次管道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

您可以使用此指南部署 BranchCache 裝載快取的伺服器分公司位置的加入網域的電腦。 您可以使用本主題以取得總覽 BranchCache 裝載快取模式部署程序。

此概觀包含需要以及部署的簡單概觀逐步 BranchCache 基礎結構。

## <a name="bkmk_components"></a>裝載快取伺服器部署基礎結構

部署，裝載快取伺服器部署服務連接點使用 Active Directory Domain Services \(AD DS\)，在 Windows Server 2012 R2、Windows Server 2016 中，您可以選擇使用 BranchCache 及 Windows Server 2012，以 prehash 網站與檔案共用的 content 根據內容伺服器，然後預先載入 content 裝載快取的伺服器上。

下圖顯示部署 BranchCache 裝載快取伺服器所需的基礎結構。

![BranchCache 裝載快取模式概觀](../../../media/BranchCache-Hcm-Overview/Bc-Hcm-Overview.jpg)

> [!IMPORTANT]
> 雖然這個部署描述內容伺服器雲端的資料中心] 中，您可以使用此指南部署無論或雲端位置中主要辦公室部署內容伺服器 – BranchCache 裝載快取伺服器。

### <a name="hcs1-in-the-branch-office"></a>在 [分公司 HCS1

您必須為裝載快取伺服器設定此電腦。 如果您選擇 prehash 內容 server 的資料，讓您可以 content 預先載入您裝載快取的伺服器上，您可以匯入的資料套件包含 content 從網路與檔案伺服器。

### <a name="web1-in-the-cloud-data-center"></a>WEB1 雲端的資料中心

WEB1 是 BranchCache\ 式內容伺服器。 如果您選擇 prehash 內容 server 的資料，讓您可以 content 預先載入您裝載快取的伺服器上，您可以 prehash 共用的 content 上 WEB1，然後建立複製到 HCS1 資料套件。

### <a name="file1-in-the-cloud-data-center"></a>1 在雲端的資料中心

1 是 BranchCache\ 式內容伺服器。 如果您選擇 prehash 內容 server 的資料，讓您可以 content 預先載入您裝載快取的伺服器上，您可以 prehash 共用的 content 在 1，然後建立複製到 HCS1 資料套件。
  
### <a name="dc1-in-the-main-office"></a>DC1 中主要辦公室

DC1 為網域控制站，您必須設定預設網域原則或另一個更適合您的部署，BranchCache 群組原則設定可讓您自動裝載快取探索服務連接點所使用的原則。

當 client 分支中的電腦已群組原則」重新整理，並且這項原則設定時，它們會自動找出並開始使用裝載快取伺服器分公司。

### <a name="client-computers-in-the-branch-office"></a>在 [分公司 client 電腦

您必須在套用新 BranchCache 群組原則設定，並允許戶端找出並使用裝載快取伺服器 client 電腦上重新整理群組原則。

## <a name="bkmk_overview"></a>裝載快取伺服器部署程序概觀

>[!NOTE]
>如何執行這些步驟的詳細資料一節中提供[BranchCache 裝載快取模式部署](4-Bc-Hcm-Deployment.md)。

部署 BranchCache 裝載快取伺服器的程序就會發生在這些階段：

>[!NOTE]
>步驟部分是選用的例如示範如何 prehash 和預先載入裝載快取的伺服器上 content 這些步驟。 當您在裝載快取模式部署 BranchCache 時的才能 prehash content 網頁和檔案內容伺服器上，以建立資料套件，並以預先載入您裝載快取的伺服器的 content 匯入的資料套件。 在本區段中，並一節中的步驟會 experience 為選擇性[BranchCache 裝載快取模式部署](4-Bc-Hcm-Deployment.md)，如果您想要跳它們。

1. 在 HCS1，使用 Windows PowerShell 命令，將電腦設定為裝載快取伺服器，並在 Active Directory 登記服務連接點。

2. 在 HCS1，如果您的部署目標的伺服器，並裝載快取，不符合 BranchCache 預設值 \(Optional\) 設定您想要為裝載快取配置的磁碟空間量。 也設定您想要裝載快取的磁碟位置。

3. \(Optional\) prehash content 內容伺服器，建立的資料的套件及預先載入 content 裝載快取伺服器上。

    > [!NOTE]
    > Prehashing 和預先載入您裝載快取的伺服器上 content 是選擇性的但是如果您選擇 prehash 和預先載入，您必須執行的所有的下列步驟都是適用於您的部署。 \ (例如，如果您不 Web 伺服器，您不需要執行任何相關 prehashing 與 Web 伺服器 content 預先載入的步驟。\)

    1. 在 WEB1，prehash 網頁伺服器，並建立的資料套件。

    2. 在 1、prehash 檔案伺服器 content 和建立的資料套件。

    3. 從 WEB1，1 資料套件複製 HCS1 裝載快取伺服器。

    4. 在 HCS1，匯入預先載入資料快取的資料套件。

4. 在 DC1 上設定加入網域分支 office client 電腦裝載快取模式透過群組原則設定使用 BranchCache 原則設定。

5. 在 client 的電腦上重新整理群組原則。

若要繼續使用此快速入門，請查看[BranchCache 裝載快取模式部署規劃](3-Bc-Hcm-Plan.md)。