---
title: MAK 啟用的已知問題
description: 說明在 MAK 啟用過程中可能發生的常見問題，並提供解決方式和指引
ms.topic: article
ms.date: 10/3/2019
ms.technology: server-general
ms.assetid: ''
author: Teresa-Motiv
ms.author: v-tea
manager: dcscontentpm
ms.localizationpriority: medium
ms.openlocfilehash: 25fa6d32571597136580afe062aa8d9c7e2d82b7
ms.sourcegitcommit: 9855d6b59b1f8722f39ae74ad373ce1530da0ccf
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/04/2019
ms.locfileid: "71962984"
---
# <a name="mak-activation-known-issues"></a>MAK 啟用：已知問題

本文說明在多次啟用金鑰 (MAK) 啟用期間可能發生的常見問題，並提供解決這些問題的指引。

## <a name="how-can-i-tell-whether-my-computer-is-activated"></a>如何分辨我的電腦是否已啟用？

在電腦上，開啟 [系統]  控制台，然後尋找 [Windows 已啟用]  。 或者，執行 Slmgr.vbs 並使用 **/dli** 命令列選項。

## <a name="the-computer-does-not-activate-over-the-internet"></a>電腦並未透過網際網路啟用

確定所需的連接埠已在防火牆中開啟。 如需連接埠清單，請參閱[大量啟用部署指南](http://go.microsoft.com/fwlink/?linkid=150083)。

## <a name="internet-and-telephone-activation-fail"></a>網際網路和電話啟用失敗

連絡當地的 Microsoft 啟用中心。 如需全球 Microsoft 啟用中心的電話號碼，請移至[全球 Microsoft 授權啟用中心電話號碼](https://www.microsoft.com/Licensing/existing-customer/activation-centers)。 當您呼叫時，請務必提供大量授權合約資訊和購買證明。

## <a name="slmgrvbs-ato-returns-an-error-code"></a>Slmgr.vbs /ato 會傳回錯誤碼

如果 Slmgr.vbs 傳回十六進位錯誤碼，請執行下列指令碼來判斷對應的錯誤訊息：

```cmd
slui.exe 0x2a 0x <ErrorCode>
```

如需特定錯誤碼和如何予以解決的詳細資訊，請參閱[解決常見啟用錯誤碼](activation-error-codes.md)。
