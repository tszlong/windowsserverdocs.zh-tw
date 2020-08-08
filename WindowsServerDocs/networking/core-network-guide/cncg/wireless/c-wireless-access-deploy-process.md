---
title: 無線存取部署程序
description: 本主題是 Windows Server 2016 網路指南「部署以密碼為基礎的 802.1 X 驗證無線存取」的一部分
manager: brianlic
ms.topic: article
ms.assetid: 2555f238-926e-4b20-9bfb-9774831062da
author: eross-msft
ms.author: lizross
ms.openlocfilehash: 9c2326df824288b6adf4453d6ef272ba632eb6c2
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87969595"
---
# <a name="wireless-access-deployment-process"></a>無線存取部署程序

>適用於：Windows Server (半年度管道)、Windows Server 2016

您用來部署無線存取的程式會發生在下列階段：

## <a name="stage-1--ap-deployment"></a>第1階段– AP 部署

規劃、部署和設定您的 Ap 以進行無線用戶端連線，以及與 NPS 搭配使用。 視您的喜好和網路相依性而定，您可以在無線 Ap 上預先進行設定，然後再將 \- 它們安裝在您的網路上，或是在安裝後從遠端設定。

## <a name="stage-2--adds-group-configuration"></a>第2階段– AD DS 群組設定

在 AD DS 中，您必須建立一或多個無線使用者安全性群組。

接下來，識別允許無線存取網路的使用者。

最後，將使用者新增至您所建立的適當無線使用者安全性群組。

>[!NOTE]
>根據預設，[使用者帳戶] [撥入內容] 中的 [**網路存取權限**] 設定是使用 [**透過 NPS 網路原則來控制存取**] 設定進行設定。 除非您有特定的理由要變更此設定，否則建議您保留預設值。 這可讓您透過 NPS 中設定的網路原則來控制網路存取。

## <a name="stage-3--group-policy-configuration"></a>第3階段–群組原則設定

\( \) 使用群組原則管理編輯器 Microsoft Management Console MMC，設定群組原則的無線網路 IEEE 802.11 原則延伸 \( \) 。

若要 \- 使用無線網路原則中的設定來設定網域成員電腦，您必須套用群組原則。 當電腦第一次加入網域時，會自動套用群組原則。 如果對群組原則進行變更，則會自動套用新的設定：

- 依群組原則預先 \- 決定的間隔

- 如果網域使用者登出後再重新登入網路

- 重新開機用戶端電腦並登入網域

您也可以在命令提示字元中執行命令**gpupdate** ，強制群組原則在登入電腦時重新整理。

## <a name="stage-4--nps-configuration"></a>第4階段– NPS 設定

使用 NPS 中的設定向導，將無線存取點新增為 RADIUS 用戶端，並建立 NPS 在處理連線要求時所使用的網路原則。

使用 wizard 建立網路原則時，請指定 PEAP 作為 EAP 類型，以及在第二個階段中建立的無線使用者安全性群組。

## <a name="stage-5--deploy-wireless-clients"></a>第5階段–部署無線用戶端

使用用戶端電腦連接到網路。

對於可登入有線 LAN 的網域成員電腦，當群組原則重新整理時，就會自動套用必要的無線配置設定。

如果您已啟用無線網路 IEEE 802.11 原則中的設定， \( \) 以便在電腦在無線網路的廣播範圍內時自動連線，您的無線、 \- 加入網域的電腦將會自動嘗試連線到無線區域網路。

若要連接到無線網路，使用者只需要在 Windows 提示時提供其網域使用者名稱和密碼認證。

若要規劃您的無線存取部署，請參閱[無線存取部署規劃](d-wireless-access-planning.md)。
