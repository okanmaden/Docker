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

- **docker rm $(docker ps -a -q)** komutu ile tüm durmuş containerlar kısayoldan silinir.
