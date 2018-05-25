## Ccache
Gcc derleyicisi için bir araç olan ccache, aynı programı tekrar tekrar derlediğimizde her derleme de daha hızlı bir şekilde derlenmesini ve derleme zamanının düşmesini sağlıyor.

#### Ayarlamalar
Ayarlamaların öncelikleri aşşağıda ki gibidir(1 En Yüksektir)
1. Ortam Değişkenleri
2. Cache spesifik ayar dosyaları (<code>$HOME/.ccache/ccache.conf</code>)
3. Sistem genelinde ki(global) ayar dosyaları (<code> /etc/ccache.conf </code>)
#### Makepkg İçin Ccache Aktif Edilmesi
Makepkg kullanırken,ccache aktif etmek için, <code>BUILDENV</code> yazılan kısmı yorum satırından kaldır aşşağıda ki gibi <code> ccache </code> ekleme yapabilirsin.
```
/etc/makepkg.conf
---------------------
BUILDENV=(fakeroot !distcc color ccache check !sign)
```

#### Komut Satırı İçin Aktif Edilmesi
Kodlarını komut satırından derliyorsan ve hızlı alternatiflere ihtiyacın varsa derlemek için, ccache komut satırında da kullanabilirsin. Aktif etmek için
```
$ export PATH="/usr/lib/ccache/bin/:$PATH"
```
Ayrıca her komut satırını açtığın da bu komutu uygulamak istemiyorsan, <code> ~/.bashrc </code> içerisine yazabilirsin.
NOT: Ccache PATH üzerinden ortam değişkenlerine eklendiğinde, <code> makepkg </code> için de ccache'in aktif olmasına neden olucaktır.

#### colorgcc'nin Aktif Edilmesi
Colorgcc, derleyicilerin ön yüzü olduğundan beri,  her parçanın düzgün bir şekilde uyarlanması için bazı adımlara dikkat edilmesi gerekiyor, bunlar:
```
export PATH="/usr/lib/colorgcc/bin/:$PATH"    # Her kullanımda colorgcc'nin yüklenmesi için,ccache ekleme, değişiklik yapmadan bırak.
export CCACHE_PATH="/usr/bin"                 # Derleyicilerin burada olduğunu ccache bildiriyoruz
```
Daha sonra colorgcc  gerçek derleyicileri çağırmak istiyecek , bunun içinde <code> /etc/colorgcc/colorgccrc </code> dosyasını değiştirerek , <code> /usr/bin </code> dizinini <code> /usr/lib/ccache/bin </code> olarak değiştirip  bütün derleyicilerin bu dizin olduğunu söylüyoruz
```
/etc/colorgcc/colorgccrc
----------------------------------------
g++: /usr/lib/ccache/bin/g++
gcc: /usr/lib/ccache/bin/gcc
c++: /usr/lib/ccache/bin/g++
cc: /usr/lib/ccache/bin/cc
g77:/usr/bin/g77
f77:/usr/bin/g77
gcj:/usr/bin/gcj
```

## Misc
#### Cache Dizinin Değiştirilmesi
SSD yada ramdisk gibi daha hızlı lokasyonlara   cache dizinin taşımak isteyebilirsin.  Terminal üzerinden lokasyonu değiştirmek için
```
$ export CCACHE_DIR=/ramdisk/ccache
```
Ya da
```
/home/user/.ccache/ccache.conf
------------------------------------
cache_dir = /ramdisk/ccache
```
#### Maksimum Karakter Sayısının Belirlenmesi
Default cache miktarı 5GB,istenilirse daha yüksek veya daha aşşağı bir miktar seçilebilir.
```
$ ccache --set-config=max_size=2.0G
```
#### Ortam Değişkenleri Yardımıyla Cache'in Kapatılması
Terminal kullanılarak cache'in kapatılması için
```
$ export CCACHE_DISABLE=1
```
#### CLI
<code>ccache</code> komutunu kullanarak,istatistikleri gösterebilirsin.
```
$ ccache -s
```
Ya da tamamıyla cacheleri temizlemek için
```
$ ccache -C
```

#### makechrootpkg
<code> makechrootpkg</code> ile de ccache kullanılabilir.
```
$ mkdir /path/of/chroot/ccache
$ makechrootpkg -d /path/to/cache/:/ccache -r /path/of/chroot -- CCACHE_DIR=/ccache
```
