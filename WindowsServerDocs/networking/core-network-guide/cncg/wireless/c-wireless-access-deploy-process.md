---
title: 無線存取部署程序
description: 本主題是 Windows Server 2016 網路指南「部署 Password-Based 802.1 X 驗證無線存取」的一部分。
manager: brianlic
ms.topic: article
ms.assetid: 2555f238-926e-4b20-9bfb-9774831062da
author: eross-msft
ms.author: lizross
ms.date: 08/07/2020
ms.openlocfilehash: d8573992748402911a7366cb6e9059ef7b31966a
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97943214"
---
# <a name="wireless-access-deployment-process"></a>無線存取部署程序

>適用於：Windows Server (半年度管道)、Windows Server 2016

您用來部署無線存取的程式會在下列階段進行：

## <a name="stage-1--ap-deployment"></a>階段1– AP 部署

規劃、部署和設定您的 Ap 以進行無線用戶端連線，以及搭配 NPS 使用。 根據您的喜好設定和網路相依性，您可以在無線 Ap 上預先設定設定，然後 \- 將它們安裝在網路上，也可以在安裝之後從遠端設定。

## <a name="stage-2--ad-ds-group-configuration"></a>第2階段– AD DS 群組設定

在 AD DS 中，您必須建立一或多個無線使用者安全性群組。

接下來，識別允許無線存取網路的使用者。

最後，將使用者新增至您建立的適當無線使用者安全性群組。

>[!NOTE]
>依預設，[使用者帳戶撥入內容] 中的 [ **網路存取權限** ] 設定會 **透過 [透過 NPS 網路原則控制存取**] 設定進行設定。 除非您有特定原因要變更此設定，否則建議您保留預設值。 這可讓您透過在 NPS 中設定的網路原則來控制網路存取。

## <a name="stage-3--group-policy-configuration"></a>第3階段–群組原則設定

\(使用群組原則管理編輯器 Microsoft Management Console MMC，設定群組原則的無線網路 IEEE 802.11 \) 原則延伸 \( \) 。

若要 \- 使用無線網路原則中的設定來設定網域成員電腦，您必須套用群組原則。 電腦第一次加入網域時，會自動套用群組原則。 如果對群組原則進行了變更，則會自動套用新的設定：

- 依預先決定的 \- 間隔群組原則

- 如果網域使用者登出再重新登入網路

- 重新開機用戶端電腦並登入網域

您也可以在命令提示字元中執行命令 **gpupdate** ，以強制在登入電腦時重新整理群組原則。

## <a name="stage-4--nps-configuration"></a>階段4– NPS 設定

使用 NPS 中的設定向導，將無線存取點新增為 RADIUS 用戶端，並建立 NPS 在處理連線要求時使用的網路原則。

使用 wizard 建立網路原則時，請將 PEAP 指定為 EAP 類型，並指定在第二個階段中建立的無線使用者安全性群組。

## <a name="stage-5--deploy-wireless-clients"></a>第5階段-部署無線用戶端

使用用戶端電腦連接到網路。

針對可登入有線區域網路的網域成員電腦，重新整理群組原則時，會自動套用必要的無線設定。

如果您已啟用 [無線網路 IEEE 802.11 原則] 中的設定，在 \( \) 電腦于無線網路的廣播範圍內時自動連線，則您的無線已加入網域的 \- 電腦將會自動嘗試連線到無線區域網路。

若要連接到無線網路，使用者只需要在 Windows 提示時提供其網域使用者名稱和密碼認證。

若要規劃您的無線存取部署，請參閱 [無線存取部署規劃](d-wireless-access-planning.md)。
