# DOCKER
## Giriş
- İşletim sistemleri bilgisayar donanımı ile uygulamalar arası köprü görevi gören temel yazılımlardır.
- işletim sistemi, kernel, uygulamalar ve kullanıcı arayüzünden oluşur. Kernel en önemlisidir.
- bilgisayar açılır bios devreye girer depolama birimlerine bakar ilk olarak boot loader denilen bir uygulama yükler bu uygulama
  da işletim sisteminin çekirdeğini yani kerneli yükler.
- kernel fiziksel altyapı ve uygulamalar arası bir katmandır görevi uygulamaları yönetmektir.
## Temel bilgiler
- Eski sistemlerde her bir uygulama için ayrı bir sunucu kurulurdu. Bunun nedeni uygulamaların birindeki problemin diğerini etkileyebilme
durumundan dolayıydı. Bu sistem verim açısından kötüydü çünkü kapasitenin büyük bir alanı boş kalıyordu yani atıl kapasite yüksekti.

- Buna çözüm olarak sanallaştırma(VM) getirildi. Bu sayede tek bir sunucu üstüne birden fazla işletim sistemi kurulup bu işletim sistemlerine
ayrı ayrı uygulamalar kurularak birbirlerinden izole bir şekilde çalışabilmeleri sağlandı fakat işletim sistemlerinin temel gereksinimleri için
de gerekecek olan bir kullanım yüzdesi olduğundan tam kapasite verimi sağlamıyordu. Bu durumun üstüne de **containerlar** geliştirildi.

- 2013 **Docker** kuruldu. Container konseptinin dünya da büyük ilgi çekmesini sağladı.

- Linux kerneline namespaces(process,network vs.) özelliği eklendi. Bu özellik sayesinde aynı sistem üzerinde izolasyon sağlandı.

- Docker Engine (Docker Deamon ,Docker Rest ,API Docker CLI)

- Docker **Image** mantığı şu şekildedir. Bir uygulamanın ve bu uygulamanın çalışabilmesi için tüm kütüphanelerin vs. paketlenmiş halidir.
- **Container** ise bu image halinin çalışır durumdaki halidir.

- Container VM'e göre daha hızlı başlatılır. Uygulama izolasyonu sağlar. VM ise sistem izolasyonu sağlar.
- Container daha taşınabilirdir.
## Temel Docker Komutları
- Öncelikle Docker'ın sisteme düzgün yüklenip yüklenilmediğinin test edilmesi önemlidir.

![docker hello world proof for correct installation](https://user-images.githubusercontent.com/99764271/167263255-ac9f6f65-9427-45a4-8ce9-d79df93ea400.PNG)

" run hello-world" komutu ile bunu görüldüğü gibi test edebiliriz.(working correctly)

- Help Komutu

![help komutu](https://user-images.githubusercontent.com/99764271/167263162-6dbfa539-660b-46d7-83cf-b917270b4142.PNG)

bu komut ile eğer komutlardan birinin ismi ya da işlevi unutulduysa yardım alınabilir.

- Container oluşturmak

![container yaratmak app1](https://user-images.githubusercontent.com/99764271/167263357-c22ab701-4347-4fc8-8c64-67dac9afcafe.PNG)

container oluşturulduktan sonra start komutu ile çalıştırılması gerekmektedir fakat kısayol olarak "container run" komutu ile yapılmaktadır.

- Çalışan ve çalışıp kapanmış containerları gözlemleme

![ls komutu](https://user-images.githubusercontent.com/99764271/167263534-463b6d1e-9452-4123-9a9a-ddc5d66aaac1.png)

"container ls " çalışmaya devam eden containerları gösterirken "container ls -a" hem çalışmaya devam eden hem de kapanan containerları göstermektedir.

**docker rm $(docker ps -a -q)** komutu ya da **docker container prune** komutu ile tüm durmuş containerlar kısayoldan silinir.
**docker rm id** şeklinde de teker teker idler girip silinebilir fakat halihazırda çalışan containerları silmek için
-f ile forced yapılması gerekmektedir.

- Detach Mode 

![detach mode](https://user-images.githubusercontent.com/99764271/167291955-2081776e-da64-4261-af34-6147173e3d93.PNG)

**-d** komutu ile container direkt arkaplanda çalışacak şekilde ayarlanmış olur. Terminaller eşlenmez.
ayrıca 127.0.0.1 adresine gidildiğinde alınan feedback sayesinde containerın bu sistemde ayağa kaldırılmış olduğunu anlarız.

-Containera bağlanıp değişiklik yapmak

![containerrefresh](https://user-images.githubusercontent.com/99764271/167298019-dd8b74e2-de09-4ace-9878-6d4044cd14b8.PNG)

![aynen](https://user-images.githubusercontent.com/99764271/167298053-e1859203-2a0b-47b4-aeeb-0c9145ae55b1.PNG)

çalışan bir containera **exec** komutu ile bağlanılır. **sh** ile containerın shelline bağlanılır. **-it** (--interactive -tty)ise terminal bağlantısı kurmaya yarar.
yapılan işlemlerle de adres refresh edildiğinde eklenen veri görülmektedir.


### Notlar
- Docker imagede birden fazla uygulama olabilir fakat container başlatıldığında otomatik çalışması içn tek bir uygulamannın çalışmasına izin verir.
- varsayılan uygulama çalıştıktan diğer çok sayıda uygulama çalıştırılabilir.
- ek komut kullanmadan direkt olarak varsayılan uygulama çalışır fakat ek komutlar sayesinde başka uygulamalar da başlangıçta çalıştırılabilir.
- Docker CLI : Engine ile REST API üzerinden haberleşir.
- Docker Deamon : Esas işi yapan, container yaratıp çalıştırmayı sağlayan ana uygulamadır.

### Docker Katmanlı Dosya Sistemi
- Docker depolama altyapısında unionfile system yapısını kullanır.
- Docker imajı --> örneğin bir temel linux işletim sisteminin alınıp üstüne yapılan her değişikliğin dosya katmanı olarak tutulup sonunda bu katmanların
tek bir bütün gibi çalışmasının sağlandığı bir docker dosyasıdır.
- Imageden container yaratılmak istenirse R/O layers yani sadece okunabilir halde tutulup bunun üstüne R/W katmanı eklenir ve değişiklikler bu katman üstünde yapılır.

## Volume
- Veri kaybını engellemek ve verileri depolamak ve erişilebilir yapmak için dockerdaki çözüm **Volumedir**.
### Volume oluşturmak

![creating volume](https://user-images.githubusercontent.com/99764271/167298474-2c0288de-afc1-49fd-af6b-5d7fe091a627.PNG)

- Volume ile Container bağlantıları

![okanekledi](https://user-images.githubusercontent.com/99764271/167298619-97389692-efc3-4c37-9577-933f98016135.PNG)

- Volume,boş ve dolu olmasına göre Containerda kullanılması

![boş dolu volume](https://user-images.githubusercontent.com/99764271/167298658-ea0aa549-3830-4339-90b3-dd5a8d4bd965.PNG)

Eğer volume container içinde olmayan bir klasöre mount edilirse dosya oluşur ve oraya eklenir.
Eğer var olan bir klasöre mount edilirse,volume boşsa klasördeki dosyalar volume'e aktarılır. Eğer volume doluysa
volume içinde ne varsa klasörde de o dosyalar görülür.

## Bind Mounts Metodu
- Bind mounts host üstündeki bir dosya ya da klasörün containera mount edilmesini sağlar.
- Örneğin; containerda kaydedilen verilerin normal bilgisayardaki bir dosyaya mount edilerek o verilerin bilgisayara her defa çekmek zorunda kalmadan kaydedilebilir.
- Ya da güncellenen bir web sitesi son haline gelene kadar c diskinde bir dosyada tutulsun, bu klasörü web sunucu yazılımının oldugu containerın, bu sunucuya bakan dosyasına mount edilmesi ile her güncellemede ayrı bir container üretmektek zorunda kalmamayı sağlayabilir.
- Fakat production ortamda kullanılması sakıncalıdır. Çünkü direkt host üzerindeki adres kullanıldığından farklı bir host kullanıldığında sıkıntı çıkarır.

### nginx web sunucu yazılımından container yaratılması

![nginx](https://user-images.githubusercontent.com/99764271/167483390-d30e3c97-a1ec-4e7c-a32b-fe71e7b4a762.PNG)

![nginx browser](https://user-images.githubusercontent.com/99764271/167484063-b2f565aa-6ac3-47d3-aac3-bd1d595e7793.PNG)

### Disk c üzerindeki dosyanın containera mount edilmesi --> bind mount

![nginx2](https://user-images.githubusercontent.com/99764271/167484317-202133d1-e006-4b7e-b46a-219333f9e089.PNG)

![merhaba](https://user-images.githubusercontent.com/99764271/167484575-2d9bd15e-1d72-442c-bc4e-238574d76373.PNG)

### diskteki dosyada değişiklik yapılması halinde containerda da değişiklik olması (bind mount)

![okancode](https://user-images.githubusercontent.com/99764271/167484607-fd13ef16-34f0-4d36-8253-e76d3fdad173.PNG)

![okanbrowser](https://user-images.githubusercontent.com/99764271/167484622-817afb1b-f52e-4894-810b-2c993ae31a7a.PNG)

## Alıştırma 1 Soruları
1: Öncelikle sistemdeki tüm container, image ve volumeleri görelim. Bunun için ayrı ayrı listeleme komutlarını girelim. Ve ardından temizlik yapmak adına makinenizdeki tüm containerları, imageleri ve volumeleri temizleyelim. Bunun iki yöntemi var. Bakalım siz kolay olanı mı seçeceksiniz. 

2: centos, alpine, nginx, httpd:alpine, ozgurozturknet/adanzyedocker, ozgurozturknet/hello-app, ozgurozturknet/app1 isimli imajları çalıştığımız sisteme çekelim. 

3: ozgurozturknet/app1 isimli imajdan bir container yaratalım.

4: httpd:alpine isimli imajdan detached bir container yaratalım. Yarattığımız container ismini ve id’sini görelim. 

5: Yarattığımız bu contaier’ın loglarına bakalım.

6: Container’ı durduralım, ardından yeniden çalıştıralım ve son olarak container’ı sistemden kaldıralım. 

7: ozgurozturknet/adanzyedocker isimli imajdan websunucu adında detached ve “-p 80:80” ile portu publish edilmiş bir container yaratalım. Kendi bilgisayarımızın browserından bu web sitesine erişelim.

8: websunucu adlı bu container’ın içerisine bağlanalım. /usr/local/apache2/htdocs klasörünün altına geçelim ve echo “denemedir” >> index.html komutuyla buradaki dosyaya denemedir yazısını ekleyelim. Web tarayıcıya geçerek dosyaya ekleme yapabildiğimizi görmek için refresh edelim. Sonrasında container içerisinden exit ile çıkalım.

9: websunucu isimli container’ı çalışırken silelim.

10: alpine isimli imajdan bir container yaratalım. Ama varsayılan olarak çalışması gereken uygulama yerine “ls” uygulamasının çalışmasını sağlayalım.

11: “alistirma1” isimli bir volüme yaratalım. 

12: alpine isimli imajdan “birinci” isimli bir container yaratalım. Bu container’ı interactive modda yaratalım ve bağlanabilelim. Aynı zamanda “alistirma1” isimli volume’u bu containerın “/test” isimli folder’ına mount edelim. Bu folder içerisine geçelim ve “touch abc.txt” komutuyla bir dosya yaratalım daha sonra “echo deneme >> abc.txt” komutuyla bu dosyanın içerisine yazı ekleyelim. 

13: alpine isimli imajdan “ikinci” isimli bir container yaratalım. Bu container’ı interactive modda yaratalım ve bağlanabilelim. Aynı zamanda “alistirma1” isimli volume’u bu containerın “/test” isimli folder’ına mount edelim. Bu folder içerisinde “ls” komutyla dosyaları listeleyelim ve abc.txt dosyası olduğunu görelim. “cat abc.txt” ile dosyanın içeriğini kontrol edelim. 

14: alpine isimli imajdan “ucuncu” isimli bir container yaratalım. Bu container’ı interactive modda yaratalım ve bağlanabilelim. Aynı zamanda “alistirma1” isimli volume’u bu containerın “/test” isimli folder’ına mount edelim fakat Read Only olarak mount edelim. Bu folder içerisine geçelim ve “touch abc1.txt” komutuyla bir dosya yaratmaya çalışalım. Ve yaratamadığımızı görelim.

15: Bilgisayarımızda bir klasör yaratalım “örneğin c:\deneme” ve bu klasörün içerisinde index.html adlı bir dosya yaratıp bu dosyanın içerisine birkaç yazı ekleyelim.

16: ozgurozturknet/adanzyedocker isimli imajdan websunucu1 adında detached ve “-p 80:80” ile portu publish edilmiş bir container yaratalım. Bilgisayarımızda yarattığımız klasörü container’ın içerisindeki /usr/local/apache2/htdocs klasörüne mount edelim. Web browser açarak 127.0.0.1’e gidelim ve sitemizi görelim. Daha sonra bilgisayarımızda yarattığımız klasörün içerisindeki index.html dosyasını edit edelim ve yeni yazılar ekleyelim. Web sayfasını refresh ederek bunların geldiğini görelim.

17: Tüm çalışan container’ları silelim. 

## Alıştırma 1 Cevapları
- 1

![-1](https://user-images.githubusercontent.com/99764271/167493694-dd654397-38b1-426c-95e9-965e064b4c09.PNG)

-2

![-2](https://user-images.githubusercontent.com/99764271/167493707-8e905d26-29e0-4380-bb4e-bfedc977dc3e.PNG)

-3

![-3](https://user-images.githubusercontent.com/99764271/167493725-ccf6ebe7-8fe9-4009-b0db-7d3e5f72525b.PNG)

-4

![-4](https://user-images.githubusercontent.com/99764271/167493749-26040b26-ec1c-4cf7-bb2e-3a32ba82e3e4.PNG)

-5

![-5](https://user-images.githubusercontent.com/99764271/167493753-f9e3a135-5534-4af4-a597-896af28b9094.PNG)

-6

![-6](https://user-images.githubusercontent.com/99764271/167493755-94d1b38f-dff5-4bbc-89d7-61a77957393b.PNG)

-7

![-7](https://user-images.githubusercontent.com/99764271/167493758-b1bc575e-cd71-41b9-86cf-fca69a8a266e.PNG)

![-7,1](https://user-images.githubusercontent.com/99764271/167493756-9894ea9e-472a-4c25-919d-7a1371a496c9.PNG)

-8

![-8](https://user-images.githubusercontent.com/99764271/167493760-eb02a8c8-b9a7-47d5-9e5b-e4f1a445beba.PNG)

![-8,1](https://user-images.githubusercontent.com/99764271/167493759-bbad406b-7a07-40e5-8baa-61cfad6aa9fe.PNG)

-9

![-9](https://user-images.githubusercontent.com/99764271/167493763-e8c3e76d-3a0f-4bea-9f8b-ef2459f9ffbd.PNG)

-10

![-10](https://user-images.githubusercontent.com/99764271/167493743-949ca477-7079-430c-b69f-40ad7545f630.PNG)

-11

![-11](https://user-images.githubusercontent.com/99764271/167493777-ee2a3ba0-0c01-4d66-a429-b7a48749c718.PNG)

- 12
- 13
- 14
- 15
- 16
- 17
