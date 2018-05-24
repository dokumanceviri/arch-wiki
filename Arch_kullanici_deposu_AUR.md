## Arch Kullanıcı Deposu
AUR(Arch kullanıcı deposu) Arch kullanıcıları için topluluk tarafından yürütülen depodur. <code>makepkg</code> ile derlenip , <code>pacman</code> ile yüklenebilmesini sağlayan <code> PKGBUILD </code> leri içerir. AUR'un oluşturulma nedenleri , topluluk tarafından paketlerin paylaşılabilmesini ve organizasyonu sağlamak. Popüler olan paketler ise topluluk depolarının gelişmesine yardımcı oluyor. Bu makale kullanıcıların nasıl AUR'a erişeceğini ve ondan nasıl faydalanacağınından bahsedecek.

AUR içerisinde kullanıcılar , kendi paketlerinin yapılandırmalarına katkı sağlayabilmektedirler. AUR kullanıcıları paketleri oylayabilmektedir ve eğer bir paket yeteri kadar popüler olabilirse, uygun lisans ve paket yapısı sağlandıktan sonra , direkt olarak <code>pacman</code> ve <code>abs</code> içinde paket olarak var olabilme şansını yakalabilir.

Kullanıcılar, AUR web arayüzü üzerinden paketleri arayabilir ve indirebilirler. Bu paketler <code> makepkg </code> ile derlenip daha sonrasında ise <code>pacman </code> ile yükleneme yapılmalıdır.

- <code> base-devel </code> paket grubunun kurulu olduğundan emin olun.
-  [SSS](#SSS) üzerinden genel bir izlenim elde edin
- <code> /etc/makepkg.conf </code> üzerinden AUR'dan paketler derlernirken işlem önceliklerini ayarlabilirsin.
- <code> MAKEFLAGS </code> değişkenleri ile  çok çekirdekli işlemcileri kullanan sistemlerde derleme zamanlarında önemli geliştirmelere yapılabilir.
- Kullanıcılar ayrıca  donanımlara özel optimazyonları <code> GCC </code> aralığıyla <code> CFLAGS </code> değişkenlerini kullanarak yapabilir.

Ayrıca AUR ile ssh üzerinden de iletişim sağlanabilir. SSH üzerinden AUR için
```
ssh aur@aur.archlinux.org help
```
yazarak, AUR yardım komutlarına erişilebilir.

#### Geçmiş
Başlarda sadece <code>ftp://ftp.archlinux.org/incoming </code> vardı ve insanlar katkı sağlamak için buradan <code> PKGBUILD </code>,gerekli dosyalar ve yapılandırılmış paketi sunucuya yüklüyordu. Yüklenen paketler , Paket bakımları ve izlemeleri yapan kişi tarafından görülüp ,program kabul edilesiye kadar orada bekliyor. Ve daha sonra Güvenli Kullanıcı Deposu Doğdu. Belirlenmiş kullanıcılar, kendi depolarının başkaları tarafından host üzerinden kullanılmasını kabul etti. Bu sayede AUR buradan gelişmeye başladı. Halen AUR bakımını yapan geliştiriciler,<code>Güvenilir Kullanıcıdır</code>, öyle çağrılırlar.
2015-06-08 - 2015-08-08 tarihleri arasında AUR versiyon 3.5.1 den 4.0.0 geçerek, PKGBUILD'lerin Git depoları kullanılarak paylaşılması için bir giriş yaptı. Bakımı yapan kişiler , paketlerin yeni alt yapıya  manuel olarak aktarımını yaparken ,mevcut paketler oraya gönderildi.

#### AUR3 Paketleri İçin Git Depoları
Githubda ki AUR arşivi ,aktarım sırasında, AUR3 teki her paket için bir depoya sahiptir. Alternatif olarak [aur3-mirror](https://github.com/felixonmars/aur3-mirror/) aynı şeyi sağlamaktadır.

#### Paketlerin Yüklenmesi
AUR'dan paketlerin yüklenmesi basittir:
- Yapılandırma dosyalarını,PKGBUILD ve diğer gerekli dosyaları içeren yapıyı sağla
- PKGBUILD'in herhangi bir tehlikeli kod,veya virüs içermediğinde emin ol
- Dosyaları kaydettiğin dizin de <code>makepkg -si </code>  komutunu çalıştır. Bu komut kodları indirecek daha sonrasında, deleyip,yapılandırıp,paketleyip yükleyecek.
NOT: AUR desteklenmez dolayısıyla kendi sorumluluğunuzda yüklediğiniz AUR paketlerinin bağlı olduğu resmi depoda ki kütüphanaler güncellenirse, AUR paketlerini yeniden yapılandırmanız gereklidir.

#### Ön Gereklilikler
İlk olarak <code> base-devel </code> ve <code> make </code> komutlarının kaynaktan derlenemek için yüklendiğinden emin ol.

NOT: AUR içerisinde ki paketler <code> base-devel </code> paketlerinin yüklenmesini gereklilik duyar. Açıkca bağımlılık olarak listede yer almazlar.

Şimdi ise uygun bir yapılandırma dizini seç. Bu dizin paketin yapılandırılması için herhangi bir dizin olabilir. Örneklerde kullanılacak yapılandırma dizini <code>~/builds </code> dizini olucak.
#### Yapılandırma Dosyalarının Sağlanması
Paket AUR içerisinde bulunur. Bunu AUR'un sitesinde bulunan paket arama özelliğinden faydalanarak yapabilir ve paket hakkında bilgilere ulaşabilirsin. Paketin bilgileri iyice oku, yorumları oku ve paket hakkında bilgiler edin.
Yapılandırma dosyalarını edinmek için belli başlı yollar bulunmaktadır:
- Git deposundan paketin kopyalanması. Paket detaylarında bulunan git url sinden yapılandırma dosyalarının elde edilmesi
```
$ git clone https://aur.archlinux.org/package_name.git
```
Bu methodun avantajı ise, kolayca <code> git pull </code> yaparak paketin güncellemeleri elde edebilirsiniz.
- Sağ tarafta "Package Actions" altında bulunan "Download Snapshots" linkine tıklayarak yapılandırma dosyalarını elde edebilirsiniz. Bu işlem sıkıştırılmış <code>tar</code> dosyasını indiricek. Daha sonrasında yapılandırma dizini olarak belirlediğiniz çalışma dizininde paketin çıkartılması gereklidir.
```
$ tar -xvf package_name.tar.gz
```
- Benzer olarak , terminalden de sıkıştırılmış dosyayı indirebilirsiniz.
```
$ curl -L -O https://aur.archlinux.org/cgit/aur.git/snapshot/package_name.tar.gz
```

#### Paketin Yapılandırılması Ve Yüklenmesi
UYARI: Dikkatlice <code> PKGBUILD </code> ve <code> .install </code> dosyasını kontrol edilmesi gerekiyor çünkü <code> PKGBUILD </code> dosyası <code>makepkg</code> tarafından çalıştırılmak üzere  bash scriptlerini ve fonksiyonlarını barındırır dolayısıyla tehlikeli ve sisteme zarar vericek kodları barındırması mümkündür o yüzden dikkatlice kodların kontrol edilmesi gerekiyor. <code>makepkg</code> asla  kodları root olarak çalıştırmaz, sahte root'u kullanır. Ne kadarda belli başlı güvenlik önlemleri olsa da dikkatli olmanızda fayda vardır. Eğer  herhangi bir sisteme zarar verici kod , paket görürseniz, forumlara ve mail listesine bildiriniz.
```
$ cd package_name
$ less PKGBUILD
$ less package_name.install
```
Paketin bütünlüğünü ve sağlamlığını kontrol ettikten sonra normal kullanıcı olarak
```
makepkg -si
```
komutunu çalıştırın.
- <code> -s / --syncdeps </code>  Yapılandırmadan önce otomatik olarak bağımlılıkları çözümler ve <code> pacman </code> aralığıyla yükleme yapar. Eğer paketin bağımlılıklarından birisi AUR üzerinde bulunmaktaysa o zaman o bağımlılığın manuel olarak yüklenmesi gerekmektedir.
- <code> -i / --install </code>   Paket düzgün yapılandırıldıysa, paketi yükler. Alternatif olarak yapılandırılan paket  <code> pacman -U package.tar.xz </code> komutu ile de yüklenebilir.

- <code> -r/ --rmdeps </code> Yapılandırmadan sonra , yapılandırma yaparken kullanılan bağımlılıkları siler.Fakat bu paketler , yapılandırılmış paketin yeniden yüklenmesi yada güncellenmesi için gerekli olan bağımlılıklarda olabilir.
- <code> -c / --clean </code>  Yapılandırmadan sonra gecici yapılandırma dosyalarını temizler. Bu dosyalar genellikle yapılandırma işlemi  debug yapılırken gerekli olur.

NOT: Bu örnekler yapılandırma işlemleri  için genel bir giriş. Daha fazla bilgi için ise [ABS](./Arch_Build_System.md) okunması faydalı olur.

#### Geri Bildirim
AUR Web Arayüzü, paketlerin gelişimini ve katkılarını arttırmak ve geliştirmek üzere bir geri bildirim sistemi bulunmaktadır. PKGBUILD içeriklerini ve  patchlerin paket yorumlarına yapıştırılmasını önlemek için yorum alanını kısa tutuyoruz,onları sınırlıyoruz. Bunun yerine paketi yapan kişinin emailine yada pastebin üzerinden de bunları gönderebilirsiniz.
Arch kullanıcıları için en kolay aktivitelerden biri ise , kendi favari paketlerini oylamaları. Bütün paketler Güvenli Kullanıcılar tarafından topluluk deposuna dahil olmak için bekliyor dolayısıyla oylama çok önemli.

#### Paketin Paylaşılması Ve Bakımının Yapılması
Kullanıcıların paketlerini Arch Kullanıcı Deposunu kullanarak paylaşabilirler. Herhangi bir derlenmiş ikili paket içermiyor sadece  başkaları tarafından indirilmek üzere basit PKGBUILD dosyalarını içermekte.
PKGBUILD dosyaları resmi olmadığı için, kendi riskini taşımaktadır.

#### Paketin Gönderilmesi
Paketin gönderilme ve yapılandırılma aşamalarından emin değilsen , bu kısmı okuduktan sonra AUR'a eklemeden önce, [AUR mail listesine](https://mailman.archlinux.org/mailman/listinfo/aur-general),[AUR Forumlarına](https://bbs.archlinux.org/viewforum.php?id=4) yada [IRC kanallarına](https://wiki.archlinux.org/index.php/IRC_channel) sorabilirsin.

###### Paketin Gönderilme Kuraları
AUR'a paket gönderimi yaparken,aşşağıda ki kuralları dikkate almalısın:
- Gönderilecek paket kesinlikle herhangi bir resmi arch deposu içerisinde bulunmamalı dolayısıyla paketi göndermeden önce resmi depoların iyice kontrol edilmesi gerekiyor. Eğer herhangi bir versiyonu bulunuyorsa kesinlikle paketi gönderme. Eğer resmi paketin süresi geçmişse, onu işaretleyin. Eğer resmi paket bozuk ve ya eksik ise o zaman [Bug Raporu](https://bugs.archlinux.org/). Paket ekstra özellikler katılacak yada patch yapılacaksa bu durum göz ardı edilebilir dolayısıyla <code> pkgname</code> değişkeni bu farklılığı göstermek için  farklı olmalı. Örneğin GNU Screen İçin , Sidebar ile ilgili patch içeriyor ve <code>screen-sidebar</code> olarak isimlendirilmiş. Ek olarak <code> provides=('screen')</code> dizisi kullanılmış çakışmaları önlemek için.

- Paketin var olup olmadığı kontrol etmek için AUR'u kontrol et . Eğer şuan ki paket üzerinde değişikler yapılıp , ek özellikler eklendiyse , değişiklikler yorum olarak bakımı yapan kişinin iznini alarak  gönderilebilir. Eğer paketin bakımı yapılmıyor ve paketin bakımını yapan geliştirici bu konuyla ilgilenmiyorsa boşver ve asla depo içerisin de aynı iki paket oluşturma .

- Yüklemek istediğiniz paketin kullanışlı olduğundan emin olun . Başkasını sizin paketiniz kullanıcak mı ?. Eğer birçok insan paketinizi kullanuşlı bulursa gönderme için uygundur.Genellikle AUR ya da resmi depolara yüklenilen yazılımların ve ya yazılım ilişkili yapıların içermesi istenilenler şunlandır:
derlenmiş(ler),ayar dosya(ları)sı , online yada offline dökümantasyonlar.

- Kesinlikle AUR PKGBUILD içerisinde ,  eğer paket yeniden adlandırılmıyorsa <code> replaces </code> ları kullanma. Örneğin <code>Ethreal</code> <code> WireShark</code> olduğunda , eğer paket var olan bir versiyona sahipse, <code> conflicts </code> kullan (Eğer paket başkaları için de gerekliyse <code> provides</code>)

- Kaynak mevcutsa, binarylerin gönderilmesi engellenir. AUR <code> makepkg </code>  tarafından oluşuturulan binary tarball'ları  ve dosya listesini içermez.

- <code>PKGBUILD </code> dosyasının başına yorum satırı olarak  **bakımı yapan kişi** ve **katkı sağlayan kişi** hakkında aşşağıda ki formata bağlı kalarak bilgi eklenmesi önemlidir.PKGBUILD dosyasını bakımını yapan kişi olarak, en üste yorum satırı olarak
```
# Maintainer: Ad <adress at domain dot tld>
```
ekleme yapmalısın.Eğer daha önce de paket bakımı yapanlar varsa, onları katkı yapanlar olarak ekle. Aynı şeyler orijinal paketi gönderen kişi için de geçerli eğer o sen değilsen. Eğer co-maintainer isen o zaman diğer paket bakımı yapanların eklediği gibi kendini de bakımı yapan kişi olarak oraya ekle.
```
# Maintainer: İsmin <adres at domain dot tld>
# Maintainer: Diğer Bakımı Yapan Kişi İsmi <adres at domain dot tld>
# Contributor: Önceki Bakımı Yapan Kişi İsmi <adres at domain dot tld>
# Contributor: Orjinal Gönderen Kişi İsmi <adres at domain dot tld>
```
###### Doğrulama
AUR üzerin de yazma yetkisine sahip olmak için SSH anahtarı eşine sahip olman gerekiyor ve açık anahtarların içeriklerinin profilinde ki 'Hesabım' kısmına kopyalanması gerekmekte. <code> aur.archlinux.org </code> hostu için yapılandırılmış özel anahtar. Örneğin:
```
~/.ssh/config
--------------------------
Host aur.archlinux.org
  IdentityFile ~/.ssh/aur
  User aur
```
Yeni bir anahtar oluşturmalı ya da varsa var olanı kullanabilirsin. Yeni bir aur anahtarı oluşturmak için
```
$ ssh-keygen -f ~/.ssh/aur
```
NOT: Ayrıca fazladan yeni açık anahtarları yeni satırla ayırarak , profiline ekleyebilirsin

###### Yeni Paketin Oluşturulması
Paket için boş bir lokal git deposu oluştur. <code> git clone </code>  komutunu uygun paket ismi ile beraber uyguladıktan sonrasında , eğer paket AUR üzerinde bulunmuyorsa, şöyle bir hata alıcaksın.
```
$ git clone ssh://aur@aur.archlinux.org/package_name.git
Cloning into 'package_name'...
warning: You appear to have cloned an empty repository.
Checking connectivity... done.
```
NOT: Paket,AUR üzerinden silindiğinde, git deposu silinmiyor. Dolayısıyla daha sonra başkları aynı isimle paket oluşturduklarında bu önceden silinmiş paketin dosyalarına ulaşmak mümkündür.

Paketin git deposunu oluşturduysan, AUR için <code> remote </code> oluştur ve onu çek.
```
$ git remote add remote_name ssh://aur@aur.archlinux.org/package_name.git
$ git fetch remote_name
```
<code> remote_name </code> remote depo oluşturmak için bir başlangıç (örneğin "origin"). İlk commit ile beraber paketi gönderdiğinde AUR üzerinde paket gözükecek. Şimdi ise kaynak dosyalarını lokal git depona eklemeye başlayabilirsin.
UYARI: Paketin için git commitlerin , git kullanıcı adı ve emailine göre onaylanır. İlk commiti attıktan sonra bunları değiştirmek zordur dolayısıyla farklı bir kullanıcı adı ve email üzerinden commit atmak istiyorsanız, <code> git config user.name [...] </code>  ve <code> git config user.email [...] </code> bakınız.

###### Paketlerin Yüklenmesi
AUR üzerinde ki yeni paketler için mevcut olan prosedür, paket güncellemeleri ve diğer paketler için de aynıdır. AUR'a paket göndermek için en azından <code> PKGBUILD</code> ve <code> .SRCINFO</code> dosyalarına bulundurman gerekir.

NOT: <code> PKGBUILD </code> dosyasını her değiştirdiğinde <code> .SRCINFO </code> dosyasını yeniden oluşturman gerekli  örneğin <code> pkgver() güncellemeleri gibi </code>  yoksa AUR güncellenmiş versiyon numarasını görmücektir.

Paketini yüklemek için , <code>PKGBUILD</code> , <code> .SRCINFO </code>  ve diğer yardımcı dosyaları(patchler,.install dosyaları vb)  <code> git add </code>  komutu ile ekleyip <code> git commit </code> komutu ile lokal agac yapısını commitledikten sonrasında <code> git push </code> komutu ile değişikliklerini AUR paketi üzerinde yayınlayabilirsin .
Örneğin:
```
$ makepkg --printsrcinfo > .SRCINFO
$ git add PKGBUILD .SRCINFO
$ git commit -m "useful commit message"
$ git push
```
Eğer <code> .SRCINFO </code> dosyasını commit'e eklemeyi unuttuysan, AUR göndermelerini engelleyecektir. çünkü <code> .SRCINFO </code> her commit de olmak zorundadır. Bu problemi çözmek için <code> git rebase </code> komutunu <code> --root </code> ayarı ile birlikte kullanabilir yada <code> git filter-branch </code>  ile birlikte <code> --tree-filter </code> ayarını kullanabilir.

Gereksiz dosyaların commitlerde olmasını engelemek için <code> .gitignore </code> içerisine eklenebilir. Çalışma dizini oldukça temiz olmalıdır paket yaparken.
