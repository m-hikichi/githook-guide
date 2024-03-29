# 最強のコミットメッセージの書き方

## 結論

コミットメッセージは，以下のように書くと，見やすく理解しやすいものになります．
```
Prefix: 変更内容
```
- Prefix（接頭辞）をつける
- 理由・目的を書く


## Prefix（接頭辞）をつける

Gitのコミットメッセージには，Prefixと呼ばれる接頭辞を付けることが推奨されています．Prefixを付けることで，どのカテゴリのものを修正したのかが簡潔にわかるようになります．また，Prefixを利用することで，コードロジックに影響があるかどうかも簡単に判断できます．
以下は，一般的に使われるPrefixの例です．
- feat: 新しい機能を追加した場合
- fix: バグを修正した場合
- docs: ドキュメントのみを変更した場合
- style: コードの意味に影響を与えない変更（空白，フォーマット，セミコロン追加など）
- refactor: 仕様に影響がないコード改善（ソースコードを見やすく書き換える，読みやすく書き換えるなど）
- perf: パフォーマンスを向上させるためのコード変更を行った場合
- test: テストの追加または修正を行った場合
- chore: その他，ビルドや補助ツール，ライブラリ関連の変更を行った場合


## 理由を書く

コミットメッセージには，変更の理由や目的を書くことモ大切です．これにより，コードレビューがしやすくなり，変更の意図やコードの改善点が明確になります．例えば，以下のようなコミットメッセージはわかりやすいです．
```
chore: ネットワーク通信するため，hogeを追加
```
上記の例では，「hogeを追加」とだけ書かれているのではなく，「ネットワーク通信するため」という理由も書かれています．このように理由を書くことで，なぜこの変更をする必要があったのかや，コミットの大きさが適切かどうかを考えるようになります．


# 参考資料
- [僕が考える最強のコミットメッセージの書き方](https://qiita.com/konatsu_p/items/dfe199ebe3a7d2010b3e)
- [Prefixの種類について](https://github.com/angular/angular.js/blob/master/DEVELOPERS.md#:~:text=Git%20Commit%20Guidelines)