# pre-commit

## masterブランチへのcommitを禁止する

```bash
branch=$(git symbolic-ref HEAD --short)
if [ ${branch} = master ]; then
    echo "Abort commit : masterブランチにcommitできません．"
    exit 1
fi
```


## サイズが大きいファイルのcommitを禁止する

```bash
echo -en "- ファイルサイズが上限を超えていないかチェック："
code=0
max_filesize=100000
readonly max_filesize

paths=$(git diff --cached --name-only)
for path in ${paths[@]}; do
    if [ $(wc -c < ${path}) -gt ${max_filesize} ]; then
        code=1
    fi
done
if [ ${code} = 0 ]; then
    echo -e "\033[32;22mOK\033[0m"
    exit 0
fi

echo -e "\033[31;22mNG"
echo -e "===================="
echo -e "ファイルサイズが上限を超えています"
echo -e ""
echo -e "禁止ファイル"
for path in ${paths[@]}; do
    filesize=$(wc -c < ${path})
    if [ ${filesize} -gt ${max_filesize} ]; then
        echo -e "${path} $((${filesize} / 1024))KB"
    fi
done
echo -e "====================\033[0m\n"
exit 1
```


# commit-msg

## コミットメッセージにPrefixを忘れてないかのチェック

```bash
MSG="$(cat "$1")"
readonly MSG

echo -en "- Prefixの存在チェック："
# 必要なPrefixを定義
readonly CORRECT_PREFIXES=("feat" "fix" "docs" "style" "refactor" "pref" "test" "chore")
# 各要素に": "を追加
for i in "${!CORRECT_PREFIXES[@]}"; do
    correct_prefixes[i]="${CORRECT_PREFIXES[i]}: "
done

# `grep -E`で配列からOR検索をするため，半角スペース(" ")の区切り文字をパイプ("|")に変更
prefixes="$(
    IFS="|"
    echo "${correct_prefixes[*]}"
)"

if ! echo "$MSG" | grep -Eq "${prefixes}"; then
    echo -e "\033[31;22mNG"
    echo -e "===================="
    echo -e "コミットメッセージにPrefixが含まれていません"
    echo -e ""
    echo -e "Prefix"
    echo -e "${correct_prefixes[*]}"
    echo -e "====================\033[0m\n"
    exit 1
else
    echo -e "\033[32;22mOK\033[0m"
fi
```