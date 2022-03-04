---
layout: post
title: GraphQL for Backend using Python
date: 2021-11-03 00:00:00 +0300
description: Exploring the integration of GraphQL/Graphene on the backend as well as showing two working examples with Tornado and Flask. # Add post description (optional)
img: graphql-with-pyton.jpg # Add image post (optional)
tags: [Python, Software] # add tag
---

GraphQL has been gaining a lot of popularity in recent years. This API Technology represents a query language which exposes the data available on the backend in a very intuitive way, enabling developers to make requests to a single endpoint. It can be used in any context where an API is required. It can be implemented in any programming language that can be used to build a web server. I decided to keep it simple and provide two basic examples on Python's most popular web servers: Flask and Tornado. Both examples make use of Graphene-Python.

### What is Graphene-Python?

[Graphene-Python][graphene-python] is a library that aids with creating the Python Objects which GraphQL understands. It helps with structuring the code neatly, so developers can write the classes which will be further used to understand certain queries (read operations), mutations (update operations) or subscriptions. Once these classes have been built, the final step would involve building the Schema which takes in as arguments the query, mutation and/or subscription classes. The following code snippet shows how easy it is to build the objects and have a schema executing a query:

{% highlight ruby %}
from graphene import ObjectType, String, Schema

class ShoppingQuery(ObjectType):
    fruits = List()
    supermarket = String()

    def resolve_fruits(root, info): # resolver method for fruits
        return f'Shopping list: {info.context.get('fruits')}'

    def resolve_supermarket(root, info): # resolver method for supermarket
        return f'My favourite supermarket is: {info.context.get('supermarket')}'

schema = Schema(query=ShoppingQuery)
result_fruits = schema.execute('{ fruits }', context={'fruits': ['apple', 'bananas', 'oranges']})
assert result_fruits.data['fruits'] == ['apple', 'bananas', 'oranges']

result_supermarket = schema.execute('{ supermarket }', context={'supermarket': 'Whole Foods'})
assert result_supermarket.data['supermarket'] == 'Whole Foods'
{% endhighlight %}

The Query class has 2 parts: the field(s) and the resolver(s) function(s). In this example, the fields 'fruits' and 'supermarket' are described as List and, respectively, String. However, they can also be Int, Enum or Object. For more details refer to the [Graphene Types Reference][types-reference]. For each field, there is a resolver function which is responsible for fetching the data requested by the client's query. The resolver functions naming convention is the prefix 'resolver_' followed by the name of its corresponding field/ Once the schema is built, it can only execute the queries defined earlier which are fruits and supermarket. You might also wonder what the context argument is. In short, the context is an object shared by all the resolvers and it is used most of the times to provide access to a database. And that's all you needed to know for the first example!


### Integrating Graphene with Flask

[Flask][flask] is a web framework which provide developers with tools in order to build a web application. It is a light micro-framework with little to no dependencies to external libraries. For absolute beginners, I recommend this [excellent tutorial in Flask][flask-tutorial].

To make things more interesting, in the example below I created a new schema which connects to a HR database. The SQL database contains details about employees and their departments. Check [the following example][flask-example] on how to create the database locally and for more reference on the code.

{% highlight ruby %}
app = Flask(__name__)
app.debug = True

app.add_url_rule(
    '/graphql',
    view_func=GraphQLView.as_view(
        'graphql',
        schema=schema,
        graphiql=True # for having the GraphiQL interface
    )
)
{% endhighlight %}

Unlike a RESTful API, there is only a single endpoint from which GraphQL is exposed. The example below shows how GraphQL schema can be accessed under '/graphql. It also uses the GraphQLView interface for executing the queries easily via the browser directly instead of executing the queries via code as we've seen in the earlier example. The picture below queries for all employees within an organization. The results are displayed via the GraphiQL interface.

![GrahpiQL Interface Flask and GraphQL]({{site.baseurl}}/assets/img/flask-graphql.png)

### Integrating Graphene with Tornado

[Tornado][tornado] is a web framework and asynchronous networking library. It is mostly used for applications which require a long-lived connection to each user, hence making it ideal for WebSockets. 

There are 2 key differences between Tornado and Flask:
1) Tornado is not using WSGI

>A mediator is required for carrying out the interaction between the web servers and the Python application. So, the standard for carrying out communication between the web server and Python application is [WSGI][wsgi] (Web Server Gateway Interface). [...] the WSGI server is responsible for handling the requests from the web server and takes decision for carrying out the communication of those requests to an application framework’s process. Here, we can divide the responsibilities among the servers for scaling web traffic.

2) Tornado is integrated with the standard Python library asyncio module and shares the same event loop.

>[asyncio][asyncio] is a library to write concurrent code using the async/await syntax.

In this example, I used the extra library [Graphene-Tornado][graphene-tornado] which offers support with integrating the GraphQL schema with the Tornado Handlers. The query has the field planets and the resolver resolve_planets and once executed, it will make an async request to the [Star Wars API][star-wars] which will return all the planets.

{% highlight ruby %}
class QueryPlanet(graphene.ObjectType):
    planets = graphene.Field(Planets)

    async def resolve_planets(self, info):
        http_client = AsyncHTTPClient()
        response = await http_client.fetch('https://swapi.dev/api/planets')
        print('Info: ', info)
        pprint(json.loads(response.body))
        planets = json.loads(response.body)
        return Planets(count=planets.get('count'),
                       next=planets.get('next'),
                       previous=planets.get('previous'),
                       results=planets.get('results'))
{% endhighlight %}

The picture below executes the query and displays all the planets available from the API.

![GrahpiQL Interface Tornado and GraphQL]({{site.baseurl}}/assets/img/tornado-graphql.png)

In conclusion, this API technology called GrahpQL can be used with any web servers in Python. You can even have a web server with multiple endpoints where some expose RESTful APIs and some GraphQL APIs. This comes especially handy for larger applications which can be easily adapted to send requests to both RESTful APIs and GraphQL APIs endpoints.

### Github Repo

The full code for the examples above can be found at the [graphql-example github repository][graphql-example]

[graphene-python]: https://graphene-python.org/
[types-reference]: https://docs.graphene-python.org/en/latest/types/#typesreference 
[flask]: https://flask.palletsprojects.com/en/2.0.x/
[flask-tutorial]: https://www.mygreatlearning.com/blog/everything-you-need-to-know-about-flask-for-beginners/
[flask-example]: https://docs.graphene-python.org/projects/sqlalchemy/en/latest/tutorial/.
[tornado]: https://www.tornadoweb.org/en/stable/
[wsgi]: https://medium.com/analytics-vidhya/what-is-wsgi-web-server-gateway-interface-ed2d290449e
[asyncio]: https://docs.python.org/3/library/asyncio.html
[star-wars]: https://swapi.dev/
[graphene-tornado]: https://github.com/graphql-python/graphene-tornado
[graphql-example]: https://github.com/andreeaionescu/graphql-example/tree/main/tornado_graphql_api