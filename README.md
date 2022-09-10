# Kitchen

## Description 

The ```Kitchen``` has a finite ```order list``` . This ```order list``` is shared across all ```kitchen``` instances. All orders which ```kitchen``` receives have to be added to a single instance of ```order-list``` .

Main work unit of the ```Kitchen``` are cooks . Their job is to take the ```order``` and "prepare" the menu item(s) from it, and return the orders as soon and with as little idle time as possible. ```Kitchen``` can prepare foods from different orders and it is not mandatory that one cook have to prepare entire order. Order is considered to be prepared when all foods from order list are prepared.

Each cook has the following characteristics:

- rank , which defines the complexity of the food that they can prepare (one caveat is that a cook can only take orders which his current rank or one rank lower that his current one):
  - Line Cook ( rank = 1 )
  - Saucier ( rank = 2 )
  - Executive Chef (Chef de Cuisine) ( rank = 3 );
- proficiency : it indicates on how may dishes he can work at once. It varies between 1 and 4 (and to follow a bit of logic, the higher the rank of a cook the higher is the probability that he can work on more dishes at the same time);
- name;
- catch phrase.

So a cook could have the following definition:

``` json
{
	"rank": 3,
	"proficiency": 3
	"name": "Gordon Ramsay",
	"catch-phrase": "Hey, panini head, are you listening to me?"
}
```

Get creative on where and when to use this precious information about the cooks.

Another requirement not for the faint of heart is to implement the ```cooking apparatus``` rule. It comprises of the fact that a ```kitchen``` has limited space, thus a finite number of ovens, stoves and the likes.

Our ```kitchen``` configuration has to include a limited number of ```cooking apparatus``` . For example, at a ```kitchen``` with 3-4 cooks, we can have no more than 2 stoves and only one oven.

Some menu items require one of these appliance and it's up to us to define what happens when a cook runs into the problem of no available machinery.

We will have to define the mechanism that will decide which cook takes which order.

## Additional Info

The ```Kitchen``` consists of ```order list``` , ```cooks``` and ```cooking apparatus``` . The ```order list``` should be a single instance which holds all orders received from ```Dinning Hall ```.

The ```Kitchen``` should handle HTTP (POST) requests of receiving ```orders``` from the ```Dinning Hall``` and add received ```order``` to order list . For all received orders ```kitchen``` has to register time it was received and time is was totally prepared. Cooking time should be added to order before sending it back to ```Dinning Hall``` .

```Cooks``` should be object instances which run their logic of preparing foods on separate ```threads``` , one thread per ```cook``` . Our task is to design a mechanism which will prepare ```orders``` by using ```cooks``` as work units. It is up to us to decide how ```orders``` will be managed and how foods will be assigned to ```cooks``` in order to be prepared. Our main goal is to reduce preparation time of each order.

The ```kitchen``` has a limited number of cooking apparatus and in our case we will use only stoves and ovens. ```Cooking apparatus``` should be object instances which work independently and in parallel. ```Cooking apparatus``` are sharable resources across all ```cooks``` and we have to carefully use them.

Number and types of ```cooks``` and ```cooking apparatus``` should be configurable.

When order is prepared, meaning that all foods from order are prepared. ```Kitchen``` should perform HTTP (POST) request with prepared order details to ```Dinning Hall``` in that way returning prepared order to be served to the ```table``` .
