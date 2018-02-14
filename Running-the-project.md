* [Running the project](#running-the-project)
* [Running tests](#running-tests)

## Running the project

Start the server:

```bash
RAILS_ENV=development bin/rails s
```

Start the single worker to proceed with PDF/GDoc generation:

```bash
RAILS_ENV=development bundle exec rake resque:work
```

To control the queues precedence and the number of workers, just pass them as variables:

```bash
QUEUES=default,low COUNT=3 bundle exec rake resque:workers
```

## Running tests

To run the specs simple:

```bash
RAILS_ENV=test bin/rspec
```
