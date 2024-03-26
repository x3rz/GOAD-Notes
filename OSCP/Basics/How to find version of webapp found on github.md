https://epi052.gitlab.io/notes-to-self/blog/2019-09-12-hack-the-box-swagshop/

1. Find a repo that hosts the older version of that webapp
2. Choosing a file by which we can compare the hash and then identify the version {*Most suitable file is CSS files like **style.css***}
3. Download the target *Style.css* file and calculate its MD5 hash.

Basic steps:
1.  clone the repo
2.  iterate over each tag in the repo
3.  check out the tag
4.  hash that version’s `styles.css`
5.  compare it with the hash we found from the target


```bash

repo="magento-mirror" # Specifying Repo
styles_file="skin/frontend/default/default/css/styles.css" # File to find and get the hash of
target_hash="d9c659f7c70e070394eff4290a0a601a" # Target file hash

# Cloning the repos
if [[ ! -d "${repo}" ]]; then
  git clone https://github.com/OpenMage/magento-mirror.git
fi
pushd "${repo}" >/dev/null || exit

# Iterate over each Tag in the repo
for tag in $(git tag -l); do
  git checkout tags/"${tag}" 2>/dev/null

# Check out the tag
  if [[ ! -e "$(pwd)/${styles_file}" ]]; then
    continue
  fi

# Hash that version's style.css
  ver_hash=$(md5sum "$(pwd)/${styles_file}" | awk '{print $1}')

# Comapre those hash from target hash
  if [[ "${ver_hash}" == "${target_hash}" ]]; then
    echo "Found version: ${tag}"
    break
  fi
done

popd >/dev/null || exit


```