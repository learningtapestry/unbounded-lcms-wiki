Start the server:

```bash
bin/rails s
```

Start the single worker to proceed with PDF/GDoc generation:

```bash
bundle exec rake resque:work
```

To control the queues precedence and the number of workers, just pass them as variables:

```bash
QUEUES=default,low COUNT=3 bundle exec rake resque:workers
```

To run the specs simple:

```bash
RAILS_ENV=test bin/rspec
```
