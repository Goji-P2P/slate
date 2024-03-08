# Public Goji API docs

https://docs.api.goji.investments/

## Deployment

After merging your PR, the changes will be automatically deployed to the https://docs.api.goji.investments via GitHub Actions.

## Run

Try using docker first:

    docker-compose build
    docker-compose up

    http://localhost:4567  # project will be rebuilt automatically, refresh page to see changes

Or if you have ruby installed:

    bundle install
    bundle exec middleman server

    http://localhost:4567
