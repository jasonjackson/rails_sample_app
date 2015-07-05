# Ruby on Rails Tutorial: sample application

This is the sample application for [*Ruby on Rails Tutorial: Learn Rails by Example*](http://railstutorial.org/) by [Michael Hartl](http://michaelhartl.com/).

This sample has been modified to run on Cloud Foundry. The `cf-autoconfig` gem was added to enable auto-configuration of database connections as described in the [Cloud Foundry documentation](http://docs.cloudfoundry.com/docs/using/services/ruby-service-bindings.html). 

This project was forked and updated to use the [Cloud Foundry MySQL Release](https://github.com/cloudfoundry/cf-mysql-release) as it does serve as an excellent example of best practices for BOSH packaging.  I updated this repo so that I could better understand the cf-mysql-release by testing it with an application sitting on top of it.

The `activerecord-mysql2-adapter` gem was also added to support connection to MySQL database proxies provided by the cf-mysql-release. 

## Running the application on Cloud Foundry

After installing in the 'cf' [command-line interface for Cloud Foundry](http://docs.cloudfoundry.org/devguide/installcf/),
targeting a Cloud Foundry instance, and logging in, the application can be pushed using these commands:

First, view a list of services and plans available in your Cloud Foundry instance: 

~~~
$ cf marketplace
Getting services from marketplace in org me / space development as admin...
OK

service   plans        description   
p-mysql   100mb, 1gb   MySQL databases on demand   
~~~

This is my marketplace, select the only service on the list and create a service instance named `rails-mysql` using a MySQL service and plan: 

~~~
$ cf create-service SERVICE PLAN rails-mysql
Creating service rails-mysql
OK
~~~

Now push the application: 

~~~
$ cf push APP-NAME --random-route
Using manifest file manifest.yml

Updating app rails-sample
OK

Creating route rails-sample-desiccative-acetylizer.cfapps.io...
OK

Binding rails-sample-desiccative-acetylizer.cfapps.io to rails-sample...
OK

Uploading rails-sample...
Uploading app files from: rails_sample_app
Uploading 41.1M, 6349 files
OK
Binding service rails-mysql to app rails-sample
OK

Starting app rails-sample
OK
...

0 of 1 instances running, 1 starting
0 of 1 instances running, 1 starting
0 of 1 instances running, 1 starting
1 of 1 instances running

App started

Showing health and status for app rails-sample
OK

requested state: started
instances: 1/1
usage: 256M x 1 instances
urls: rails-sample-desiccative-acetylizer.cfapps.io

     state     since                    cpu    memory          disk
#0   running   2014-05-29 03:34:22 PM   0.0%   50.3M of 256M   80.2M of 1G
~~~

The application will be pushed using settings in the provided `manifest.yml` file. The `--random-route` option adds random
words in the host to make sure the URL for the app is unique in the Cloud Foundry environment. The output of the
`cf push` command shows the URL that was assigned. Using the provided URL you can browse to the running application.
