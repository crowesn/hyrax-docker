# Docker Setup for Hyrax Development

__NOT FOR PRODUCTION USE.__

This configuration should only be used for local development of a [Hyrax](https://github.com/samvera/hyrax) application.

## Requirements

[Docker Toolbox](https://www.docker.com/products/docker-toolbox) or [Docker for Mac](https://www.docker.com/docker-mac) or [Docker for Windows](https://www.docker.com/docker-windows)

## Containers

Includes seperate containers for each component of the stack.

## Usage

To use this docker configuration just drop this repo in the
root directory of your Hyrax app.

Start a docker machine, navigate into ./hyrax-docker and run:

```
docker-compose up -d
```

The default is to run the rails server on port 3000 of the docker machine.

- Rails Server: [localhost:3000/]()
- Solr: [localhost:8983/solr/]()
- FCREPO: [localhost:8984/fcrepo/]()

## Hyrax Configuration

See the `default_config` directory for default config files.

Requires the `mysql2` gem. Add `gem 'mysql2'` to `Gemfile`.

Set the `fits_path` in `config/initializers/hyrax.rb`:

```rb
  # Path to the file characterization tool
  config.fits_path = '/opt/fits/fits.sh'
```

The `worker` container requires Sidekiq. Follow the Hyrax guide for [Using Sidekiq with Hyrax](https://github.com/samvera/hyrax/wiki/Using-Sidekiq-with-Hyrax). If containers are already running restart after adding sidekiq, `docker-compose restart`.

## Help

### Running Rails Commands

All rails commands should be run on the `web` container. This just requires prefixing the commands with `docker-compose run --rm web`. For example to run DB migrations you could do this:

```
docker-compose run --rm web bundle exec rake db:migrate
```

### Cleaning FCREPO, Solr, and DB

The following command clears all data, including downloaded gems.

```
docker-compose down -v
```

To ensure a faster startup after cleaning the data use the following commands, which do not delete the volume that stores the ruby gems. Note, you may need to replace the `hyrax_` volume prefix with the name of your project directory.

```
docker-compose down
docker volume rm hyrax_fcrepo hyrax_pgdata hyrax_solr hyrax_redis hyrax_fcrepo_db
```

### Running Tests

The `web` container provides an entry point for running tests:

```bash
# For All Tests
docker-compose run --rm web test

# For a Single Test File
docker-compose run --rm web test spec/models/work_spec.rb
```
