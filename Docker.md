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



