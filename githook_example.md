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
# 最大ファイルサイズ（単位:KB）
MAX_FILESIZE=1024

# 変更されたファイルを検索し、ファイルサイズが最大サイズを超えるファイルがあるかどうかチェックする
echo -e "- Checking if any files exceed the maximum file size limit..."
code=0
readonly MAX_FILESIZE
while read -r path; do
  filesize=$(wc -c < "${path}")
  if [ "${filesize}" -gt "$((MAX_FILESIZE * 1024))" ]; then
    echo -e "\033[31m✖ ${path} is too large ($((filesize / 1024)) KB)\033[0m"
    code=1
  else
    echo -e "\033[32m✔ ${path} is within the size limit ($((filesize / 1024)) KB)\033[0m"
  fi
done < <(git diff --cached --name-only)

# エラーコードがある場合は、エラーメッセージを表示してコミットを中止する
if [ "${code}" -ne 0 ]; then
  echo -e "\033[31mERROR: Commit aborted due to the above error(s). Please reduce the file size and try again.\033[0m"
  exit 1
else
  echo -e "\033[32mSUCCESS\033[0m"
  exit 0
fi
```


## ファイルをaddし忘れている場合commitを中止する

```bash
if git status --porcelain | grep -q '^??'; then
  echo "Some files are untracked. Please add them before committing."
  exit 1
fi
```


# commit-msg

## コミットメッセージにPrefixを忘れてないかのチェック

```bash
echo -e "- Checking if the commit message contains a prefix..."
# コミットメッセージをファイルから読み込む
MSG="$(cat "$1")"

# 正しいPrefixの配列を定義
CORRECT_PREFIXES=("feat" "fix" "docs" "style" "refactor" "pref" "test" "chore")

# コミットメッセージが正しいPrefixで始まっているかをチェックする
if ! printf "%s\n" "${CORRECT_PREFIXES[@]}" | grep -qw "$(echo "$MSG" | cut -d' ' -f1 | sed 's/:$//')"; then
    # エラーメッセージを表示してステータスコード1で終了する
    echo -e "\033[31;22mERROR: The commit message must start with one of the following prefixes (with a colon):\033[0m"
    printf "    %s:\n" "${CORRECT_PREFIXES[@]}"
    exit 1
fi

# Prefixの後にコロンがあるかをチェックする
if ! echo "$MSG" | grep -qE "^[^:]+: "; then
    # エラーメッセージを表示してステータスコード1で終了する
    echo -e "\033[31;22mERROR: The commit message must have a colon after the prefix, and a brief explanation of the commit.\033[0m"
    exit 1
else
    # 成功メッセージを表示する
    echo -e "\033[32;22mSUCCESS\033[0m"
fi
```