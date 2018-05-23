## Arch Paketleme Standartları
Arch linux paketi geliştirirken aşşağıda ki paket kurallarına uygun olarak geliştirme yapabilirsiniz eğer ki yeni bir paket geliştirmek ve katkıda bulunmak istiyorsanız o zaman PKGBUILD ve makepkg 'nin  man sayfalarına da ayrıca bakmanız gerekmektedir .

#### PKGBULD Örneği
```
# Bakımı Yapan Kişi: İsminiz <emailiniz@domain.com>
pkgname=İSİM
pkgver=VERSİYON
pkgrel=1
pkgdesc=""
arch=()
url=""
license=('GPL')
groups=()
depends=()
makedepends=()
optdepends=()
provides=()
conflicts=()
replaces=()
backup=()
options=()
install=
changelog=
source=($pkgname-$pkgver.tar.gz)
noextract=()
md5sums=() #updpkgsums kullanarak otomatik doldurma

build() {
  cd "$pkgname-$pkgver"

  ./configure --prefix=/usr
  make
}

package() {
  cd "$pkgname-$pkgver"

  make DESTDIR="$pkgdir/" install
}
```
pacman ve abs paketleri için diğer örnekler <pre>/usr/share/pacman</pre> dizinin de bulunmaktadır.

#### Paket Etiketi
- Paketler asla <pre>/usr/local </pre> dizinine yüklenmemeli
-  makepkg içerisinde ki fonksiyon ve değişken isimleri ile çakışabileceğinden dolayı,PKGBUILD scriptleri içine yeni değişkenler ve fonksiyonlar tanımlanmamalı.
- Eğer gerçekten yeni bir fonksiyon ya da değişken tanımlanması gerekliyse ,değişkenin adıyla birlikle <pre> _ </pre>(Alt tire) kullanılması gereklidir.
```
_yenidegisken=
```
- Herhangi bir durumda <code> /usr/libexec </code> kullanımını önlemek amacıyla  onun yerine <code>   /usr/lib/$pkgname</code> kullanılmalı.

- <code> /etc/makepkg.conf </code>  dosyasından paket yapılandırıcı yardımıyla paket meta verilerini tutan dosyanın içerinde ki <code> packager </code> kısmı değiştirilebilir. Alternatif olarak <code> ~/.makepkg.conf </code> dosyasını oluşturup üzerine yazma olarak istenilen değişikliği aynı şekilde gerçekleştirebilirsiniz.

- <pre> .install dosyası </pre> kullanarak yükleme yapılırken bütün önemli mesajlar kullanıcıya gösterilir.Örneğin bir paket fazladan kuruluma ihitiyaç duyarsa, yolu gösterilmelidir.

- Paket bağımlılıkları en yaygın paketleme hatalarındandır. Paket bağımlılıklarını bulurken lütfen yeteri kadar zamanınızı harcayarak dikkatli bir şekilde bağımlılıkları belirleyin. <pre>namcap </pre> aracı bu konuda size yardımcı olabilir. Bu araç PKGBUILD ve paketin tarball'ını analiz ederek; eksik bağımlılıkları,izinleri, gereksiz bağımlılıkları  ve yaygın hataları göstericektir.

- Herhangi bir paketin çalışmasında kullanılması zorunlu olmayan bağımlılık ya da <pre> dependsarray</pre> içinde olmayan fonksiyonlar , <pre>optdepends</pre> içerisine eklenmelidir.
```
optdepends=('cups: printing support'
            'sane: scanners support'
            'libgphoto2: digital cameras support'
            'alsa-lib: sound support'
            'giflib: GIF images support'
            'libjpeg: JPEG images support'
            'libpng: PNG images support')
```
Yukarıda ki örnek <pre> extra </pre> içerisin de bulunan <pre> wine </pre> paketinden alınmıştır.  optdepends otomatik olarak yükleme ve  yükseltme durumlarını yazdırmaktadır dolayısıyla <pre> .install </pre> içerisinde bu tip bilgilerin tutulmasına gerek yoktur.

- Paket açıklamasını yazarken kesinlikle paketin kendi ismini açıklamaya eklemeyin.Örneğin "Nedit X11 İçin Bir Text Editördür". Bu örnek daha da basit hale getirilerek ve **80 karakteri geçmiyecek** şekilde şu hale getirilebilir. "X11 İçin Text Editörü".

- PKGBUILD dosyası içerisinde her bir satır yaklaşık olarak **100 katakterin** altında olmalı.
- PKGBUILD içerinde mümkün olduğunca boş satırların bulunmaması gerekli.
- PKGBUILD dosyasının yukarıda gösterildiği gibi sırasına göre yazılması gayet güzel bir pratik olur. Bu durum PKGBUILD dosyası için zorunlu birşey değil ama tam bir  bash sözdizimi(yapısı) için gerekli.
- <code> "$pkgdir" </code> ve <code> "$srcdir" </code> gibi yinelenen değişkenler boşluklar içerebilir.
- Paketlerin bütünlüğünü sağlamak amacıyla , bütünlük sağlayan değişkenlerin(**sha1,sha256,md5...**) doğru değerler içerdiğinden emin olunması gerekiyor. Bunlar <code>updpkgsums</code> aracı kullanılarak güncellenebilir.

#### Paket İsimlendirmeleri
-Paket isimleri yanlızca alfanümerik karakterleri içerebilir. **@,.,_,+,-** başlayan isimler kabul edilmemektedir ve bütün paket isimleri küçük harfli olmalıdır.

- Paket isimleri ana versiyonun numarasını içermemelidir örneğin eğer libfoo v2.3.4 gibi bir versiyonu varsa ve öyle kullanılıyorsa, paket ismi olarak <pre>libfoo2</pre> kullanılmamalıdır.

- Paket versiyonları, paket geliştiricisinin yayınladığı versiyonlarda olmalıdır. Versiyonlar harfler içerebilir (nmap versiyonu: 2.54BETA32) . Versiyon numaraları tireleri içermez. Harfleri,sayıları ve periotları içerir.

- Arch linux paketleri için, paket sürümleri  farklıdır. Bu durum kullanıcıların yeni ve eski paket mi olduğu hakkında ayrım yapmasını sağlar. Eğer ki yeni oluşturulmuş bir paket  ve versiyonu ilk versiyonsa, sürüm numarası olarak  1 den başlar daha sonra diyelim ki pakete yeni optimizasyonlar getirildi ve **yeniden yayınlandı**  dolayısıyla versiyon numarası artıcak.
Yeni bir sürüm çıktığında ise paket yayın sayısı 1'e düşürülür. Paket yayın etiketleri ,versiyon etiketleri gibi  isimlendirme kısıtlamalarını takip eder.

#### Dizinler
- Ayar dosyaları <pre> /etc </pre> dizini içerisinde bulunmalıdır.  Eğer fazladan ayar dosyaları varsa , alt dizin kullanılarak  <pre> /etc </pre>  içerisinde düzenli ve temiz bir şekilde tutulabilir. <pre> /etc/paket_ismi </pre> kullanılarak bu dizin içerisin de ayar dosyaları ve gerekli ayarlamaları yapılabilir,barındırılabilir. Örneğin apachenin kullandığı <pre> /etc/httpd </pre> gibi.

- Paket dosyaları aşşağıdaki kuralları takip etmek zorundadır:
| Bölüm 1                   | Bölüm 2                                                         |
| --------------------------| ----------------------------------------------------------------|
|  **/etc**                 | Gerekli sistem ayar dosyaları                                   |
|  **/usr/bin**             | Derlenmişler(Binaryler(ikililer))                               |
|  **/usr/lib**             | Kütüphaneler                                                    |
|  **/usr/include**         | Header Dosyaları                                                |
|  **/usr/lib/{pkg}**       | Modüller,eklentiler vb.                                         |
|  **/usr/share/doc/{pkg}** |	Uygulama dökümantasyonları                                      |
|  **/usr/share/info**      | GNU Info sistem dosyaları                                       |
|  **/usr/share/man**       | Man sayfaları                                                   |
|  **/usr/share/{pkg}**     | Uygulama verileri                                               |
|  **/var/lib/{pkg}**       | Kalıcı uygulama depolama                                        |
|  **/etc/{pkg}**           | <code>{pkg}</code> için ayar dosyaları                          |
|  **/opt/{pkg}**           | Geniş, kendi kendine yeten, başka bağımlılığı olmayan paketler  |

- Paketler aşşağıda ki dizinlerden hiçbirini içermemelidir.
  |-------------|
  |**/bin**     |
  |**/sbin**    |
  |**/dev**     |
  |**/home**    |
  |**/srv**     |
  |**/media**   |
  |**/mnt**     |
  |**/proc**    |
  |**/root**    |
  |**/selinux** |
  |**/sys**     |
  |**/tmp**     |
  |**/var/tmp** |
  |**/run**     |

#### Makepkg Görevleri
makepkg paket yapılandırmaları için kullandıldığında, otomatik olarak şu adımları takip eder:
  1. Paket bağımlılıkları ve **makedepends** yüklü olup olmadığını kontrol eder
  2. Kaynak dosyalarını sunucudan indirir
  3. Kaynak dosyalarının bütünlüğü kontrol eder
  4. Kaynak dosyaları çıkartır
  5. Gerekli patchleme varsa, yapar
  6. Yazılımları yapılandırır ve yüklemeleri  sahte dizine yapar
  7. Binaryden sembolleri ayırır.
  8. Debug sembollerini de kütüphanelerden ayırır.
  9. Manual ve info sayfalarını sıkıştırma yapar.
  10. Her paketin meta verilerini içeren meta dosyasını oluşturur.
  11. Paket dosyası içerisine sahte root dizinini sıkıştırır.
  12. Paket  dosyalarını , ayarlanmış hedef dizin içerisin de depolar.

#### Mimari
Eğer paket mimariye bağımlıysa  **arch** dizisi <code>x86_64</code> bilgisini içermeli. Eğer mimariden bağımsızsa <code>any</code> bilgisini içermeli.  
