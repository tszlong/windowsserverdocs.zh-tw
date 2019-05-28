---
title: 常用的 Git Bash 命令，以搭配 GitHub
description: 一些 Git bash 使用 GitHub 時最常使用的命令清單。
author: eross-msft
ms.author: lizross
ms.date: 05/06/2019
ms.openlocfilehash: 210acaf2b7911892bcfd81b6bbe1975f141308a1
ms.sourcegitcommit: 7e54a1bcd31cd2c6b18fd1f21b03f5cfb6165bf3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/09/2019
ms.locfileid: "65461693"
---
# <a name="common-git-bash-commands"></a>常用的 Git Bash 命令

這些是一些最常用的命令在 Git Bash 中，根據您將用於您的內容建立和編輯處理程序。

## <a name="master-branch-related"></a>主要分支相關

您一律必須使用作為基礎的主要任何新的分支。

| 命令 | 描述 |
|---------|-------------|
| `git checkout master` | 從任何其他分支切換為主要 |
| `git pull upstream master` | 更新 master 從實際執行存放庫本機複本 |

## <a name="branch-related"></a>相關的分支

| 命令 | 描述 |
|---------|-------------|
| `git branch` | 查看您現有的分支 |
| `git checkout -B <name-of-branch>` | 建立新的分支 |
| `git checkout <name-of-branch>` | 將變更為另一個分支 |
| `git status` | 檢查您的分支中運作 |
| `git branch -D <name-of-branch>` | 刪除現有的分支 （確定您已不存在） |

## <a name="check-in-related-done-as-a-group-in-order"></a>檢查在-相關 （為群組，在訂單完成）

| 命令 | 描述 |
|---------|-------------|
| `git add --all` | 儲存您的工作之後，會將它新增至分支 |
| `git commit -m “public comment, including quotes”` | 將變更認可到您的分支 |
| `git pull upstream master` | 更新 master 從實際執行存放庫本機複本 |
| `git push origin <name-of-branch>` | 推送至您的本機分支的遠端版本 |