# Slide 1

Hello, my name is Alice, and I am here with Nicola and Kevin to introduce our project named Route Rocket.

# Slide 2

Route Rocket is a web platform where users can book car trips offered by users registered as drivers.

We have three types of users: the passenger, the driver, and the administrator.

Let's explore what each can do through the use cases. 

# Slide 3

As we can see, the unregistered user can only perform the registration. 

Once completed, he becomes  *(becoms)* a registered user who can login, logout, and view his personal information. The driver instead can make their car available for earning money through the passenger trips.

The passenger can book a trip, view driver information in charge *(ciarg)* of the trip, write reviews, and check active and past reservations.

The driver can manage reservations, view statistics, and request a pick-up location for passengers.

Finally, the administrator can manage *(mèneig)* users and approve locations proposed by drivers.

# Slide 8

We chose to implement a responsive web application. To achieve this, we decided to use two frameworks: Angular JS that is a Javascript Framework TailwindCSS that is a CSS Framework. 


# KEVIN

As a dataset we found data on Google cloud platform of NewYork and Chicago taxi drivers. For both cieties we have information regarding serveral year, so we have a huge amount of data but we will user only a subset and we aim to obtain a 150 Mb dump.
The inclusion of data relative to two different cities gives us a rich variaty of information. The platform captures a huge amount of data as it saves details for each trip facilitated through its booking system. However, the information variability is limited by the recurent nature of the trips which often target tourist destionations and trasportaion hubs.

We decided to use key-values db to mantain information about active reservation and refresh token, a token regardin the authentication implemated with jwt token.
So we make an entity for reservation with a key that contain the ids of the driver and the passeger that are necessery to search the active user reservations. also the key contain main information about the trip like date derature and destination name. So we can efficiently search data about active reservation of the passenger and driver.
We make also an entity to store data about refresh_token so when we receive an API request to obtain an another jwt token we can effieciently check if the refresh token is valid.

We decided to use a graph database to store about location and distance information. The nodes contain geographical coordinates and a common name place. The edges contain the distance in spatial and time terms.
The main porpose for this database is the calculation of dijkstra algoritm. That allows us to calculate the shoerts path between two locations.