---
title: 保護您的 RDS 部署 - 災害復原
description: 了解遠端桌面服務的災害復原選項
ms.topic: article
ms.assetid: 9ff6a3b0-ea14-424e-9524-209252e9f1a8
author: lizap
ms.author: elizapo
ms.date: 06/12/2017
ms.openlocfilehash: e9270c6d1609e67d4875afb2a9ba971de14c1c7c
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87936892"
---
# <a name="configure-disaster-recovery-for-remote-desktop-services"></a>設定遠端桌面服務的災害復原

當您將遠端桌面服務部署到您的環境後，它就會成為您基礎結構的重要部分，特別是您與使用者共用的應用程式和資源。 如果 RDS 部署因任何因素 (從網路故障乃至於自然災害) 而失效，使用者即無法存取這些應用程式和資源，而您的業務將受到負面影響。 若要避免此狀況，您可以設定災害復原解決方案以便將部署容錯移轉 - 如果您的 RDS 部署因任何原因而無法使用，將有備用的部署可自動接管。

為了讓您的 RDS 部署在單一元件或電腦停止運作的情況下仍可執行，建議您為 RDS 部署進行高可用性設定。 為此，您可以設定 [RDSH 伺服器陣列](rds-scale-rdsh-farm.md)，並確定您的[連線代理人位於高可用性叢集中](rds-connection-broker-cluster.md)。

我們在此建議的災害復原解決方案，是為了要保護您的部署不受嚴重災害影響 - 會導致您整個 RDS 部署 (包括針對高可用性而設定的備援角色) 停止運作的狀況。 萬一發生這類災害時，在您的部署中建置災害復原解決方案，可讓您將整個部署容錯移轉，並且為您的使用者快速啟動並執行應用程式和資源。

請使用下列資訊在 RDS 中部署災害復原解決方案：

- [使用多個 Azure 資料中心，以確保即使在一個 Azure 資料中心停止運作時，使用者仍可存取您的 RDS 部署 (異地備援)](rds-multi-datacenter-deployment.md)
- [部署 Azure Site Recovery，為 RDS 元件提供站台對站台或站台對 Azure 的容錯移轉](rds-disaster-recovery-with-azure.md)


