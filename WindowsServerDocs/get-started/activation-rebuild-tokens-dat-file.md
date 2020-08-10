---
title: 重建 Tokens.dat 檔案
description: 在對 Windows 啟用問題進行疑難排解時如何重建 Tokens.dat 檔案
ms.topic: troubleshooting
ms.date: 09/18/2019
author: Teresa-Motiv
ms.author: v-tea
manager: dcscontentpm
ms.localizationpriority: medium
ms.openlocfilehash: bc44dae97422e4d9d9e55b32004f806bbb7860f7
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87941780"
---
# <a name="rebuild-the-tokensdat-file"></a>重建 Tokens.dat 檔案

在對 Windows 啟用問題進行疑難排解時，您可能必須重建 Tokens.dat 檔案。 本文將詳細說明如何執行這項作業。

## <a name="resolution"></a>解決方法

若要重建 Tokens.dat 檔案，請遵循下列步驟：

1. 開啟提升權限的 [命令提示字元] 視窗：**針對 Windows 10**

   1. 開啟 [開始]  功能表，然後輸入 **cmd**。
   1. 在搜尋結果中，以滑鼠右鍵按一下 [命令提示字元]  ，然後選取 [以系統管理員身分執行]  。

   **針對 Windows 8.1**
   1. 從螢幕右邊緣向內撥動，然後點選 [搜尋]  。 或者，如果您使用滑鼠，請指向螢幕右下角，然後選取 [搜尋]  。
   1. 在搜尋方塊中，輸入 **cmd**。
   1. 撥動或以滑鼠右鍵按一下顯示的 [命令提示字元]  圖示。
   1. 點選或按一下 [以系統管理員身分執行]  。

   **針對 Windows 7**
   1. 開啟 [開始]  功能表，然後輸入 **cmd**。
   1. 在搜尋結果中，以滑鼠右鍵按一下 **cmd.exe**，然後選取 [以系統管理員身分執行]  。

1. 輸入您的作業系統適用的命令清單。

   針對 Windows 10、Windows Server 2016 和更新版本的 Windows，請依序輸入下列命令：
   ```cmd
   net stop sppsvc
   cd %windir%\system32\spp\store\2.0
   ren tokens.dat tokens.bar
   net start sppsvc
   cscript.exe %windir%\system32\slmgr.vbs /rilc
   ```
   針對 Windows 8.1、Windows Server 2012 和 Windows Server 2012 R2，請依序輸入下列命令：
   ```cmd
   net stop sppsvc
   cd %windir%\ServiceProfiles\LocalService\AppData\Local\Microsoft\WSLicense
   ren tokens.dat tokens.bar
   net start sppsvc
   cscript.exe %windir%\system32\slmgr.vbs /rilc
   ```
   針對 Windows 7、Windows Server 2008 和 Windows Server 2008 R2，請依序輸入下列命令：
   ```cmd
   net stop sppsvc
   cd %windir%\ServiceProfiles\NetworkService\AppData\Roaming\Microsoft\SoftwareProtectionPlatform
   ren tokens.dat tokens.bar
   net start sppsvc
   cscript.exe %windir%\system32\slmgr.vbs /rilc
   ```
1. 重新啟動電腦。

## <a name="more-information"></a>其他資訊

重建 Tokens.dat 檔案之後，您必須使用下列其中一種方法重新安裝您的產品金鑰：

- 在同一個提升權限的命令提示字元中，輸入下列命令，然後按 Enter 鍵：

   ```cmd
   cscript.exe %windir%\system32\slmgr.vbs /ipk <Product key>
   ```

  > [!IMPORTANT]
  > 請勿使用 **/upk** 參數來解除安裝產品金鑰。 若要使用現有的產品金鑰安裝產品金鑰，請使用 **/ipk** 參數。
- 以滑鼠右鍵按一下 [我的電腦]  ，選取 [屬性]  ，然後選取 [變更產品金鑰]  。

如需 KMS 用戶端安裝金鑰的詳細資訊，請參閱 [KMS 用戶端安裝識別碼](kmsclientkeys.md)。
