---
title: "自動安裝在設定期間的增益集"
description: "告訴您如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2e6ff6e4-8d68-4d49-9e38-8088bc8bf95e
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: d4c547c2fec8e2b11e5c1d9bde46e55e91c9d6fa
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="automate-installation-of-add-ins-during-setup"></a>自動安裝在設定期間的增益集

>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集

##  <a name="BKMK_AddIns"></a>在設定期間中自動安裝增益集  
 在設定期間安裝增益集，請使用中所述 PostIC.cmd 方法[建立執行文章初始設定工作 PostIC.cmd 檔案](Create-the-PostIC.cmd-File-for-Running-Post-Initial-Configuration-Tasks.md)區段本文件。  
  
 將下列項目新增至您 PostIC.cmd:  
  
```  
C:\Program Files\Windows Server\bin\Installaddin.exe <full path to wssx file> -q  
```  
  
 增益集，現在支援預先安裝並自訂解除安裝步驟。  
  
 安裝所有之前的預先安裝步驟執行**.msi**中 addin.xml 指定的檔案。 互動模式中執行時，進度對話方塊將會顯示，但不需要進行的變更。 [取消] 按鈕已停用階段預先安裝。 若要實作預先安裝步驟，請在 [addin.xml （直接在套件） 中新增下列到：  
  
> [!NOTE]
>  完全遵循下一個 xml 架構需要：  
  
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
  
 Wherein **exefile**來執行預先安裝的步驟，增益集套件中的可執行檔，您必須指定。 **NormalArgs**指定引數傳送到 exefile 模式命令列互動時使用。 在此模式下，exefile 可以快顯功能表有些對話方塊的使用者互動。 **SilentArgs**指定使用命令列無訊息時模式傳遞至 exefile 引數 (-q 指定叫用 installaddin.exe 時)。 Exefile 應該不快顯功能表任何 windows 在此模式。 如果**IgnoreExitCode**指定為 true，以預先安裝步驟一定會被視為成功，否則結束代碼 0 表示成功、 1 表示取消，和其他值表示失敗。 標記**NormalArgs**， **SilentArgs**，並**IgnoreExitCode**是所有選用。  
  
 自訂解除安裝步驟可使用下列其中一項：  
  
-   取代建確認對話方塊。  
  
-   填入解除安裝之前自訂的對話方塊。  
  
-   執行特定工作之前解除安裝。  
  
 若要實作解除安裝步驟，請在 [addin.xml （直接在套件） 中新增下列到：  
  
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
  
 Wherein**完整-path-到-exefile**指定 exefile 已安裝在系統。 **引數**是選擇性的並在指定的命令列 exefile 引數。 之前建解除安裝確認叫用 exefile 對話方塊彈出。  
  
 Exefile 可以執行下列這個階段中的任務：  
  
-   跳出有些對話方塊的使用者互動。  
  
-   執行一些背景工作。  
  
 Exe 檔案結束代碼判斷解除安裝程序如何向前移動：  
  
-   0： 使用者已經有確認一樣持續填入建確認對話方塊，而解除安裝程序。 （這種方式可以用來更換建確認對話方塊。）  
  
-   1： 取消解除安裝程序，並取消的郵件最後會顯示使用者。 所有保持不變。  
  
-   其他： 解除安裝程序就會繼續建確認] 對話方塊中，就像不存在自訂解除安裝步驟。  
  
 Exefile 傳回 0 或 1 以外的程式碼在相同的行為會導致 exefile 叫用的任何失敗。  
  
## <a name="see-also"></a>也了  
 [建立和自訂映像](Creating-and-Customizing-the-Image.md)   
 [其他的自訂項目](Additional-Customizations.md)   
 [準備部署映像](Preparing-the-Image-for-Deployment.md)   
 [測試客戶體驗](Testing-the-Customer-Experience.md)