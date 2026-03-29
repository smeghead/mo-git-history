# Plan: mo-git-history CLI

mo-git-history を単一の実行可能スクリプトとして実装し、指定 Markdown ファイルの git 履歴を新しい順に一時ファイルへ復元して mo --foreground にまとめて渡す。最新タブには現在ファイルを含め、リネーム追跡は行わず、bash 系シェルで実装する。mo は毎回ランダムポートで起動し、既存セッションや session restore から隔離する。

## Steps

1. mo-git-history <file> の単一引数 CLI とし、未導入・管理外・履歴なしなどのエラー方針を決める。
2. 毎回ランダムな空きポートを割り当て、mo --foreground -p <random-port> で起動する。
3. git log で現在パスに対する履歴を新しい順に取得する。リネーム追跡はしない。
4. mktemp -d で専用ディレクトリを作り、各コミット内容を git show で一時ファイルへ復元する。
5. 現在ファイルを先頭、その後ろに履歴ファイルを並べて mo に渡す。
6. EXIT INT TERM HUP の trap で一時ディレクトリを削除し、mo の終了コードを返す。
7. README に、ランダムポート方針、現在版を含むこと、リネーム非対応、履歴件数が多い場合の注意を書く。
8. 専用ポート隔離、cleanup、特殊文字パス、異常系を検証する。

## Relevant files

* README.md — 使い方と制約の整理
* /home/smeghead/work/github.com/smeghead/mo-git-history/mo-git-history — 実装本体
* /home/smeghead/work/github.com/smeghead/mo-git-history/.gitignore — 必要なら補助生成物の無視設定
## Verification

1. 3コミット以上ある Markdown で、現在版 + 履歴版が新しい順に並ぶことを確認する。
2. 通常の mo が動作中でも、表示対象が混ざらないことを確認する。
3. 通常終了と Ctrl+C の両方で temp directory が消えることを確認する。
4. 空白や日本語を含むパスでも壊れないことを確認する。
5. git 管理外、履歴なし、mo 未導入時のエラー動作を確認する。

残る実装上の注意点は、ランダムポートを「乱数で決める」だけでなく「空いていることを確認してから使う」ことです。ここまでで計画は実装着手可能な状態です。次はこの計画に沿って実装へ渡せます。