---
title: BranchCache 託管快取模式部署規劃
description: 本指南提供在執行 Windows Server 2016 和 Windows 10 電腦上的託管快取模式部署 BranchCache 的指示
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: bc44a7db-f7a5-4e95-9d95-ab8d334e885f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e7232f8732e7476b955115741b5582a585dc6068
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890679"
---
# <a name="branchcache-hosted-cache-mode-deployment-planning"></a>BranchCache 託管快取模式部署規劃

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

若要規劃部署 BranchCache 託管快取模式中的，您可以使用本主題。

>[!IMPORTANT]
>您的託管快取伺服器必須執行 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012。

部署託管快取伺服器之前，您必須規劃下列項目：

- [規劃基本伺服器設定](#bkmk_basic)

- [規劃網域存取](#bkmk_domain)

- [計劃託管的快取的大小與位置](#bkmk_cachelocation)

- [計劃是要複製的內容伺服器的套件共用](#bkmk_package)

- [計劃預先雜湊處理和資料封裝內容的伺服器上的建立](#bkmk_prehash)

## <a name="bkmk_basic"></a>規劃基本伺服器設定
  
如果您打算在使用您的分公司中的現有伺服器，做為託管快取伺服器，您不需要執行這個計畫的步驟，因為電腦已有名為，且具有 IP 位址設定。

在您的託管快取伺服器上安裝 Windows Server 2016 之後，您必須將電腦重新命名和指派與設定本機電腦的靜態 IP 位址。

>[!NOTE]
>在本指南中，託管快取伺服器稱為 HCS1，不過您應該使用適合您的部署的伺服器名稱。

## <a name="bkmk_domain"></a>規劃網域存取

如果您打算在使用您的分公司中的現有伺服器，做為託管快取伺服器，您不需要執行此計劃的步驟中，除非電腦目前未加入網域。
  
若要登入網域，電腦必須是網域成員電腦，必須先登入嘗試的 AD DS 中建立使用者帳戶。 此外，您必須將電腦加入網域的帳戶具有適當的群組成員資格。

## <a name="bkmk_cachelocation"></a>計劃託管的快取的大小與位置

在上 HCS1，決定您的託管快取伺服器上您要尋找託管快取。 比方說，決定硬碟、 磁碟區，以及您計劃用來儲存快取資料夾位置。

此外，決定您要為託管快取配置的磁碟空間百分比。

## <a name="bkmk_package"></a>計劃是要複製的內容伺服器的套件共用

您內容的伺服器上建立資料套件之後，您必須加以複製透過網路共用您的託管快取伺服器上。

計劃資料夾位置] 與 [共用的共用資料夾的權限。 此外，如果您的內容伺服器裝載大量的資料，因此您所建立的封裝大型檔案，計劃執行複製作業在 off\ – 尖峰時段，如此當其他人需要使用期間的複製作業不使用 WAN 頻寬 一般的商務作業的頻寬。

## <a name="bkmk_prehash"></a>計劃預先雜湊處理和資料封裝內容的伺服器上的建立

您在預先在內容伺服器上的內容雜湊之前，您必須識別包含您想要加入至資料封裝內容的檔案和資料夾。 

此外，您必須規劃您可以在其中儲存之前將它們複製到託管快取伺服器資料套件的本機資料夾位置。

若要繼續進行本指南，請參閱[BranchCache 託管快取模式部署](4-Bc-Hcm-Deployment.md)。
