---
title: 安裝期間自動安裝增益集
description: 瞭解如何使用 Postic.cmd 方法，在 Windows Server Essentials 安裝期間自動安裝增益集。
ms.date: 10/03/2016
ms.topic: article
ms.assetid: 2e6ff6e4-8d68-4d49-9e38-8088bc8bf95e
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: 0982cfc5064167044466a82ee44a575717596e2a
ms.sourcegitcommit: d2224cf55c5d4a653c18908da4becf94fb01819e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/21/2020
ms.locfileid: "97711483"
---
# <a name="automate-installation-of-add-ins-during-setup"></a>安裝期間自動安裝增益集

>適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials

##  <a name="automate-installing-add-ins-during-setup"></a><a name="BKMK_AddIns"></a> 在安裝期間自動安裝增益集
 若要在安裝過程中安裝增益集，請使用本文件中的[建立 PostIC.cmd 檔案以便執行初始設定後續的工作](Create-the-PostIC.cmd-File-for-Running-Post-Initial-Configuration-Tasks.md)一節中描述的 PostIC.cmd 方法。

 將下列項目新增到您的 PostIC.cmd：

```
C:\Program Files\Windows Server\bin\Installaddin.exe <full path to wssx file> -q
```

 增益集現在支援前置安裝和自訂解除安裝步驟。

 安裝 addin.xml 中指定的所有 **.msi** 檔案之前，會先執行前置安裝步驟。 以互動模式執行時，將顯示進度對話方塊，但不會變更進度。 在前置安裝階段，會停用取消按鈕。 若要實作前置安裝步驟，請在 addin.xml 中新增下列內容 (直接位於 Package 下)：

> [!NOTE]
>  xml 結構描述需要嚴格遵循下列規定：

```
<Package xmlns="https://schemas.microsoft.com/WindowsServerSolutions/2010/03/Addins" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
  <Id>...</Id>
  <Version>...</Version>
  <Name>...</Name>
  <Allow32BitOn64BitClients>...</Allow32BitOn64BitClients>
  <ServerBinary>...</ServerBinary>
  <ClientBinary32>...</ClientBinary32>
  <ClientBinary64>...</ClientBinary64>
  <SupportedSkus>...</SupportUrl>
  <SupportUrl>...</SupportUrl>
  <Location>...</Location>
  <PrivacyStatement>...</PrivacyStatement>
  <OtherBinaries>...</OtherBinaries>
  <Preinstall>
<Executable>exefile</Executable>
<NormalArgs>args-for-interactive-mode</NormalArgs>
<SilentArgs>args-for-silent-mode</SilentArgs>
<IgnoreExitCode>true</IgnoreExitCode>
  </Preinstall>
  <UninstallConfirm>...</UninstallConfirm>
</Package>
<¦>
<¦>
```

 其中 **exefile** 是增益集套件中的可執行檔，用於執行前置安裝步驟，必須加以指定。 **NormalArgs** 指定在使用互動模式時，要在命令行中遞送給 exefile 的引數。 在此模式中，exefile 會快顯一些對話方塊，用來與使用者互動。 **SilentArgs** 指定在使用無訊息模式時，要在命令行中遞送給 exefile 的引數 (叫用 installaddin.exe 時，會指定 -q)。 在此模式中，exefile 不應快顯任何視窗。 如果將 **IgnoreExitCode** 指定為 true，將一律認為前置安裝步驟成功，否則將以結束碼 0 表示成功，1 表示取消，其他值表示失敗。 **NormalArgs**、**SilentArgs** 和 **IgnoreExitCode** 標記都是選用的。

 可針對下列任何情況使用自訂解除安裝步驟：

- 取代內建確認對話方塊。

- 在解除安裝之前填入自訂對話方塊。

- 在解除安裝之前執行特定的工作。

  若要實作解除安裝步驟，請在 addin.xml 中新增下列內容 (直接位於 Package 下)：

```
<Package xmlns="https://schemas.microsoft.com/WindowsServerSolutions/2010/03/Addins" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
  <Id>...</Id>
  <Version>...</Version>
  <Name>...</Name>
  <Allow32BitOn64BitClients>...</Allow32BitOn64BitClients>
  <ServerBinary>...</ServerBinary>
  <ClientBinary32>...</ClientBinary32>
  <ClientBinary64>...</ClientBinary64>
  <SupportedSkus>...</SupportUrl>
  <SupportUrl>...</SupportUrl>
  <Location>...</Location>
  <PrivacyStatement>...</PrivacyStatement>
  <OtherBinaries>...</OtherBinaries>
  <Preinstall>¦</Preinstall>
<UninstallConfirm>
<Executable>full-path-to-exefile</Executable>
<Arguments>command-line-arguments</Arguments>
</UninstallConfirm>
</Package>
```

 其中 **full-path-to-exefile** 表示 exefile 已安裝在系統中。 **Arguments** 是選用的，並可指定 exefile 的命令行引數。 exefile 會在快顯內建解除安裝確認對話方塊之前叫用。

 exefile 可在此階段中執行下列工作：

- 快顯某些對話方塊，用來與使用者互動。

- 執行某些背景工作。

  此 exe 檔案的結束碼會決定解除安裝程序如何推進：

- 0：解除安裝程序繼續執行，但不填入內建確認對話方塊，就如同使用者已確認那樣。 (可使用此方法來取代內建確認對話方塊)；

- 1：解除安裝程序取消，最終會向使用者顯示已取消的訊息。 一切保持不變；

- 其他：解除安裝程序繼續執行內建確認對話方塊，就像自訂解除安裝步驟不存在一樣。

  任何叫用 exefile 失敗的情況，都會導致與 exefile 傳回非 0 或 1 字碼相同的行為。

## <a name="see-also"></a>另請參閱
 [建立和自訂映射](Creating-and-Customizing-the-Image.md)[其他自訂](Additional-Customizations.md)專案[準備映射以進行部署](Preparing-the-Image-for-Deployment.md)[測試客戶體驗](Testing-the-Customer-Experience.md)