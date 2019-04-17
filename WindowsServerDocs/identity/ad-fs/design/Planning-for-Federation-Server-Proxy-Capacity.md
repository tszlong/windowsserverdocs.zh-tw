---
ms.assetid: 3ecb6e87-17f1-4d38-97d2-9c4d52b7cf39
title: "規劃區域的聯盟 Proxy 伺服器的容量"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 2e57f34b173c10e9e753c7f3b8dcd88d7bf6742c
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="planning-for-federation-server-proxy-capacity"></a>規劃區域的聯盟 Proxy 伺服器的容量

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

規劃區域的聯盟的 proxy 伺服器的容量，可協助您估計：  
  
-   每個聯盟伺服器 proxy 適當的硬體需求。  
  
-   聯盟伺服器及聯盟伺服器 proxy 將每個組織中的數字。  
  
聯盟伺服器 proxy 重新導向從伺服器受保護的聯盟公司網路中的安全性權杖給聯盟使用者。 部署聯盟 proxy 伺服器的目的是讓使用者外部連接聯盟伺服器。 不會不確實權杖登入或寫入 AD FS 設定資料庫中的資料。 因此，聯盟 proxy 伺服器的硬體需求的通常低於聯盟伺服器的硬體需求。  
  
聯盟 proxy 伺服器的每個要求的結果在要求中聯盟伺服器或聯盟伺服器發電廠，因為必須同時執行規劃伺服器聯盟和聯盟的 proxy 伺服器的容量。  
  
估計的山峰 sign\ 單元聯盟 proxy 伺服器秒需要了解使用模式的聯盟使用者將會透過聯盟 proxy 伺服器登入。 在許多部署，聯盟使用聯盟 proxy 伺服器登入的使用者都位於網際網路。 您可以藉由在現有的 Web 應用程式將會受到 AD FS 查看這些聯盟使用者的使用模式估計的山峰 sign\ 單元秒。  
  
> [!NOTE]  
> 針對 production 部署，我們建議您的兩個聯盟伺服器 proxy 部署每個聯盟伺服器發電廠執行個體。  
  
## <a name="estimate-the-number-of-federation-server-proxies-required-for-your-organization"></a>估計聯盟伺服器 proxy 您的組織所需的數目  
您可以估計的數字之前所需 AD FS 聯盟伺服器 proxy 電腦，您必須先判斷聯盟伺服器，您將會在組織中部署總數。 如需如何執行此動作，請查看[規劃聯盟伺服器容量](Planning-for-Federation-Server-Capacity.md)。  
  
一旦您有認為同盟伺服器的數目乘這個數量的伺服器，連入聯盟驗證的百分比，要求您預期會進行外部使用者 \（位於公司 network\ 以外）。 這項計算的值，將提供您聯盟伺服器 proxy 將會處理您的外部使用者傳入驗證要求的預估數量。  
  
例如，如果建議的聯盟伺服器數目 3，您所預期的外部使用者將會進行驗證要求總數將約 60%總數聯盟的驗證要求的計算想等於 1.8 \ (3 X。60\)，您可以將最多 2。  因此，如此，您必須將有兩個聯盟伺服器 proxy 容納外部使用者的三個聯盟伺服器的驗證要求的電腦。  
  
AD FS product 小組所執行的測試，每個聯盟伺服器 proxy 的整體 CPU 使用率找會大幅低於觀察到的相同發電廠聯盟伺服器的 cpu。  單一測試，請在時同盟伺服器 CPU 已，它完全飽和，指出聯盟伺服器 proxy 提供的相同農地 proxy 服務的 CPU 觀察到，只有 20%使用量。 因此，我們的測試揭露的負載 CPU 聯盟伺服器 proxy，使用類似硬體規格所述先前在本區段中，可能會合理處理處理負載約三個聯盟伺服器。  
  
不過，錯誤容錯用途，我們建議針對每個您要部署的聯盟伺服器農場兩個聯盟伺服器 proxy 時最少提供。  
  
## <a name="see-also"></a>也了
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
