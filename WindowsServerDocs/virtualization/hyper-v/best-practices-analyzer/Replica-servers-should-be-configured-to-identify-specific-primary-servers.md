---
title: 應設定複本伺服器，以識別授權傳送複寫流量的特定主伺服器
description: 提供指示以解決這個最佳做法分析程式規則所報告的問題。
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 0aeb1f4b-2e75-430b-9557-fe64738c4992
ms.date: 8/16/2016
ms.openlocfilehash: 2771a9814fb9061756626466f446ca6131580dc2
ms.sourcegitcommit: d08965d64f4a40ac20bc81b14f2d2ea89c48c5c8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/08/2020
ms.locfileid: "96864657"
---
# <a name="replica-servers-should-be-configured-to-identify-specific-primary-servers-authorized-to-send-replication-traffic"></a>應設定複本伺服器，以識別授權傳送複寫流量的特定主伺服器

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。

|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|警告|
|**類別**|組態|

在下列各節中，斜體指出出現在此問題的最佳做法分析程式工具中的 UI 文字。

## <a name="issue"></a>問題
*此複本伺服器會依照設定，接受來自所有主伺服器的複寫流量，並將其儲存在單一位置。*

### <a name="impact"></a>影響
*所有主伺服器的所有複寫都會儲存在一個位置，而這可能會導致隱私權或安全性問題。*

## <a name="resolution"></a>解決方法
*使用 Hyper-v 管理員為特定的主伺服器建立新的授權專案，並為每個伺服器指定個別的儲存位置。您可以使用萬用字元，將主伺服器分組為每個授權專案的集合。*

#### <a name="create-authorization-entries-using-hyper-v-manager"></a>使用 Hyper-v 管理員建立授權專案

1.  開啟 Hyper-V 管理員。 從伺服器管理員 (，請按一下 [**工具**  >  **hyper-v 管理員**]。 ) 

2.  從主機清單中，以滑鼠右鍵按一下您要的主機，然後按一下 [ **Hyper-v 設定**]。

3.  在流覽窗格中 **，按一下 [** 複寫設定]。

4.  在 [ **授權與存放裝置**] 下，按一下 **[允許從指定的伺服器** 複寫]。

5.  在伺服器清單底下，按一下 [ **新增**]。

6.  在 [ **新增授權專案**：

    -   輸入第一部伺服器的完整名稱。

    -   指定專用位置，只儲存該伺服器的檔案。

7.  按一下 [確定]。

8.  針對每一部主伺服器重複執行。

9. 再次按一下 **[確定** ] 以完成並關閉視窗。

### <a name="create-authorization-entries-using-windows-powershell"></a>使用 Windows PowerShell 建立授權專案

1.  開啟 Windows PowerShell。 從桌面 (中，按一下 [開始]，然後開始輸入 **Windows PowerShell**。 ) 

2.  以滑鼠右鍵按一下 **Windows PowerShell** ，然後按一下 [以 **系統管理員身分執行**]。

3.  執行類似下列的命令，並取代：

    -   Server01.domain01.contoso.com 的主伺服器名稱，以及伺服器的完整功能變數名稱。

    -   D:\ReplicaVMStorage 與您的位置的位置。

    -   名為 DEFAULT 的信任群組，如果您已建立，則為您群組的名稱。 如果沒有，請使用預設值。

```
New-VMReplicationAuthorizationEntry server01.domain01.contoso.com D:\ReplicaVMStorage DEFAULT
```

## <a name="see-also"></a>另請參閱
[新 New-vmreplicationauthorizationentry](/powershell/module/hyper-v/new-vmreplicationauthorizationentry)
