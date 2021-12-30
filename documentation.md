# Solution

## Both services accounts (2222) and web are running

In order to run these services, first we must ensure that the registration service is running, then we can use ``./gradlew :accounts:bootRun`` and ``./gradlew :web:bootRun``. Instead I,ve runned them
from Intellij IDE which provides an auto run button.

The following logs are produced:

- Accounts

  - 1st part of the log 
![accounts-running-1](https://user-images.githubusercontent.com/33655360/147776578-1f7a2421-da84-4e51-bf61-bd2fb1fba348.jpg)
  - 2nd part of the log
![account-running-2](https://user-images.githubusercontent.com/33655360/147776594-2ca75e47-39d1-4bdf-a529-ff1c17ec0363.jpg)

- Web service
![web-running](https://user-images.githubusercontent.com/33655360/147776606-41ed02cd-4a6b-4d2b-b6e4-21a34cc07cd2.jpg)

## The service registration service has these two services registered

Lets take a look at the dashboard on ``localhost:1111``

![dash-board eureka](https://user-images.githubusercontent.com/33655360/147776628-7df831bd-2a3b-44a6-bc6d-3d56dea02aa0.jpg)

After a short period of time both services are registered in Eureka with status up.

## A second accounts service instance is started and will use the port 4444

To run a second account service, we have to change the application.yml which is located on ``accounts/src/resources`` folder
and change ``port:2222`` to ``port:4444`` , then we run the service showing the following log:

![terminal-accounts-4444-running](https://user-images.githubusercontent.com/33655360/147776639-b9941420-b7d7-44bf-8b12-6f7a0cc9beef.jpg)

And if we check the dashboard on ``localhost:1111`` we now see two accounts services up and running:

![2nd account service launched on port 4444](https://user-images.githubusercontent.com/33655360/147776660-228091db-00bd-455e-9c7f-de5455a393d3.jpg)

## What happens when you kill the service accounts (2222) and do requests to web? Can the web service provide information about the accounts again? Why?

When we kill the accounts service (2222) the server takes some time to reconfigure and some requests will still be sent to service(2222) until
Eureka realises is down, then all of the request will be sent to service(4444).

Photo showing an internal error as the service has been shutdown:

![error server shutdonw](https://user-images.githubusercontent.com/33655360/147776685-38e97042-6082-44dc-8f54-cd56d99b0c72.jpg)

As we can see in this photo the service (2222) has been leased:

![leases cancelada](https://user-images.githubusercontent.com/33655360/147776705-64763d86-2d43-475d-a6f9-b2854d5ae1b5.jpg)

And now Eureka dashboard no longer shows the service 2222 on the dashboard:

![eurkea-dashboard after shutting down 2222](https://user-images.githubusercontent.com/33655360/147776720-71b9b3a7-2e11-4dfc-a68d-06fd0611d8d6.jpg)

The web service can provide information about the accounts as we can see:

![account detail](https://user-images.githubusercontent.com/33655360/147776740-c13dc872-677c-48e0-9245-fac5950cba82.jpg)

This is due to the fact that there is a 2nd service running and Eureka acts as an intermediator between the web service and the accounts one, making
possible to discover the new service on runtime through a logical route.
