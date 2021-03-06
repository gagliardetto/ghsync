# ghsync

## Build

This project does not support building as a go module. It needs to be compiled in `$GOPATH/src/github.com/src-d/ghsync`.

To build use:

```shell
make build
```

To update the vendor dependencies, run 

```shell
GO111MODULE=on go mod tidy
GO111MODULE=on go mod vendor
```

## Kallax Models

In order to update the kallax models, place this project in `$GOPATH/src/github.com/src-d/ghsync`.

Then follow these steps:

```shell
# Make sure you are not using modules
unset GO111MODULE

# Get kallax, replace it with mcuadros fork, branch ghsync
go get -u gopkg.in/src-d/go-kallax.v1/...
cd $GOPATH/src/gopkg.in/src-d/go-kallax.v1
git remote add mcuadros git@github.com:mcuadros/go-kallax.git
git fetch --all
git checkout -b ghsync mcuadros/ghsync

# Build kallax
rm $GOPATH/bin/kallax
go get -u github.com/golang-migrate/migrate
cd generator/cli/kallax
go install

# Make sure the $GOPATH/bin is in your path, if not run
export PATH=$GOPATH/bin:$PATH

# Back to ghsync, create the dependencies vendor folder
cd $GOPATH/src/github.com/src-d/ghsync
GO111MODULE=on go mod vendor

# Run kallax generation
go generate ./...

# Create the migration files
kallax migrate --input models --out models/sql --name some_name

# Update bindata
make bindata
```


## Contribute

[Contributions](https://github.com/src-d/ghsync/issues) are more than welcome, if you are interested please take a look to
our [source{d} Contributing Guidelines](https://github.com/src-d/guide/blob/master/engineering/documents/CONTRIBUTING.md).


## Code of Conduct

All activities under source{d} projects are governed by the
[source{d} code of conduct](https://github.com/src-d/guide/blob/master/.github/CODE_OF_CONDUCT.md).


## License

GPL v3.0, see [LICENSE](LICENSE.md).
