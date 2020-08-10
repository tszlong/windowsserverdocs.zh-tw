---
title: 概述適用於 Windows 的 OpenSSH
description: 概述由 Linux 系統管理員和其他非 Windows 的系統管理員用於跨平台遠端系統管理的 OpenSSH 工具。
ms.date: 01/07/2019
contributor: damaerteMSFT
author: maertendmsft
ms.type: conceptual
ms.openlocfilehash: 1c0686d7bbe6e78baf924919c6733b733fe65f64
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87879497"
---
# <a name="openssh-in-windows"></a>Windows 的 OpenSSH

OpenSSH 是由 Linux 和其他非 Windows 系統管理員使用的安全殼層 (SSH) 工具的開放原始碼版本，用於跨平台管理遠端系統。
2018 年秋天時，OpenSSH 已新增到 Windows，並包含在 Windows 10 和 Windows Server 2019 中。

SSH 是以用戶端伺服器架構為基礎，其中使用者所使用的系統是用戶端，而受管理的遠端系統則是伺服器。
OpenSSH 包含一系列的元件和工具，其設計目的是為了提供安全且直接的遠端系統管理方法，包括：

* sshd.exe，這是在被遠端管理的系統上必須執行的 SSH 伺服器元件
* ssh.exe，這是在使用者的本機系統上執行的 SSH 用戶端元件
* ssh-keygen.exe 可產生、管理及轉換 SSH 的驗證金鑰
* ssh-agent.exe 會儲存公開金鑰驗證所使用的私密金鑰
* ssh-add.exe 會將私密金鑰新增至伺服器允許的清單
* ssh-keyscan.exe 協助從許多主機收集公用 SSH 主機金鑰
* sftp.exe 是提供安全檔案傳輸通訊協定的服務，並透過 SSH 執行
* scp.exe 是在 SSH 上執行的檔案複製公用程式

本節的文件著重於如何在 Windows 上使用 OpenSSH (包括安裝)，以及 Windows 特定的設定和使用案例。 主題如下：

其他有關一般 OpenSSH 功能的詳細文件可以從 [OpenSSH.com](https://www.openssh.com/manual.html) 線上取得。

主要 [OpenSSH 開放原始碼專案](https://www.openssh.com) 是由開發人員在 OpenBSD 專案中管理。
此專案的 Microsoft 分支是 [GitHub](https://github.com/PowerShell/openssh-portable)。
我們很歡迎您對 Windows OpenSSH 的意見反應，您可以在我們的 [OpenSSH GitHub 存放庫](https://github.com/PowerShell/openssh-portable)中建立 GitHub 問題來提出意見。
