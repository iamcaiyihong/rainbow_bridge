## 基本命令

Git 是一个分布式版本控制系统，用于跟踪文件的更改并管理项目代码。以下是一些常用的 Git 命令及其示例说明：

### 1. **git init**

- **说明**：初始化一个新的 Git 仓库。

- **示例：**

  ```bash
  git init
  ```

  在当前目录下初始化一个新的 Git 仓库。

### 2. **git checkout**

- **说明**：切换到指定的分支或恢复工作目录文件。

- **示例**：

  ```
  git checkout main
  ```

  切换到 `main` 分支。

  ```
  git checkout -b feature-branch
  ```

  创建并切换到 `feature-branch` 分支。

### 3. **git merge**

~~~bash
- **说明**：将另一个分支的更改合并到当前分支。
- **示例**：
  ```bash
  git merge feature-branch
  ```
  将 `feature-branch` 的更改合并到当前分支。
~~~

### 4. **git log**

~~~bash
- **说明**：查看提交历史记录。
- **示例**：
  ```bash
  git log
  ```
  查看当前分支的提交历史。

  ```bash
  git log --oneline --graph --all
  ```
  查看所有分支的提交历史，以简洁的图形格式显示。
~~~

### 5. **git reset**

~~~bash
- **说明**：重置当前分支的状态。
- **示例**：
  ```bash
  git reset --soft HEAD~1
  ```
  回退上一个提交，但保留文件的更改。

  ```bash
  git reset --hard HEAD~1
  ```
  回退上一个提交，并丢弃所有未提交的更改。
~~~

### 6. **git stash**

~~~bash
- **说明**：暂存当前未提交的更改，以便切换分支或执行其他操作。
- **示例**：
  ```bash
  git stash
  ```
  将未提交的更改暂存起来。

  ```bash
  git stash pop
  ```
  恢复上次的暂存更改。
~~~

### 7. **git remote**

~~~bash
- **说明**：管理远程仓库。
- **示例**：
  ```bash
  git remote -v
  ```
  查看所有配置的远程仓库。

  ```bash
  git remote add origin https://github.com/user/repository.git
  ```
  添加名为 `origin` 的远程仓库。
~~~

### 8. **git tag**

~~~bash
- **说明**：给某个提交打标签，通常用于标记版本发布。
- **示例**：
  ```bash
  git tag v1.0.0
  ```
  给当前提交打上 `v1.0.0` 标签。

  ```bash
  git push origin v1.0.0
  ```
  将标签推送到远程仓库。
~~~

这些命令涵盖了Git的基础操作，可以帮助你管理代码版本和协作开发。



## 合并相关

除了 `git merge` 之外，Git 还有其他命令可以用于合并分支和更改，常见的包括 `git rebase` 和 `git cherry-pick`。

### 1. **git rebase**

- **说明**：`git rebase` 是一种将一个分支的更改应用到另一个分支上的方法。它通过将当前分支的更改重新应用到目标分支的基础上，来保持提交历史的整洁。

- **使用场景**：适用于需要保持线性历史的场景，即不想看到合并提交（merge commit）时。

- **示例**：

  ```bash
  git checkout feature-branch
  git rebase main
  ```

  将 `feature-branch` 的更改重新应用在 `main` 分支的最新提交之上。

- **交互式 rebase**：

  ```
  git rebase -i HEAD~3
  ```

  进行交互式 rebase，可以编辑、压缩、删除或重新排序提交。

### 2. **git cherry-pick**

- **说明**：`git cherry-pick` 用于选择性地将一个或多个特定的提交从一个分支应用到当前分支，而不需要合并整个分支的更改。

- **使用场景**：当你只想从另一个分支中挑选特定的提交而不想合并整个分支时使用。

- **示例**：

  ```bash
  git checkout main
  git cherry-pick abc1234
  ```

  将 `abc1234` 提交（通常是提交的哈希值）应用到当前的 `main` 分支。

- **多个提交**：

  ```
  git cherry-pick abc1234 def5678
  ```

  将 `abc1234` 和 `def5678` 两个提交应用到当前分支。

### 3. **git merge --squash**

- **说明**：`git merge --squash` 是一种合并方式，它会将分支上的所有更改压缩为一个提交，这样可以避免生成多个历史提交。

- **使用场景**：当你希望合并某分支但不保留该分支的提交历史，仅保留所有更改合并后的结果时使用。

- **示例：**

  ```bash
  git checkout main
  git merge --squash feature-branch
  git commit -m "Merged feature-branch as a single commit"
  ```

  将 feature-branch 的所有更改合并到 main，并压缩为一个提交。

### 4. **git pull --rebase

- **说明**：`git pull --rebase` 是在拉取远程分支最新更改时，将这些更改应用在本地未提交的更改之下。与 `git pull` 不同的是，它会先将本地的更改临时移开，应用远程的更改，然后再把本地的更改重新应用，这样可以避免生成合并提交。

- **使用场景**：适用于希望保持提交历史线性、避免不必要的合并提交的场景。

- **示例**：

  ```
  git pull --rebase origin main
  ```

  从  origin 仓库的 main  分支拉取更改并重新应用本地更改。

### 总结

- **`git merge`**：合并两个分支，保留所有提交历史。
- **`git rebase`**：将一个分支的更改重新应用在另一个分支的基础上，适用于保持线性历史。
- **`git cherry-pick`**：选择性地将特定提交应用到当前分支。
- **`git merge --squash`**：将整个分支的更改合并为单个提交。
- **`git pull --rebase`**：拉取远程分支时，将更改重新应用在本地更改之下。

每个命令都有其特定的使用场景和目的，可以根据项目需求和团队开发流程来选择合适的合并方式。



## 工作区(worktree)、暂存区(stage)管理相关的命令

在 Git 中，工作区（Worktree）和暂存区（Stage，也称为索引区）是管理代码和变更的重要概念。以下是一些与工作区和暂存区管理相关的常用命令及其功能说明：

### 1. **git reset**

- **说明**：用于将暂存区的更改撤回到工作区或撤销提交。具体用法取决于选项：

  - `--soft`：仅回退提交，保留暂存区和工作区的更改。
  - `--mixed`（默认）：回退提交，同时将暂存区的更改撤回到工作区。
  - `--hard`：彻底回退到指定提交点，丢弃工作区和暂存区的所有更改。

- **示例**：

  ```
  git reset HEAD filename.txt
  ```

  将 `filename.txt` 从暂存区移回到工作区（即取消暂存）。

  ```
  git reset --soft HEAD~1
  ```

  回退上一个提交，但保留文件的更改在暂存区。

  ```
  git reset --hard HEAD~1
  ```

  回退到上一个提交，并丢弃所有未提交的更改。

### 2. **git diff**

- **说明**：显示工作区和暂存区之间的差异，或暂存区与最后一次提交之间的差异。可以帮助你查看具体的更改内容。

- **示例**：

  ```
  git diff
  ```

  显示工作区和暂存区之间的差异。

  

  **``git diff --staged ``**

  显示暂存区与最后一次提交之间的差异。

### 3. **git rm**

- **说明**：从工作区和暂存区中删除文件。删除的文件将在下一次提交时被移除。

- **示例**：

  ```
  git rm filename.txt
  ```

  删除 `filename.txt`，并将此删除操作添加到暂存区。

  ```
  git rm --cached filename.txt
  ```

  将 `filename.txt` 从暂存区中移除，但保留在工作区中（即不再跟踪该文件）。

### 4. **git mv**

- **说明**：在 Git 中移动或重命名文件，同时更新暂存区。

- 示例：

  ```
  git mv oldname.txt newname.txt
  ```

  将 oldname.txt 重命名为 newname.txt，并将此更改添加到暂存区。

### 5. **git restore** 

- **说明**：还原工作区的文件或撤销暂存区的更改。

- **示例**：

  ```
  git restore filename.txt
  ```

  将 `filename.txt` 从工作区还原到与暂存区一致的状态，丢弃在工作区中的修改。

  ```
  git restore --staged filename.txt
  ```

  取消 `filename.txt` 在暂存区的更改，将文件移回工作区。

### 6. **git clean**

- **说明**：清理工作区中的未跟踪文件和目录。

- **示例**：

  ```
  git clean -f
  ```

  强制删除工作区中所有未跟踪的文件。

  ```
  git clean -fd
  ```

  强制删除工作区中所有未跟踪的文件和目录。

### 7. git stash

- **说明**：将工作区中的未提交更改暂时保存起来，以便清空工作区进行其他操作，稍后可以恢复这些更改。

- **示例**：

  ```
  git stash
  ```

  将当前的工作区更改（包括已跟踪和未跟踪的文件）保存到栈中，并将工作区恢复到干净状态。

  ```
  git stash apply
  ```

  恢复最近一次 `git stash` 保存的更改。

### 总结

- **工作区**：`git status`、`git diff`、`git restore`、`git stash`、`git clean`
- **暂存区**：`git add`、`git reset`、`git diff --staged`、`git rm`、`git mv`

这些命令是管理工作区和暂存区的基础工具，可以帮助你有效地管理代码的变更。



## 重要命令扩展

**git stash** 

**git restore**







## 待实践

**git pull --rebase**

**git merge --squash**

**git cherry-pick**

**git diff --staged** 