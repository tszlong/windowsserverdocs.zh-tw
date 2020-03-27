---
title: BranchCache 託管快取模式部署規劃
description: 本指南提供的指示說明如何在執行 Windows Server 2016 和 Windows 10 的電腦上，以託管快取模式部署 BranchCache。
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: article
ms.assetid: bc44a7db-f7a5-4e95-9d95-ab8d334e885f
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 09e6d6db0347e36c2cea01a0bc200edcaf4eb474
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80319050"
---
# <a name="branchcache-hosted-cache-mode-deployment-planning"></a>BranchCache 託管快取模式部署規劃

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

您可以使用本主題來規劃以託管快取模式部署 BranchCache。

>[!IMPORTANT]
>您的託管快取伺服器必須執行 Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012。

在您部署託管快取伺服器之前，必須先規劃下列專案：

- [規劃基本伺服器設定](#bkmk_basic)

- [規劃網域存取](#bkmk_domain)

- [規劃託管快取的位置和大小](#bkmk_cachelocation)

- [規劃要複製內容伺服器套件的共用](#bkmk_package)

- [規劃在內容伺服器上建立 prehashing 和資料封裝](#bkmk_prehash)

## <a name="plan-basic-server-configuration"></a><a name="bkmk_basic"></a>規劃基本伺服器設定
  
如果您打算使用分公司的現有伺服器做為託管快取伺服器，則不需要執行此規劃步驟，因為電腦已經命名，而且具有 IP 位址設定。

在您的託管快取伺服器上安裝 Windows Server 2016 之後，您必須重新命名電腦，並為本機電腦指派和設定靜態 IP 位址。

>[!NOTE]
>在本指南中，託管快取伺服器的名稱為 HCS1，但您應該使用適用于您的部署的伺服器名稱。

## <a name="plan-domain-access"></a><a name="bkmk_domain"></a>規劃網域存取

如果您打算使用分公司的現有伺服器做為託管快取伺服器，則不需要執行此規劃步驟，除非電腦目前未加入網域。
  
若要登入網域，電腦必須是網域成員電腦，而且必須在登入嘗試之前 AD DS 中建立使用者帳戶。 此外，您必須使用具有適當群組成員資格的帳戶，將電腦加入網域。

## <a name="plan-the-location-and-size-of-the-hosted-cache"></a><a name="bkmk_cachelocation"></a>規劃託管快取的位置和大小

在 HCS1 上，判斷您想要在託管快取伺服器上的哪個位置找出託管快取。 例如，決定您打算儲存快取的硬碟、磁片區和資料夾位置。

此外，決定您想要為託管快取配置的磁碟空間百分比。

## <a name="plan-the-share-to-which-the-content-server-packages-are-to-be-copied"></a><a name="bkmk_package"></a>規劃要複製內容伺服器套件的共用

在內容伺服器上建立資料套件之後，您必須透過網路將它們複製到託管快取伺服器上的共用。

規劃共用資料夾的資料夾位置和共用許可權。 此外，如果您的內容伺服器裝載大量的資料，而您建立的套件將是大型檔案，請規劃在離峰時間執行複製作業，如此一來，當其他人需要使用時，複製作業就不會耗用 WAN 頻寬一般商務作業的頻寬。

## <a name="plan-prehashing-and-data-package-creation-on-content-servers"></a><a name="bkmk_prehash"></a>規劃在內容伺服器上建立 prehashing 和資料封裝

在內容伺服器上預先雜湊內容之前，您必須識別包含您要新增至資料封裝之內容的資料夾和檔案。 

此外，您還必須規劃本機資料夾位置，讓您可以儲存資料封裝，然後再將它們複製到託管快取伺服器。

若要繼續進行本指南，請參閱[BranchCache 託管快取模式部署](4-Bc-Hcm-Deployment.md)。
