---
title: BranchCache 裝載快取模式部署計劃
description: 本指南部署 BranchCache 裝載快取模式執行的 Windows Server 2016 和 Windows 10 電腦上提供指示
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: bc44a7db-f7a5-4e95-9d95-ab8d334e885f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e645dd96ec85e3a23df6717cfa43d7627cb938e7
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="branchcache-hosted-cache-mode-deployment-planning"></a>BranchCache 裝載快取模式部署計劃

>適用於：Windows Server（以每年次管道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

您可以使用本主題以計劃裝載快取模式中的 BranchCache 部署。

>[!IMPORTANT]
>Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012 必須執行您裝載快取的伺服器。

部署裝載快取伺服器之前，您必須計劃下列項目：

- [計劃基本的伺服器設定](#bkmk_basic)

- [規劃網域存取](#bkmk_domain)

- [規劃裝載快取的大小與位置](#bkmk_cachelocation)

- [計劃的內容伺服器套件的複製分享](#bkmk_package)

- [計畫 prehashing 和資料套件建立內容伺服器](#bkmk_prehash)

## <a name="bkmk_basic"></a>計劃基本的伺服器設定
  
如果您計劃在您分公司使用現有的伺服器，為您裝載快取的伺服器，您不需要執行此計畫的步驟，因為您的電腦已為且為 IP 位址設定。

您裝載快取的伺服器上安裝 Windows Server 2016 之後，您必須重新命名電腦指派並設定為 [本機電腦靜態 IP 位址。

>[!NOTE]
>本指南，裝載快取伺服器稱為 HCS1，但您應該使用適用於您的部署伺服器的名稱。

## <a name="bkmk_domain"></a>規劃網域存取

如果您計劃在您分公司使用現有的伺服器，為您裝載快取的伺服器，您不需要執行此步驟計劃，除非您電腦目前正在未加入網域。
  
若要登入至網域中，電腦必須成員網域的電腦和使用者 account 必須建立在嘗試登入之前 AD DS。 此外，您必須將電腦加入網域的 account 的適當的群組成員資格。

## <a name="bkmk_cachelocation"></a>規劃裝載快取的大小與位置

HCS1，判斷您裝載快取的伺服器上您想要尋找裝載快取。 例如，進而硬碟、音量及要市集快取的資料夾位置。

此外，選擇您想要裝載快取配置的磁碟空間百分比。

## <a name="bkmk_package"></a>計劃的內容伺服器套件的複製分享

建立內容伺服器上的資料套件之後，您必須將它們複製到網路共用裝載快取伺服器上。

規劃資料夾位置，以及分享的權限的共用資料夾。 此外，如果您內容伺服器裝載大量的資料，您所建立的套件，將會大檔案計劃複製期間執行作業 off\ – 尖峰使 WAN 頻寬不由其他人需要時使用的頻寬，正常營運期間複製作業。

## <a name="bkmk_prehash"></a>計畫 prehashing 和資料套件建立內容伺服器

您在您內容伺服器 prehash content 之前，您必須辨識的資料夾和檔案，包含您想要新增到的資料套件 content。 

此外，您必須計劃在本機的資料夾位置，您可以將資料套件才能將它們複製到裝載快取伺服器上。

若要繼續使用此快速入門，請查看[BranchCache 裝載快取模式部署](4-Bc-Hcm-Deployment.md)。
