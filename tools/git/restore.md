## git restore

`git restore` 是 Git 2.23 引入的一个命令，用于恢复或重置工作区文件的状态。它是 `git checkout` 的部分替代命令，旨在简化和明确不同操作之间的区分。`git restore` 可以用于以下几种常见操作：

### 1. **恢复工作区中的文件**

- **命令**：`git restore [<options>] <path>`

- **说明**：将工作区中的文件恢复到暂存区或最后一次提交的状态。这意味着它会撤销工作区中的未提交更改。

- **示例**：

  ```
  git restore filename.txt
  ```

  这将 `filename.txt` 恢复到暂存区的状态，丢弃工作区中的修改。

- **恢复所有文件**：

  ```
  git restore .
  ```

  这将恢复当前目录下所有文件的更改，恢复到暂存区或最后一次提交的状态。

### 2. **恢复暂存区中的文件**

- **命令**：`git restore --staged <path>`

- **说明**：将暂存区中的文件恢复到最后一次提交的状态，也就是取消暂存，将其从暂存区移回工作区。以前需要使用 `git reset` 命令来取消暂存。

- **示例**：

  ```
  git restore --staged filename.txt
  ```

  将 `filename.txt` 从暂存区移回工作区，也就是取消暂存。

- **恢复多个文件**：

  ```
  git restore --staged .
  ```

  取消暂存当前目录下所有已暂存的文件。

### 3. **恢复文件到特定提交**

- **命令**：`git restore --source=<commit> <path>`

- **说明**：将工作区中的文件恢复到特定提交的状态。可以指定任意的提交哈希值或分支名称。

- **示例**：

  ```
  git restore --source=HEAD~1 filename.txt
  ```

  将 `filename.txt` 恢复到上一次提交（`HEAD~1`）的状态。

- **恢复到特定分支的状态**：

  ```
  bash
  复制代码
  git restore --source=main filename.txt
  ```

  将 `filename.txt` 恢复到 `main` 分支的最新状态。

### 4. **混合使用选项**

- **命令**：`git restore --source=<commit> --staged <path>`

- **说明**：将文件恢复到特定提交的状态，并将其直接添加到暂存区。此操作可以用来将某个文件的状态从某个特定提交中直接恢复到暂存区。

- 示例：

  ```
  git restore --source=HEAD~2 --staged filename.txt
  ```

  将 filename.txt 恢复到两次提交之前的状态，并将其暂存。

### 5. **不覆盖现有文件**(@rainbow)

- **命令**：`git restore --no-overwrite <path>`

- **说明**：防止覆盖文件。如果在恢复时遇到文件冲突，Git 会跳过该文件，而不是强制覆盖。这在你不希望丢失工作区更改时特别有用。

- 示例：

  ```
  git restore --no-overwrite filename.txt
  ```

  恢复 filename.txt，但如果工作区中有更改且与恢复状态冲突，则不覆盖文件。

### 6. **清除未跟踪文件**

- **命令**：`git restore --source=<commit> --staged --worktree -- path`

- **说明**：用于清理未跟踪文件。此选项组合会恢复暂存区和工作区的文件状态，但不会处理未跟踪的文件。若要删除未跟踪的文件，可以使用 `git clean`。

- 示例：

  ```
  git restore --staged --worktree -- filename.txt
  ```

  将 filename.txt 恢复到与当前提交匹配的状态，并同时更新工作区和暂存区。

### 7. **忽略所有文件**

- **命令**：`git restore --ignore-unmerged <path>`

- **说明**：用于在合并冲突中选择忽略所有文件，意味着将不会恢复那些存在合并冲突的文件。

- 示例：

  ```
  git restore --ignore-unmerged filename.txt
  ```

  如果 

  ```
  filename.txt
  ```

   存在合并冲突，该命令会跳过这个文件的恢复。

### 8. **备选：保留本地修改**

- **命令**：`git restore --ours <path>`

- **说明**：在合并冲突中保留本地分支的修改，而丢弃来自合并的更改。它相当于 `git checkout --ours` 的用法。

- 示例：

  ```
  git restore --ours conflicted-file.txt
  ```

  保留 conflicted-file.txt 中本地分支的修改内容。

### 总结

- **`git restore`** 是一个灵活且强大的命令，用于恢复工作区和暂存区的文件状态。
- **基础恢复**：将工作区文件恢复到暂存区状态 (`git restore <path>`)，或将文件从暂存区移回工作区 (`git restore --staged <path>`）。
- **指定提交**：可以通过 `--source=<commit>` 来指定恢复文件的来源提交。
- **保护更改**：使用 `--no-overwrite` 可防止恢复时覆盖现有工作区更改。
- **解决冲突**：在处理合并冲突时，可以选择保留本地 (`--ours`) 或远程 (`--theirs`) 的更改。

`git restore` 通过其丰富的选项和功能，极大地增强了文件管理和状态恢复的灵活性。





## 清除未跟踪文件 进一步解释

### 清理未跟踪文件的正确方法

未跟踪的文件是那些没有被 Git 记录和管理的文件。它们存在于你的工作目录中，但不在 Git 的版本控制中。为了清理这些文件，你通常会使用 `git clean` 命令，而不是 `git restore`。`git restore` 主要是用于恢复和重置已跟踪的文件状态。

### **`git clean` 命令**

`git clean` 是专门用于清理未跟踪文件和目录的命令。使用这个命令时要非常谨慎，因为它会永久删除未跟踪的文件。

### 1. **查看未跟踪的文件**

- **命令**：`git clean -n`

- **说明**：显示将要被删除的未跟踪文件，但不实际执行删除操作。`-n` 表示 dry-run 模式，仅用于查看效果。

- 示例：

  ```
  git clean -n
  ```

  输出类似如下内容：

  ```
  Would remove untracked_file.txt
  ```

### 2. **删除未跟踪的文件**

- **命令**：`git clean -f`

- **说明**：强制删除未跟踪的文件。`-f` 表示 force，Git 默认情况下不会执行清理操作，除非加上这个选项。

- 示例：

  ```
  git clean -f
  ```

  删除未跟踪的文件。

### 3. **删除未跟踪的目录**

- **命令**：`git clean -fd`

- **说明**：删除未跟踪的目录。`-d` 表示递归删除未跟踪的目录和其内部内容。

- 示例：

  ```
  git clean -fd
  ```

  删除未跟踪的目录和文件。

### 4. **删除已忽略的文件**

- **命令**：`git clean -fX`

- **说明**：删除被 `.gitignore` 文件忽略的未跟踪文件。`-X` 表示仅删除已忽略的文件。

- 示例：

  ```
  git clean -fX
  ```

  删除所有被 .gitignore 忽略的文件。

### 5. **删除所有未跟踪文件和已忽略文件**

- **命令**：`git clean -fx`

- **说明**：删除所有未跟踪文件，包括已忽略和未被忽略的文件。`-x` 表示删除所有未跟踪的文件，无论是否被忽略。

- 示例：

  ```
  git clean -fx
  ```

  删除所有未跟踪的文件和目录。

### 总结

- **`git clean`** 是用于清理未跟踪文件的命令，`git restore` 并不处理未跟踪文件。
- 你可以使用 **`git clean -n`** 先查看哪些文件会被删除，然后使用 **`git clean -f`** 或其他选项来执行清理操作。
- 要小心使用 **`git clean -fdX`** 等选项，因为它们会永久删除文件，这些操作是不可逆的。

通过 `git clean`，你可以高效地管理和清理项目中的未跟踪文件和目录，使工作目录更加整洁。





## 对于新建的文件或者目录，git restore 无法清除

是的，`git restore` 无法清除新建的未跟踪文件或目录。

### 解释：

- **`git restore`**：该命令主要用于恢复或重置已跟踪文件的状态，帮助你将工作区或暂存区的已跟踪文件恢复到特定提交的状态。它不处理未跟踪的文件或目录。
- **未跟踪文件/目录**：这些是新建的、但没有被 `git add` 命令添加到暂存区的文件或目录。因为它们没有被 Git 版本控制，所以 `git restore` 对它们不起作用。

### 处理未跟踪文件/目录的正确方法：

- **`git clean`**：如前所述，`git clean` 是专门用于清理这些未跟踪的文件或目录的命令。通过 `git clean -f` 可以删除未跟踪的文件，通过 `git clean -fd` 可以删除未跟踪的文件和目录。

### 总结：

- **`git restore`** 适用于恢复已跟踪文件的状态，但不能清除未跟踪的文件或目录。
- 如果你需要清除新建的未跟踪文件或目录，应该使用 **`git clean`** 命令。

这样可以确保你的工作区保持干净，只保留那些已经被 Git 版本控制管理的文件。