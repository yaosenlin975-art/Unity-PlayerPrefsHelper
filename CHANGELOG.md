# Changelog

## Unreleased

- 变更：**存档路径改为缓存**，不再每次读/写重算。`GetArchiveDir()` 只解析一次（编辑器下 `EditorPrefs`、Player 下 `persistentDataPath/Temps`，并顺带只 `CreateDirectory` 一次）；`PrefsArchive<T>` 的 `archiveFileName` / `archiveFilePath` 改成每个封闭泛型类型 `static readonly` 构造一次，替换掉每次调用都遍历类型名算哈希再 `Path.Combine` 的写法。`Save` 的非 WebGL 分支改用缓存的 `archiveFilePath` 与缓存目录；移除实例 `filePath` 字段与 `GetPath()`。对外 `GetArchiveFileName(Type)` 行为不变（单测里两条哈希断言不受影响）。
- 变更：WebGL 不再计算文件路径。`Load()` 在 `#if UNITY_WEBGL` 下直接把 `filePath` 置空（存档走 `PlayerPrefs`/IndexedDB，路径本来就没用到），只有非 WebGL 才解析路径。
