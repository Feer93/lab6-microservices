# Solution

## Both services accounts (2222) and web are running

In order to run these services, first we must ensure that the registration service is running, then we can use ``./gradlew :accounts:bootRun`` and ``./gradlew :web:bootRun``. Instead I,ve runned them
from Intellij IDE which provides an auto run button.

The following logs are produced:

- Accounts

  - 1st part of the log 
![](C:\Users\ferna\Documents\Ingenieria Web\capturas pr6\accounts-running-1.jpg)
  - 2nd part of the log
![](C:\Users\ferna\Documents\Ingenieria Web\capturas pr6\account-running-2.jpg)

- Web service
![](C:\Users\ferna\Documents\Ingenieria Web\capturas pr6\web-running.jpg)

## The service registration service has these two services registered

Lets take a look at the dashboard on ``localhost:1111``

![](C:\Users\ferna\Documents\Ingenieria Web\capturas pr6\dash-board eureka.jpg)

After a short period of time both services are registered in Eureka with status up.

## A second accounts service instance is started and will use the port 4444

To run a second account service, we have to change the application.yml which is located on ``accounts/src/resources`` folder
and change ``port:2222`` to ``port:4444`` , then we run the service showing the following log:

![](C:\Users\ferna\Documents\Ingenieria Web\capturas pr6\terminal-accounts-4444-running.jpg)

And if we check the dashboard on ``localhost:1111`` we now see two accounts services up and running:

![](C:\Users\ferna\Documents\Ingenieria Web\capturas pr6\2nd account service launched on port 4444.jpg)

## What happens when you kill the service accounts (2222) and do requests to web? Can the web service provide information about the accounts again? Why?

When we kill the accounts service (2222) the server takes some time to reconfigure and some requests will still be sent to service(2222) until
Eureka realises is down, then all of the request will be sent to service(4444).

Photo showing an internal error as the service has been shutdown:

<img src="C:\Users\ferna\Documents\Ingenieria Web\capturas pr6\error server shutdonw.jpg"/>

As we can see in this photo the service (2222) has been leased:

![](C:\Users\ferna\Documents\Ingenieria Web\capturas pr6\leases cancelada.jpg)

And now Eureka dashboard no longer shows the service 2222 on the dashboard:

![](C:\Users\ferna\Documents\Ingenieria Web\capturas pr6\eurkea-dashboard after shutting down 2222.jpg)

The web service can provide information about the accounts as we can see:

![](C:\Users\ferna\Documents\Ingenieria Web\capturas pr6\account detail.jpg)

This is due to the fact that there is a 2nd service running and Eureka acts as an intermediator between the web service and the accounts one, making
possible to discover the new service on runtime through a logical route.