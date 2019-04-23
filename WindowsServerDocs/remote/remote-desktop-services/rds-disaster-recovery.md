---
title: 保護您的 RDS 部署-嚴重損壞修復
description: 深入了解遠端桌面服務的災害復原選項
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9ff6a3b0-ea14-424e-9524-209252e9f1a8
author: lizap
ms.author: elizapo
ms.date: 06/12/2017
ms.openlocfilehash: a6eac3a50999633d15b1b6dc28608f60f6fef6c7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834079"
---
# <a name="configure-disaster-recovery-for-remote-desktop-services"></a>設定遠端桌面服務的災害復原

當您將遠端桌面服務部署到您的環境時，它會變成基礎結構，特別是應用程式和您與使用者共用的資源的重要部分。 如果 RDS 部署故障任何項目因為網路故障自然災害，使用者無法存取這些應用程式和資源，並會嚴重影響您的業務。 若要避免這個問題，您可以設定災害復原解決方案，可讓您容錯移轉您的部署-如果因為任何原因無法使用，您的 RDS 部署，則沒有備份可用來自動接管。

為了讓您在單一元件或電腦停止運作的情況下執行的 RDS 部署，我們建議您針對高可用性的 RDS 部署設定。 您可以藉由設定[RDSH 伺服器陣列](rds-scale-rdsh-farm.md)，並確保您[連線代理人高可用性叢集](rds-connection-broker-cluster.md)。 

災害復原解決方案，我們建議您要保護您的部署，從嚴重損毀-會關閉整個 RDS 部署 （包括設定為高可用性的備援角色） 的項目。 如果這類災害發生時，您的部署內建的災害復原解決方案會讓您容錯移轉整個部署而快速應用程式和資源並執行您的使用者。

您可以使用下列資訊來部署 rds 的災害復原解決方案：

- [利用多個 Azure 資料中心，以確保使用者可以存取 RDS 部署中，即使一個 Azure 資料中心中斷 （異地備援）](rds-multi-datacenter-deployment.md)
- [部署 Azure Site Recovery，以提供容錯移轉，若是站對站或站台至 Azure 的容錯移轉中的 RDS 元件](rds-disaster-recovery-with-azure.md)


