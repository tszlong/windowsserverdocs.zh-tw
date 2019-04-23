---
title: 無線存取部署程序
description: 本主題是 Windows Server 2016 網路功能指南 」 部署密碼型的 802.1 X 驗證無線存取 」 的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 2555f238-926e-4b20-9bfb-9774831062da
author: shortpatti
ms.author: pashort
ms.openlocfilehash: 6a286cf10e066043ee6f514bbf468bfb2b13f162
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879839"
---
# <a name="wireless-access-deployment-process"></a>無線存取部署程序

>適用於：Windows Server （半年通道），Windows Server 2016

您用來部署無線存取的程序分為下列階段：

## <a name="stage-1--ap-deployment"></a>階段 1 – AP 部署

規劃、 部署及設定您的 Ap，無線用戶端連接性，以及用於 nps。 您可以在根據您的喜好設定和網路相依性，請先\-您之前將它們安裝在您的網路上的無線 Ap 上設定，或者您可以從遠端安裝後設定它們。

## <a name="stage-2--adds-group-configuration"></a>階段 2-將 AD DS 群組組態

在 AD DS 中，您必須建立一或多個無線使用者的安全性群組。

接下來，識別允許無線網路存取權的使用者。

最後，將使用者新增至您所建立的適當無線使用者安全性群組中。

>[!NOTE]
>根據預設，**網路存取權限**使用者帳戶撥入內容中的設定已設定使用**透過 NPS 網路原則控制存取**。 除非您有特定的理由要變更此設定時，建議您保留預設值。 這可讓您控制您在 NPS 中設定網路原則透過網路存取。

## <a name="stage-3--group-policy-configuration"></a>階段 3-群組原則設定

設定無線網路\(IEEE 802.11\)原則群組原則中的延伸使用群組原則管理編輯器 Microsoft Management Console \(MMC\)。

若要設定網域\-成員電腦使用中的無線網路原則的設定，您必須套用群組原則。 當電腦第一次加入網域時，會自動套用群組原則。 如果群組原則進行變更，會自動套用新的設定：

- 由群組原則，在預先\-決定間隔

- 如果是網域使用者登出，再備份到網路

- 重新啟動用戶端電腦並登入網域

您也可以強制群組原則 」 來執行命令來登入電腦時，重新整理**gpupdate**在命令提示字元。

## <a name="stage-4--nps-configuration"></a>階段 4-NPS 設定

使用設定精靈 在 NPS 中，將無線存取點新增為 RADIUS 用戶端，以及建立處理連線要求時，會使用 NPS 網路原則。

當使用精靈建立網路原則，指定 PEAP EAP 類型，以及無線使用者安全性群組中的第二個階段所建立。

## <a name="stage-5--deploy-wireless-clients"></a>階段 5-部署無線用戶端

您可以使用用戶端電腦連線到網路。

有線區域網路登入的網域成員電腦，必要的無線組態設定會自動套用群組原則重新整理。

如果您已啟用無線網路中的設定\(IEEE 802.11\)原則，以自動連線電腦時內廣播無線網路、 無線、 網域範圍\-接著會加入的電腦會自動嘗試連線到無線區域網路。

若要連線到無線網路，使用者只需要提供其網域使用者名稱和密碼認證的 Windows 出現提示時。

若要規劃您的無線存取部署，請參閱[無線存取部署規劃](d-wireless-access-planning.md)。
