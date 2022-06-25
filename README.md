# HyperLedgerTestNetworkTurkish
 in this repository,  Hyperledger Test Network  will be practised as Turkish 
Test Network uygulamasına geçmeden önce temel gereksinimlerin yüklenmesi gerekmektedir. 

## Ön yüklemeler

İlk olarak Docker ve Docker Compose yapılarının yüklenmesi gerekmektedir. Bunun için terminala aşağıdaki komutu giriniz.

`sudo apt -y install docker.io`
`sudo curl -L "https://github.com/docker/compose/releases/download/1.26.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
 sudo chmod +x /usr/local/bin/docker-compose`
 
 İkinci olarak Go derleyicisinin yüklemesi yapılır. 
 `sudo wget -c https://dl.google.com/go/go1.14.9.linux-amd64.tar.gz -O - | sudo tar -xz -C /usr/local`
 
 Şimdi Go uygulamasını evrensel ortam değişkeni olarak atamasının yapılması gerekmektedir.
 
 `echo 'export PATH="$PATH:/usr/local/go/bin:/root/fabric/fabric-samples/bin"' >> $HOME/.profile
  echo 'export GOPATH="$HOME/fabric"' >> $HOME/.profile
  source $HOME/.profile`
  
  Test için bu öncelikler yeterli olmakla birlikte istenirse node.js, nmp ve python yüklemelerinin de yapılması faydalı olabilecektir. 
  
  ## Hyperledger Fabric imaj ve örneklerini kurma
  
  Ön yüklemeler 
