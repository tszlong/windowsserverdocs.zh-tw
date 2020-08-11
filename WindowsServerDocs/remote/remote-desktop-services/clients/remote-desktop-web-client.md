---
title: 開始使用 Web 用戶端
description: 描述如何登入遠端桌面網頁用戶端。
ms.author: helohr
ms.date: 08/27/2019
ms.topic: article
author: Heidilohr
ms.localizationpriority: medium
ms.openlocfilehash: 6d408c70d75de0e2a14260f951209348e2aea63e
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87961872"
---
# <a name="get-started-with-the-web-client"></a>開始使用 Web 用戶端

遠端桌面網頁用戶端可讓您使用相容的網頁瀏覽器，存取系統管理員發佈給您的組織遠端資源 (應用程式和桌面)。不論身處何地，您都可以與遠端應用程式和桌面互動，就像與本機電腦互動一樣，而不需切換至不同的桌上型個人電腦。 一旦系統管理員設定好遠端資源，您只需要具備網域、使用者名稱、密碼、系統管理員傳送給您的 URL 及支援的網頁瀏覽器，就可以開始使用。

>[!NOTE]
>想知道適用於網頁用戶端的新版本嗎？ 查看[遠端桌面網頁用戶端的新功能](web-client-whatsnew.md)

## <a name="what-youll-need-to-use-the-web-client"></a>使用網頁用戶端時所需的項目

* 若要使用網頁用戶端，您必須具備執行 Windows、macOS、ChromeOS 或 Linux 的電腦。 目前不支援行動裝置。
* 新式瀏覽器，例如 Microsoft Edge、Internet Explorer 11、Google Chrome、Safari 或 Mozilla Firefox (v55.0 和更新版本)。
* 系統管理員傳送給您的 URL。

>[!NOTE]
>目前 Internet Explorer 的網頁用戶端版本並沒有音訊。
>如果您調整瀏覽器大小或進入全螢幕多次，Safari 可能會顯示灰色螢幕。

## <a name="start-using-the-remote-desktop-client"></a>開始使用遠端桌面用戶端

若要登入用戶端，請移至系統管理員傳送給您的 URL。 在登入頁面中，以 ```DOMAIN\username``` 格式輸入您的網域和使用者名稱，並輸入密碼，然後選取 [登入]  。

>[!NOTE]
>登入網頁用戶端，即表示您同意電腦遵循組織的安全性原則。

登入之後，用戶端會帶您前往 [所有資源]  索引標籤，其中包含以一或多個可摺疊群組形式，例如「工作資源」群組來發佈給您的所有項目。 您會看到幾個圖示，其代表應用程式、桌面或資料夾，而資料夾中包含系統管理員提供給工作群組的多個應用程式或桌面。 您可以隨時回到此索引標籤以啟動其他資源。

若要開始使用應用程式或桌面，請選取您要使用的項目，並在系統提示時，輸入您用來登入網頁用戶端的相同使用者名稱和密碼，然後選取 [提交]  。 系統可能也會顯示存取本機資源的同意對話方塊，例如剪貼簿和印表機。 您可以選擇不重新導向這些項目，或選取 [允許]  以使用預設設定。 等候網頁用戶端建立連線，然後即可按照一般方式開始使用資源。

完成時，您可以選取螢幕頂端工具列中的 [登出]  按鈕或關閉瀏覽器視窗，以結束您的工作階段。

## <a name="printing-from-the-remote-desktop-web-client"></a>從遠端桌面網頁用戶端進行列印

請遵循下列步驟，從網頁用戶端進行列印：

1. 針對您想要用來列印的應用程式，按照一般方式開始列印程序。
2. 當系統提示您選擇印表機時，請選取 [Remote Desktop Virtual Printer] \(遠端桌面虛擬印表機\)  。
3. 選擇您的喜好設定之後，選取 [列印]  。
4. 瀏覽器即會產生列印工作的 PDF 檔案。
5. 您可以選擇開啟 PDF 並使用本機印表機列印內容，或將它儲存到電腦以供稍後使用。

## <a name="copy-and-paste-from-the-remote-desktop-web-client"></a>從遠端桌面網頁用戶端複製和貼上

網頁用戶端目前僅支援複製和貼上文字。 您無法在網頁用戶端中複製或貼上檔案。 此外，您只能使用 **Ctrl+C** 和 **Ctrl+V** 複製和貼上文字。

## <a name="use-an-input-method-editor-ime-in-the-remote-session"></a>在遠端工作階段中使用輸入法編輯器 (IME)

若要使用輸入法編輯器在遠端工作階段中輸入複雜字元，選取導覽列中的齒輪圖示以開啟 [設定]  側面板，然後將 [啟用輸入法編輯器]  開始設為 [開啟]  。 您的遠端工作階段必須已安裝並啟用輸入法編輯器 (IME)。

## <a name="get-help-with-the-web-client"></a>取得網頁用戶端的協助

如果遇到本文資訊無法解決的問題，您可以傳送電子郵件至網頁用戶端 [關於] 頁面上的位址，以取得網頁用戶端的協助。
