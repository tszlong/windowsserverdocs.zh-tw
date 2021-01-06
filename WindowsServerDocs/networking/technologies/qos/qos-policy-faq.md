---
title: QoS 常見問題
description: 本主題提供有關 Windows Server 2016 中的服務品質 (QoS) 原則的問題解答。
ms.topic: article
ms.assetid: 74c97a14-b957-4568-b48e-8963a674fdb3
manager: brianlic
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: a8e7fb93a10fe1d37845d1297c79ae7e389e8444
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97947264"
---
# <a name="qos-policy-frequently-asked-questions"></a>QoS 原則的常見問題

>適用於：Windows Server (半年度管道)、Windows Server 2016

以下是適用于 QoS 原則的常見問題（以及這些問題的答案）。

1.  **我的網域控制站必須執行哪些作業系統才能使用 QoS 原則？**

     Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2 或 Windows Server 2008

2.  **哪些作業系統支援將 QoS 原則應用至使用者或電腦？**

     您可以將 QoS 原則套用至執行 Windows Server 2016、Windows 10、Windows Server 2012 R2、Windows 8.1、Windows Server 2012、Windows 8、Windows Server 2008 R2、Windows Server 2008 和 Windows Vista 的使用者或電腦。

3.  **QoS 原則是否適用于流量的寄件者或接收者？**

     您必須在傳送的電腦上套用 QoS 原則，以影響其輸出流量。 為了影響兩部電腦的雙向流量，必須將 QoS 原則套用到這兩部電腦。

4.  **如果有衝突的 QoS 原則部署到同一部電腦，會發生什麼事？**

     如果套用了多個原則，則會優先使用較特定的 QoS 原則。 例如， (192.168.4.12 的) 的原則會套用，而不是 (192.168.0.0/16) 進行較不特定的網路位址。 如果電腦層級和使用者層級原則具有相同的明確，則會套用使用者層級的 QoS 原則，而不是電腦層級的 QoS 原則。

5.  **預設會啟用 QoS 原則嗎？**

     否，預設不會啟用 QoS 原則。 您必須手動建立 QoS 原則，才能啟用 QoS。  如需詳細資訊，請參閱 [管理 QoS 原則](qos-policy-manage.md)。

本指南的第一個主題中，請參閱 [服務品質 (QoS) 原則](qos-policy-top.md)。
