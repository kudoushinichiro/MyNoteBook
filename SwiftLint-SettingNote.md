# SwiftLint導入時のymlファイルフォーマット

# 記載のフォルダのソースのみ対象
included:
  - App

# 対象から外すフォルダ
excluded:
  - Pods

# 無効にするルール
disabled_rules:
  # 空の連続改行
  - vertical_whitespace



# 空行のスペースを許容する。
# https://github.com/realm/SwiftLint/pull/616 (公式ドキュメント)
trailing_whitespace:
  ignores_empty_lines: true

# 1行の文字数制限
line_length: 300 # warning
