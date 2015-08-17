pkgname=protobuf-net
pkgdesc="Fast, portable, binary serialization for .NET"
pkgver=668
pkgrel=3
arch=(any)
license=("Apache 2.0")
options=(!emptydirs)
url="https://code.google.com/p/protobuf-net"
source=("$pkgname::svn+http://protobuf-net.googlecode.com/svn/trunk/"
"protobuf-net.pc.in"
"protogen.sh")
depends=(mono)
makedepends=(subversion)
sha1sums=('SKIP'
          'b652ed0840c697cd17493be479ae1017ab669471'
          '14329bd1206887de917feed8f4cbeca6f18c3853')

prepare() {
	cd "$srcdir/$pkgname"
	svn up -r$pkgver
	sed -i "s,usage.txt,Usage.txt," ProtoGen/Properties/Resources.resx
	
}

build() {
	cd "${srcdir}/$pkgname/protobuf-net"
	xbuild protobuf-net.csproj /p:Configuration=Release
	cd ../ProtoGen
	xbuild ProtoGen.csproj /p:Configuration=Release
}

package() {
	cd "${srcdir}/$pkgname/protobuf-net/bin/Release"
	find . -name '*.dll*' -exec install -Dm644 {} "$pkgdir/usr/lib/protobuf-net/"{} \;
	install -Dm644 Licence.txt "$pkgdir/usr/share/licenses/protobuf-net/Licence.txt"
	cd ../../../ProtoGen/bin/Release
	find . -name 'protogen.exe*' -exec install -Dm644 {} "$pkgdir/usr/lib/protobuf-net/"{} \;
	install -m644 descriptor.proto "$pkgdir/usr/lib/protobuf-net/descriptor.proto"
	find . -name '*.xslt' -exec install -m644 {} "$pkgdir/usr/lib/protobuf-net/" \;
	find "$pkgdir" -name '*.dll' -exec gacutil -i {} -root "$pkgdir/usr/lib" \;
	install -Dm755 "$srcdir/protogen.sh" "$pkgdir/usr/bin/protogen"
	install -Dm644 "$srcdir/protobuf-net.pc.in" "$pkgdir/usr/lib/pkgconfig/protobuf-net.pc"
	sed -i "s,@VERSION@,$pkgver," "$pkgdir/usr/lib/pkgconfig/protobuf-net.pc"
}
