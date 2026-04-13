# envsubst

[![ci](https://github.com/cloudspinx/envsubst/actions/workflows/ci.yml/badge.svg)](https://github.com/cloudspinx/envsubst/actions/workflows/ci.yml)
[![Go Reference](https://pkg.go.dev/badge/github.com/cloudspinx/envsubst.svg)](https://pkg.go.dev/github.com/cloudspinx/envsubst)
[![Go Report Card](https://goreportcard.com/badge/github.com/cloudspinx/envsubst)](https://goreportcard.com/report/github.com/cloudspinx/envsubst)

`envsubst` is a Go package for expanding variables in a string using `${var}` syntax,
with support for bash string replacement functions. It ships both a library and a
small CLI (`cmd/envsubst`).

> This is a maintained fork of [`drone/envsubst`](https://github.com/drone/envsubst),
> which has been unmaintained since 2020. The fork keeps the package API stable while
> refreshing the Go toolchain, dependencies, and CI. Drop-in replacement for existing
> callers — only the import path changes.

## Install

### As a library

```bash
go get github.com/cloudspinx/envsubst
```

```go
import "github.com/cloudspinx/envsubst"

out, err := envsubst.EvalEnv("hello ${NAME:-world}")
```

### As a CLI

```bash
go install github.com/cloudspinx/envsubst/cmd/envsubst@latest
```

Reads stdin, writes substituted output to stdout:

```bash
echo 'hello ${USER}' | envsubst
```

## Supported Functions

| Expression                    | Meaning                                                             |
| ----------------------------- | ------------------------------------------------------------------- |
| `${var}`                      | Value of `$var`                                                     |
| `${#var}`                     | String length of `$var`                                             |
| `${var^}`                     | Uppercase first character of `$var`                                 |
| `${var^^}`                    | Uppercase all characters in `$var`                                  |
| `${var,}`                     | Lowercase first character of `$var`                                 |
| `${var,,}`                    | Lowercase all characters in `$var`                                  |
| `${var:n}`                    | Offset `$var` `n` characters from start                             |
| `${var:n:len}`                | Offset `$var` `n` characters with max length of `len`               |
| `${var#pattern}`              | Strip shortest `pattern` match from start                           |
| `${var##pattern}`             | Strip longest `pattern` match from start                            |
| `${var%pattern}`              | Strip shortest `pattern` match from end                             |
| `${var%%pattern}`             | Strip longest `pattern` match from end                              |
| `${var-default}`              | If `$var` is not set, evaluate expression as `$default`             |
| `${var:-default}`             | If `$var` is not set or is empty, evaluate expression as `$default` |
| `${var=default}`              | If `$var` is not set, evaluate expression as `$default`             |
| `${var:=default}`             | If `$var` is not set or is empty, evaluate expression as `$default` |
| `${var/pattern/replacement}`  | Replace as few `pattern` matches as possible with `replacement`     |
| `${var//pattern/replacement}` | Replace as many `pattern` matches as possible with `replacement`    |
| `${var/#pattern/replacement}` | Replace `pattern` match with `replacement` from `$var` start        |
| `${var/%pattern/replacement}` | Replace `pattern` match with `replacement` from `$var` end          |

For a deeper reference, see [bash-hackers](https://wiki.bash-hackers.org/syntax/pe#case_modification)
or [GNU pattern matching](https://www.gnu.org/software/bash/manual/html_node/Pattern-Matching.html).

## Unsupported Functions

- `${var+default}`
- `${var:?default}`
- `${var:+default}`

## Development

```bash
go test ./...
go vet ./...
```

CI runs tests against Go 1.22, 1.23, and 1.24 on every push and pull request.

## License

MIT — see [LICENSE](LICENSE). Original copyright (c) 2017 drone.io; fork
copyright (c) 2025 CloudSpinx.
