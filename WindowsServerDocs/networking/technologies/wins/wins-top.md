---
title: Windows 網際網路名稱服務 (WINS)
description: 本主題提供將 WINS 解除委任的相關資訊，以及在網路上使用 DNS 進行名稱解析服務的相關資訊。
manager: brianlic
ms.topic: article
ms.assetid: 32eabe7d-1130-4001-a79a-8ddb31993e5b
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: b512df968dcaee068ce014a1ec59e86aacbac812
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97947424"
---
#  <a name="windows-internet-name-service-wins"></a>Windows 網際網路名稱服務 (WINS)

>適用於：Windows Server (半年度管道)、Windows Server 2016

Windows 網際網路名稱服務 (WINS) 是舊版電腦名稱登錄與解析服務，可將電腦 NetBIOS 名稱對應至 IP 位址。

如果您還沒有在網路上部署 WINS，請不要部署 WINS，而是部署網域名稱系統 \( DNS \) 。 DNS 也提供電腦名稱稱註冊和解析服務，並包含許多優於 WINS 的額外優點，例如與 Active Directory Domain Services 整合。

如需詳細資訊，請參閱 [網域名稱系統 (DNS) ](../../dns/dns-top.md)

如果您已在網路上部署 WINS，建議您先部署 DNS，再解除委任 WINS。