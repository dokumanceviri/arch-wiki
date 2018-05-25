## 64 Bit Sistemde 32 Bit Paketlerin Geliştirilmesi
NOT: <code> devtools </code> gerekli ayrıca <code> arch-install-scripts </code>'leri de yüklemelisin.

Bu örnekte chroot ortamını oluşturmak için <code>mkarchroot</code> aracı kullanılmaktadır. İlk olarak istediğin herhangi bir dizin yada benim gibi <code> /opt/arch32 </code>  şeklinde bir dizin oluşturabilirsin.

NOT:Bu örneklerde standart makepkg.conf ve pacman.conf kullanılacak. Eğer daha önce özelleştirdiğin makepkg.conf ve pacman.conf varsa standart hallerini kullanmalısın. Ayrıca  /etc/pacman.d/mirrorlist <code> $arch </code> değişkeni yerine <code> x86 </code> ya da <code> i686 </code> içerdiğinden emin ol.

#### /opt/arch32/pacman.conf Düzenlenmesi
<code> Architecture = auto </code> yu <code> Architecture = i686 </code> olarak değiştir. Ayrıca herhangi multi lib'i yorum satırına alman gerekli.
#### /opt/arch32/makepkg.conf Düzenlenmesi
```
Change CARCH="x86_64" to CARCH="i686"
CHOST="x86_64-unknown-linux-gnu" to CHOST="i686-unknown-linux-gnu".
CFLAGS="-march=x86-64 -mtune=generic -O2 -pipe" to CFLAGS="-march=i686 -mtune=generic -O2 -pipe" .
CXXFLAGS="-march=x86-64 -mtune=generic -O2 -pipe" to CXXFLAGS="-march=i686 -mtune=generic -O2 -pipe" .
```
Değişiklikler yapıldıktan sonra,yeni bir dizin oluşturman gerekli. Ben <code> /aur </code> olarak oluşturdum.
```
sudo mkarchroot -C /opt/arch32/pacman.conf -M /opt/arch32/makepkg.conf <chrootdir>/root base base-devel
```
Eğer benim gibi <code> /aur </code> olarak dizinini oluşturduysan, şöyle bir komutu çalıştıracaksın
```
sudo mkarchroot -C /opt/arch32/pacman.conf -M /opt/arch32/makepkg.conf /aur/root base base-devel
```
Ayrıca, <code> /aur/copy/etc/pacman.d/mirrorlist </code> düzenleyerek kullanılacak mirror'ları seçmen gerekli.
Şimdi <code> makechrootpkg </code> kullanarak i686 paketleri geliştirebilirsin.
Bunun gibi:
```
# makechrootpkg -r /aur/
```
