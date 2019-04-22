---
title: RDS-建置和部署
description: 若要建置的遠端桌面部署的步驟
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 04/18/2017
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 176ae424-96e9-4c78-88f5-da418e76c3d7
author: lizap
manager: dongill
ms.openlocfilehash: 0ab4be6ba326db05e8fc6a9c14e84c8cbb855926
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814749"
---
# <a name="build-and-deploy-your-remote-desktop-services-deployment"></a>建置及部署您的遠端桌面服務部署

遠端桌面服務部署是用來與使用者共用應用程式和資源的基礎結構。 根據您想要提供的體驗，您可以讓它調小或複雜視需要。 輕鬆地向遠端桌面部署。 您可以增加和減少遠端桌面 Web 存取，將會位於閘道、 連線代理人和工作階段主機伺服器。 您可以使用遠端桌面連線代理人來分散工作負載。 Active Directory 型驗證提供高度安全的環境。 

[遠端桌面用戶端](clients\remote-desktop-clients.md)啟用存取，從任何 Windows、 Apple 或 Android 的電腦、 平板電腦或電話。

請參閱[遠端桌面服務架構](desktop-hosting-logical-architecture.md)的不同部分，會一起運作以構成您的遠端桌面服務部署的詳細討論。

是否有現有的遠端桌面部署建置在舊版的 Windows Server 上？ 請參閱移至 WIndows Server 2016，其中您可以利用新的和更佳的功能，前後的效能和延展性的選項：

- [將您的 RDS 部署移轉至 Windows Server 2016](migrate-rds-role-services.md)
- [RDS 部署升級到 Windows Server 2016](upgrade-to-rds-2016.md)

想要建立新的遠端桌面部署嗎？ 您可以使用下列資訊來部署 Windows Server 2016 中的遠端桌面：

- [部署遠端桌面服務基礎結構](rds-deploy-infrastructure.md)
- [建立工作階段集合，來保存應用程式和您想要共用的資源](rds-create-collection.md)
- [授權您的 RDS 部署](rds-client-access-license.md)
- 讓使用者安裝[遠端桌面用戶端](clients/remote-desktop-clients.md)讓他們可以存取應用程式和資源。 
- 藉由新增其他連線代理人及工作階段主機啟用高可用性：
   - [相應放大現有 RDS 集合與 RD 工作階段主機伺服器陣列](rds-scale-rdsh-farm.md)
   - [新增 RD 連線代理人基礎結構高可用性](rds-connection-broker-cluster.md)
   - [將 RD Web 和 RD 閘道的 web 前端的高可用性](rds-rdweb-gateway-ha.md)
   - [部署兩個節點儲存空間直接存取檔案系統針對 UDP 儲存體](rds-storage-spaces-direct-deployment.md)


如果您是主控夥伴想要使用遠端桌面來提供應用程式和資源給客戶，或是尋找某人來裝載您的應用程式的客戶，請參閱[遠端桌面服務裝載夥伴](rds-hosting-partners.md)的相關資訊您可以採取關於在 Azure 中使用 RDS，作為裝載環境中，以及一份已將它傳遞給合作夥伴的評估。
