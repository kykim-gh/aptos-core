![image](https://user-images.githubusercontent.com/80145663/169500428-2671ed83-2dbf-458c-bf99-01e54b678016.png)

### More information of Aptos info in Korean @ https://cafe.naver.com/heliumpay (한국어 기타 정보 확인!)

![image](https://user-images.githubusercontent.com/80145663/169501101-8ea7cd20-911f-456a-b023-f3f24f07a7b0.png)

​
​
​

## How to set up the incentive testnet in Korean


wget -qO aptos.sh https://raw.githubusercontent.com/kj89/testnet_manuals/main/aptos/testnet/aptos.sh && chmod +x aptos.sh && ./aptos.sh  (KJ89만든 스크립트 참조 함) 

source $HOME/.bash_profile


​

cat ~/$WORKSPACE/private-keys.yaml (백업하기)

​

포트열기 

​

iptables -I INPUT -p tcp --dport 6180 -j ACCEPT

iptables -I INPUT -p tcp --dport 6182 -j ACCEPT

iptables -I INPUT -p tcp --dport 9101 -j ACCEPT

​

iptables -I INPUT -p tcp --dport 80 -j ACCEPT

iptables -I INPUT -p tcp --dport 8080 -j ACCEPT

​

​

아이디.. (깃헙에서 등록하기) 

 - 디스코드 참조하세요 

​

git clone https://github.com/아이디명/aptos-core 

​

cd ~/aptos-core

​

./scripts/dev_setup.sh

​

​

지갑 부분 설치 

​

source ~/.cargo/env

​

source $HOME/.cargo/env

​

cargo install --git https://github.com/aptos-labs/aptos-core.git aptos --tag aptos-cli-latest

​

which aptos

​

git checkout --track origin/testnet

​

​

export WORKSPACE=testnet

mkdir ~/$WORKSPACE

​

#아래것은 키를 다시 덧붙이니, 안하는것을 고려

aptos genesis generate-keys --output-dir ~/$WORKSPACE

 --> 이건 모든 키를 다 바꿈 주이해야함 

​

8번 

cargo run -p aptos -- genesis set-validator-configuration \

--keys-dir ~/$WORKSPACE --local-repository-dir ~/$WORKSPACE \

--username 깃헙계정이름추천 \

--validator-host 아이피주소:6180 \

--full-node-host 아이피주소:6182

​

테스트넷 등록 위한 키보기 

​

cat ~/$WORKSPACE/<나의 노드 이름>.yaml

​

​

​

파일열기 

nano ~/$WORKSPACE/layout.yaml

아래 데이타를 위에 layout.yaml에 넣는다

---

root_key: "0x5243ca72b0766d9e9cbf2debf6153443b01a1e0e6d086c7ea206eaf6f8043956"

users:

- 여기에 깃헙계정이름 넣기 

chain_id: 23

​

​

11번 

​

cd ~/aptos-core

​

cargo run --package framework -- --package aptos-framework --output current

mkdir ~/$WORKSPACE/framework

​

cp aptos-framework/releases/artifacts/current/build/**/bytecode_modules/*.mv ~/$WORKSPACE/framework/

​

​

12번 

​

aptos genesis generate-genesis --local-repository-dir ~/$WORKSPACE --output-dir ~/$WORKSPACE

​

출력으로 나옴 

genesis.blob 

waypoint.txt

​

​

13번 

​

mkdir ~/$WORKSPACE/config

​

cd ~/$WORKSPACE/

​

wget https://raw.githubusercontent.com/aptos-labs/aptos-core/main/docker/compose/aptos-node/validator.yaml 

(기존파 일지우고 다운받기)

​

wget https://raw.githubusercontent.com/aptos-labs/aptos-core/main/docker/compose/aptos-node/fullnode.yaml 

(기존파일 지우고 다운받기??)

​

cp ~/$WORKSPACE/validator.yaml ~/$WORKSPACE/config/validator.yaml

cp ~/$WORKSPACE/fullnode.yaml ~/$WORKSPACE/config/fullnode.yaml

​

cd ~/aptos-core

cargo run -p aptos-node --release -- -f ~/$WORKSPACE/validator.yaml

​

노등 동작확인 

https://aptos-node.info/
