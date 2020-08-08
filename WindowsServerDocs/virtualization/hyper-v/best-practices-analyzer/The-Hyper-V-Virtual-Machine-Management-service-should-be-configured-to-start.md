---
title: Hyper-v 虛擬機器管理服務應設定為自動啟動
description: 提供解決此最佳做法分析程式規則所回報之問題的指示。
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: 222bbe76-c514-4a3f-b61b-860a4dc2826a
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 73ef8f7de89da5a05fedd53b9b23a32fed683dbf
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87960481"
---
# <a name="the-hyper-v-virtual-machine-management-service-should-be-configured-to-start-automatically"></a>Hyper-v 虛擬機器管理服務應設定為自動啟動

>適用於：Windows Server 2016

如需最佳做法和掃描的詳細資訊，請參閱[最佳做法分析程式](https://go.microsoft.com/fwlink/?LinkId=122786)。

|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|警告|
|**類別**|組態|

在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。

## <a name="issue"></a>問題

*Hyper-v 虛擬機器管理服務未設定為自動啟動。*

## <a name="impact"></a>影響

*在服務啟動之前，無法管理虛擬機器。*

正在執行的虛擬機器將會繼續執行。 不過，在服務執行之前，您將無法管理虛擬機器，或建立或刪除它們。

## <a name="resolution"></a>解決方法

*使用 [服務] 嵌入式管理單元或 sc config 命令列工具，將服務重新設定為自動啟動。*

> [!TIP]
> 如果您在桌面應用程式中找不到該服務，或命令列工具報告該服務不存在，可能是尚未安裝 Hyper-v 管理工具。 若要進行安裝：
>
> - 在 Windows Server 上，開啟伺服器管理員並使用 [新增角色及功能]。 如需詳細資訊，請參閱[在 Windows Server 2016 上安裝 hyper-v 角色](../get-started/Install-the-Hyper-V-role-on-Windows-Server.md)。
> - 在 Windows 上，從桌面開始輸入**程式**，按一下 [**程式和功能**] (控制台) > [**開啟或關閉 Windows 功能**] [hyper-v] [hyper-v  >  **Hyper-V**  >  **管理工具**]。 然後按一下 **[確定]**。

#### <a name="to-reconfigure-the-service-to-start-automatically-using-the-services-desktop-app"></a>若要將服務重新設定為使用服務桌面應用程式自動啟動

1.  開啟 [服務] 桌面應用程式。  (按一下 [**開始**]，在 [搜尋] 方塊中按一下，開始輸入**服務**，然後按一下結果清單中的 [服務]。

2.  在詳細資料窗格中，以滑鼠**按右鍵 [** **Hyper-v 虛擬機器管理**]，然後按一下 [內容]。

3.  在 [**一般**] 索引標籤的 [**啟動**類型] 中，按一下 [**自動**]。

#### <a name="to-reconfigure-the-service-to-start-automatically-using-the-sc-config-command"></a>若要將服務重新設定為使用 SC Config 命令自動啟動

1.  開啟 Windows PowerShell。

2.  輸入：

    ```
    set-service  vmms -startuptype automatic
    ```

3.  如果服務尚未執行，請輸入：

    ```
    start-service -name vmms
    ```



