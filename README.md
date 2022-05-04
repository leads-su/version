# Versioning Package for Go Lang


This package provides ability to version final binaries.

In order to add version information for the binary, you need to provide LDFLAGS for the target binary.

## Providing LDFLAGS
This is the list of LDFLAGS you can provide to the builder while building target binary
- `-X github.com/leads-su/version.version` - sets version of binary
- `-X github.com/leads-su/version.commit` - sets commit with which binary was built
- `-X github.com/leads-su/version.buildDate` - sets build date for binary
- `-X github.com/leads-su/version.builtBy` - sets builder name for binary

## LDFLAGS in pipeline
- `-X github.com/leads-su/version.version={{.Version}}`
- `-X github.com/leads-su/version.commit={{.Commit}}`
- `-X github.com/leads-su/version.buildDate={{.Date}}`
- `-X github.com/leads-su/version.builtBy=builder-name`

## Using GoReleaser (Preferred way of building binaries)
This is an example configuration for GoReleaser (`.goreleaser.yaml`)
```yaml
builds:
  - env:
      - CGO_ENABLED=0
      - GO111MODULE=on
    goos:
      - linux
      - windows
      - darwin
    goarch:
      - 386
      - amd64
    ldflags:
      - -s -w -X github.com/leads-su/version.version={{.Version}} -X github.com/leads-su/version.commit={{.Commit}} -X github.com/leads-su/version.buildDate={{.Date}} -X github.com/leads-su/version.builtBy=goreleaser
archives:
  - format: zip
    name_template: "{{ .ProjectName }}_{{ .Version }}_{{ .Os }}_{{ .Arch }}"
    files:
      - changelog*
      - CHANGELOG*
      - readme*
      - README*
checksum:
  name_template: "{{ .ProjectName }}_{{ .Version }}_SHA256SUMS"
  algorithm: sha256
changelog:
  sort: asc
  filters:
    exclude:
      - "^docs:"
      - "^test:"
```