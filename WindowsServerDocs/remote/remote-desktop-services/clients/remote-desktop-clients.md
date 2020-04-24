---
title: 遠端桌面用戶端
description: 了解適用於您所有裝置的不同遠端桌面用戶端
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.topic: article
ms.assetid: b7d8158c-aee1-4c60-8a46-40ce5595b8e8
author: HeidiLohr
manager: lizross
ms.author: helohr
ms.date: 01/07/2020
ms.localizationpriority: medium
ms.openlocfilehash: 711fa87fe697ae616d9bd8c696d29fabf1586055
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/23/2020
ms.locfileid: "80856041"
---
# <a name="remote-desktop-clients"></a>遠端桌面用戶端

>適用於：Windows 10、Windows 8.1、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2

您可以使用 Microsoft 遠端桌面用戶端，從幾乎任何地方透過大部分任何裝置來連線至遠端電腦及工作資源。 您可以連線到您的工作電腦並且存取所有應用程式、檔案和網路資源，就像您是坐在您的辦公桌前面一樣。 您可以讓應用程式在工作時保持開啟，然後在家查看相同的應用程式 - 只需要使用 RD 用戶端。

在您開始前，請務必先查看[支援的設定](remote-desktop-supported-config.md)一文，其中討論您可以使用遠端桌面用戶端連線到的電腦。 此外，另請查看[用戶端常見問題集](remote-desktop-client-faq.md)。

可用的用戶端應用程式如下：

| 裝置          | 取得應用程式                                                                                                  | 設定指示                                                                |
|-----------------|-----------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------|
| Windows 桌面 | [Windows 桌面用戶端](windowsdesktop.md#install-the-client)                                               | [開始使用 Windows 桌面用戶端](windowsdesktop.md) |
| Windows 市集   | [Microsoft Store 中的 Windows 10 用戶端](https://go.microsoft.com/fwlink/?LinkID=616709)                   | [ Windows Store 用戶端](windows.md)          |
| Android         | [Google Play 中的 Android 用戶端](https://play.google.com/store/apps/details?id=com.microsoft.rdc.android)     | [開始使用 Android 用戶端](remote-desktop-android.md) |
| iOS             | [iTunes Store 中的 iOS 用戶端](https://itunes.apple.com/app/microsoft-remote-desktop/id714464092?mt=8)     | [開始使用 iOS 用戶端](remote-desktop-ios.md)         |
| macOS           | [iTunes Store 中的 macOS 用戶端](https://itunes.apple.com/app/microsoft-remote-desktop/id1295203466?mt=12) | [開始使用 macOS 用戶端](remote-desktop-mac.md)       |

## <a name="configuring-the-remote-pc"></a>設定遠端電腦

若要先設定您的遠端電腦，再從遠端存取它，請[允許存取您的電腦](remote-desktop-allow-access.md)。

## <a name="remote-desktop-client-uri-scheme"></a>遠端桌面用戶端 URI 配置

您可以啟用統一資源識別項 (URI) 配置，跨平台整合遠端桌面用戶端的功能。 請查看您可搭配 iOS、Mac 及 Android 用戶端使用的[所支援 URI 屬性](remote-desktop-uri.md)。
