---
title: 管理 NPS 範本
description: 本主題提供有關如何建立、 套用、 匯出和匯入 Windows Server 2016 中的網路原則伺服器的 NPS 範本的指示。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 989b00c5-4767-4081-ace5-6321f8b2c55e
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 170a0e86f7dcca77c6efe841318b522554f8e78e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845129"
---
# <a name="manage-nps-templates"></a>管理 NPS 範本

>適用於：Windows Server （半年通道），Windows Server 2016

您可以使用網路原則伺服器\(NPS\)範本來建立組態項目，例如遠端驗證撥入使用者服務\(RADIUS\)用戶端或可重複使用在本機的共用的密碼NPS 與用於其他 NPSs 的匯出。 

範本管理提供您可以建立、 修改、 刪除、 複製，而檢視 NPS 範本的使用中的 [NPS] 主控台中的節點。 NPS 範本的設計目的是減少的時間與一或多個伺服器上設定 NPS 所花費的成本。

下列的 NPS 範本類型可供範本管理 中的組態。

- **共用祕密**。 此範本類型可讓您指定共用的密碼，您可以重複使用 （藉由選取範本，在 NPS 主控台中的適當位置中），當您設定 RADIUS 用戶端和伺服器。 

- **RADIUS 用戶端**。 此範本類型，您可以設定 RADIUS 用戶端設定，您可以藉由選取範本，在 NPS 主控台中的適當位置中重複使用。

- **遠端 RADIUS 伺服器**。 此範本可讓您設定遠端 RADIUS 伺服器設定，您可以藉由選取範本，在 NPS 主控台中的適當位置中重複使用。 

- **IP 篩選器**。 此範本可讓您建立網際網路通訊協定第 4 版 (IPv4) 和網際網路通訊協定第 6 版\(IPv6\)您可以重複使用的篩選器\(在 NPS 中的適當位置中選取範本主控台\)當您設定網路原則。

## <a name="create-an-nps-template"></a>建立 NPS 範本

設定範本是不同於直接設定 NPS。 建立範本並不會影響 NPS 的功能。 它是只有當您選取適當的位置，在 NPS 主控台中的範本和套用範本，會影響 NPS 功能的範本。 

例如，如果您在 NPS 主控台的 設定 RADIUS 用戶端**RADIUS 用戶端和伺服器**，alter NPS 設定，並需要設定 NPS 與其中一個網路存取伺服器進行通訊的其中一個步驟。 \(下一個步驟是設定網路存取伺服器\(NAS\)與 NPS 進行通訊。\) 

不過，如果您設定的新**RADIUS 用戶端**範本，在 NPS 主控台的 **範本管理**而非建立新的 RADIUS 用戶端下**RADIUS 用戶端和伺服器**，您已建立範本時，但您不具有尚未改變的 NPS 功能。 若要改變的 NPS 功能，您必須套用範本從 NPS 主控台中的正確位置。

下列程序說明如何建立新的範本。

中的成員資格**系統管理員**，或同等權限，才能完成此程序的最小值。

### <a name="to-create-an-nps-template"></a>若要建立 NPS 範本


1. 在 NPS 中，在 伺服器管理員 中，按一下 **工具**，然後按一下**網路原則伺服器**。 NPS 主控台隨即開啟。 

2. 在 NPS 主控台中，依序展開**範本管理**，例如以滑鼠右鍵按一下範本型別**RADIUS 用戶端**，然後按一下**新增**。

3. 新範本內容 對話方塊隨即開啟，您可用來設定您的範本。

## <a name="apply-an-nps-template"></a>套用 NPS 範本

您可以使用您在中建立的範本**範本管理**瀏覽至在 NPS 主控台中，您可以套用範本的位置。 例如，如果您想要套用到 RADIUS 用戶端設定的共用祕密範本，您可以使用下列程序。

中的成員資格**系統管理員**，或同等權限，才能完成此程序的最小值。

### <a name="to-apply-an-nps-template"></a>若要套用的 NPS 範本

1. 在 NPS 中，在 伺服器管理員 中，按一下 **工具**，然後按一下**網路原則伺服器**。 NPS 主控台隨即開啟。

2. 在 NPS 主控台中，依序展開**RADIUS 用戶端和伺服器**，然後展開**RADIUS 用戶端**。

3.在**RADIUS 用戶端**，在 詳細資料 窗格中，以滑鼠右鍵按一下您想要套用 NPS 範本，然後按一下 RADIUS 用戶端**屬性**。

4. 在 屬性 對話方塊 方塊中的 RADIUS 用戶端，在**選取現有的共用祕密範本**，選取您想要套用的範本清單中的範本。

## <a name="export-or-import-nps-templates"></a>匯出或匯入 NPS 範本

您可以匯出範本用於其他 NPSs，或您可以匯入到的範本**範本管理**本機電腦上使用。 

中的成員資格**系統管理員**，或同等權限，才能完成此程序的最小值。

### <a name="to-export-or-import-nps-templates"></a>若要匯出或匯入 NPS 範本

1. 若要匯出 NPS 範本，在 NPS 主控台中，以滑鼠右鍵按一下**範本管理**，然後按一下**將範本匯出到檔案**。

2. 若要匯入 NPS 範本，在 NPS 主控台中，以滑鼠右鍵按一下**範本管理**，然後按一下**匯入的範本，從電腦**或**從檔案匯入範本**。


