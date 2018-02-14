# Google Cloud Platform setup

An application calls Google Application Script in order to post-process generated lessons materials. Instructions below 
contains all the necessary steps need to setup integration.

All environment variables and user names provided here have been setup to automate deployment on 
[Cloud66](https://www.cloud66.com) service stacks. If you plan to use another service provider please change all the 
variables accordingly.

1. [Set up](#setup)
2. [How to update Google Application Script](#update)

## 1. Set up<a name="setup"></a>

1.1 Login to google (all google docs will be saved under this account)

1.2 Create project (i.e. LCMS-dev) at [google cloud](https://console.cloud.google.com)

1.3 Enable Google Drive API, Google Apps Script Execution API for that [project](https://console.cloud.google.com/apis/library)

1.4 Set up google auth for local development (for production see in [3.](#generate)):
- create credentials:  
  - type oAuth Client ID
  - Application type: Other, 
  - Aplication Name: up to you, i.e. lcms-cli-dev
- download client secret json to LCMS project root/config/google/client_secret.json and set appropriate ownership and 
access rights for it (where `cloud66-user` is a system username under which web application is running)

```bash
cd $STACK_BASE/shared/google
sudo chown cloud66-user:cloud66-user client_secret.json
sudo chmod 755 client_secret.json
```

- run rake task: `RAILS_ENV=[your env] bundle exec rake google:setup_auth`. You will be asked to go by link, give app 
permissions and paste code into terminal. This will create `root/config/google/app_token.yaml`. You need to copy it 
to `$STACK_BASE/shared/google/app_token.yaml` 
- change the ownership of the `app_token.yaml` otherwise it will not be accessible fir the application:
```bash
cd $STACK_BASE/shared/google
sudo chown cloud66-user:app_writers app_token.yaml
```

1.5 Add script
- go to https://www.google.com/script/start/
- copy-paste content of `config/scripts/Code.gs` there and save
- at top menu Resources->Cloud Platform Project set project from step 2 (you need to paste *project number*)
- at top menu Publish->Deploy As API Executable set version v1, access Only myself
- save Current API ID somewhere, click Close on that annoying window (update will not close it)
- choose any function and run it, it'll request permissions - grant them (there will be security warnings, just ignore 
them)

1.6 Create folder (all materials will be saved there) on Google Drive, give view only to all by link, keep folder id 
(last part of url), copy to the root [LANSCAPE](https://docs.google.com/document/d/1pXQDNKYOJYT6OTPnp8gsTWAydg5B9GTRibaWspmX4oE), 
[PORTRAIT](https://docs.google.com/document/d/1ijuZhGQXkPBxcZT4DRyNVY-qmI0xyVvSzVFqckOpsCc) templates 
and keep their IDs.

1.7 Update corresponding env-file in the project
```
GOOGLE_APPLICATION_FOLDER_ID=saved from step 6
GOOGLE_APPLICATION_SCRIPT_ID=saved id from step 5
GOOGLE_APPLICATION_TEMPLATE_PORTRAIT=saved from step 6
GOOGLE_APPLICATION_TEMPLATE_LANSCAPE=saved from step 6
```

## 2. How to update Google Application Script<a name="update"></a>

2.1 Proceed to Google Drive for account which has been used to create the Google Application Script from [1.5](#1.5)
2.2 Update script content
2.3 Re-publish the script:
- `Publish->Deploy As API Executable`
- Set new version
- `Update` & `Close`
- Choose any function and run it, it'll request permissions - grant them (there will be security warnings, just ignore 
them)

