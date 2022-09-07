# meltano.yml project file samples
This repository contains sample [meltano.yml project files](https://docs.meltano.com/concepts/project#meltanoyml-project-file) you can use to kickstart your [Meltano](https://meltano.com/) project.

Example:

```yaml
version: 1
default_environment: dev
environments:
- name: dev
- name: staging
- name: prod

plugins:
  extractors:
  - name: tap-postgres
    variant: transferwise
    pip_url: pipelinewise-tap-postgres
    config:
      host: host.docker.internal #TODO replace with your host
      port: 5432 #TODO replace with your port, default is 5432
      user: admin #TODO replace
      password: password #TODO replace 
      dbname: demo_source #TODO replace 
      default_replication_method: FULL_TABLE #TODO replace if wanted.
    select: # TODO define what tables/streams you want to retrieve
      - public-raw_customers.* # select all three attributes from the public schema inside the raw_customers table. 
                               # use meltano select tap-postgres --list --all to view all selectable attributes

  loaders: 
  - name: target-postgres
    variant: transferwise
    pip_url: pipelinewise-target-postgres
    config:
      host: host.docker.internal #TODO replace with your host
      port: 5432 #TODO replace with your port, default is 5432
      user: admin #TODO replace
      password: password #TODO replace
      dbname: demo_target #TODO replace
```

_Note: These files are meant to get you started quickly. To help you fill out the details, the values are filled with example values (because there's no good YAML schema validation happening on the Meltano side of things yet). So don't forget to put in your own values._

## How to use this repository
1. Pick your project sample from the repository tree above or from the list below. Each file is named after the key plugins, so ```el-postgres-postgres.yml``` is an example of a PostgreSQL to PostgreSQL Extract and Load (EL) Meltano project.
2. C/P into your Meltano project into the ```meltano.yml``` file, leaving your ```project_id``` in place. (If you don't have one yet, run ```meltano init``` one to get an empty template with a new id)
3. Modify all values that have a "TODO" comment inside the YAML.
4. Run ```meltano install``` to install the new plugins defined inside your new Meltano project file.

## Contribute ```meltano.yml```samples
Yes! Please contribute your meltano samples if they aren' already contained in here. Just follow the examples, make sure you provide sample values and mark places with "#TODO".

## ```meltano.yml```samples
- [el-postgres-postgres.yml](el-postgres-postgres.yml): E & L between two different PostgreSQL databases.
- [el-s3-csv-postgres.yml](el-postgres-postgres.yml): E & L between an AWS S3 bucket with 3 CSV files and a PostgreSQL database.
- [elt-s3-csv-dbt-postgres.yml](elt-s3-csv-dbt-postgres.yml): ELT process, AWS S3 bucket with 3 CSV files into a PostgreSQL database, topped off with dbt run.

- [elt-s3-csv-dbt-postgres-datahub.yml](elt-s3-csv-dbt-postgres-datahub.yml): ELT process with the DataHub utility. AWS S3 bucket with 3 CSV files into a PostgreSQL database, topped off with dbt run & DataHub as metadata catalog ingesting data from all three sources (S3, PostgreSQL & dbt).