# DuplicateRemover

[中文](#中文) | [English](#english)

---

<a name="中文"></a>
# DuplicateRemover

`DuplicateRemover` 是一个为 [FreshRSS](https://github.com/FreshRSS/FreshRSS) 开发的插件，它能够自动检测并标记跨订阅源的重复文章。支持基于标题或标题+链接的去重策略。

## 功能特点

- ✅ **跨订阅源去重**：自动检测不同 RSS 源中的重复文章
- ✅ **灵活的去重策略**：支持基于标题或标题+链接的去重
- ✅ **自动标记为已读**：发现重复时，将新文章自动标记为已读
- ✅ **保留最早文章**：保留最早的文章为未读状态
- ✅ **可配置**：支持通过 Web 界面配置去重模式和日志
- ✅ **与内置功能互补**：与 FreshRSS 内置的 `same_title_in_feed` 功能互补

## 安装方法

1. 下载 `DuplicateRemover` 插件。
2. 将 `DuplicateRemover` 文件夹（位于仓库的 `DuplicateRemover/` 子目录）放置在您的 FreshRSS 实例的 `./extensions` 目录下。
3. 登录到您的 FreshRSS 实例。
4. 进入管理面板，然后导航到"扩展"部分。
5. 在插件列表中找到 `DuplicateRemover`，点击"启用"。

### 从 GitHub 克隆安装

```bash
# 方法一：克隆到临时目录，然后复制子目录
cd /tmp
git clone https://github.com/wgding/DuplicateRemover.git
cp -r DuplicateRemover/DuplicateRemover /path/to/FreshRSS/extensions/

# 方法二：直接克隆到 extensions 目录
cd /path/to/FreshRSS/extensions
git clone https://github.com/wgding/DuplicateRemover.git
# 然后将 DuplicateRemover/DuplicateRemover 目录移动到 extensions 目录
mv DuplicateRemover/DuplicateRemover ./DuplicateRemover-temp
rm -rf DuplicateRemover
mv DuplicateRemover-temp DuplicateRemover
```

**重要提示**：需要将仓库中的 `DuplicateRemover/` 子目录（包含 `extension.php`、`metadata.json` 等文件）复制到 `FreshRSS/extensions/` 目录。

## 使用方法

安装并启用插件后，进入插件的配置页面进行相关设置。在这里，您可以：

### 去重模式

- **仅基于标题**：如果标题相同，则视为重复
  - ✅ 优点：去重范围更广
  - ⚠️ 缺点：可能误判（不同文章可能有相同标题）

- **基于标题+链接（推荐）**：标题和链接都相同才视为重复
  - ✅ 优点：更准确，减少误判
  - ⚠️ 缺点：去重范围相对较小

### 日志记录

启用后会在 PHP 错误日志中记录去重操作，用于调试。

## 工作原理

1. 当新文章被导入时，扩展会在 `entry_before_insert` 钩子中检查
2. 查询数据库中是否已存在相同标题（或标题+链接）的文章
3. 如果存在，将新文章标记为已读
4. 保留最早的文章为未读状态

## 与 FreshRSS 内置功能的区别

| 功能 | FreshRSS 内置 `same_title_in_feed` | DuplicateRemover 扩展 |
|------|-----------------------------------|----------------------|
| 去重范围 | 单个订阅源内 | 跨所有订阅源 |
| 去重策略 | 仅标题 | 标题 或 标题+链接 |
| 配置方式 | 全局设置 | 扩展配置界面 |

**两者可以同时使用，互补不足。**

## 系统要求

- FreshRSS 1.18.0 或更高版本
- PHP 7.4 或更高版本
- PostgreSQL 或 MySQL/MariaDB 数据库

## 注意事项

1. **性能影响**：每次导入文章时都会查询数据库，如果订阅源很多，可能影响性能。建议使用"基于标题+链接"模式以减少查询。
2. **误判可能**：如果使用"仅基于标题"模式，可能误判不同文章为重复。
3. **已读标记**：重复文章会被标记为已读，但不会删除，仍可在已读列表中查看。
4. **数据库兼容性**：已测试 PostgreSQL，MySQL/MariaDB 应该也能正常工作。

## 卸载

1. 在 FreshRSS Web 界面中禁用扩展
2. 删除扩展目录（可选）
3. 扩展会自动清理配置

## 贡献

如果您对 `DuplicateRemover` 有任何改进建议或想要贡献代码，请通过 GitHub 仓库提交 Pull Request 或 Issue。

## 许可

该项目根据 [MIT License](DuplicateRemover/LICENSE) 许可证开源。

## 致谢

- 参考了 [TranslateTitlesCN](https://github.com/jacob2826/FreshRSS-TranslateTitlesCN) 扩展的实现方式
- 感谢 FreshRSS 团队提供的优秀扩展系统

---

<a name="english"></a>
# DuplicateRemover

`DuplicateRemover` is an extension for [FreshRSS](https://github.com/FreshRSS/FreshRSS) that automatically detects and marks duplicate articles across different RSS feeds. It supports deduplication strategies based on title only or title + link.

## Features

- ✅ **Cross-feed deduplication**: Automatically detects duplicate articles across different RSS feeds
- ✅ **Flexible deduplication strategies**: Supports title-only or title+link based deduplication
- ✅ **Auto-mark as read**: Automatically marks new duplicate articles as read
- ✅ **Preserve earliest article**: Keeps the earliest article unread
- ✅ **Configurable**: Supports configuring deduplication mode and logging via web interface
- ✅ **Complements built-in features**: Works alongside FreshRSS's built-in `same_title_in_feed` feature

## Installation

1. Download the `DuplicateRemover` extension.
2. Place the `DuplicateRemover` folder (located in the `DuplicateRemover/` subdirectory of the repository) in your FreshRSS instance's `./extensions` directory.
3. Log in to your FreshRSS instance.
4. Go to the administration panel, then navigate to the "Extensions" section.
5. Find `DuplicateRemover` in the extension list and click "Enable".

### Install from GitHub

```bash
# Method 1: Clone to a temporary directory, then copy the subdirectory
cd /tmp
git clone https://github.com/wgding/DuplicateRemover.git
cp -r DuplicateRemover/DuplicateRemover /path/to/FreshRSS/extensions/

# Method 2: Clone directly to the extensions directory
cd /path/to/FreshRSS/extensions
git clone https://github.com/wgding/DuplicateRemover.git
# Then move the DuplicateRemover/DuplicateRemover directory to the extensions directory
mv DuplicateRemover/DuplicateRemover ./DuplicateRemover-temp
rm -rf DuplicateRemover
mv DuplicateRemover-temp DuplicateRemover
```

**Important**: You need to copy the `DuplicateRemover/` subdirectory (containing `extension.php`, `metadata.json`, etc.) from the repository to the `FreshRSS/extensions/` directory.

## Usage

After installation and enabling the extension, go to the extension's configuration page to adjust settings. Here you can:

### Deduplication Mode

- **Title only**: Articles with the same title are considered duplicates
  - ✅ Pros: Wider deduplication scope
  - ⚠️ Cons: May misidentify different articles with the same title

- **Title + Link (Recommended)**: Both title and link must be the same to be considered duplicates
  - ✅ Pros: More accurate, reduces false positives
  - ⚠️ Cons: Smaller deduplication scope

### Logging

When enabled, deduplication operations will be logged in the PHP error log for debugging purposes.

## How It Works

1. When a new article is imported, the extension checks in the `entry_before_insert` hook
2. Queries the database to check if an article with the same title (or title+link) already exists
3. If found, marks the new article as read
4. Keeps the earliest article unread

## Differences from FreshRSS Built-in Features

| Feature | FreshRSS Built-in `same_title_in_feed` | DuplicateRemover Extension |
|---------|----------------------------------------|---------------------------|
| Deduplication Scope | Within a single feed | Across all feeds |
| Deduplication Strategy | Title only | Title or Title + Link |
| Configuration | Global settings | Extension configuration interface |

**Both can be used together to complement each other.**

## System Requirements

- FreshRSS 1.18.0 or higher
- PHP 7.4 or higher
- PostgreSQL or MySQL/MariaDB database

## Notes

1. **Performance impact**: Each article import triggers a database query. If you have many feeds, this may affect performance. It's recommended to use "Title + Link" mode to reduce queries.
2. **False positives**: Using "Title only" mode may misidentify different articles as duplicates.
3. **Read status**: Duplicate articles are marked as read but not deleted, and can still be viewed in the read list.
4. **Database compatibility**: Tested with PostgreSQL. MySQL/MariaDB should also work.

## Uninstallation

1. Disable the extension in the FreshRSS web interface
2. Delete the extension directory (optional)
3. The extension will automatically clean up its configuration

## Contributing

If you have any suggestions for improvements or want to contribute code to `DuplicateRemover`, please submit a Pull Request or Issue through the GitHub repository.

## License

This project is open source under the [MIT License](DuplicateRemover/LICENSE).

## Acknowledgments

- Implementation approach referenced from [TranslateTitlesCN](https://github.com/jacob2826/FreshRSS-TranslateTitlesCN) extension
- Thanks to the FreshRSS team for providing an excellent extension system
