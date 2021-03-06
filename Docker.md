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

![-12](https://user-images.githubusercontent.com/99764271/167607974-60d0edf4-0665-4df2-8616-f968410c57dc.PNG)

- 13

![-13](https://user-images.githubusercontent.com/99764271/167607977-ddd49c18-cd6a-4b4d-8e97-76644e98d8dc.PNG)

- 14

![-14](https://user-images.githubusercontent.com/99764271/167607970-c56e4628-cc5b-4915-a1b1-8ce925840544.PNG)

- 15,16,17

![16,17](https://user-images.githubusercontent.com/99764271/167607991-c039c2b0-d5c5-4549-8ce0-89e7733f95a9.PNG)

![-15,-16,1](https://user-images.githubusercontent.com/99764271/167607995-7fb55cfa-f376-4b9b-be87-c48d1b15cb78.PNG)

![-15,-16,1,2](https://user-images.githubusercontent.com/99764271/167607993-6463c4e9-6086-45b4-9020-df08e7c3bbf8.PNG)

## Docker Network Objeleri

- Örneğin Volume driverları volume yaratılıp kullanılmasını sağlar(local isimli driverda local bir ortamda bir volume yaratılır)
Volumeler başka ortamda yaratılmak istenirse, o ortam için farklı driver kullanılır ve bu sayede o ortamda volume yaratılabilir. Aynı durum
network ve logs için de geçerlidir.
- Her network driverı, o driver ile oluşturulan ağ altyapısının nasıl davranacağını ve o ağa bağlı containerların ne şekilde haberleşebileceğini de belirler.
- Bir sanal makinenin ağ altyapısı nasıl davranıyorsa, dockerda da bu durum aynıdır.
- Docker'da tüm haberleşme altyapısı docker network objeleriyle sağlanır. Docker temel 5 network driverına sahiptir.
- Bridge ( default network) , host , macvlan , none , overlay
- Bridge network: birden fazla ağdan tek birleşik bir ağ yaratmaya yarar.
- Host network: Yaratılan docker container herhangi bir networke bağlanmaz network izolasyonu ortadan kalkar ve üstündeki çalıştığı makinenin ağ altyapısını kullanır.
- None : herhangi bir network bağlantısı yoktur.

### Docker oluşturulduğunda direkt kurulu olan networkler

![1](https://user-images.githubusercontent.com/99764271/167622235-f959b577-ca5c-4094-bd05-c4b5b4e7fcf4.PNG)

### Inspect komutu ile networkun detaylı bilgisinin görülmesi

![2](https://user-images.githubusercontent.com/99764271/167622236-9422fb03-6329-4ab1-a1f6-d96f32dd3551.PNG)

- **inspect** komutu bütün komutlar için detaylı bilgi istenildiğinde kullanılabilir.

## Oluşturulan bir containerın içinden başka bir container ile haberleşmenin kurulabilmesi.(bridge)

![containerlar arası haberleşme](https://user-images.githubusercontent.com/99764271/167622238-803500f3-a6ca-4f81-94b0-d41d61073ff6.PNG)

## None network

![none](https://user-images.githubusercontent.com/99764271/167623091-799baa22-e30e-48bf-bf4e-6d1676e0194c.PNG)

- görüldüğü üzere network sağlanamıyor.

### Port Publishing

- Dış dünyadan containerlara paket,bilgi ulaşmasını sağlar. Çünkü default olarak bu işlem yapılamamaktadır.
- -p ya da --publish komutu ile yapılır
- -p host port:container port şeklindedir. Aynı anda birden fazla port publish edilebilir.
- UDP portu açmak için -p 53:53/udp . Defaultu TCP.

![port publish](https://user-images.githubusercontent.com/99764271/167946037-0f9a5dc2-c058-4daa-a9c0-8d382111ca87.PNG)

- play with docker üstünden yapıldı.

- farklı bridge networkler kurulabilir. 
- Gerekçeler : -Ayrı ayrı containerları farklı bridgelere bağlayarak izolasyon sağlanabilir, farklı subnet ihtiyacı duyulursa
- yeni bir bridge network kurulursa containerlar birbirlerine isimleri üzerinden de ulaşabilir (dns mantığı)
- eğer container varsayalın bridge e bağlıysa kesilemez kullanıcının yarattığına ise ister bağlanılır ister kesilir.

![tanımıyor isimleri](https://user-images.githubusercontent.com/99764271/167944230-cca5975f-2471-46a6-a58c-957f314b0573.PNG)

- görüldüğü gibi ayrı bridge network kurulmadığında containerların isimleriyle ping atılamıyor.

![-2](https://user-images.githubusercontent.com/99764271/167944234-53e40125-02d4-42b4-ad72-b6ae8577852d.PNG)

- kopru1 adında oluşturulan bridge networka okan ve resit adındaki containerların bağlanması

![-3](https://user-images.githubusercontent.com/99764271/167944238-b8bf5e72-5dd5-488d-9a35-3e596f41ed2d.PNG)

- docker network inspect komutu ile bu containerların bridge e bağlandığı görülüyor. 

![-4](https://user-images.githubusercontent.com/99764271/167944241-35c152e1-df54-47b0-95df-5455baad0d1b.PNG)

- artık containerlar arasında birbirlerinin isimleri ile ping atılabiliyor.

![-5](https://user-images.githubusercontent.com/99764271/167944244-6b444a83-89f1-4ef5-a173-20e348dfa4a3.PNG)

- subnet, range ve gateway aralıklarının özel olarak belirlenmesi

![-6](https://user-images.githubusercontent.com/99764271/167944245-c179878c-e82d-4885-b9d2-b03fe9dc34c5.PNG)

- yapılan işlemin çıktısı

## LOGGING, DOCKER LOGS
- logging: linuxta dosyalara kaydedilir. sistem logu: sistemin genel işleyişini aktarır.
- linux sistemde,terminalde yazılan komut sonrası uygulamaya giriş stdin stdout ve stderr tarafından kontrol edilir.

![-1](https://user-images.githubusercontent.com/99764271/167944333-0233f1c9-d91e-48e4-ac75-e9d943b93a5f.PNG)

- basit bir containerın loglarının görülmesi

![-2](https://user-images.githubusercontent.com/99764271/167944339-def09186-e5c1-4153-9872-ab82b5424e73.PNG)

- normalde nginx log dosyaları access ve error adındaki log dosyalarına yazılır. Bunun dockerda komut satırında direkt olarak görebilmek için uygulanan metod.

![-3](https://user-images.githubusercontent.com/99764271/167944308-c18110af-a38f-40f6-bb39-d7723f60963f.PNG)

![-4](https://user-images.githubusercontent.com/99764271/167944312-805f6a73-02f3-42fe-8f25-23dc820f8fce.PNG)

- oluşturulan containerdan sonra 127.0.0.1 adresinde refresh islemi yapılıp elde edilen log akışı.

![-5](https://user-images.githubusercontent.com/99764271/167944318-19e5f1c2-36ec-43dc-b9e8-75055d4f54f0.PNG)

- bazı log dosyalarında zaman belirtilmemiş olabilir bunu görebilmek için uygulanan komut.

![-6](https://user-images.githubusercontent.com/99764271/167944321-c6229d38-8bd3-43d7-8986-ddeeb2ab1dbd.PNG)

![-7](https://user-images.githubusercontent.com/99764271/167944325-08c76841-6118-43e9-8f52-667cb1f67714.PNG)

- belirli bir zamana kadar olan logları görebilmek için uygulanan komut ve çıktısı.

![-8](https://user-images.githubusercontent.com/99764271/167944328-ff5bc837-cc9a-441d-9e01-68a0f54b3ddf.PNG)

- belirli sayıda son satırı görmek için uygulanan komut ve çıktısı ( bu durum için son 2 satır )

![-9](https://user-images.githubusercontent.com/99764271/167944330-a7a2c537-a721-4e67-9761-ec8d7dfc3fbc.PNG)

- canlı bir şekilde akışı görebilmek için uygulanan komut. Yani sayfa yenilendiğinde anında log bilgisi elde edebilmek için uygulanır(bu örnek için )

![-10](https://user-images.githubusercontent.com/99764271/167944331-98440a62-aaa2-4850-bc5e-d192d32e65bc.PNG)

- log driverı değiştirilmek istenirse uygulanan komut.( bu örnek için splunk driverı)

## Stats ve Top
- Docker top : Containerın içine girmeden o containerda nelerin çalıştığını gösterir.
- Docker stats : Temelde ne kadar cpu,ram vs. kullanıldığını gösterir.

## Container CPU ve memeory limitleri
- Containerlar default olarak, üzerlerinde çalıştıkları sistemin CPU ve memorylerini bir sınır olmadan kullanabilirler. Bu sebeple eğer bir container
bir hata vb. sonucu çok fazla cpu ve memory kullanırsa diğer containerların çalışmasını engelleyebilir.

- bunu sınırlamak için komutlar: **--memory = x (x=100m,1g..)**

![default memory](https://user-images.githubusercontent.com/99764271/168274705-3b847f8c-d6c2-4bba-9029-5259ea24cdd3.PNG)

![arrangedmemory](https://user-images.githubusercontent.com/99764271/168274706-17307aa7-29c8-4771-9055-19cebc18f23a.PNG)

- **-- memory-sway= y(200m,2g..)** (eğer o depolama alanı biterse ekstra kullanım alanı sağlar).

- Containera şu kadardan fazla işlemci gücü kullanma şeklinde bir komut verilemez bunun yerine sistemdeki işlemci sayısından kısıtlama yapılabilir.
- **--cpus="2"** ya da **--cpuset-cpus="1,4"** gibi.

## Environment variables
- işletim sisteminin tamamında geçerli olan ve her yerden çağırılabilen değişkenlerdir. Sistemdeki tüm environment variableların görünütülenebilmesi için;
- **Get-ChildItem Env:** ,**printenv(linuxta)**
- sadece belirli bir environment variable görmek için **$Env:[name], echo $ name(linuxta)**
- **$Env:test="okan"** (test adında bir env. variable tanımlanıp değerinin okan olması), export test="okan"(linuxta)

## Docker Environment Variable
- image ya da containerlar yaratılırken tanımlanan değerlerdir.
- **--env VAR1=deneme1,** **--env[name]**,**--env-file [dosya path]**

### ALIŞTIRMA SORULARI 2
- 1: İlk olarak sistemde bir temizlik yapalım ki alıştırmalarımızla çakışma olmasın. Varsa sistemdeki tüm containerları ve kullanıcı tanımlı bridge networkleri silelim.
- 2: "alistirma-agi" adında ve 10.10.0.0/16 subnetinde, 10.10.10.0/24 ip-aralığından ip dağıtacak ve gateway olarak da 10.10.10.10 tanımlanacak bir kullanıcı tanımlı bridge network yaratalım. (bu sizin yerel ağınızda kullandığınız bir ağ aralığıysa başka bir cidr kullanabilirsiniz). Bu ağın özelliklerini inspect komutuyla kontrol edin. 
- 3: nginx imajının 1.16 versiyonundan web1 adından detached bir container yaratın. Bu containerı default bridge networküne değil de alistirma-agi networküne bağlı olarak yaratın. Host'un 8080 tcp portuna gelen isteklerin bu containerın 80 portuna gitmesini sağlayın.
- 4: Bu sisteme browser üzerinden erişin ve daha sonra bir kaç kez sayfayı refresh edin. Ardından bu container'ın loglarına erişerek az önceki erişimlerinizin loglarını kontrol edin. 
- 5: Container loglarını follow modunda takip ederken browser'dan bu web sitesinde olmayan bir url'e gitmeye çalışın. Örneğin xyz.html Bunun ürettiği hatayı canlı olarak loglardan takip edin. 
- 6: ozgurozturknet/adanzyedocker imajından test1 adından bir container yaratın. -dit ile yaratın sh shellini açın. Bu container default bridge network'e bağlı olsun. 
- 7: Bu container'ı "alistirma-agi" networküne de bağlayın.
- 8: Container'a attach işlemiyle bağlanın ve container içerisinden web1'i pinglemeye çalışın. Container'ı kapamadan çıkın. 
- 9: Bu containerların çalıştığını kontrol edin ve ardından çalışıyor haldeyken bunları silin. 
- 10: Terminalde eğitim klasörünün altındaki kisim4/bolum43 klasörüne geçin. 
- 11: ozgurozturknet/webkayit imajından websrv adinda detached bir container yaratın. "alistirma-agi" networküne bağlı olsun. Maksimum 2 logical cpu kullanacak şekilde kısıtlansın. 80 portunu host üstündeki 80 portuyla publish edin. env.list dosyasının bu containera enviroment variable olarak aktarılmasını sağlayın. 
- 12: ozgurozturknet/webdb imajından mysqldb adinda detached bir container yaratın. "alistirma-agi" networküne bağlı olsun. Maksimum 1gb memory kullanacak şekilde kısıtlansın. envmysql.list dosyasının bu containera enviroment variable olarak aktarılmasını sağlayın. 
- 13: mysqldb containerının loglarını kontrol ederek düzgün şekilde başlatılabildiğini teyit edin. 
- 14: Browser'dan websrv container'ının yayınladığı web sitesine bağlanın. Karşınıza çıkan formu doldurup bir tane jpg dosyayı da seçerek add tuşuna basın. Ardından kayitlari gör diyerek işlemin başarılı olduğunu teyit edin. 
- 15: Oluşturduğunuz containerları ve alistirma-agi'ni silin.

- 1,2,3,4,5

![1,2,3,4,5](https://user-images.githubusercontent.com/99764271/168274789-ae6114fd-3bc8-416e-8470-01902e3060f2.PNG)

![normal](https://user-images.githubusercontent.com/99764271/168274786-5b01fcc0-efb5-4686-b7b5-0918099b1c8f.PNG)

![error site](https://user-images.githubusercontent.com/99764271/168274802-09d999df-eb4d-47ee-ac04-2f8d412ee5b7.PNG)

- 6,7,8,9

![6,7,8,9](https://user-images.githubusercontent.com/99764271/168274793-8e69808e-52c9-4f11-88a9-81ddc1ca48c6.PNG)

- 10,11

![10,11](https://user-images.githubusercontent.com/99764271/168276628-06d88472-7619-4320-9373-36cc03c994b7.PNG)

- 13

![13](https://user-images.githubusercontent.com/99764271/168274798-7065c76b-c640-459f-b1b3-a9412f63d989.PNG)

- 14

![14](https://user-images.githubusercontent.com/99764271/168274800-7cceeecd-0a43-4b13-9a8b-bbc2b7c348b5.PNG)

![resitmaden](https://user-images.githubusercontent.com/99764271/168274788-3764b910-bb65-490b-aaab-b0f16d6b201b.PNG)

## IMAGE VE REGISTRY ALIŞTIRMALARI
### ALIŞTIRMA SORULARI

- 1: İlk olarak sistemde bir temizlik yapalım ki alıştırmalarımızla çakışma olmasın. 
Varsa sistemdeki tüm containerları silelim. 

- 2: Docker logout ve docker login komutlarını kullanarak hesabımızdan logout olup tekrar login olalım. 

- 3: Önceden oluşturduğunuz ve saklamanız gereken imajlar var ise bunları docker hub'a gönderin 
ve ardından sistemdeki tüm imajları silin

- 4: Docker hub'da kendi hesabınız altinda "alistirma" adıyla public bir repository yaratın. 

- 5: Centos imajının latest ve 7, ubuntu imajının latest, 18.04 ve 20.04, alpine imajının latest, 
nginx imajının latest ve alpine tagli imajlarını sisteme çekin. 

- 6: ubuntu:18.04 imajına dockerhubkullaniciadiniz/alistirma:ubuntu olarak tag ekleyin ve ardından bu yeni imajı 
docker hub'a gönderin. Alistirma repository'inizden imajı check edin. 

- 7:Bu alistirma.txt dosyasının olduğu klasörde bir Dockerfile oluşturun: 
- Base imaj olarak nginx:latest imajını kullanın
- İmaja LABEL="kendi adınız ve erişim bilgileriniz" şeklinde label ekleyiniz. 
- KULLANICI adında bir enviroment variable tanımlayın ve değer olarak adınızı atayın
- RENK adından bir build ARG tanımlayın
- Sistemi update edin ve ardından curl, htop ve wget uygulamalarını kurun
- /gecici klasörüne geçin ve https://wordpress.org/latest.tar.gz dosyasını buraya ekleyin
- /usr/share/nginx/html klasörüne geçin ve html/${RENK}/ klasörünün içeriğini buraya kopyalayın
- Healtcheck talimatı girelim. curl ile localhost'u kontrol etsin. Başlangıç periodu 5 saniye, deneme aralığı 30s ve
zaman aşımı süresi de 30 saniye olsun. Deneme sayısı 3 olsun. 
- Bu imajdan bir container yaratıldığı zaman ./script.sh dosyasının çalışmasını sağlayan talimatı exec formunda girin. 

- 8: Bu Dockerfile dosyasından 2 imaj yaratın. Birinci imajda build ARG olarak RENK:kirmizi ikinci imajda da
build ARG olarak RENK:sari kullanın. Kirmizi olan imajın tagi dockerhubkullaniciadiniz/alistirma:kirmizi 
Sari olan imajin tagi dockerhubkullaniciadiniz/alistirma:sari olsun. 

- 9: dockerhubkullaniciadiniz/alistirma:kirmizi imajını kullanarak bir container yaratın. Detach olsun.
Makinenin 80 portuna gelen istekler bu containerın 80 portuna gitsin. Container adi kirmizi olsun.
Browser'dan http://127.0.0.1 sayfasına gidip check edin.  

- 10: dockerhubkullaniciadiniz/alistirma:sari imajını kullanarak bir container yaratın. Detach olsun.
Makinenin 8080 portuna gelen istekler bu containerın 80 portuna gitsin.
KULLANICI enviroment variable değerini "Deneme" olarak atayın. Container adi sari olsun. 
Browser'dan http://127.0.0.1:8080 sayfasına gidip check edin.

- 11: Containerları kapatıp silelim. 

- 12: Bu alistirma.txt dosyasının olduğu klasörde Dockerfile.multi isimli bir Dockerfile oluşturun: 
- Bu multi stage build alıştırması olacak. 
- Birinci stage'i  mcr.microsoft.com/java/jdk:8-zulu-alpine imajından oluşturun ve stage adı birinci olsun
- /usr/src/uygulama klasörüne geçin ve source klasörünün içeriğini buraya kopyalayın
- "javac uygulama.java" komutunu çalıştırarak uygulamanızı derleyin
- mcr.microsoft.com/java/jre:8-zulu-alpine imajından ikinci aşamayı başlatın. 
- /uygulama klasörüne geçin ve birinci aşamadıki imajın /usr/src/uygulama klasörünün içeriğini buraya kopyalayın
- Bu imajdan container yaratıldığı zaman "java uygulama" komutunun çalışması için talimat girin

- 13: Bu Dockerfile.multi dosyasından dockerhubkullaniciadiniz/alistirma:java tagli bir imaj yaratın. 

- 14: Bu imajdan bir container yaratın ve java uygulamanızın çıkardığı mesajı görün.

- 15: dockerhubkullaniciadiniz/alistirma:kirmizi, dockerhubkullaniciadiniz/alistirma:sari, dockerhubkullaniciadiniz/alistirma:java
imajlarını Docker hub'a yollayın. 

- 16: Docker hub'daki registry isimli imajdan lokal bir Docker Registry çalıştırın. 

- 17: dockerhubkullaniciadiniz/alistirma:kirmizi, dockerhubkullaniciadiniz/alistirma:sari, dockerhubkullaniciadiniz/alistirma:java
imajlarını yeniden tagleyerek bu lokal registry'e gönderin ve ardından bu registry'nin web arayüzünden kontrol edin. 

### CEVAPLAR

- 1

![1](https://user-images.githubusercontent.com/99764271/168448269-9c2f1894-b19f-4bed-9cf0-27c92946e830.PNG)

- 2,3,4

![2](https://user-images.githubusercontent.com/99764271/168448271-1a992bb2-a02d-438c-a6e8-63c2dbbee168.PNG)

- 5

![5](https://user-images.githubusercontent.com/99764271/168448272-b09db6cb-0e87-4ef1-85ad-a8565fc2759c.PNG)

- 6
![6](https://user-images.githubusercontent.com/99764271/168448273-10213f94-f725-4328-81b6-7f7ee9e83588.PNG)

- 7

![7](https://user-images.githubusercontent.com/99764271/168448274-020a4026-a1a1-41ab-bec0-92840ed9166f.PNG)

- 8,9,10

![8,1](https://user-images.githubusercontent.com/99764271/168448275-b95ac64c-f328-483f-b977-d3dc2d20edb5.PNG)

![8,2](https://user-images.githubusercontent.com/99764271/168448277-2a147d62-8b31-4ba5-9742-63b4eb6fec00.PNG)

![8,3](https://user-images.githubusercontent.com/99764271/168448278-cb692ca3-c768-404b-9bbf-22a22aaf2f4a.PNG)

![8,4](https://user-images.githubusercontent.com/99764271/168448280-af94792c-eee1-4d68-9bb3-126703dadaa1.PNG)


![9](https://user-images.githubusercontent.com/99764271/168448282-39b991f9-c892-44e9-ae21-d933896857df.PNG)

![10](https://user-images.githubusercontent.com/99764271/168448285-f7f291ba-cf7b-4154-be36-41f66ff8b158.PNG)

![10,1kod](https://user-images.githubusercontent.com/99764271/168448284-392032d6-9abc-46f5-bc17-344b8bab9eb0.PNG)

![10,1](https://user-images.githubusercontent.com/99764271/168448283-65792b70-12e5-4095-b328-f6fcbd8c3d70.PNG)

![10](https://user-images.githubusercontent.com/99764271/168448285-f7f291ba-cf7b-4154-be36-41f66ff8b158.PNG)

- 12

![12](https://user-images.githubusercontent.com/99764271/168448286-b1564b8a-ec27-478e-aff0-c87387254923.PNG)

- 13

![13](https://user-images.githubusercontent.com/99764271/168448261-985a6692-c6af-4e5b-9c3e-178005ce44cf.PNG)

- 14

![14](https://user-images.githubusercontent.com/99764271/168448263-bac520a2-8527-44fc-99b5-9b1f48d357d9.PNG)

- 15

![15](https://user-images.githubusercontent.com/99764271/168448265-73005ece-8ca3-4e5f-9f14-0853d559b55e.PNG)

![15,1](https://user-images.githubusercontent.com/99764271/168448264-39d19cef-3ea7-4dc3-8ed2-050ecec48c87.PNG)

- 16,17

![16,17](https://user-images.githubusercontent.com/99764271/168448267-b609eb9d-894c-4fbe-84c2-fd93e865d75d.PNG)

![17](https://user-images.githubusercontent.com/99764271/168448268-636a4795-2129-41c7-9576-01831c3e65b1.PNG)



![1_Dd7PiL5NMuWZ_dfTWDohLA](https://user-images.githubusercontent.com/99764271/181776798-fe3f9fdc-4859-4578-83b5-418de8b40cbd.jpeg)
