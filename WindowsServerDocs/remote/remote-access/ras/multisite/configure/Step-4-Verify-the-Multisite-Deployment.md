---
title: 步驟4驗證多網站部署
description: 瞭解如何確認您已正確設定遠端存取多網站部署。
manager: brianlic
ms.topic: article
ms.assetid: 345b676a-a397-4d51-9973-8b25bc05fa55
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 29c4c65673399b017716b2bfc299742f1d0c1307
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97949194"
---
# <a name="step-4-verify-the-multisite-deployment"></a>步驟4驗證多網站部署

>適用於：Windows Server (半年度管道)、Windows Server 2016

本主題說明如何確認您已正確設定遠端存取多網站部署。

### <a name="to-verify-access-to-internal-resources-through-the-multisite-deployment"></a>透過多網站部署確認內部資源的存取權

1.  將 DirectAccess 用戶端電腦連線到公司網路並取得群組原則。

2.  將用戶端電腦連線到外部網路，並嘗試存取內部資源。

    您應該能夠存取所有公司資源。

3.  藉由關閉或中斷外部網路（除了其中一個遠端存取服務器）的方式，在多網站部署中的每部伺服器上測試連線能力。 在用戶端電腦上，嘗試存取公司資源。 在不同的多網站伺服器上重複測試。 用戶端電腦最多可能需要10分鐘的時間才能連接到新的進入點。 這是因為探查會在被視為無法連線的進入點時關閉10分鐘，以便將頻寬和電池壽命優化。 或者，您可以從執行 **daprop.exe** 時所顯示的下拉式方塊中選擇所需的進入點，以手動方式在不同的進入點之間切換。

    您應該能夠透過每部多網站伺服器來存取所有公司資源。

4.  將 Windows 7 &reg;  用戶端電腦連線到公司網路，並取得群組原則。

5.  將 Windows 7 用戶端電腦連線到外部網路，並嘗試存取內部資源。

    您應該能夠存取所有公司資源。

6.  藉由存取 Active Directory 消費者和電腦主控台，並將用戶端電腦移至對應至每部伺服器的安全性群組，來測試 Windows 7 用戶端在多網站部署中每一部伺服器的連線能力。 在變更複寫到整個網域之後，請在連線到公司網路時重新開機用戶端電腦，以取得新的群組原則。 嘗試存取公司資源。 在不同的多網站伺服器上重複測試。

    您應該能夠透過每部多網站伺服器來存取所有公司資源。

    在生產環境中，此方法可能因為在整個網域中複寫變更所需的時間量而不可行。 您可能會想要盡可能強制進行複寫。 測試也可以從多個不同的 Windows 7 用戶端電腦進行，這些電腦已經是多網站部署中不同 Windows 7 安全性群組的成員。



