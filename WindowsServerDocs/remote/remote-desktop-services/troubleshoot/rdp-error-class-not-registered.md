---
title: 用戶端無法連線並收到「類別未登錄」錯誤
description: 針對遠端桌面連線的「類別未登錄」錯誤進行疑難排解。
ms.reviewer: rklemen
ms.topic: troubleshooting
author: kaushika-msft
manager: dcscontentpm
ms.author: delhan
ms.date: 07/24/2019
ms.localizationpriority: medium
ms.openlocfilehash: 52e696bd4229b947ea63a379211192b8664a9f93
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857181"
---
# <a name="clients-cant-connect-and-get-the-class-not-registered-error"></a>用戶端無法連線並收到「類別未登錄」錯誤

當您嘗試使用執行 Windows 10 版本 1709 或更新版本的用戶端來連線到遠端電腦，用戶端可能無法連線，且遠端桌面工作階段主機伺服器會回報包含「類別未登錄 (0x80040154)」錯誤碼的訊息。

如果嘗試連線的使用者具有強制使用者設定檔，就會發生此問題。 若要解決此問題，請安裝 [2018 年 7 月 24 日—KB4338817 (作業系統組建 16299.579)](https://support.microsoft.com/help/4338817/windows-10-update-kb4338817) Windows 10 更新。
