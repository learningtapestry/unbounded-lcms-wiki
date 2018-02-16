# Installation

- [Project setup](Project-setup)
- [Additional components](Additional-components)  
  - [Fonts](Fonts)
  - [Mathjax](Mathjax)
  - [SVGExport](Svgexport)
  - [Wkhtmltopdf](Wkhtmltopdf)
  

## Project setup

Clone the source code:

```bash
git clone git@github.com:learningtapestry/unbounded-lcms.git
```

Install required gems:

```bash
cd unbounded-lcms
gem install bundler && bundler
```

Create files with environment variables: 

```bash
cp .env.template .env.development
cp .env.template .env.test
```

Template file contains minimum set of predefined variables for application to run. Update variables 
`POSTGRESQL_*` if you have created database with different name. Don't forget to change the database name in the `.env.test` 
template.  

You can find the full list of supported variables 
[here](Environment-variables).

Export variables:

```bash
source .env.development
```

Create database user:

```bash
psql postgres -c "create user ${POSTGRESQL_USERNAME} with password '${POSTGRESQL_PASSWORD}' createdb;"
```  

Restore the database from the pre-populated UnboundEd template:

```bash
cp db/dump/content.dump.freeze db/dump/content.dump
bundle exec rake db:restore
bundle exec rake db:migrate
```

After that there will be single user with `admin` role, email `admin@example.org`, password: `password`. For security 
reasons we recommend you add a new admin user to the production environment and delete this one once the project is 
running.

Run the following task to setup your index and import the data into ES (make sure ElasticSearch is running):

```bash
bundle exec rake es:load
```

## Additional components

### Fonts

Install required fonts:

```bash
apt-get install fontconfig -y
cp -R app/assets/fonts/* /usr/local/share/fonts
fc-cache -f -v
```

For macOS:

```bash
cp -R app/assets/fonts/* ~/Library/Fonts
```

### Mathjax

[Mathjax binaries](https://github.com/mathjax/MathJax-node):

```bash
npm install mathjax-node
npm install mathjax-node-cli
```

### SVGExport

```bash
npm install svgexport
npm install pngquant-bin
```

### Wkhtmltopdf 

```bash
wget https://downloads.wkhtmltopdf.org/0.12/0.12.4/wkhtmltox-0.12.4_linux-generic-amd64.tar.xz
tar xvfJ wkhtmltox-0.12.4_linux-generic-amd64.tar.xz
cp wkhtmltox/bin/wkhtmltopdf /usr/local/bin
```
For macOS you can download the package from [the website](https://wkhtmltopdf.org/downloads.html)
