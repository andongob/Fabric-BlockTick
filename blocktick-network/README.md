# Proyecto de de blockchain en HLF : Análisis Funcional y de Diseño (incluye puesta en producción)

\---

# Análisis Funcional del Chaincode para la Venta de Entradas de Cine en Blockchain

El presente documento describe el análisis funcional de un chaincode desarrollado en Go que se encargará de gestionar la venta de entradas de cine en una cadena de bloques (blockchain). El sistema se ha diseñado para administrar las ventas de entradas de cine en un teatro que tiene cuatro ventanillas de venta, cinco películas en exhibición en todo momento, y cada película se proyecta cuatro veces al día. Además, el sistema debe gestionar la entrega de una botella de agua y una bolsa de palomitas de maíz al comprador de una entrada, así como la posibilidad de que el comprador pueda cambiar el agua por Refresco en la cafetería. También se llevará un registro de las compras en la cadena de bloques.

## Requisitos Funcionales
|**1**|*Nombre Rol*|
| :- | :- |
|**Descripción**|Ventanillas de Venta|
|**Comentarios**|<p>*El sistema debe admitir la venta de entradas a través de cuatro ventanillas de venta en el teatro.* </p><p>*Para venta de entrada dentro de la plataforma*</p><p></p>|

|**2**|*Nombre Rol*|
| :- | :- |
|**Descripción**|Compra de Entradas|
|**Comentarios**|<p>*Los usuarios pueden comprar una o más entradas para las películas en exhibición.* </p><p>*Para venta de entrada dentro de la plataforma*</p><p></p>|

|**3**|*Nombre Rol*|
| :- | :- |
|**Descripción**|Límite de Entradas (**promoter**)|
|**Comentarios**|<p>*Una vez se vendan 100 entradas para una película en particular, esa función de cine se considerará llena, y no se podrán vender más entradas para esa función.* </p><p>*Para venta de entrada dentro de la plataforma*</p><p></p>|

|**4**|*Nombre Rol*|
| :- | :- |
|**Descripción**|Películas en Cartel|
|**Comentarios**|<p>*El cine tiene cinco películas en cartél en todo momento.* </p><p>*Para venta de entrada dentro de la plataforma*</p><p></p>|

|**5**|*Nombre Rol*|
| :- | :- |
|**Descripción**|Proyecciones Diarias|
|**Comentarios**|<p>*Cada película se proyecta cuatro veces al día en diferentes horarios.* </p><p>*Para venta de entrada dentro de la plataforma*</p><p></p>|

|**6**|*Nombre Rol*|
| :- | :- |
|**Descripción**|Regalo con la Compra de Entrada|
|**Comentarios**|<p>*Al comprar una entrada, el comprador recibirá automáticamente una botella de agua y una bolsa de palomitas de maíz en la Ventanilla-1.* </p><p>*Para venta de entrada dentro de la plataforma*</p><p></p>|

|**7**|*Nombre Rol*|
| :- | :- |
|**Descripción**|Impresión de Entrada y Recibo|
|**Comentarios**|<p>*Al finalizar la compra, se imprimirá un boleto de entrada y un recibo.* </p><p>*Para venta de entrada dentro de la plataforma*</p><p></p>|

|**8**|*Nombre Rol*|
| :- | :- |
|**Descripción**|Registro en la Cadena de Bloques|
|**Comentarios**|<p>*Todas las compras realizadas deben registrarse en la cadena de bloques para garantizar la transparencia y la inmutabilidad de la información.* </p><p>*Para venta de entrada dentro de la plataforma*</p><p></p>|

|**9**|*Nombre Rol*|
| :- | :- |
|**Descripción**|Intercambio de Agua por Soda|
|**Comentarios**|<p>*El comprador tiene la opción de cambiar su botella de agua por soda en la cafetería.* </p><p>*Para venta de entrada dentro de la plataforma*</p><p></p>|

|**10**|*Nombre Rol*|
| :- | :- |
|**Descripción**|Generación de Número Aleatorio|
|**Comentarios**|<p>*\- La Ventanilla-1 generará un número aleatorio.* </p><p>*Para venta de entrada dentro de la plataforma*</p><p></p>|

|**11**|*Nombre Rol*|
| :- | :- |
|**Descripción**|Intercambio de Agua por Soda en la Cafetería|
|**Comentarios**|<p>*Si el número aleatorio generado es par, el comprador podrá canjear su botella de agua por soda en la cafetería. Solo se admitirán las solicitudes de los primeros 200 compradores.
* </p><p>*Para venta de entrada dentro de la plataforma*</p><p></p>|

|**12**|*Nombre Rol*|
| :- | :- |
|**Descripción**|Gestión de Inventario de Soda en la Cafetería|
|**Comentarios**|<p>*La cafetería tendrá un inventario de soda limitado a 200 unidades. El sistema debe llevar un registro de las sodas disponibles y permitir el canje solo si hay existencias disponibles.* </p><p>*Para venta de entrada dentro de la plataforma*</p><p></p>|

|**13**|*Nombre Rol*|
| :- | :- |
|**Descripción**|Soporte para Múltiples Teatros|
|**Comentarios**|<p>*El sistema debe ser escalable y admitir más de un teatro en la cadena de bloques. Cada teatro tendrá sus propias funciones de cine y su inventario de entradas.* </p><p>*Para venta de entrada dentro de la plataforma*</p><p></p>|



# Pasos pra levantar el entorno de producción

# Comenzar proyecto desde 0

    >     docker stop $(docker ps -a -q)
    >     docker rm $(docker ps -a -q)
    >     docker volume prune
    >     docker network prune

    >     rm -rf crypto-config/peerOrganizations
    >     rm -rf cripto-config/ordererOrganizations
    >     rm -rf channel-artifacts/
    >     mkdir channel-artifacts

## Arquitectura 

![Arquitectura ](/img/APP_HLF_blocktick.png)

### Peers

Un peer es un nodo de la red que mantiene una copia del libro mayor (ledger) y participa en la validación y ejecución de transacciones. Los peers en Hyperledger Fabric son responsables de mantener la consistencia y la integridad de la red.

### Orderer

El orderer es responsable de gestionar el consenso y el ordenamiento de las transacciones en la red. Es el encargado de garantizar que las transacciones sean incluidas en el libro mayor de manera consistente y en el mismo orden en todos los peers de la red.

El orderer actúa como un servicio centralizado o distribuido que recibe las transacciones propuestas por los peers endorsantes, las agrupa en bloques y las distribuye a los peers validadores para su validación y posterior inclusión en el libro mayor.

y tambien es el encargado de asegurar la ejecución del protocolo de consenso, algunos de estos protocolos pueden ser:

* Solo(Testing). 
* Kafka-based(producción) es mas centralizada.
* RAFT(Producción) desde v1.4.2, cada organizacion puede ser parte del cluster y proponer nuevos participantes, lo que permite descentralizar mas la red. Aunque aun usa un sistema (lider-seguidor) donde el lider va cambiando entre los participantes de la red. 
Por lo tanto lo mas ideoneo en producción actualmente es, buenas politicas de endorsamiento en combinación con RAFT.

### Organizaciones

Llamamos organizaciones a los miembros y/o participantes que conforman la red y que conforman parte del consenso.

### Channel

Un canal (channel) es una característica fundamental que permite la creación de subredes privadas dentro de una red blockchain, proporciona a los participantes la capacidad de realizar transacciones y compartir información de manera confidencial y selectiva con un grupo específico de miembros de la red.

Cada canal en Hyperledger Fabric tiene su propio libro mayor (ledger) y políticas de consenso. Los canales permiten que un subconjunto de organizaciones y peers participe en transacciones y mantenga un registro separado de esas transacciones, que no es accesible para otros canales en la red.

### CLI

Es un servicio que nos da hyperledger para comunicarnos con las organizaciones y consumir/administrar los smartcontract.
El cliente utiliza el SDK de Hyperledger fabric para enviar una transaccion a la red.

## Generamos configuraciones

### Material criptografico básico para crear la red 

Conceptualmente representan las configuraciones que van a representar la identidad de nuestros participantes.

* Creamos el archivo crypto-config.yml

    > Previsualización del archivo

    [ir a archivo crypto-config.yml](/blocktick-network/crypto-config.yaml)

* Ejecutamos en la terminal el comando

    >     cryptogen generate --config=./crypto-config.yaml


Genera una carpeta crypto-config que incluye las configuraciones de las organizaciones que conforman la red, y la indentidad digital de sus participantes.


### Creamos configuraciones de bloque y transacciones.

Conceptualmente representa las configuraciones que van a determinar, comó se van a comunicar los participantes de la red.

* Creamos el archivo configtx.yml

    > Previsualización del archivo

    [ir a archivo configtx.yml](/blocktick-network/configtx.yaml)

* Ejecutamos los siguientes comandos

    >### Generamos el genesis.block
    >     configtxgen -profile ThreeOrgsOrdererGenesis -channelID system-channel -outputBlock ./channel-artifacts/genesis.block
    >### Generamos channel.tx 
    >     configtxgen -profile ThreeOrgsChannel -channelID attendance -outputCreateChannelTx ./channel-artifacts/channel.tx
    >### Generamos Org1MSPanchors.tx
    >     configtxgen -profile ThreeOrgsChannel -channelID attendance -outputAnchorPeersUpdate ./channel-artifacts/Org1MSPanchors.tx -asOrg Org1MSP
    >### Generamos Org2MSPanchors.tx
    >     configtxgen -profile ThreeOrgsChannel -channelID attendance -outputAnchorPeersUpdate ./channel-artifacts/Org2MSPanchors.tx -asOrg Org2MSP
    >### Generamos Org3MSPanchors.tx
    >     configtxgen -profile ThreeOrgsChannel -channelID attendance -outputAnchorPeersUpdate ./channel-artifacts/Org3MSPanchors.tx -asOrg Org3MSP


### Resultados

Verificamos que se haya creado los archivos en channel-artifacts.


## Levantamos la red

Con docker compose vamos a levantar la red que utiliza todas las configuraciones previamente creadas.

### Contenedor base de los peers
Creamos el directorio de trabajo /base.

* En el directorio /base creamos el archivo peer-base.yaml, que se se encarga de crear contenedor base para los peers de (HyL-f)

    > ### peer-base.yaml

    > Previsualización del archivo

    [ir a archivo peer-base.yaml](/blocktick-network/base/peer-base.yaml)

### Contenedores de los participantes y del servicio de ordenamiento

* Creamos el archivo docker-compose-base.yaml que se va a encargar de levantar los contenedores para los participantes y del servicio de ordenamiento.

    > ### docker-compose-base.yaml
    > 
    > Tipo : Levantar Red

    > Previsualización del archivo

    [ir a archivo docker-compose-base.yml](/blocktick-network/base/docker-compose-base.yaml)

### Orquestamos los contenedores creados y ademas se agregan las bases de datos y servicios CA y CLI

* Creamos el archivo docker-compose-cli-couchdb.yaml que se va a encargar de orquestar todos los contenedores en una sola red y ademas agrega las bases de datos de cada participante y servicios como CA y CLI necesarios para la arquitectura (HyL-f).

    > ### docker-compose-cli-couchdb.yaml
    > 
    > Tipo : Levantar Red
    > 
    > Previsualización del archivo :


    [ir a archivo docker-compose-cli-couchdb.yaml](/blocktick-network/docker-compose-cli-couchdb.yaml)

### Levantamos todo

* Vamos a trabajar con un cliente para administrar los contenedores como portainer.
Para eso creamos primero un volumen

      docker volume create portainer_data
    
* Levantamos el contenedor

      docker run -d -p 8000:8000 -p 9000:9000 -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer
    
* Nos conectamos al localhost:9000 y creamos una cuenta antes de los 3 minutos.



* Exportamos las variables de entorno que necesitamos.

      export CHANNEL_NAME=attendance
      export VERBOSE=false
      export FABRIC_CFG_PATH=$PWD
    
* Ahora sí, levantamos el docker compose.

      CHANNEL_NAME=$CHANNEL_NAME docker compose -f docker-compose-cli-couchdb.yaml up -d

* Verificamos si se levanto todo.
      
  
### Terminamos de configurar la red

* Creamos el canal "attendance". Nos conectamos a la consola de comando del contenedor CLI.

      -export CHANNEL_NAME=attendance
      -peer channel create -o orderer.blocktick.com:7050 -c $CHANNEL_NAME -f ./channel-artifacts/channel.tx --tls true --cafile /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/blocktick.com/orderers/orderer.blocktick.com/msp/tlscacerts/tlsca.blocktick.com-cert.pem 

    Se nos tiene que crear el archivo attendance.block


* Añadimos la primer organizacion al canal que toma por defecto CLI.

      -peer channel join -b attendance.block

* Añadimos la segunda organización al canal

      -CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.blocktick.com/users/Admin@org2.blocktick.com/msp CORE_PEER_ADDRESS=peer0.org2.blocktick.com:7051 CORE_PEER_LOCALMSPID="Org2MSP" CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.blocktick.com/peers/peer0.org2.blocktick.com/tls/ca.crt peer channel join -b attendance.block

* Añadimos la tercer organización al canal.

      -CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org3.blocktick.com/users/Admin@org3.blocktick.com/msp CORE_PEER_ADDRESS=peer0.org3.blocktick.com:7051 CORE_PEER_LOCALMSPID="Org3MSP" CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org3.blocktick.com/peers/peer0.org3.blocktick.com/tls/ca.crt peer channel join -b attendance.block

### Terminamos de configurar a los participantes de la red

Tenemos que tener seteado en cada organizacion el anchorpeer correspondiente.

* Para la organización 1

      peer channel update -o orderer.blocktick.com:7050 -c $CHANNEL_NAME -f ./channel-artifacts/Org1MSPanchors.tx --tls --cafile /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/blocktick.com/orderers/orderer.blocktick.com/msp/tlscacerts/tlsca.blocktick.com-cert.pem

* Para la organización 2

      CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.blocktick.com/users/Admin@org2.blocktick.com/msp CORE_PEER_ADDRESS=peer0.org2.blocktick.com:7051 CORE_PEER_LOCALMSPID="Org2MSP" CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.blocktick.com/peers/peer0.org2.blocktick.com/tls/ca.crt peer channel update -o orderer.blocktick.com:7050 -c $CHANNEL_NAME -f ./channel-artifacts/Org2MSPanchors.tx --tls --cafile /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/blocktick.com/orderers/orderer.blocktick.com/msp/tlscacerts/tlsca.blocktick.com-cert.pem

* Para la organización 3

      CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org3.blocktick.com/users/Admin@org3.blocktick.com/msp CORE_PEER_ADDRESS=peer0.org3.blocktick.com:7051 CORE_PEER_LOCALMSPID="Org3MSP" CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org3.blocktick.com/peers/peer0.org3.blocktick.com/tls/ca.crt peer channel update -o orderer.blocktick.com:7050 -c $CHANNEL_NAME -f ./channel-artifacts/Org3MSPanchors.tx --tls --cafile /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/blocktick.com/orderers/orderer.blocktick.com/msp/tlscacerts/tlsca.blocktick.com-cert.pem
      
### HASTA AQUÍ YA TENEMOS LA INFRAESTRUCTURA LEVANTADA 

## Agregamos los chaincode y/o smartsontract

Un chaincode es un bloque de codigo que tiene logica de persisitnencia y consulta de informacion en la blockchain.
Un smartcontract hace referencia a la logica del negocio, es decir que información queremos persisitir.
Un chaincode puede agrupar varios smartcontract.

### Creamos nuestro chaincode

En este caso vamos a usar Golang y se va a llamar movies, 

* Creamos el archivo movies.go en el directorio /chaincode.

    > Previsualización del archivo

[ver archivo movies.go](/chaincode/movies/movies.go)

### Agregamos nuestro chaincode a la red

Ahora nos conectamos al servicio de CLI ejecutamos las siguientes lineas.

* Configuramos las siguientes variables de entorno

      export CHANNEL_NAME=attendance
      export CHAINCODE_NAME=movies
      export CHAINCODE_VERSION=1
      export CC_RUNTIME_LANGUAGE=golang
      export CC_SRC_PATH="../../../chaincode/$CHAINCODE_NAME"  --> no reconoce $CHAINCODE_NAME, eliminarlo si al cargar la variable diese error
      export ORDERER_CA=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/blocktick.com/orderers/orderer.blocktick.com/msp/tlscacerts/tlsca.blocktick.com-cert.pem

* Generamos el package del chaincode.

      peer lifecycle chaincode package ${CHAINCODE_NAME}.tar.gz --path ${CC_SRC_PATH} --lang ${CC_RUNTIME_LANGUAGE} --label ${CHAINCODE_NAME}_${CHAINCODE_VERSION} >&log.txt

* Instalamos el chaincode en la primer organización

      peer lifecycle chaincode install ${CHAINCODE_NAME}.tar.gz

    Nos tiene que retornar un identificador para el chaincode que vamos a usar para commitear.
    
      movies_1:d7dcb74d448855c2b545033b3c985f92c245642198f61283dadfa24c4204f32b

* Instalamos el chaincode en la segunda organización

      CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.blocktick.com/users/Admin@org2.blocktick.com/msp CORE_PEER_ADDRESS=peer0.org2.blocktick.com:7051 CORE_PEER_LOCALMSPID="Org2MSP" CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.blocktick.com/peers/peer0.org2.blocktick.com/tls/ca.crt peer lifecycle chaincode install ${CHAINCODE_NAME}.tar.gz

    Nos retorna el siguiente identificador
      
      movies_1:d7dcb74d448855c2b545033b3c985f92c245642198f61283dadfa24c4204f32b

* Instalamos el chaincode en la tercer organización

      CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org3.blocktick.com/users/Admin@org3.blocktick.com/msp CORE_PEER_ADDRESS=peer0.org3.blocktick.com:7051 CORE_PEER_LOCALMSPID="Org3MSP" CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org3.blocktick.com/peers/peer0.org3.blocktick.com/tls/ca.crt peer lifecycle chaincode install ${CHAINCODE_NAME}.tar.gz

    Nos retorna el siguiente identificador.

      movies_1:d7dcb74d448855c2b545033b3c985f92c245642198f61283dadfa24c4204f32b

    Observar que los identificadores deben ser los mismos.

* Definimos las politicas de aprobación para el chaincode, aca es donde decidimos que organizaciones tienen permisos para firmar transacciones.
En este caso solo las primera y tercera organizacion van a tener permisos de escritura.
   
   Para la primer organización.

      peer lifecycle chaincode approveformyorg --tls --cafile $ORDERER_CA --channelID $CHANNEL_NAME --name $CHAINCODE_NAME --version $CHAINCODE_VERSION --sequence 1 --waitForEvent --signature-policy "OR ('Org1MSP.peer','Org3MSP.peer')" --package-id movies_1:d7dcb74d448855c2b545033b3c985f92c245642198f61283dadfa24c4204f32b

   Para la tercer organización.

      CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org3.blocktick.com/users/Admin@org3.blocktick.com/msp CORE_PEER_ADDRESS=peer0.org3.blocktick.com:7051 CORE_PEER_LOCALMSPID="Org3MSP" CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org3.blocktick.com/peers/peer0.org3.blocktick.com/tls/ca.crt peer lifecycle chaincode approveformyorg --tls --cafile $ORDERER_CA --channelID $CHANNEL_NAME --name $CHAINCODE_NAME --version $CHAINCODE_VERSION --sequence 1 --waitForEvent --signature-policy "OR ('Org1MSP.peer','Org3MSP.peer')" --package-id movies_1:d7dcb74d448855c2b545033b3c985f92c245642198f61283dadfa24c4204f32b

   Comprobamos que las politicas esten correctas, ejecutando la siguiente linea de comando:
      
      peer lifecycle chaincode checkcommitreadiness --channelID $CHANNEL_NAME --name $CHAINCODE_NAME --version $CHAINCODE_VERSION --sequence 1 --signature-policy "OR ('Org1MSP.peer','Org3MSP.peer')" --output json

   resultado esperado

      "approvals": {
            "Org1MSP": true,
            "Org2MSP": false,
            "Org3MSP": true
      }


* Por ultimo comiteamos el chaincode.

      peer lifecycle chaincode commit -o orderer.blocktick.com:7050 --tls --cafile $ORDERER_CA --peerAddresses peer0.org1.blocktick.com:7051 --tlsRootCertFiles /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.blocktick.com/peers/peer0.org1.blocktick.com/tls/ca.crt --peerAddresses peer0.org3.blocktick.com:7051 --tlsRootCertFiles /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org3.blocktick.com/peers/peer0.org3.blocktick.com/tls/ca.crt --channelID $CHANNEL_NAME --name $CHAINCODE_NAME --version $CHAINCODE_VERSION --sequence 1 --signature-policy "OR ('Org1MSP.peer','Org3MSP.peer')"

### Probamos la persisitencia del chaincode

> consultar

[ver archivo movies.go](/chaincode/movies/commands.txt)