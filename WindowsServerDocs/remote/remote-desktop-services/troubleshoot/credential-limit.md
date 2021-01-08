---
title: 每個應用程式的認證限制
description: 針對每個應用程式只能使用二十個認證的問題進行疑難排解。
ms.topic: troubleshooting
author: HeidiLohr
manager: lizross
ms.author: helohr
ms.date: 12/14/2020
ms.localizationpriority: medium
ms.openlocfilehash: 5ce9b606e9205758b819dfe89cadbd0187357243
ms.sourcegitcommit: 4f7308430a69fe7965e16aa5b31f87c5d68e4a09
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/16/2020
ms.locfileid: "97578263"
---
# <a name="credential-limit-per-app"></a>每個應用程式的認證限制

Windows 每個應用程式最多只允許 20 個認證。 如果每個應用程式需要超過 20 個認證，請遵循這篇文章中的指示。

## <a name="bypass-the-20-credential-limit"></a>略過 20 個認證的限制

若要略過 20 個認證的限制：

1. 以系統管理員身分啟動 PowerShell。
2. 將 **HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Vault\MaxPerAppCredentialNumber** DWORD 登錄項目值設定為大於 20 的數字。
3. 重新啟動您的電腦。
4. 在遠端桌面用戶端中建立一組新的認證，以測試您的設定。

## <a name="potential-risks"></a>潛在風險

變更此登錄設定時，請務必記住下列事項：

- 這是系統管理作業。 在登錄中發生的任何錯誤都可能會使電腦變得不穩定。 使用者應自行承擔變更登錄項目的風險。
- 此登錄變更會影響您電腦上的所有應用程式。
