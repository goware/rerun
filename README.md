rerun: re-run command on file system changes
============================================

Credit for this library goes to https://github.com/VojtechVitek/rerun

Lightweight file-watcher that re-runs given command on FS changes. It has simple CLI and optional config file. By default, it uses 200ms delay, which gives enough time for tools like git to update all directories/files within repository before killing the old process (when you switch branches etc).

#### In development. Only CLI MVP works right now.

# Usage
### `rerun [-watch DIR...] [-ignore DIR...] -run COMMAND [ARG...]`

#### Examples:
```bash
$ rerun -watch ./ -ignore vendor bin '*.md' -run go run ./cmd/rerun/main.go
```
```bash
$ rerun -watch ./ -ignore vendor bin -run sh -c 'go build -i -o ./bin/rerun ./cmd/rerun/main.go && ./bin/rerun'
```
```bash
$ cd tests && rerun -watch '*_test.go' ../pkg -ignore vendor bin -run go test -run=Test
```

# Installation

```bash
go get -u github.com/goware/rerun/cmd/rerun
```
*You might need to [download Go](https://golang.org/dl/) first.*


```yaml
api:
  watch:
    - cmd
    - *.go
  ignore:
    - bin
    - *_test.go
  cmd:
    - go run cmd/api/main.go -flags args

test-login:
  name: Test login
  watch:
    - tests/e2e
    - services/auth
    - data
  run:
    - go test -run=Login
```

Uses [fsnotify](https://github.com/fsnotify/fsnotify) behind the scenes, so technically it should work on most platforms including Linux, Mac OS and Windows.

# License

Licensed under the [MIT License](./LICENSE).
