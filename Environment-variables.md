* [AirBrake](#airbrake-config)
* [Amazon](#amazon-config)
* [Common standards](#common-standards-config)
* [Google](#google-config)
* [Mailer](#mailer-config)
* [Miscellaneous settings](#miscellaneous-settings)
* [New Relic](#new-relic)
* [Postgres](#postgres-config)
* [UB Components compiler](#UB-Components-compiler-config)

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
|GOOGLE_APPLICATION_SCRIPT_ID|Id of the Google Script created to post-process generated Google documents. More details can be found [here](Google-cloud-platform-setup.md)|
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
