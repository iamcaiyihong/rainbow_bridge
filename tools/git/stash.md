`git stash` 是一个非常实用的 Git 命令，用于临时保存工作区的修改（包括已跟踪和未跟踪的文件），以便清空工作区进行其他操作。当你完成其他操作后，可以将这些修改恢复回来继续工作。以下是 `git stash` 的详细讲解，包括常用的子命令和场景示例。

### 1. **基本用法**

- **命令**：`git stash`

- **说明**：将当前工作区的所有未提交修改保存到栈中，工作区恢复为干净状态（与最后一次提交一致）。

- 示例：

  ```
  git stash
  ```

  保存当前工作区的所有修改，并清空工作区。

### 2. **查看 stash 列表**

- **命令**：`git stash list`

- **说明**：列出所有已保存的 stash 记录，显示每个 stash 的索引和相关的提交信息。

- 示例：

  ```
  git stash list
  ```

  输出类似如下的 stash 记录：

  ```less
  stash@{0}: WIP on main: 9fceb02 Add feature
  stash@{1}: WIP on main: 8c5e0b8 Fix bug
  ```

### 3. **恢复（应用）stash**

- **命令**：`git stash apply`

- **说明**：将最近的 stash（默认 `stash@{0}`）恢复到工作区，但不会从 stash 列表中删除该记录。

- **示例**：

  ```bash
  git stash apply
  ```

  恢复最近保存的 stash。

- **恢复指定 stash**：

  ```bash
  git stash apply stash@{1}
  ```

  恢复 `stash@{1}` 对应的保存状态。

### 4. **恢复并删除 stash**

- **命令**：`git stash pop`

- **说明**：恢复最近的 stash 并将其从 stash 列表中删除。与 `git stash apply` 的区别是 `pop` 会移除已应用的 stash。

- **示例**：

  ```
  git stash pop
  ```

  恢复并删除最近的 stash。

- **恢复并删除指定 stash**：

  ```
  git stash pop stash@{1}
  ```

  恢复并删除 `stash@{1}`。

### 5. **保存特定的文件或路径**

- **命令**：`git stash push`

- **说明**：`git stash push` 是 `git stash` 的较新版本，允许你指定只保存某些文件或路径的更改。

- **示例**：

  ```
  git stash push -m "Save my changes" -- filename.txt
  ```

  仅保存 `filename.txt` 的更改，并附加描述信息。

- **保存未跟踪文件**：

  ```
  git stash push -u
  ```

  将未跟踪文件（Untracked files）也一起保存到 stash 中。

### 6. **删除 stash**

- **命令**：`git stash drop`

- **说明**：删除指定的 stash 记录。如果不指定，默认删除最近的 stash。

- **示例**：

  ```
  git stash drop stash@{0}
  ```

  删除 `stash@{0}` 对应的 stash 记录。

- **删除最近的 stash**：

  ```
  git stash drop
  ```

  删除最近的 stash 记录。

### 7. **清除所有 stash**

- **命令**：`git stash clear`

- **说明**：清空所有的 stash 记录。这个命令不可逆，应谨慎使用。

- **示例：**

  ```
  git stash clear
  ```

  清除所有 stash 记录。

### 8. **查看 stash 内容**

- **命令**：`git stash show`

- **说明**：显示指定 stash 的修改内容摘要。

- **示例**：

  ```
  git stash show
  ```

  显示最近 stash 的修改内容摘要。

- **查看详细的差异**：

  ```
  git stash show -p
  ```

  显示详细的差异（diff）。

### 9. **创建分支并恢复 stash**

- **命令**：`git stash branch`

- **说明**：基于 stash 创建一个新分支，并将该 stash 恢复到新分支中。该命令适用于当你希望在新分支上继续处理保存的更改时。

- **示例：**

  ```
  git stash branch my-feature-branch
  ```

  创建一个名为 my-feature-branch 的新分支，并恢复最近的 stash。

### 10. **与工作区冲突**

- **场景**：当你恢复 stash 时，可能会出现工作区文件与 stash 内容冲突的情况。
- **处理**：你需要手动解决冲突，完成后执行 `git add` 并继续你的工作。`git stash` 只是帮助你暂时保存工作，并不会自动解决冲突。

### 11. **保存带有信息的 stash**

- **命令**：`git stash save "message"`

- **说明**：为 stash 添加一条信息，以便更好地描述这次保存的内容。虽然较老版本的 Git 中使用 `git stash save`，现在推荐使用 `git stash push -m "message"` 。

- **示例：**

  ```
  git stash push -m "Working on feature X"
  ```

  保存工作区修改，并附加说明信息 "Working on feature X"。

### 总结

- **`git stash`**：将当前工作区的修改保存并清空工作区。
- **`git stash list`**：查看所有 stash 记录。
- **`git stash apply`**：恢复指定的 stash，但不删除记录。
- **`git stash pop`**：恢复并删除指定的 stash。
- **`git stash drop`**：删除指定的 stash。
- **`git stash clear`**：清空所有 stash 记录。
- **`git stash branch`**：基于 stash 创建新分支并恢复该 stash。

`git stash` 是处理临时性需求的强大工具，能够让你在处理其他任务时不丢失当前工作进度，并且可以灵活地恢复或管理这些暂存的更改。