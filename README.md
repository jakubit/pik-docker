## Porty
* Kafak - broker 0: `9092`<br>
* Kafka - broker 1: `9093`
* MongoDB: `27017`
* Zookeeper: `2181`
* MongoDB Sink Connector: `8083`

## Nazwy kontenerów
* Kafka - broker 0: `kafka0`
* Kafka - broker 1: `kafka1`
* Mongo: `mongo`
* Zookeeper: `zookeeper`
* MongoDB Sink Connector: `connector`

## Wymagane
* Docker (zainstalowany i uruchomiony)
* Konto na DockerHub (do zalogowania w Dockerze)
* Docker-compose (zainstalowany)

## Uruchomienie
1. Uruchomić kontenery Dockera (w terminalu, będąc w katalogu, który zawiera plik `docker-compose.yml`): <br>
    `docker-compose up`
2. <u>Ten punkt wystarczy wykonać tylko raz - przy pierwszym uruchomieniu kontenerów.</u> <br>
    Zarejestrować konektor do Mongo (w terminalu, będąc w katalogu, który zawiera plik `mongo-connector-setup.json`): `curl -X POST -H "Content-Type: application/json" --data @mongo-connector-setup.json http://localhost:8083/connectors`

## Korzystanie z Connectora
Ogólnie całą robotę z wyciągniem rekordów z Kafki i wstawianiem ich do bazy jako dokumenty załatwia właśnie Connector.<br>
Wystarczy więc dodać rekord do odpowiedniego tematu.<br>
<b>Rekordy wstawiane do tematu muszą być poprawnym JSONem inaczej Connector się wywali i dupa.</b>

## Konfiguracja Connectora - opcjonalne
Interesują nas głównie trzy pola z pliku `mongo-connector-setup.json`:
* `db.name` - nazwa bazy w Mongo, do której chcemy zapisywać dokumenty - jeśli nie istnieje, to zostanie automatycznie utworzona
* `db.collections` - kolekcje, do których chcemy zapisywać dokumenty z Kafki - jeśli nie istnieją, to zostaną automatycznie utworzone
* `topics` - tematy Kafki, z których rekordy będą pobierane i wrzucane do bazy

Jeśli chcemy zatwierdzić zmiany w wyżej wymienonych polach, to musimy najpierw odłączyć Connector, a następnie ponownie go podłączyć:
1. Odłączenie: <br>
    `curl -i -X DELETE "http://localhost:8083/connectors/mongo-connector-testTopic"`
2. Podłączenie: <br>
    `curl -X POST -H "Content-Type: application/json" --data @mongo-connector-setup.json http://localhost:8083/connectors`

## Zatrzymanie
* `Ctrl + C`

## Diagnostyka
* Lista uruchomionych kontenerów: `docker ps`
* Lista uruchomionych oraz zatrzymanych kontenerów: `docker ps -a`
* Zatrzymanie wszystkich kontenerów: `docker stop $(docker ps -a -q)`
* Usunięcie wszystkich zatrzymanych kontenerów - <u>stracimy wszytkie dane z Kafki, Mongo i Connectora</u>: `docker container prune`
* Dotęp do terminala kontenera: `docker exec -it [nazwa kontenera] bash`