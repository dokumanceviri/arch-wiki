## Arch Build Sistemi

Bu makale, Arch Build Sistemi (ABS) hakkında yeni başlayanlar için genel bir bakış sunmaktadır.
Tam bir referans kılavuzu değildir.

#### Arch Build Sistemi Nedir ?
Bu sistem ports-like(FreeBSD sistemin de kullanılan,paketlerin Makefile'lar kullanılarak kaynak kodlarından derlenip sisteme yüklenmesi olayı) kaynak kodundan yazılımların paketlenmesini ve yapılandırılmasını
sağlamaktadır. Binary paket yönetimi için (ABS ile yapılandırılmış paketleride içeren) pacman özelleştirilmiş arch aracı iken, ABS yüklenebilir <code>.pkg.tar.gz</code> uzantılı kaynakların derlenmesini sağlayan araçların toplandığı bir sistemdir.

#### Ports-Like Sistemi Nedir ?
Ports kaynak kodundan yazılımların yapılandırılma ve yüklenme süreçlerinin BSD üzerinde otomatikleştirilmesini sağlayan bir sistemdir. Verilen yazılımın/programın indirilmesi,paketlerin çıkartılması,patch yapılması,derlenmesi ve yüklenmesi için kullanılır. Bir port kullanıcının bilgisayarında uygun yazılım yüklendikten sonra bir kaç yükleme ve yapılandırma dosyalarından oluşan ve ona göre bir isim içeren sadece küçük bir dizindir. Port dizinin de bulunan yazılımların <code>make </code> yada <code>make install clean</code> komutları uygulanarak yüklenmesini kolaştırır.

#### ABS'de Benzer Bir Yapıya Sahiptir
ABS ise SVN kullanılarak kontrol edilebilen bir dizin ağacından meydana gelir. Bu ağaç tüm resmi arch yazılımlarını gösterir ama hepsini içermez. Alt dizinlerinde yazılım ve kaynak kodlarını bulundurmaz ama PKGBUILD ve duruma göre de başka bazı dosyalar içerebilir.

#### ABS Genel Yapısı
ABS'in içerdiği araçlar:

###### SVN TREE
arch paketlerinin yapılandırılması ve yüklenmesi için gerekli olan dosyaları için dizin yapısı(Fakat yazılım hakkında kaynak kodlarını yada paketleri içermez). Mevcut olan paketler svn ve git depolarındadır.
###### PKGBUILD
Yüklenecek olan yazılım kaynak kodlarının urlsini içeren bir bash scripti
###### makepkg
PKGBUILD dosyalarını okuyup içerdikleri yazılımları indirip, onları derleyen <code>makepkg.conf</code> dosyası
içerisinde bulunan PKGEXT  dizisine göre  <code>pkg.tar*</code> uzantılı dosyalarını oluşturur.Ayrıca <code>makepkg</code> komutu ile AUR üzerinden yada 3.parti yazılım kullanarak kendi paketini geliştirebilirsin.
###### pacman
pacman tamamıyla ayrı fakat paketler yüklenirken onların bağımlılıklarının yüklenmesi ve paketlerin yapılandırılması için makepkg tarafından yada manual olarak kullanılır.
###### AUR
AUR(Arch User Repository - Arch Kullanıcı Deposu) ABS sisteminden ayrıdır. Desteklenmeyen PKGBUILD'ler kullanılarak AUR deposu üzerinden yazılımlar yapılandırılır ve yüklenir. ABS lokalde bulunan bir ağaç yapısıyken, AUR'un bir websitesi vardır  ve binlerce kullanıcılar tarafından oluşturulmuş paketleri içerir.Bu paketler Arch sistemi tarafından desteklenen resmi paketler değillerdir. Eğer Arch sistemi dışından paket yüklenmek istenirse AUR kullanılabilir.

UYARI: Resmi PKGBULD dosyaları temiz ve düzenli bir chroot sağlar.  AUR dan derlenen paketler sistem de hatalara
neden olabilmektedir .

#### SVN Tree
core,extra ve testing depoları SVN deposu içerisindedir . community ve multilib depoları ise community SVN depoları içerisindedir. Her paket kendi alt dizinine sahiptir.Bununla beraber <code>repos</code> ve <code>trunk</code> dizinleri bulunmaktadır. <code>repos</code>  dizini depo ismi ve mimari olarakta kendi
içinde ayrılmaktadır(örneğin core..). <code>repos</code> içerisinde bulunan PKGBUILD'ler ve dosyalar resmi
arch yapılandırılmaları ve yüklenmeleri için kullanılır. <code>trunk</code> dizini içerisinde bulunan dosyalar ise geliştirici dosyalarını <code>repos</code> dizinine kopyalamadan önce orada hazırlıklarını tamamlar.

Örneğin <code>acl</code> paketi için ağaç yapısı şöyledir:

```
acl
acl/repos
acl/repos/core-i686
acl/repos/core-i686/PKGBUILD
acl/repos/core-x86_64
acl/repos/core-x86_64/PKGBUILD
acl/trunk
acl/trunk/PKGBUILD

```
ABS  dizini içerisinde paketin kaynak kodları gösterilmez onun yerine paketin urlsini ve yapılandırılma komutlarını içeren PKGBUILD dosyaları bulunur. Paket yükleneceği zaman PKGBUILD dosyaları içerdiği urller ve komutlar kullanılarak yükleme yapılır.


#### Neden ABS Kullanmalıyım ?
Arch Build Sisteminin Yaptıkları/Kullandıkları/Uyguladıkları Adımlar:
- Herhangi bir durumda paketlerin derlenmesi yada yeniden derlenmesini sağlamak
- Sistemde bulunmayan paketlerin kaynak kodların yüklenmesini ve derlenmesini sağlamak. Onu sistemde mevcut yüklenmiş hale getirmek
- İhtiyaçlara göre harici paketlerin düzenlenmesini sağlamak(ayarlarını aktif etme,deaktif etme,patchleme vb)
- Tüm sistemin derleyici bayraklarını(flags) kullanarak yeniden başlatılmasını sağlamak
- Düzgün ve temiz bir şekilde kendi kernelinizin yüklenmesi sağlamak
- Kendi kernelinize uygun kernel modüllerini sağlamak
- Yeni,eski,beta veya geliştirme aşamasında olan paketlerin PKGBUILD dosyası içerinden versiyon numaralarını düzenleyerek kolay bir şekilde derlenmesini sağlamak.

ABS arch sistemi için kullanılması çok gerekli bir sistem değildir fakat belli görevlerin ve yapılandırılmaların kolayca sistemde çalışmasını,onların otomatikleştirilmesini sağlamak için gerçekten çok kullanışlıdır.


#### ABS Nasıl Kullanılır
Kaynaktan belirli bir paketi oluşturmak amacıyla gerekli olan PKGBUILD dosyasını almak için her iki svn ve git tabanlı asp paketi kullanılarak yapılabilir. Aşşağı da Svn tabanlı aynı zamanda git-based method açıklanmıştır.

###### SVN Kullanılarak PKGBUILD Dosyasının Getirilmesi
Gereklilikler:
- Paketin bir alt versiyonunun yüklenmesi

UYARI: Tüm deponun indirilmesi çok zaman alabilir ve ayrıca servisleri meşgul edebilir. sadece verilen adımları takip etmelisin çünkü bütün svn deposu  çok büyük gerçekten ve diskte çok büyük alanlar kaplayabilir. Eğer servisler suistimal edilirse  , adresiniz bloklanabilir ayrıca asla scriptlerinizi uygulamak için  public svn depolarını kullanmayınız.

core,extra ve testing depolarını kontrol etmek için:
```
$ svn checkout --depth=empty svn://svn.archlinux.org/packages
```
community ve multilib depolarını kontrol etmek için:
```
$ svn checkout --depth=empty svn://svn.archlinux.org/community
```
Her iki durumda  boş bir dizin oluşturur.


###### Paketin Kontrol Edilmesi
SVN deposunu kontrol ettiğiniz dizinde yapmanız gereken:
```
$ svn update package-name
```
Bu komut istenilen paketi  svn deposununda oluşturulan dizine çekicektir. İstenilen paket eğer yoksa svn bir uyarıda bulunmucaktır. Sadece buna benzer birşey ekrana yazdıracaktır  "At revision 115847". Eğer hiçbir şey oluşturmadan bu hatayı alıyorsanız bu adımları takip edin:
- Paket ismini kontrol edin
- Paketin başka bir depoya taşınıp taşınmadığı kontrol edin
- Paketin hangi paketden yapılandırılmış olduğunu [buradan]( https://www.archlinux.org/packages ) kontrol edin. Örneğin <code>python-tensorflow</code> paketi <code>tensorflow</code> paketinden yapılandırılmıştır.

Periodik olarak kontrol edilen paketlerini güncellemelisin.
```
$ svn update
```
###### Git Kullanılarak PKGBUILD Dosyasının Getirilmesi
Ön koşul olarak <code>asp</code> paketini yüklenmelisin.
Özel bir paketi svn deposundan klonlamak için:
```
$ asp checkout paketismi
```
Bu komut paket ismiyle adlandırılmış bir dizini dizininize klonlucaktır.Klonlanmış dizini güncellemek için ise
<code>asp update </code> birlikte <code>  git pull </code>  komutunu klonlanmış dizin içerisinde uygulanmalıdır.Ayrıca diğer git komutlarını da burada kullanabilirsiniz. Örneğin eski veya diğer versiyonlarını kontrol etmek yada özel değişiklikler yapmak için.
Paketin şuan ki PKGBUILD dosyasını kopyalamak istenirse  uygulanacak komut:
```
$ asp export pkgname
```
###### Paket Yapılandırılması
PKGBUILD dosyasını kopyalayıp yeni dizinine taşıdıktan sonra <code>makepkg</code> kullanılarak istenilen değişiklikler yapıldıktan sonra yeni paket oluşturulabilir.
