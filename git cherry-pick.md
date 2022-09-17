# cherry-pick

## cherry-pick 使用

### 🍒 什么是 `git cherry-pick`？

`git cherry-pick` 是一个 Git 命令，用于将**某个特定的提交（commit）** 从一个分支“摘取”并应用到当前分支上。它常用于：

- 把某个修复（hotfix）从开发分支应用到主分支；
- 选择性地合并某些提交，而不是整个分支。

### ✅ 基本用法

```bash
git cherry-pick <commit-hash>
```

例如：

```bash
git cherry-pick a1b2c3d
```

这会把提交 `a1b2c3d` 的更改应用到你当前所在的分支。

### 🔧 常用选项

| 选项 | 说明 |
|------|------|
| `-n` 或 `--no-commit` | 应用更改但不自动提交（方便修改后再提交） |
| `-e` 或 `--edit` | 在提交前编辑提交信息 |
| `-x` | 在提交信息末尾自动添加 `(cherry picked from commit ...)`，便于追踪来源 |
| `<commit1>..<commit2>` | 挑选一个范围内的多个提交（注意：**不包含** `<commit1>`） |
| `--continue` / `--abort` | 解决冲突后继续，或中止 cherry-pick |

### ⚠️ 冲突处理

如果 cherry-pick 过程中发生冲突：

1. Git 会暂停操作并提示冲突文件；
2. 手动解决冲突；
3. 使用 `git add <file>` 标记已解决；
4. 执行 `git cherry-pick --continue` 继续；
5. 如果想放弃，执行 `git cherry-pick --abort`。

### 📌 示例场景

假设你在 `main` 分支，想把 `feature` 分支中的某次关键修复（比如提交 `abc1234`）拿过来：

```bash
git checkout main
git cherry-pick abc1234
```

## cherry-pick和rebase有什么区别？

`git cherry-pick` 和 `git rebase` 都是 Git 中用于**重放提交（replay commits）**的工具，但它们的**目的、作用范围和使用场景**有明显区别。下面从多个维度详细对比：

## 🍒 1. **基本概念**

| 命令 | 作用 |
|------|------|
| `cherry-pick` | **选择性地复制某个（或某些）提交**，应用到当前分支。 |
| `rebase` | **将整个分支的提交“重新播放”到另一个分支的顶端**，重写历史。 |

## 🔁 2. **操作粒度**

- **`cherry-pick`**：  
  - **按提交（commit）为单位**操作。  
  - 可以只选一个或几个特定的 commit，不管它们来自哪个分支。

- **`rebase`**：  
  - **按分支为单位**操作。  
  - 默认会把当前分支上**自分叉点以来的所有提交**依次重放到目标分支上。

> 💡 你可以把 `rebase` 看作是“批量 cherry-pick”，但它有更严格的线性历史要求。

## 🌲 3. **对提交历史的影响**

| 操作 | 是否创建新提交 | 是否保留原提交哈希 | 是否改变历史 |
|------|----------------|--------------------|--------------|
| `cherry-pick` | ✅ 是（新 commit hash） | ❌ 否 | ✅ 改变（新增） |
| `rebase` | ✅ 是（所有被 rebase 的提交都会生成新 hash） | ❌ 否 | ✅ 改变（重写） |

> ⚠️ 两者都会**产生新的提交对象**（因为父提交变了，hash 必然不同），所以都属于“重写历史”的操作，**不要在已推送的公共分支上随意使用**。

## 🧩 4. **典型使用场景**

### ✅ `cherry-pick` 适合：
- 把某个 bugfix 提交从 `develop` 分支“摘”到 `main` 或 `release` 分支；
- 只需要某一次关键提交，不需要整个分支的变更；
- 临时修复、紧急补丁等**选择性合并**。

```bash
git checkout main
git cherry-pick a1b2c3d   # 只拿这个 commit
```

### ✅ `rebase` 适合：
- 在开发功能时，**同步上游最新代码**（如 `main` 分支的更新）到你的 feature 分支，保持历史整洁；
- 整理本地提交（配合 `git rebase -i` 交互式 rebase）；
- 让分支历史呈**线性结构**，避免 merge commit。

```bash
git checkout feature
git rebase main   # 把 feature 分支的提交“挪”到 main 最新提交之后
```

## 🔄 5. **与 merge 的关系**

- `cherry-pick` 和 `rebase` 都**不会创建 merge commit**，而是直接应用更改。
- `merge` 会保留分支拓扑结构，而 `rebase` 会“抹平”分支结构，变成一条直线。

## 🛠️ 6. **冲突处理**

两者在遇到冲突时行为类似：
1. 暂停操作；
2. 手动解决冲突；
3. `git add` 标记解决；
4. 继续：  
   - `cherry-pick` → `git cherry-pick --continue`  
   - `rebase` → `git rebase --continue`
5. 也可中止：`--abort`

## 📌 总结一句话

> **`cherry-pick` 是“摘樱桃”——只拿你需要的提交；  
> `rebase` 是“搬家”——把一整段提交搬到另一个地方重放。**
