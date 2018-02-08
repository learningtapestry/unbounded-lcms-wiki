* [Requirements](#requirements)
* [Project config](#project-config)
  * [AirBrake](#airbrake-config)
  * [Amazon](#amazon-config)
  * [Common standards](#common-standards-config)
  * [Google](#google-config)
  * [Mailer](#mailer-config)
  * [Miscellaneous settings](#miscellaneous-settings)
  * [New Relic](#new-relic)
  * [Postgres](#postgres-config)
  * [UB Components compiler](#ub-Components-compiler-config)
* [Project setup](#project-setup)
* [Running the project](#running-the-project)
* [Running tests](#running-tests)

## Requirements

* [ElasticSearch](https://www.elastic.co/guide/en/elasticsearch/reference/current/_installation.html) `>=v2.2.0`
* [node](https://nodejs.org/en/download/package-manager/) `>=v5.6.0`
* [PostgreSQL](https://www.postgresql.org/download/) `>=v9.6`
* [Redis](https://redis.io/topics/quickstart) `>= v3.0`
* [ruby](https://www.ruby-lang.org/en/documentation/installation/) `v2.3.3`

## Project config

To be able to set up and run the application you need to set special environment variables. You can do that by using 
1. Set up `.env.test`, `.env.development`

```bash
cp .env.template .env.development
cp .env.template .env.test
```

### AirBrake config
The project is set up to support Airbrake, if you use it. You can provide an Airbrake ID and key in the config to enable it (Should be omitted for `development`&`test` environments)

|Name|Description|
|----|-----------|
|AIR_BRAKE_PROJECT_ID|Project id in the AirBrake service|
|AIR_BRAKE_PROJECT_KEY|Project key in the AirBrake service|

### Amazon config
The project uses AWS S3 to store and serve generated PDFs of learning content. You will need to set up an S3 bucket and provide a key and secret here:

|Name|Description|
|----|-----------|
|AWS_ACCESS_KEY_ID|Public part of the AWS credentials|
|AWS_REGION|Name of the region used for AWS service|
|AWS_SECRET_ACCESS_KEY|Private part of the AWS credentials|
|AWS_S3_BUCKET_NAME|Bucket used by application to store generated files|
|SWAP_DOCS|Folder inside `AWS_S3_BUCKET_NAME` bucket where generated PDFs of lessons and materials will be stored|
|SWAP_DOCS_LATEX|Folder inside `AWS_S3_BUCKET_NAME` bucket where generated PNGs of `LatexTag` will be stored|

### Common standards config
Common Standards is an API used to map educational content. If you have an AP key you can enter it here:

|Name|Description|
|----|-----------|
|COMMON_STANDARDS_PROJECT_API_KEY||
|COMMON_STANDARDS_PROJECT_API_URL||
|COMMON_STANDARDS_PROJECT_JURISDICTION_ID||

### Google config
The project uses several Google products, including analytics, OAuth for allowing the application to use Google APIs, and several Google Drive folders.

|Name|Description|
|----|-----------|
|GOOGLE_ANALYTICS_ID|Id for Google Analytics service|
|GOOGLE_OAUTH2_CLIENT_ID|Public part of the key which is used for OAuth Google authentication|
|GOOGLE_OAUTH2_CLIENT_SECRET|Private part of the key which is used for OAuth Google authentication|
|GOOGLE_APPLICATION_FOLDER_ID|Id of the Google Drive folder where generated lessons and materials will be placed(It's `0B7` for url like `https://drive.google.com/drive/u/0/folders/0B7/...`)|
|GOOGLE_APPLICATION_SCRIPT_ID|Id of the Google Script created to post-process generated Google documents. More details can be found [here](wiki/Google-Cloud-Platform-setup)|
|GOOGLE_APPLICATION_TEMPLATE_PORTRAIT|Id of the Google document which is a template for portrait materials(can be identified the same way as `GOOGLE_APPLICATION_FOLDER_ID `)|
|GOOGLE_APPLICATION_TEMPLATE_LANDSCAPE|Id of the Google document which is a template for landscape materials(can be identified the same way as `GOOGLE_APPLICATION_FOLDER_ID `)|
|GOOGLE_APPLICATION_PREVIEW_FOLDER_ID| Folder ID where preview documents should get placed

### Mailer config
The project supports Amazon Simple Email Service for sending emails (for account generation, password reset, etc) 

|Name|Description|
|----|-----------|
|AWS_SES_SERVER_NAME|SMPT address|
|AWS_SES_USER_NAME|SMPT username|
|AWS_SES_PASSWORD|SMPT password|

### Miscellaneous settings
|Name|Description|
|----|-----------|
|BITLY_API_TOKEN|Token of the Bitly service|
|ENABLE_BASE64_CACHING|Turns on/off caching of assets used for PDF generation (`true` by default)|
|FRESHDESK_URL|A link to the Fresh Desk instance. It would add a new icon in the header as well
|HEAP_ANALYTICS_ID|Id used to send data to HeapAnalytics service|
|RESQUE_NAMESPACE|Value is used to separate data stored in Redis when the same redis server is used by multiple environments|
|SOUNDCLOUD_CLIENT_ID||
|UNBOUNDED_DOMAIN|The application domain|
|WKHTMLTOPDF_PATH|Path to the [wkhtmltopdf](https://wkhtmltopdf.org) binary. Default to `/usr/local/bin/wkhtmltopdf`|
|TWITTER_USERNAME||
|FB_PAGE||
|YOUTUBE_CHANNEL||
|VIMEO_CHANNEL||
|ELASTICSEARCH_ADDRESS|Elasticsearch server address|

Obs: if you're setting a local dev machine on OSX and getting small fonts when generating pdfs, try downgrading wkhtmltopdf to `0.12.3`

### New relic
The project includes support for New Relic application monitoring, if you wish to use it.

|Name|Description|
|----|-----------|
|NEW_RELIC_LICENSE_KEY|License key for the service|
|NEW_RELIC_MONITOR_MODE|Observer by New Relic (`true` or `false` values)|

### Postgres config
|Name|
|----|
|POSTGRESQL_ADDRESS|
|POSTGRESQL_DATABASE|
|POSTGRESQL_USERNAME|
|POSTGRESQL_PASSWORD|
|POSTGRESQL_PORT|

### UB Components compiler config
|Name|Description|
|----|-----------|
|UB_COMPONENTS_API_TOKEN|Auth token to access Components Compiler API|
|UB_COMPONENTS_API_URL|Components Compiler API URL|
 
## Project setup

1. You should run the task `tmp:cache:clear` every time routes are updated. That will reset generated routes used by Javascript

2. For convenience, a copy of a reference unboundED database is available at `db/dump/content.dump.freeze`.

```bash
cp db/dump/content.dump.freeze db/dump/content.dump
RAILS_ENV=development rake db:restore
RAILS_ENV=development rake db:migrate
```

You may need to add the `hstore` extension to Postgres if it is not there already. Note that this requires superuser privileges on the database, so pass a username with superuser credentials if your local postgres requires it:

```bash
psql -d [DATABASE_NAME]
CREATE EXTENSION hstore;
```

You can also add the extension to your template1, so every new database (for example, when you restore) will have the extension already created. 

```bash
psql -d template1 -c 'create extension hstore;'
```

### For local development

1. Install required fonts

```bash
apt-get install fontconfig -y
cp -R $STACK_PATH/app/assets/fonts/* /usr/local/share/fonts
fc-cache -f -v
```

2. Mathjax binaries

```bash
npm install -g mathjax-node
npm install -g mathjax-node-cli
```

3. SVGExport

```bash
npm install svgexport -g
npm install pngquant-bin -g
```

4. Wkhtmltopdf

```bash
wget https://downloads.wkhtmltopdf.org/0.12/0.12.4/wkhtmltox-0.12.4_linux-generic-amd64.tar.xz
tar xvfJ wkhtmltox-0.12.4_linux-generic-amd64.tar.xz
cp wkhtmltox/bin/wkhtmltopdf /usr/local/bin
```

### ElasticSearch index

run the following task to setup your index and import the data into ES:

```bash
rake es:load
```

## Running the project

Start the server:

```bash
RAILS_ENV=development bin/rails s
```

Start the single worker to proceed with PDF/GDoc generation:

```bash
RAILS_ENV=development bundle exec rake resque:work
```

For controlling the queues precedence and the number of workers, just pass them as vars:

```bash
QUEUES=default,low COUNT=3 bundle exec rake resque:workers
```

## Running tests

To run the specs simple:

```bash
RAILS_ENV=test bin/rspec
```