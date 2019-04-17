---
title: Wireless 存取部署程序
description: 本主題是 Windows Server 2016 網路指南「部署密碼基礎 802.1 X 驗證無線存取」的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 2555f238-926e-4b20-9bfb-9774831062da
author: shortpatti
ms.author: pashort
ms.openlocfilehash: fcdbe796e4d530604dfec448e817eab877d34c4f
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="wireless-access-deployment-process"></a>Wireless 存取部署程序

>適用於：Windows Server（以每年次管道）、Windows Server 2016

您用來部署 wireless 存取程序就會發生在這些階段：

## <a name="stage-1--ap-deployment"></a>步驟 1 – 部署 AP

規劃、 部署、 和設定 wireless client 連接和使用您 Ap 具有 NPS。 根據您的喜好設定和網路相依性，您可以任一 pre\-設定您的 wireless Ap 之前安裝在您的網路，或您在安裝之後從遠端設定。

## <a name="stage-2--ad-ds-group-configuration"></a>步驟 2: AD DS 群組設定

在 AD DS，您必須建立一或多個 wireless 使用者安全性群組。

接下來，找出允許 wireless 網路存取權的使用者。

最後，將使用者新增至您所建立的適當 wireless 使用者安全性群組。

>[!NOTE]
>根據預設，**的網路存取權限**在撥號屬性 account 使用者設定的設定**控制 NPS 的網路原則透過**。 除非您有特定的理由，來變更此設定時，建議您持續預設值。 這可讓您控制透過 NPS 設定您的網路原則的網路存取。

## <a name="stage-3--group-policy-configuration"></a>步驟 3-群組原則設定

設定無線網路 \ (IEEE 802.11\) 原則擴充功能的群組原則來使用群組原則編輯器] 管理 Microsoft Management Console \(MMC\)。

若要設定 domain\ 成員電腦使用中的 wireless 的網路原則設定，您必須適用於群組原則。 當您第一次加入網域的電腦時，會自動套用群組原則。 如果的變更群組原則，會自動套用新的設定：

- 群組原則 pre\ 判斷的時間間隔

- 如果使用者網域登出，然後返回入網路

- Client 開機並登入的網域

您也可以強制群組原則來重新整理時登入電腦的執行命令**gpupdate**在命令提示字元。

## <a name="stage-4--nps-server-configuration"></a>步驟 4 – NPS 伺服器設定

使用設定精靈中 NPS RADIUS 戶端，以新增 wireless 存取點，該 NPS 建立的網路原則時使用處理連接要求。

使用時，精靈建立的網路原則，指定 PEAP EAP 類型、 以及所建立的第二個階段中的使用者 wireless 安全性群組。

## <a name="stage-5--deploy-wireless-clients"></a>步驟 5 – 部署 wireless 戶端

使用 client 電腦連接到網路。

網域成員可以登入有線的區域網路電腦，所需的 wireless 設定會自動套用更新群組原則時。

如果您有支援 Wireless 網路中的設定 \ (IEEE 802.11\) 原則，以自動將連接電腦時廣播範圍 wireless 網路，然後您 wireless、 domain\ 加入電腦將會自動嘗試 wireless 區域網路來連接。

若要連接到 wireless 網路，使用者必須只提供網域使用者名稱和密碼憑證出現提示時，Windows。

若要計畫 wireless 存取部署，請查看[Wireless 存取部署規劃](d-wireless-access-planning.md)。
