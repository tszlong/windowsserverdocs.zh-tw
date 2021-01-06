---
title: BranchCache 託管快取模式部署規劃
description: 本指南提供在執行 Windows Server 2016 和 Windows 10 的電腦上，以託管快取模式部署 BranchCache 的指示。
manager: brianlic
ms.topic: article
ms.assetid: bc44a7db-f7a5-4e95-9d95-ab8d334e885f
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 71b4988c488eca5c9cc371b2f3a7e4e5ecc2a145
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97949504"
---
# <a name="branchcache-hosted-cache-mode-deployment-planning"></a>BranchCache 託管快取模式部署規劃

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

您可以使用本主題來規劃以託管快取模式部署 BranchCache。

>[!IMPORTANT]
>您的託管快取伺服器必須執行 Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012。

部署託管快取伺服器之前，您必須先規劃下列專案：

- [規劃基本伺服器設定](#bkmk_basic)

- [規劃網域存取](#bkmk_domain)

- [規劃託管快取的位置和大小](#bkmk_cachelocation)

- [規劃要將內容伺服器套件複製到其中的共用](#bkmk_package)

- [在內容伺服器上建立 prehashing 和資料套件的規劃](#bkmk_prehash)

## <a name="plan-basic-server-configuration"></a><a name="bkmk_basic"></a>規劃基本伺服器設定

如果您打算使用分公司的現有伺服器作為託管快取伺服器，就不需要執行這項規劃步驟，因為電腦已經命名並有 IP 位址設定。

在託管快取伺服器上安裝 Windows Server 2016 之後，您必須將電腦重新命名，並為本機電腦指派並設定靜態 IP 位址。

>[!NOTE]
>在本指南中，託管快取伺服器的名稱為 HCS1，不過您應該使用適合您部署的伺服器名稱。

## <a name="plan-domain-access"></a><a name="bkmk_domain"></a>規劃網域存取

如果您打算使用分公司的現有伺服器作為託管快取伺服器，除非電腦目前未加入網域，否則您不需要執行此規劃步驟。

若要登入網域，電腦必須是網域成員電腦，而且必須在嘗試登入之前在 AD DS 中建立使用者帳戶。 此外，您必須使用具有適當群組成員資格的帳戶將電腦加入網域。

## <a name="plan-the-location-and-size-of-the-hosted-cache"></a><a name="bkmk_cachelocation"></a>規劃託管快取的位置和大小

在 HCS1 上，判斷您想要在託管快取伺服器上找出託管快取的位置。 例如，決定您打算儲存快取的硬碟、磁片區和資料夾位置。

此外，請決定您想要為託管快取配置的磁碟空間百分比。

## <a name="plan-the-share-to-which-the-content-server-packages-are-to-be-copied"></a><a name="bkmk_package"></a>規劃要將內容伺服器套件複製到其中的共用

在內容伺服器上建立資料封裝之後，您必須透過網路將它們複製到託管快取伺服器上的共用。

規劃共用資料夾的資料夾位置和共用許可權。 此外，如果您的內容伺服器裝載大量資料，而您建立的封裝會是大型檔案，請規劃在離峰時間執行複製作業，如此一來，當其他人需要使用頻寬來進行一般商務作業時，複製作業就不會耗用 WAN 頻寬。

## <a name="plan-prehashing-and-data-package-creation-on-content-servers"></a><a name="bkmk_prehash"></a>在內容伺服器上建立 prehashing 和資料套件的規劃

在內容伺服器上 prehash 內容之前，您必須識別包含您要新增至資料封裝之內容的資料夾和檔案。

此外，您必須在本機資料夾位置進行規劃，以便將資料封裝複製到託管快取伺服器。

若要繼續進行本指南，請參閱 [BranchCache 託管快取模式部署](4-Bc-Hcm-Deployment.md)。
