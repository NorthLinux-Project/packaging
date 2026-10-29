# North Linux Packaging

Official package build recipes and repository generator for North Linux.

## Repository Layout

```
packaging/
├── .github/workflows/package.yml  # Automated CI build & indexing pipeline
├── builder/
│   ├── nlbuild                    # Recipe parser, fetcher, and package builder
│   └── nlrepo                     # Repository index (.nldb) generator
├── core/                          # Core system packages
│   └── dash/NLBUILD
└── extra/                         # Extra applications and utilities
```

## Recipe Format (`NLBUILD`)

Each package recipe specifies upstream sources, cryptographic checksums, dependencies, and build/package lifecycle functions:

```bash
pkgname="pkg"
pkgver="1.0.0"
pkgdesc="Package description"
url="https://example.org"
arch="x86_64"
license=("MIT")
depends=("glibc>=2.30")
sources=("https://example.org/files/pkg-1.0.0.tar.gz")
sha256sums=("e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855")

build() {
    cd "${srcdir}/${pkgname}-${pkgver}"
    ./configure --prefix=/usr
    make -j$(nproc)
}

package() {
    cd "${srcdir}/${pkgname}-${pkgver}"
    make DESTDIR="${pkgdir}" install
}
```

## Local Building

```bash
# Build a single package into an output directory
./builder/nlbuild core/dash out/core

# Generate repository database
./builder/nlrepo out/core core
```
