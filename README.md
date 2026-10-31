# renovate-config

## プリセット

- `default.json5`: すべてのリポジトリで使う共通設定
- `mise-lockfile.json5`: `mise.lock` を持つリポジトリ向けの設定。`default.json5` の後に指定する

```json
{
  "extends": [
    "github>snhr-1019/renovate-config:default.json5",
    "github>snhr-1019/renovate-config:mise-lockfile.json5"
  ]
}
```

`mise-lockfile.json5` を使うリポジトリの更新ルールは次のとおり。

| 更新の種類 | PR | 自動マージ |
| --- | --- | --- |
| `latest` や `24` など範囲指定の範囲内更新 | 週1回の Lock file maintenance (`mise.lock` のみ変更) | する |
| 完全一致指定の minor / patch 更新 | `dependencies non-major` グループ (`mise.toml` も変更) | する (`default.json5` の既存ルール) |
| 完全一致指定の major 更新 | 個別の PR (`mise.toml` も変更) | しない |

範囲指定を超える更新 (`node = "24"` から 26 へなど) は Renovate が PR を作らないので、`mise.toml` を手で変更する。

vfox などのプラグイン経由のツールは Renovate の safe mode で解決できず、lock の更新全体が失敗するため、`mise.lock` の対象となる設定ファイルに書かない。
