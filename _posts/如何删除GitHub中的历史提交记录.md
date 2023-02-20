---
title: 如何删除Git中的历史提交记录
tags:
  - Git
  - GitHub
categories: Git
date: 2022-05-26 01:57:59
---


删除`.git`文件夹可能会导致git存储库中的问题。如果要删除所有提交历史记录，但将代码保持在当前状态，可以按照本文方式安全地执行此操作。

<!--more-->

1. 在仓库目录运行：
    ```git
    git checkout --orphan latest_branch
    ```

    这相当于创建了一个新的分支。
    
2. 添加所有文件：

    ```git
    git add -A
    ```

3. 提交更改：

    ```git
    git commit -am "commit message"
    ```

4. 删除原分支：

    ```git
    git branch -D main
    ```

    > 这里假设要删除的原分支为`main`，具体情况需根据自己的需求来决定。

5. 将当前分支重命名：

    ```git
    git branch -m main
    ```

6. 最后，强制更新存储库：

    ```git
    git push -f origin main
    ```

    
