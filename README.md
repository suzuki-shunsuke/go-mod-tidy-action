# go-mod-tidy-action

GitHub Actions to run `go mod tidy` and push a commit to a pull request.
Keep go.mod and go.sum clean!

![image](https://user-images.githubusercontent.com/13323303/223891482-2495d7c5-6d92-483d-82cc-9275038c9b7e.png)

--

![image](https://user-images.githubusercontent.com/13323303/223891658-0594823a-8a26-4bc5-b3ea-3a804d6923b1.png)

[action.yaml](action.yaml)

Please see [go-mod-tidy-workflow](https://github.com/suzuki-shunsuke/go-mod-tidy-workflow) too.

## Requirements

- [Go](https://go.dev/)
- [Git](https://git-scm.com/)
- [int128/ghcp](https://github.com/int128/ghcp)

:bulb: You can install ghcp by [aqua](https://aquaproj.github.io/) `aqua g -i int128/ghcp`

## GitHub Access Token

The following permissions are required to push a commit.

- `contents: write`

To trigger GitHub Actions Workflows by a new commit, we recommend using GitHub App token or personal access token rather than `${{github.token}}`.

## Usage

```yaml
---
name: go mod tidy
on: pull_request
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout HEAD
        uses: actions/checkout@v3.3.0
        with:
          ref: ${{ github.event.pull_request.head.sha }}
      - name: Set up Go
        uses: actions/setup-go@v3.5.0
        with:
          go-version: 1.20.1
      - name: Install int128/ghcp
        uses: aquaproj/aqua-installer@v2.0.2
        with:
          aqua_version: v1.35.0
      - name: Generate token
        id: generate_token
        uses: tibdex/github-app-token@v1
        with:
          app_id: ${{secrets.APP_ID}}
          private_key: ${{secrets.APP_PRIVATE_KEY}}
      - name: go mod tidy
        uses: suzuki-shunsuke/go-mod-tidy-action@67d43e82d9b5b3280b7082052dce18b77649476c # v0.1.1
        with:
          github_token: ${{steps.generate_token.outputs.token}}
```

## LICENSE

[MIT](LICENSE)
