# HyperLedgerTestNetworkTurkish
 in this repository,  Hyperledger Test Network  will be practised as Turkish 
 
 ![dd](https://user-images.githubusercontent.com/59863925/175784069-307fd2d6-6668-4be7-bb82-bb63128a5e0d.png)

 

Bu uygulamada, Ubuntu üzerinden  bir localhost sunucusunda Hyperledger Test Network'unu çalıştıracağız.
Test Network uygulamasına geçmeden önce temel gereksinimlerin yüklenmesi gerekmektedir. 

## Ön yüklemeler

İlk olarak Docker ve Docker Compose yapılarının yüklenmesi gerekmektedir. Bunun için terminala aşağıdaki komutu giriniz.

```
sudo apt -y install docker.io
```
```
sudo curl -L "https://github.com/docker/compose/releases/download/1.26.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```
 
 İkinci olarak Go derleyicisinin yüklemesi yapılır. 
 
 ```
 sudo wget -c https://dl.google.com/go/go1.14.9.linux-amd64.tar.gz -O - | sudo tar -xz -C /usr/local
 ```
 
 Şimdi Go uygulamasını evrensel ortam değişkeni olarak atamasının yapılması gerekmektedir.
 
 ```
  echo 'export PATH="$PATH:/usr/local/go/bin:/root/fabric/fabric-samples/bin"' >> $HOME/.profile
  echo 'export GOPATH="$HOME/fabric"' >> $HOME/.profile
  source $HOME/.profile
 ```
  
  Test için bu öncelikler yeterli olmakla birlikte istenirse node.js, nmp ve python yüklemelerinin de yapılması faydalı olabilecektir. 
  
  ## Hyperledger Fabric imaj ve örneklerini kurma
  
  Ön yüklemeler başarıyla bitirildikten sonra, imaj ve örnek dosyalarının indirilmesi gerekecektir. 
  
  ```
  curl -sSL http://bit.ly/2ysbOFE | bash -s
  ```
  
  İçinde testnetwork klasörünün de bulunduğu kaynaklar başarıyla yüklenecektir. 
  
    
  Buradan testnetwork klasörüne giriş yapılır. 
  
  ```
  cd fabric-samples/test-network$ 
  ```  
  
  ilk komutumuz içinde bir dizi komut ve uygulama emri bulunan ./network.sh  bash script dosyasını ayağa kaldırmak olacaktır. Bu dosyanın içinde bir networkun ayağa kalkması için gereken tüm düzenlemeler mevcuttur. 
  
  ```
  ./network.sh up
  ```
  
  Artık networkumuz ayağa kalkmış ve dockerlar aktifleşmiştir. aktif dockerları görmek için 
  
  ```
  docker ps -a
  ``` 
komutu işlenebilir. Artık belirli üyelerin kendi aralarında işlem yapacağı bir kanal oluşturma safhasına geçilir. İstenirse bir zincirde birden çok kanak oluşturulabilir.  Bir kanal, katılımcı üyeler, eşler, defter, akıllı sözleşmeler ve sipariş verenler tarafından oluşur. Hyperledger Fabric kurulumunda gelen ikili dosyalar `configtxgen` komutu kullanılmaktadır.Configtxgen komutu, kullanıcıların kanal yapılandırmasıyla ilgili ayarları oluşturmasına ve incelemesine olanak tanır. Hyperledger Fabric ağlarında her kanal konfigürasyonu bir genesis blokunun oluşturulması ile başlar.Kanal konfigürasyonları ve kanal politikaları oluşturulmuştur.Önce kanalın oluşturacak komut yazılır
  
  
  
  
  
