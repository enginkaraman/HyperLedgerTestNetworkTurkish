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
  
  ilk komutumuz içinde bir dizi komut ve uygulama emri bulunan  ` ./network.sh ` bash script dosyasını ayağa kaldırmak olacaktır. Bu dosyanın içinde bir networkun ayağa kalkması için gereken tüm düzenlemeler mevcuttur. 
  
  ```
  ./network.sh up
  ```
  
  Artık networkumuz ayağa kalkmış ve dockerlar aktifleşmiştir. aktif dockerları görmek için 
  
  ```
  docker ps -a
  ``` 
komutu işlenebilir. Artık belirli üyelerin kendi aralarında işlem yapacağı bir kanal oluşturma safhasına geçilir. İstenirse bir zincirde birden çok kanak oluşturulabilir.  Bir kanal, katılımcı üyeler, eşler, defter, akıllı sözleşmeler ve sipariş verenler tarafından oluşur. Hyperledger Fabric kurulumunda gelen ikili dosyalar `configtxgen` komutu kullanılmaktadır.Bu komut, kullanıcıların kanal yapılandırmasıyla ilgili Kanal konfigürasyonları ve kanal politikaları oluşturulmasını sağlar. Hyperledger Fabric ağlarında her kanal konfigürasyonu bir genesis blokunun oluşturulması ile başlar. İlgili komut aşağıda verilmştir.  

```
./network.sh createChannel
```

Şimdi kanalda bulunan eşlere chaincode yani akıllı sözleşme atanacaktır. Eşler, ağın temel unsurlarıdır çünkü akıllı sözleşmeleri ve defterleri barındırırlar. ağa chaincode yüklemesini yapmak için bir geri klasördeki akıllı kontrat ve onun bağımlılıkları çağrılır.

```
./network.sh deployCC -ccn basic -ccp ../asset-transfer-basic/chaincode-go -ccl go
```


Bir eş, akıllı sözleşmeler ve defterlere erişim için bağlantı noktası olduğu için uygulamalar bu noktalara erişmek istiyorlarsa, bir eşle etkileşime girmelidirler. Organizasyonlara  kriptofik bir dijital kimliğe sahip olmasını  “Sertifika Otoritesi” sağlar. Ağda etkileşimde bulunmak isteyen her aktörün bir kimliğe ihtiyacı vardır.

```
export PATH=${PWD}/../bin:$PATH
export FABRIC_CFG_PATH=$PWD/../config/
export CORE_PEER_TLS_ENABLED=true
export CORE_PEER_LOCALMSPID="Org1MSP"
export CORE_PEER_TLS_ROOTCERT_FILE=${PWD}/organizations/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt
export CORE_PEER_MSPCONFIGPATH=${PWD}/organizations/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp
export CORE_PEER_ADDRESS=localhost:7051

```
Ağ içerisindeki bir organizasyonun geçerli kimliklerini yöneten ve kurallarını tanımlayan bileşen “Üyelik Servis Sağlayıcısı (MSP) ”dır. Org1 için kriptografik kimlik oluştu. Aynı şeyi ikinci bir organizasyon içinde oluşturabiliriz. 

```
export PATH=${PWD}/../bin:$PATH
export FABRIC_CFG_PATH=$PWD/../config/
export CORE_PEER_TLS_ENABLED=true
export CORE_PEER_LOCALMSPID="Org2MSP"
export CORE_PEER_TLS_ROOTCERT_FILE=${PWD}/organizations/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt
export CORE_PEER_MSPCONFIGPATH=${PWD}/organizations/peerOrganizations/org2.example.com/users/Admin@org2.example.com/msp
export CORE_PEER_ADDRESS=localhost:9051

```
Şu ana ağımızda iki organizasyon ve bir adet orderer mevcuttur. Bu iki organizasyon kanal içinde transaction gerçekleşririp kayıt oluşturabilecektir. Orderer (siparişciler) ise bu işlemleri kontrol edip  blok yaratırlar ve  akabinde  eşlere dağıtarak yayımlarlar.

transactionlara geçmeden önce chaincod daki varsayılan  değerleri (asset) aşağıdaki resimden görebilirsiniz.

![jhgg](https://user-images.githubusercontent.com/59863925/175786062-6c73565b-be04-46dd-8085-6a31ea4bb243.png)

Bu assetleri kanala yüklemek için organizasyonlardan biri aşağıdaki komutu girerek kayıt(invoke) oluşturur. 

```
peer chaincode invoke -o localhost:7050 --ordererTLSHostnameOverride orderer.example.com --tls --cafile ${PWD}/organizations/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem -C mychannel -n basic --peerAddresses localhost:7051 --tlsRootCertFiles ${PWD}/organizations/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt --peerAddresses localhost:9051 --tlsRootCertFiles ${PWD}/organizations/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt -c '{"function":"InitLedger","Args":[]}'
```
yüklenen bu bilgileri sorgulamak için query komutu kullanılır. 

```
peer chaincode query -C mychannel -n basic -c '{"Args":["GetAllAssets"]}'

```
Görüldüğü gibi invoke yeni bir değer girişini, query ise sorgulama işlemini sağlamaktadır. 

Chaincode daki fonksiyonlara göre bir çok transaction ve sorgulama işlemleri geliştirilebilinir. Mesala 6. assetin sahibinin ismini değiştiren bir çağrı oluşturabiliriz. Michel bu varlığını MemetSeref isimli bir vatandaşa devretmiş olsun. Bunun için gönderilecek çağrı şu şekilde oluşturulur. 

```
peer chaincode invoke -o localhost:7050 --ordererTLSHostnameOverride orderer.example.com --tls --cafile ${PWD}/organizations/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem -C mychannel -n basic --peerAddresses localhost:7051 --tlsRootCertFiles ${PWD}/organizations/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt --peerAddresses localhost:9051 --tlsRootCertFiles ${PWD}/organizations/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt -c '{"function":"TransferAsset","Args":["asset6","Acun"]}'
```
Hemen bu durumu sorgulayacak kot oluşturulur. 

```
peer chaincode query -C mychannel -n basic -c '{"Args":["ReadAsset","asset6"]}'
```

