# ocl_web [![Build Status](https://app.snap-ci.com/OpenConceptLab/ocl_web/branch/master/build_image)](https://app.snap-ci.com/OpenConceptLab/ocl_web/branch/master)

Client interface for Open Concept Lab terminology services API.


## Developer Setup

### Prerequisites

1. ocl_web  
   * ``` git clone git@github.com:OpenConceptLab/ocl_web.git ```
2. python
3. npm
4. pip
5. python virtualenv
  * ``` pip install virtualenv ```
6. OCL API must be setup, up and running.

### OCL Web Setup

1. Change working directory to repository root
   ```sh
   cd ocl_web 
   ```
2. Create a virtualenv for the project
   ```sh 
   virtualenv ocl #Creates a virtual env (ocl is the name of virtualenv, can give any name) 
   ```
3. Create the file that will be used as the DB for ocl_web
   ```sh
   touch ocl.db 
   ```
4. Activate the virtual environment created in step __2__ . For deactivation of virtual env just write 'deactivate'.
   ```sh
   source ./ocl/bin/activate
   ```
   
5. Set the environment variables below to let web connect to API and set db location. Note that OclAPI must be already setup for this, you can see the token at http://0.0.0.0:8000/admin/authtoken/token/ (8000 is the port where oclapi server is running)
   
   ```sh
   export OCL_API_HOST='<your_api_server_ip>'
   export OCL_API_TOKEN='<root_token_from_api>'
   export OCL_ANON_API_TOKEN='<root_token_from_api>'
   export DATABASE_URL=sqlite:////<OCL_WEB_ROOT>/ocl.db
   
   ```
   
6. Install python and node.js dependencies
   ```sh 
   pip install -r requirements/local.txt
   npm install
   ```
7. Install grunt cli 
   ```sh
   npm install -g grunt-cli
   ```
8. Prepare database (create tables for models and apply migrations)
   ```sh
   python ocl_web/manage.py syncdb 
   python ocl_web/manage.py migrate 
   ```
9. Create a user. Make sure to get status=201 on the output. Otherwise the user is not created.
   ```sh
   python ocl_web/manage.py create_test_user --username <username> --password <password>
   ```
10. Serve the application 
   ```sh 
   grunt serve 
   ```
11. Application should be up at http://localhost:7000 and you should be able to login with the user created in step __9__



## Settings

__cookiecutter-django__ relies extensively on environment settings which **will not work with Apache/mod_wsgi setups**. It has been deployed successfully with both Gunicorn/Nginx and even uWSGI/Nginx.

For configuration purposes, the following table maps the cookiecutter-django environment variables to their Django setting:

|          Environment Variable         | Django Setting                 | Development Default                            | Production Default                          |
|:-------------------------------------:|--------------------------------|------------------------------------------------|---------------------------------------------|
| DJANGO_AWS_ACCESS_KEY_ID              | AWS_ACCESS_KEY_ID              | n/a                                            | raises error                                |
| DJANGO_AWS_SECRET_ACCESS_KEY          | AWS_SECRET_ACCESS_KEY          | n/a                                            | raises error                                |
| DJANGO_AWS_STORAGE_BUCKET_NAME        | AWS_STORAGE_BUCKET_NAME        | n/a                                            | raises error                                |
| DJANGO_CACHES                         | CACHES                         | locmem                                         | memcached                                   |
| DJANGO_DEBUG                          | DEBUG                          | True                                           | False                                       |
| DJANGO_EMAIL_BACKEND                  | EMAIL_BACKEND                  | django.core.mail.backends.console.EmailBackend | django.core.mail.backends.smtp.EmailBackend |
| DJANGO_SECRET_KEY                     | SECRET_KEY                     | CHANGEME!!!                                    | raises error                                |
| DJANGO_SECURE_BROWSER_XSS_FILTER      | SECURE_BROWSER_XSS_FILTER      | n/a                                            | True                                        |
| DJANGO_SECURE_SSL_REDIRECT            | SECURE_SSL_REDIRECT            | n/a                                            | True                                        |
| DJANGO_SECURE_CONTENT_TYPE_NOSNIFF    | SECURE_CONTENT_TYPE_NOSNIFF    | n/a                                            | True                                        |
| DJANGO_SECURE_FRAME_DENY              | SECURE_FRAME_DENY              | n/a                                            | True                                        |
| DJANGO_SECURE_HSTS_INCLUDE_SUBDOMAINS | SECURE_HSTS_INCLUDE_SUBDOMAINS | n/a                                            | True                                        |
| DJANGO_SESSION_COOKIE_HTTPONLY        | SESSION_COOKIE_HTTPONLY        | n/a                                            | True                                        |
| DJANGO_SESSION_COOKIE_SECURE          | SESSION_COOKIE_SECURE          | n/a                                            | False                                       |
* TODO: Add vendor-added settings in another table


### Running Tests

OCL_WEB has a suite of unit tests written in python (django test) and end-to-end tests written in [protractor](https://github.com/angular/protractor) running either headless (PhantomJS) or Chrome.

1. Unit Tets
  ```sh
  python ocl_web/manage.py test 
  ``` 
2. Running E2E tests
  * Headless on showcase server: 
  ```sh
  ./run_ui_tests.sh 
  ```
  * Locally on Chrome: 
  ```sh 
  OCL_WEB=. browser=chrome env=local username=<username> password=<pwd> ./run_ui_tests.sh
  ```



> Copyright (C) 2016 Open Concept Lab. Use of this software is subject
> to the terms of the Mozille Public License v2.0. Open Concept Lab is
> also distributed under the terms the Healthcare Disclaimer
> described at http://www.openconceptlab.org/license/.
