## Porty
Wszytkie porty są domyślne:<br>
* Kafka `9092`
* MongoDB `27017`
* Zookeeper `2181`

## Nazwy kontenerów
* Kafka: `pik_kafka_1`
* Mongo: `pik_mongo_1`
* Zookeeper: `pik_zookeeper_1`

## Wymagane
* Docker (zainstalowany i uruchomiony)
* Konto na DockerHub (do zalogowania w Dockerze)
* Docker-compose (zainstalowany)

## Uruchomienie
W terminalu, będąc w katalogu, który zawiera plik docker-compose.yml wykonaj komendę: `docker-compose up`

## Zatrzymanie
`Ctrl + C`

## Diagnostyka
* Lista uruchomionych kontenerów: `docker ps`
* Lista uruchomionych oraz zatrzymanych kontenerów: `docker ps -a`
* Dotęp do terminala kontenera: `docker exec -it [nazwa kontenera] bash`