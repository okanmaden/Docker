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
- Help Komutu

