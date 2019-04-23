---
title: 存取遠端桌面 Web 用戶端
description: 描述如何登入遠端桌面 web 用戶端。
ms.prod: windows-server-threshold
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 07/20/2018
ms.topic: article
author: Heidilohr
ms.localizationpriority: medium
ms.openlocfilehash: f4433ad592219d6ed15b28fd0514790b078525fd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849369"
---
# <a name="access-the-remote-desktop-web-client"></a>存取遠端桌面 Web 用戶端

遠端桌面 web 用戶端可讓您使用相容的網頁瀏覽器存取貴組織的遠端資源 （應用程式和桌上型電腦） 發行至您的系統管理員。您將能夠與遠端應用程式和桌面就像您身處何地，而不需要切換至不同的桌上型個人電腦的本機電腦進行互動。 一旦您的系統管理員設定您的遠端資源，您只需要為您的網域、 使用者名稱、 URL 傳送，而且支援的網頁瀏覽器中，您的系統管理員的密碼，您可以大顯身手。

>[!NOTE]
>想知道 web 用戶端的新版本嗎？ 請參閱[有關遠端桌面 web 用戶端的新功能？](web-client-whatsnew.md)

## <a name="what-youll-need-to-use-the-web-client"></a>您必須使用 web 用戶端

* Web 用戶端中，您必須執行 Windows、 macOS、 ChromeOS 或 Linux 的電腦。 在此階段不支援行動裝置。
* 新式的瀏覽器，例如 Microsoft Edge、 Internet Explorer 11、 Google Chrome、 Safari、 或 Mozilla Firefox (v55.0 和更新版本)。
* URL 您的系統管理員傳送給您。

>[!NOTE]
>在這個階段，Internet Explorer 的 web 用戶端版本並沒有音訊。
>如果瀏覽器會調整大小，或進入全螢幕多次 safari 可能會顯示灰色的螢幕。

## <a name="start-using-the-remote-desktop-client"></a>開始使用遠端桌面用戶端

若要登入用戶端，請移至您的系統管理員傳送給您的 URL。 在登入頁面中，輸入您的網域和使用者名稱格式```DOMAIN\username```，輸入您的密碼，然後選取**登入**。

>[!NOTE]
>登入 web 用戶端，即表示您同意您的電腦，符合貴組織的安全性原則。

登入之後，用戶端會帶您前往**的所有資源**下一或多個可摺疊群組，例如 「 工作資源 」 群組發佈給您的索引標籤上，其中包含所有的項目。 您會看到幾個圖示代表應用程式、 桌面或包含多個應用程式或系統管理員已提供給工作群組的桌面資料夾。 您可以回到此索引標籤上隨時啟動額外的資源。

若要開始使用應用程式或桌面，請選取您想要使用中，輸入相同的使用者名稱和密碼來登入 web 用戶端出現提示時，您使用的項目，然後按**送出**。 您可能也會顯示同意對話方塊，來存取本機資源，例如剪貼簿和印表機。 您可以選擇不重新導向其中之一，或選取**允許**以使用預設設定。 等候建立連線，web 用戶端，然後再啟動 可以用正常方式使用資源。

當您完成時，您可以選取結束您的工作階段**登出**螢幕或關閉瀏覽器視窗的頂端工具列中的按鈕。

## <a name="printing-from-the-remote-desktop-web-client"></a>從遠端桌面 web 用戶端列印

請遵循下列步驟來從 web 用戶端列印：

1. 可以用正常方式為您想要從列印應用程式，請開始列印程序。
2. 出現提示時選擇印表機，請選取**遠端桌面的虛擬印表機**。
3. 選擇您的喜好設定之後, 選取**列印**。
4. 您的瀏覽器將會產生列印工作的 PDF 檔案。
5. 您可以選擇開啟 PDF，並列印其內容至您的本機印表機，或將它儲存到您的電腦，以供稍後使用。

## <a name="copy-and-paste-from-the-remote-desktop-web-client"></a>複製並貼上從遠端桌面 web 用戶端

Web 用戶端目前支援複製並貼上純文字。 無法複製或貼上與 web 用戶端檔案。 此外，您只能使用**Ctrl + C**並**Ctrl + V**複製並貼上文字。

## <a name="get-help-with-the-web-client"></a>取得 web 用戶端的協助

如果您遇到無法在這篇文章中的資訊來解決問題，可以傳送電子郵件給 web 用戶端的 About 頁面上的位址，以取得與 web 用戶端說明。