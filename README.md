# Branford Station

This application
 finds sources of [General Transit Feed Specification (GTFS)](https://developers.google.com/transit/gtfs/) open transit data from community-maintained sites like
  the [GTFS Data Exchange](http://www.gtfs-data-exchange.com/) and
  the [Google Transit Data Feed Wiki](https://code.google.com/p/googletransitdatafeed/wiki/PublicFeeds),
 periodically checks feed sources to confirm current data or download new data,
 and makes the data available for public consumption through an [API](http://en.wikipedia.org/wiki/Application_programming_interface#Web_APIs) and front-end interface.

## Usage

### View Web Application

Visit [next-train.info](http://next-train.info) in a web browser.

### Use Web Service API Endpoints

You may make HTTP requests or view GTFS data in a browser using the endpoint urls listed below.

 + **/agencies** (lists all participating transit agencies)
 + **/agencies/`:agency_abbrev`** (lists all train stations serviced by a given agency)
 + **/agencies/`:agency_abbrev`/stations/`:station_abbrev`** (lists upcoming departures from a given station)

These endpoints return HTML by default. To request a JSON response instead, suffix `.json` to the endpoint url.

### Schedule Background Tasks

You may schedule tasks to load all known GTFS data, or just the data you specify.

#### All Data Sources

##### Identify

```` sh
bundle exec rake station_attendant:find_data_exchange_feeds
````

```` sh
bundle exec rake station_attendant:find_google_transit_data_feeds
````

##### Identify and Load

``` sh
bundle exec rake station_attendant:find_and_consume_and_load
```
#### Ad-hoc Data Sources

You may download GTFS data from specified source(s) into a filesystem or a database.

##### Filesystem

```` rb
FeedConsumer.perform(:source_urls => ["http://www.shorelineeast.com/google_transit.zip", "http://web.mta.info/developers/data/mnr/google_transit.zip"])
````

##### Database

 ```` rb
FeedConsumer.perform(:load => true, :source_urls => ["http://www.shorelineeast.com/google_transit.zip", "http://web.mta.info/developers/data/mnr/google_transit.zip"])
````

## Contributing

To request a new feature, [create an issue](https://github.com/data-creative/branford_station/issues/new).

To deliver a new feature, [fork the repo](https://github.com/data-creative/branford_station/issues#fork-destination-box), make your changes, add tests if possible, and submit a pull request.

### Development Environment Setup

Install dependencies for a Ruby on Rails application.
 + ruby (via rbenv)
 + bundler
 + rails
 + mysql (or postgres, etc. according to database.yml)

Obtain source code.

```` sh
git clone git@github.com:data-creative/branford_station.git
cd branford_station
````

Install gem dependencies.

```` sh
bundle install
````

Create database user.

```` sql
-- mysql:
CREATE USER 'branford_station'@'localhost' IDENTIFIED BY 'branford_station';
GRANT ALL ON *.* to 'branford_station'@'localhost';
````

Create database.

```` sh
bundle exec rake db:create
bundle exec rake db:migrate
````

Populate database.

```` sh
bundle exec rake:db:seed
````

Start web server.

```` sh
rails server
````

View in browser at [localhost:3000](localhost:3000).

### Production Environment Setup



```` sh
git push heroku master
````
