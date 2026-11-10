# Example: Multi-Module MoonBit Workspace (`moondark`)

## `.github/workflows/release.yml`

```yaml
name: Release

on:
  release:
    types: [published]

permissions:
  contents: read

concurrency:
  group: release-${{ github.ref }}
  cancel-in-progress: false

jobs:
  publish:
    name: Publish to mooncakes
    runs-on: ubuntu-latest
    timeout-minutes: 15
    steps:
      - name: Check out tagged source
        uses: actions/checkout@v6

      - name: Verify release version
        shell: bash
        run: |
          set -euo pipefail
          matched=0
          for mod in core sqlite postgresql; do
            version="$(sed -n 's/^version = "\([^"]*\)"/\1/p' "${mod}/moon.mod")"
            if [[ "${GITHUB_REF_NAME}" == "v${version}" ]]; then
              matched=1
            fi
          done
          if [[ "${matched}" -eq 0 ]]; then
            echo "Tag ${GITHUB_REF_NAME} does not match any module version in core/sqlite/postgresql" >&2
            exit 1
          fi

      - name: Set up MoonBit
        shell: bash
        run: |
          curl -fsSL https://cli.moonbitlang.com/install/unix.sh | bash -s latest
          echo "${HOME}/.moon/bin" >> "${GITHUB_PATH}"

      - name: Publish to mooncakes
        shell: bash
        env:
          MOONCAKES_TOKEN: ${{ secrets.MOONCAKES_TOKEN }}
        run: |
          set -euo pipefail
          moon version --all
          moon update
          username="$(sed -n 's/^name = "\([^/]*\)\/.*"/\1/p' core/moon.mod)"
          mkdir -p "${HOME}/.moon"
          printf '{"token":"%s","username":"%s"}' \
            "${MOONCAKES_TOKEN}" "${username}" > "${HOME}/.moon/credentials.json"

          publish_if_needed() {
            local dir="$1"
            local wait_index="${2:-false}"
            local name version
            name="$(sed -n 's/^name = "\([^"]*\)"/\1/p' "${dir}/moon.mod")"
            version="$(sed -n 's/^version = "\([^"]*\)"/\1/p' "${dir}/moon.mod")"
            if moon view "${name}@${version}" > /dev/null 2>&1; then
              echo "Skipping ${name}@${version} (already published)"
              return 0
            fi
            echo "Publishing ${name}@${version}..."
            moon -C "${dir}" publish
            if [[ "${wait_index}" == "true" ]]; then
              for _ in $(seq 1 60); do
                if moon view "${name}@${version}" > /dev/null 2>&1; then
                  break
                fi
                sleep 5
              done
              moon view "${name}@${version}" > /dev/null
            fi
          }

          publish_if_needed core true
          publish_if_needed sqlite false
          publish_if_needed postgresql false
```

## `AGENTS.md` (`### Release`)

```markdown
### Release

版本策略：三个发布模块（`core/`、`sqlite/`、`postgresql/`）在 `0.x` 阶段保持大/中版本（`0.minor.0`）对齐，破坏性变更时同步升级；兼容的小版本（`0.minor.patch`，如仅修复单个模块 bug 或文档）按需独立 bump，无需强迫未变更的模块空升版本。`examples/todo` 标记了 `private = true`，不发布。

当被要求发布新版本时，准备 **draft** release 并交由维护者确认发布：

1. 按需 bump 变更模块的 `moon.mod` `version`（若 `core` 有破坏性变更则同步更新集成模块中的 `KKKIIO/moondark` 依赖版本与自身 `version`）。
2. 给验证过的 commit 打 tag 并推送：`git tag v<version> && git push origin v<version>`.
3. 在该 tag 上创建 **draft** GitHub release（`gh release create v<version> --draft`），把草稿链接发给维护者，检查后再点击 Publish。`release.yml` 由 `release: published` 触发：按依赖顺序（core → sqlite → postgresql）检查各模块当前 `version` 是否已在 mooncakes 发布（`moon view <name>@<version>`），仅发布尚未发布的模块，已发布的自动跳过（需要仓库 secret `MOONCAKES_TOKEN`）。
```

