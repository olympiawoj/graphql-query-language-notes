# GraphQL Query Language

![course image](https://d2eip9sf3oo6c2.cloudfront.net/series/square_covers/000/000/236/thumb/EGH_GraphQLQuery_Final.png)

Transcripts for [Eve Porcello](https://egghead.io/instructors/eve-porcello) course on [egghead.io](https://egghead.io/courses/graphql-query-language).

## Description

GraphQL is gaining traction as one of the most popular ways to create an API. Regardless of which GraphQL implementation you pick, **you’ll use the QL in GraphQL — the query language — to query data, change data with mutations, and listen for data changes with subscriptions.

You need to know the Query Language regardless of the server-side implementation.  In this course, we will learn the GraphQL query language by sending an assortment of GraphQL operations to an existing API. 

To start, we’ll learn how to write queries to obtain all the data needed for an app in one response. As the course progresses, we’ll use mutations to add and change data. To wrap up, we’ll investigate GraphQL subscriptions and realtime data. 

After the course, you’ll be ready to communicate with a GraphQL API regardless of server-side implementation using the GraphQL query language.

## Send a Query with GraphQL Playground

_*"**Description**: The GraphQL Playground is an IDE for interacting with a GraphQL API. GraphQL APIs have a single endpoint. Queries are used to request specific data from that endpoint. In this lesson, we will send a query to obtain the total number of pets registered at the Pet Library."*_

Instructor: [00:00] To get started sending GraphQL queries, we'll go to `https://pet-library.moonhighway.com`. When we go to this route, a tool called GraphQL Playground will pop up in the browser.

[00:12] **GraphQL Playground** is an in-browser IDE that lets you send queries to GraphQL endpoints. The endpoint for the pet library is here in the center of the screen. **With GraphQL, there's only one endpoint**, so I need to specify what data I want by writing a query.

[00:27] _I'll write the query on the left-hand side of the screen starting with the `query` keyword._ I am going to ask for `totalPets`. How many pets are at the pet library?

```graphql
query {
  totalPets
}
```

[00:35] When I click play, the data I requested will be returned to me as **JSON**. _Notice that the shape of the query matches the shape of the response exactly. All of the fields are the same._

## Output

```graphql
{
  "data": {
    "totalPets": 25
  }
}
```

[00:46] We've sent our first query using GraphQL Playground. On the left, I wrote a `query` to describe what data I want to get from the pet library API. Then I click play which sends an **http request**, the post request to our GraphQL endpoint. I get the data back as **JSON.**


## Query a List of Objects with GraphQL
_**Description:** Now that we understand how to write a simple query to check a total value, we're going to write a query to return a list of pet objects. Along the way, we'll learn a bit more about the GraphQL query language, tackling vocabulary like selection sets and fields. All of the queries are sent to the Pet Library API._

Instructor: [00:00] Let's add onto our query a bit and request some data about the pets that are available at the pet library. If I wanted a list of all of our pets, I'm going to query the `allPets` field. I'll open up our curly braces to select `name` and `weight` for each of these pets, and then I'll click play.

```graphql
query {
  allPets {
    name
    weight
  }
  totalPets
}
```

[00:18] I'll see that `allPets` returns an array of pets with name and weight for each of them.

## Output

```graphql
{
  "data": {
    "allPets": [
      {
        "name": "Biscuit",
        "weight": 10.2
      },
      {
        "name": "Jungle",
        "weight": 9.7
      }
    ]
  }
}
```

 Also, if I collapse the `allPets` field, we'll see that `totalPets` is also being sent in the query, and we get that data as well.

[00:31] If we take a closer look at the query, _everything wrapped with curly braces is called a **selection set**._ _Each piece of data that we're requesting is called a **field**._ I can also add comments to the query by using the pound sign or hashtag `# Adding a comment to the allPets query`

[00:45] Then if I were to use this on one of the fields, we'll see that name is now removed from the query and is not returned.


## Query an Enumeration Type with GraphQL
_**Description:** In the GraphQL Query Language, an enum or enumeration type is a restricted list of values for a particular field. We'll query an enum field, category, in this lesson to find out the different pet categories. This lesson also takes a look at the GraphQL schema for the API._

Instructor: [00:00] We've sent the `allPets` query, so we have some information about the pet, their name and their weight. Now, I know that Biscuit and Jungle are cats, because I know that cats, but I might not have all of them memorized.

[00:11] We want to be able to find out what category the pet belongs to. When we're using GraphQL Playground, we can hit **control-space**. This will surface all of the different fields that are available on this query.

[00:23] Let's go ahead and add `category` to our query. When I click play, we should see `category` being returned.

```graphql
query {
  allPets {
    name
    weight
    category
  }
  totalPets
}
```

 Now, `categories` look like strings, but they all seem pretty similar. `CAT`, `DOG`, `STINGRAY`, and `RABBIT`. **GraphQL is a query language for your API, but it's also a type system for your API.**

[00:43] The **GraphQL spec** describes a **schema definition language**, which we'll use to define all of the different queries and all of the different types that are available on this API. If you click the schema tab, you can take a look at this schema.

` See 0:55 in lesson `
![Schema tab shown on the side of the playground](https://res.cloudinary.com/dg3gyk0gu/image/upload/v1563555708/transcript-images/query-an-enumeration-type-with-graphql-schema-tab.png)

[00:56] `allPets` returns a list of pets, and I can access all of the different fields on this pet type by scrolling down. Here, it says that category returns an option called `petCategory`. `petCategory` is an **enumeration type** that represents a **restricted list of options** for this field. Cat, dog, rabbit, and stingray.

[01:18] Here's another tip you can use when exploring APIs with GraphQL Playground. You can **hover** over one of these field names and **press command**. This will allow you to click on that field, and it'll take you directly to that field definition in the schema.

[01:31] We can do that for `weight`, command-click, command-click for `name`, but we can also do it for `allPets` and for `totalPets`.


## Send a Nested GraphQL Query
_**Description:** In addition to returning lists or scalar values, it’s possible to return GraphQL objects from fields. In this lesson, we’ll take a closer look at the Photo type and how it can be used to store more complex data types.._

Instructor: [00:00] I'd like to adjust this query to include a photo. I want to return a photo for each of these pets. Let's see if there's anything in the schema that'll help us out with that.

[00:09] I'm going to search for photo using this search box at the top. I can select `photo`. It looks like photo is an object. The photo type has fields for full-size image and thumbnail-size image, both of which are strings. Each pet would have a photo.

[00:26] If I query `photo`, it gives me this error message that says, `"Field photo of type photo must have a selection of subfields."`

```graphql
query {
  allPets {
    name
    weight
    category
    photo
  }
  totalPets
}
```

![Error message shown in the return panel produced from the query above](https://res.cloudinary.com/dg3gyk0gu/image/upload/v1563555709/transcript-images/send-a-nested-graphql-query-error-message.png)

 Because `photo` is an object, I'm going to need to add another **selection set** here.

```graphql
query {
  allPets {
    name
    weight
    category
    photo {
      thumb
    }
  }
  totalPets
}
```

[00:38] This allows us to have some flexibility when we are sending a query. A `photo` may have more than one field associated with it. Then we can request just the fields that we want with our GraphQL query.

```graphql
query {
  allPets {
    name
    weight
    category
    photo {
      thumb
      full
    }
  }
  totalPets
}
```


## Filter a GraphQL Query Result Using Arguments
_**Description:** Some GraphQL queries will have arguments. **Arguments** can be used to send additional options about the data set that you wish to receive. They may be used to specify sorting preferences, limit the number of records returned, or filter data. In this lesson, we will filter a list of pets by status._

Instructor: [00:00] So far, we've gotten data about all of the pets in the library. We've gotten a list of our pets. We've gotten total pets. Total pets tells us that there are 25 pets that are part of our library, but I might want to filter this list to see just the pets that are available or just the pets that are checked out.

[00:18] To do this, I'm going to add a GraphQL argument. There's an argument on the `totalPets` query called `status`. This will take in either available or checked out. If we add `Available`, we'll see that there are 20 available pets.

```graphql
query {
  totalPets (status: AVAILABLE)
  allPets {
    name
    weight
    category
    photo {
      thumb
      full
    }
  }
}
```

![Available pets shown in return panel](https://res.cloudinary.com/dg3gyk0gu/image/upload/v1563555709/transcript-images/filter-a-graphql-query-result-using-arguments-available-pets.png)

 If we change this to `CHECKEDOUT`, we'll see that the total checked out pets is 5.

```graphql
query {
  totalPets (status: CHECKEDOUT)
  allPets {
    name
    weight
    category
    photo {
      thumb
      full
    }
  }
}
```
![Checkedout pets shown in the return panel](https://res.cloudinary.com/dg3gyk0gu/image/upload/v1563555709/transcript-images/filter-a-graphql-query-result-using-arguments-checked-out-pets.png)

[00:38] If we look at the `totalPets` query in the schema, we'll see that it has this optional `status` argument.

![totalPets query shown in the graphql docs on the right of the screen](https://res.cloudinary.com/dg3gyk0gu/image/upload/v1563555708/transcript-images/filter-a-graphql-query-result-using-arguments-pets-query.png)

The value that I need to send is for `PetStatus`. `PetStatus` is an **enum**, either available or checked out. Now as we saw before, `totalPets` will work without a filter, but if I do provide a `status` filter, this will filter the list based on the value that I provide for `PetStatus`.


## Renaming Fields with GraphQL Aliases
_**Description:** When writing GraphQL queries, you may want to query the same field with different arguments. In this lesson, we’ll show the problem with naming collisions in GraphQL queries and how they can be solved with aliases._

Instructor: [00:00] Our query is telling us how many pets are checked out, but I also want to see how many pets are available. I'm going to add the `totalPets` query to line two, and I'll add the `status` `Available` as an argument.

```graphql
query {
  totalPets (status: AVAILABLE)
  totalPets (status: CHECKEDOUT)
  allPets {
    name
    weight
    category
    photo {
      thumb
      full
    }
  }
}
```

[00:13] If we try to hit play on this, we're going to see some errors returned.

![Error message shown in return panel about different arguments for the same query](https://res.cloudinary.com/dg3gyk0gu/image/upload/v1563555709/transcript-images/renaming-fields-with-graphql-aliases-error.png)

 It lets us know that fields `totalPets` conflict, because they but have differing arguments, and _it recommends that we use different **aliases** on these fields._

[00:24] If I scroll down a little further, we're going to see where this is happening, line two, column three, and line three, column three. We also see this little faint hit of red (left side), letting us know that there's some sort of a problem.

[00:36] We have a **naming collision here**. What we'll need to do is preface both of these queries with an alias. I can pick a new name for this field. I'll call it `available` and add a colon. Then I'll add `checkedOut` with a colon in front of that.

```graphql
query {
  available: totalPets (status: AVAILABLE)
  checkedOut: totalPets (status: CHECKEDOUT)
  allPets {
    name
    weight
    category
    photo {
      thumb
      full
    }
  }
}
```

[00:52] I can hit play, and now, I see that `available` and `checkedOut` are returned. The query is successful, and I've renamed these fields in our JSON response.

![queries fixed and data is returned](https://res.cloudinary.com/dg3gyk0gu/image/upload/v1563555709/transcript-images/renaming-fields-with-graphql-aliases-queries-fiexed.png)

 This means that I could also grab the `totalPets` without any filters.

```graphql
query {
  available: totalPets (status: AVAILABLE)
  checkedOut: totalPets (status: CHECKEDOUT)
  total: totalPets
  allPets {
    name
    weight
    category
    photo {
      thumb
      full
    }
  }
}
```

[01:04] This will tell us that 25 total pets are part of the library, all bundled in the same query. **Aliases can be added to any field**, so I added them to top-level queries, like `totalPets`, before. If you wanted to add them to more nested fields, you could do so.

[01:20] I could rename the photo field with an alias called `petPhoto`, and this is going to rename that in the response in all cases.

```graphql
query {
  available: totalPets (status: AVAILABLE)
  checkedOut: totalPets (status: CHECKEDOUT)
  total: totalPets
  allPets {
    name
    weight
    category
    petPhoto {
      thumb
      full
    }
  }
}
```

![Photo renamed in query and data is returned as petPhoto](https://res.cloudinary.com/dg3gyk0gu/image/upload/v1563555709/transcript-images/renaming-fields-with-graphql-aliases-photo-renamed.png)


## Use Variables to Filter a Query Result with GraphQL
_**Description:** If you want to pass dynamic arguments to a GraphQL query, you can do so with GraphQL **query variables**. In this lesson, we’ll replace static or inline values with dynamic values and pass data as JSON to the query from the Query Variables panel._

Instructor: [00:00] We'll start by opening up a new tab in GraphQL Playground by clicking the plus, and we're going to use this new tab to write a new `query`. We'll use our `allPets` query from before, but this time, we want to pass in some optional arguments.

```graphql
query {
  allPets()
}
```

[00:14] If we take a look at all pets in the schema, we'll see that category and status are potential filters for this query. I'm going to add `category:` `DOG`. We just want to see the dogs, and we just want to see those dogs that are `AVAILABLE`.


```graphql
query {
  allPets(category: DOG status: AVAILABLE) {
  }
}
```

[00:29] Now, I can make a selection for `id`, `name`, `status`, and `category`, and I'll just see those dogs that are available.

```graphql
query {
  allPets(category: DOG status: AVAILABLE) {
    id
    name
    status
    category
  }
}
```

 Right now, these values are being passed **in-line as strings** with the query, but we also have the option to provide values as **dynamic values**.

![allPets query with hard coded values passed in](https://res.cloudinary.com/dg3gyk0gu/image/upload/v1563555709/transcript-images/use-variables-to-filter-a-query-result-with-graphql-all-pets-query.png)

[00:44] Let's say you wanted to build a UI for this. You might have some dropdown filters. You'd need some sort of mechanism for passing back dynamic user input. To pass **dynamic values** with a GraphQL query, we're going to use **variables.**

[00:59] The first thing I want to do is I want to set up these variables on line one. We're going to use the dollar sign and `category`, and then we'll define what type category is, which is `petCategory`. Then we'll provide `status`, which is `petStatus`.

```graphql
query ($category: PetCategory $status: PetStatus){
  allPets(category: DOG status: AVAILABLE) {
    id
    name
    status
    category
  }
}
```

[01:13] Next, we'll provide these variables to our `allPets` query by using the dollar sign and the name of the argument.

```graphql
query ($category: PetCategory $status: PetStatus){
  allPets(category: $category status: $status) {
    id
    name
    status
    category
  }
}
```

 Now, if I click play, we're going to have these variables set up, but we're not passing any values yet. This is just going to give me everything.

![Variables set up in the query](https://res.cloudinary.com/dg3gyk0gu/image/upload/v1563555709/transcript-images/use-variables-to-filter-a-query-result-with-graphql-variables-set-up.png)

[01:28] Because those values are nullable, we can provide `null`, and this will just show me all of the pets. When I want to provide those values, I'm going to open up the **query variables panel**. Here, I'm going to **pass the values as JSON.**

#### Query variables panel

```graphql
{
  "category": "DOG"
  "status": "AVAILABLE"
}
```

[01:42] I'll provide the `category` and the `status` to find those available dogs. These values will then be passed, and our response should reflect that.

![query variables panel is set with dog and available](https://res.cloudinary.com/dg3gyk0gu/image/upload/v1563555709/transcript-images/use-variables-to-filter-a-query-result-with-graphql-query-variables-panel.png)

 **This will be dynamic**, so I can change it to `cat`. This will show me all the available cats.

```graphql
{
  "category": "CAT"
  "status": "AVAILABLE"
}
```
![Cats available arguments set](https://res.cloudinary.com/dg3gyk0gu/image/upload/v1563555709/transcript-images/use-variables-to-filter-a-query-result-with-graphql-cats-available.png)

[01:58] The GraphQL query language also gives us a way to pass default values for these variables. For example, if I set the default with an equals sign to `STINGRAY` for `PetCategory`, and I click play, the default isn't going to be used. We're still going to pull those values from the query variables.

#### GraphQL playground

```graphql
query ($category: PetCategory=STINGRAY $status: PetStatus){
  allPets(category: $category status: $status) {
    id
    name
    status
    category
  }
}
```

[02:16] However, if I remove that, and I don't pass the cat value, then it will use the default. If that value is not provided, we're going to use the default.


## Query Connected Data with the GraphQL Query Language
_**Description:** One of the most useful features of a GraphQL query is that **you can collect data about multiple types in one request.** In this video, we'll look at how to use nested fields to gather data about the Customer type and the Pet type._


Instructor: [00:00] So far, we've sent queries for the pet type, and if we search the schema for pet, we should see all the available fields. Now, there's another main type that's part of this API, and that's called customer.

[00:11] **The real power of GraphQL starts to show up when we start to talk about connecting data points.** Let's write some queries that connect the pet type with the customer type. The query that we'll send is `petById`.

[00:24] This is going to take in `id` as an argument. This is going to return Biscuit.

```graphql
query {
  petById(id: "C-1") {
    name
  }
}
```

![Pet by id query shown](https://res.cloudinary.com/dg3gyk0gu/image/upload/v1563555708/transcript-images/query-connected-data-with-the-graphql-query-language-pet-by-id.png)

 There's another field on pet called `inCareOf`. `inCareOf` is going to return the customer who has checked out this pet. Biscuit has been checked out by this customer.

```graphql
query {
  petById(id: "C-1") {
    name
    inCareOf {
      name
      username
    }
  }
}
```

![Customer information added to query](https://res.cloudinary.com/dg3gyk0gu/image/upload/v1563555708/transcript-images/query-connected-data-with-the-graphql-query-language-customer-info.png)

[00:40] To draw the line from customer to pet, we're going to use the `allCustomers` query. `allCustomers` is going to return a list of customers, so we can ask for their name, their username, and we also are going to get their `currentPets`.

```graphql
query {
  allCustomers {
     name
     username
     currentPets {
       name
     }
  }
}
```

[00:54] `currentPets` is going to return a list of any pets that they currently have checked out. That connection is made on the `currentPets` field. `allCustomers` returns a list of customers for each of those customer objects that are going to have a list of current pets that are checked out.

[01:11] This could be an empty array, or this could return a list of custom objects. To go back the other way, `inCareOf` is going to connect a pet with a customer. On the `inCareOf` field, we're going to return the customer who has checked out this pet.


## Create Operation Names for GraphQL Queries 
_**Description:** If there are **multiple GraphQL queries** in the same query document, you need to **give the query an operation name**. In this lesson, we’ll show how to get around anonymous operation errors with operation names._

Instructor: [00:00] Right now, on the left-hand side of our screen, we have a huge query. It's collecting a bunch of data about customers and then about pets. GraphQL will let us do this. We can grab information about multiple types in one query, but I want to break this down into two separate queries, one of which will be for pets. The other will be for customers.

[00:21] **As soon as I break this down into two separate queries, we're going to run into an issue. When I click play, there are two unnamed queries.**

```graphql
query {
  availablePets: totalPets(status: AVAILABLE)
  checkedOutPets: totalPets(status: CHECKOUT)
  dogs: allPets(category: DOG, status: AVAILABLE) {
    name
    weight
    status
    category
    photo {
      full
      thumb
    }
  }
}

query {
  totalCustomers
  allCustomers {
    name
    username
    dataCreated
    checkoutHistory {
      pet {
        name
      }
      checkOutDate
      checkInDate
      late
    }
  }
}
```

![Query error showing that there are two unnamed queries](https://res.cloudinary.com/dg3gyk0gu/image/upload/v1563555708/transcript-images/create-operation-names-for-graphql-queries-query-error.png)

 If I click the second one of these, it says, `"This anonymous operation must be the only defined operation."` If there's more than one query in your query document, you're going to need to give both of these a name.

[00:40] Right now, these are anonymous queries. Think of those like anonymous functions. We need to give them a name. I'll call the first one, "`PetPage`." I'll call the second one, "`CustomerPage`."

```graphql
query PetPage{
  availablePets: totalPets(status: AVAILABLE)
  checkedOutPets: totalPets(status: CHECKOUT)
  dogs: allPets(category: DOG, status: AVAILABLE) {
    name
    weight
    status
    category
    photo {
      full
      thumb
    }
  }
}

query CustomerPage{
  totalCustomers
  allCustomers {
    name
    username
    dataCreated
    checkoutHistory {
      pet {
        name
      }
      checkOutDate
      checkInDate
      late
    }
  }
}
```

Now if I select the play button, we'll see the drop-down. We'll also see the operation name, so I can execute these queries one at a time.

![Query drop down shown under the play button in graphiql playground](https://res.cloudinary.com/dg3gyk0gu/image/upload/v1563555709/transcript-images/create-operation-names-for-graphql-queries-query-drop-down.png)

[01:00] Just to recap, whenever you have more than one query inside of a query document, you need to give it an operation name. The operation name can be whatever you want to call it but conventionally is capitalized.


## Use an Input Type to Create an Account with a GraphQL Mutation 
_**Description:** Mutations are another type of GraphQL operation that are similar to queries, but they are used when you need to change any data on the backend. In this lesson, we will send mutations to register new users._


Instructor: [00:00] _To change data with GraphQL, we use a **mutation**._ These are named just like queries and the schema. Within the schema, we have a mutation called createAccount. Let's write that mutation.

[00:10] We're going to use the `mutation` keyword. We're going to use the name of the mutation `createAccount`. It looks it takes in something called an input, which is `createAccount` input.

```graphql
mutation {
  createAccount
}
```

[00:21] If we scroll down a little bit and click on the input, we're going to see that `createAccount` is actually a wrapper around name, user name, and password.

![Create account is a wrapper](../images/use-an-input-type-to-create-an-account-with-a-graphql-mutation-create-account.png)

**Every time I create an account, I'm going to need to provide those things.**

[00:33] Now here's where the input comes in handy. Instead of sending all of these variables one at a time, I can wrap them in the input, and then I can send them as one thing.

[00:42] Here I am going to use the `input`, and I am going to pass in the `input` as a variable. We'll set this up at the top. Input is of type `CreateAccountInput`, and is non-nullable. We'll use the exclamation point. We'll set up this argument to take in the input.

```graphql
mutation ($input: CreateAccountInput!) {
  createAccount(input: $input) {}
}
```

[00:58] I'll use the query variables panel to pass in these variables, but this time we are going to put everything on that `input` key. We're going to nest in object here with `name`, with `username`, and with `password`. Now that I have these input values defined, I need to return something from this mutation.

#### Query variables panel

```graphql
{
  "input": {
    "name": "Eve Porcello",
    "username": "ep123",
    "password": "pass"
  }
}
```

[01:17] What this mutation returns is a customer object. This will give us access to all of the fields on the customer. Whenever I create my account, it's going to echo back those account details that I provided in the input. We'll ask for `username` and `name`.

[01:33] When we send this mutation, we're going to send all of the values from the input object. We're going to get back the `username` and the `name` for the customer that's just been created.

[01:42] `CreateAccount` takes in an `input` called `createAccountInput`. We pass the variables in the query variables panel, and then the mutation returns a customer object so I can access those values that I've just supplied.

![Account information returned after createAccount mutation ran](https://res.cloudinary.com/dg3gyk0gu/image/upload/v1563555710/transcript-images/egghead-use-an-input-type-to-create-an-account-with-a-graphql-mutation-account-info-returned.png)


## Authenticate a User with a GraphQL Mutation
_**Description:** Mutations give you the ability to invoke backend functions from the client. In this lesson, we will use a mutation to authenticate a user with their username and password. Authorized users will receive a token that can be used to identify the current user in future operations._


Instructor: [00:00] Now that we have an account, we can log in. Let's look at our mutation's list. We should see that there is a logIn mutation. I'm going to go ahead and write that here in our query document. We'll use `logIn` with the capital I. We'll use our `username`, our `password`.

```graphql
mutation {
  logIn(username: "ep123" password: "pass") {

  }
}
```

[00:16] What's returned from the `logIn` mutation is a type called the `logIn` payload. This is a custom object that returns both the customer, all the of the customer details, and the user token. We're going to use the user token to validate that the user is authorized.

![Log in schema](https://res.cloudinary.com/dg3gyk0gu/image/upload/v1563555709/transcript-images/authenticate-a-user-with-a-graphql-mutation-log-in-schema.png)

[00:31] When we send the `logIn` mutation, we're going to have access to all of the customer details. Grab their `name`. We're going to grab the `token`.

```graphql
mutation {
  logIn(username: "ep123" password: "pass") {
    customer {
      name
    }
    token
  }
}
```

[00:40] Let's go ahead and hit play. We see our customer name, which is my name that I provided when I created my account. I also see my token.

![Customer information returned](https://res.cloudinary.com/dg3gyk0gu/image/upload/v1563555709/transcript-images/authenticate-a-user-with-a-graphql-mutation-customer-info.png)

We're going to place this in another panel here at the bottom called HTTP headers.

[00:53] Now, this is easy to get mixed up with query variables. We'll make sure that we're in the HTTP header section and we'll add the authorization key. We'll add `Bearer`. We'll paste in this token.

#### HTTP Headers panel

```graphql
{
  "Authorization": "Bearer your-token-here"
}
```

[01:07] Once I provide this token in the HTTP headers, I'm going to be able to send queries that are only for authorized uses. Now the query I am going to send here is called "`me`". Me is going to give me information about myself, the currently authenticated user.

#### GraphQL playground

```graphql
query {
  me {

  }
}
```

[01:23] The Me query returns customer details for anyone who's logged in. Here I'll query the name field. I'm going to add an operation `name`, because I have two different operations here in my query document. I'll call query `Me`, and I'll call the mutation `LogIn`.

```graphql
query Me {
  me {
   name
  }
}

mutation LogIn {
```

[01:37] Now, I can send this query and I should see all of the details for myself, because I am a logged in user. Since I'm logged in, I'll be able to check in and check out pets.


## Send a checkOut Mutation with GraphQL as an Authorized User
_**Description:** Once logged in, the user will be able to check out pets with a checkOut mutation. In this lesson, we’ll look at how to send a mutation based on current data._

Instructor: [00:00] The next step I want to take is to actually check out a pet. The checkout mutation takes in an ID of a pet to check out. I need to look at the pets that are currently available and find a pet to check out.

[00:10] I'll send the `allPets` query with a `status` `Available` filter. I'll grab the `id`, the `name`, and the `category`.

```graphql
query {
  allPets(status: AVAILABLE) {
    id
    name
    category
  }
}
```

 Let me scroll down a little bit to find out who I want to check out. I want to check out this `STINGRAY` called `Switchblade`.

![Pets available to checkout](https://res.cloudinary.com/dg3gyk0gu/image/upload/v1563555708/transcript-images/send-a-checkout-mutation-with-graphql-as-an-authorized-user-pet-checkout.png)

[00:23] In order to do so, I'm going to need to send `S-2` as an ID. At this point, I can add one more mutation to our query document. Our mutation is going to be called `checkout`. We'll give the mutation an operation name of `Checkout`, and we'll use the `checkOut` mutation.

[00:40] We'll pass the `id` of `S-2`, and we want to grab details about the pet, so we'll grab their name. Let's also get some details about the customer. The customer is going to be me, but I'll grab my name as well.

```graphql
mutation Checkout {
  checkOut(id: "S-2") {
    pet {
      name
    }
    customer {
      name
    }
  }
}
query Me {
  me {
    name
  }
}
```

 When I send the `Checkout` mutation, I should see the pet has been checked out, and I see the customer who has checked that pet out.

![Checkout mutation is working with data returned](https://res.cloudinary.com/dg3gyk0gu/image/upload/v1563555709/transcript-images/send-a-checkout-mutation-with-graphql-as-an-authorized-user-checkout-mutation.png)

[01:02] If we look at the checkout mutation, we see that this returns the checkout payload. The checkout payload gives us the entire customer object, the entire pet object, and the checkout date. These custom response objects that we can return for mutations are pretty cool. We're able to grab customer fields, pet details, and the checkout date, all bundled into one object.


## Change Check In Status with a GraphQL Mutation
_**Description:** Following up on the checkOut mutation from the previous video, we’ll take a closer look at how to check in a pet using a pet id. Then we’ll look at the allCustomers query and find out the history of pet checkouts for all customers._

Instructor: [00:00] We had a lot of fun with the pet, but now it's time to check them back in. We have another named mutation in the schema called `CheckIn`, and this should going to take in the ID of the pet we want to check in.

[00:11] Remember, we need to have our authorization token present in order to do this. Let's check in that same pet that we checked out, `S-2`. The `checkIn` mutation returns an object called `checkout`. Let's figure out what that is.

[00:26] `Checkout` returns a pet, a checkout date, a check-in date, and whether or not the pet is late. There are a bunch of different metadata fields on that type. Let's look at the name of the pet, get the `checkOutDate`, get the `checkInDate`, and whether or not the pet is `late`.

```graphql
mutation CheckIn {
  checkIn(id: "S-2") {
    pet {
      name
    }
    checkOutDate
    checkInDate
    late
  }
}
mutation Checkout {
  checkOut(id: "S-2") {
    pet {
      name
    }
    customer {
      name
    }
  }
}
```

[00:42] When I click play on this, we should see all of those different fields for our pet.

![Checked in mutation shows Switchblade checked in](https://res.cloudinary.com/dg3gyk0gu/image/upload/v1563555708/transcript-images/change-check-in-status-with-a-graphql-mutation-check-in-mutation.png)

 Now that we've checked out and checked in a pet, we can look at this data on this query called `allCustomers`. We can query `username`, which should return all of the usernames to us.

```graphql
query {
  allCustomers {
    username
  }
}
```

![All usernames returned](https://res.cloudinary.com/dg3gyk0gu/image/upload/v1563555709/transcript-images/change-check-in-status-with-a-graphql-mutation-usernames-returned.png)

[00:56] We see my `username` here at the bottom. Then I can also add `checkoutHistory`. Now, `checkoutHistory` is going to return a list of checkouts. That `checkout` object we saw before should have all of the pet details.

```graphql
query {
  allCustomers {
    username
    checkoutHistory {
      pet {
        id
        name
      }
    }
  }
}
```

[01:11] We see the pet details for other folks, and we see `Switchblade` here toward the bottom. For each of them, we can find the name of the pet and the ID.


## Reuse GraphQL Selection Sets with Fragments
_**Description:** Fragments are selection sets that can be used across multiple queries. They allow you to refactor redundant selection sets, and they are essential when querying unions or interface types. In this lesson, we will improve our query logic by creating a fragment for the activity selection set._

Instructor: [00:00] Let's start by adding a query for `allPets`, and we want to filter this by `category`. We'll look for all the rabbits, and we'll also filter by `status` to find the `AVAILABLE` rabbits. Next, we want to select a few fields.


```graphql
query {
  allPets(category: RABBIT, status: AVAILABLE) {}
}
```

[00:12] We'll grab `name`, `weight`, and `category`, and we should see all of these available rabbits. We also want to grab their `status`.

```graphql
query {
  allPets(category: RABBIT, status: AVAILABLE) {
    name
    weight
    category
    status
  }
}
```

 If only there was a way in the GraphQL query language to make all of these selection sets a little bit more reusable.

[00:27] The good news is, there is. I'm going to create a `fragment` called `PetDetails`. Think of a fragment of being a wrapper around several fields. `PetDetails` is going to be a fragment on a certain type. This is going to be for `Pet`.

```graphql
fragment PetDetails on Pet {

}
```

[00:40] Then we're going to cut and paste all of these fields into the fragment, and we'll use spread syntax, `...`, to push all of the `PetDetails` into this query.

```graphql
query {
  allPets(category: RABBIT, status: AVAILABLE) {
    ...PetDetails
  }
}

fragment PetDetails on Pet {
  name
    weight
    category
    status
}
```

 We're able to send the query and see that all of the field are still being sent with the query.

![Query still working while useing fragment](https://res.cloudinary.com/dg3gyk0gu/image/upload/v1563555709/transcript-images/reuse-graphql-selection-sets-with-fragments-query-still-working.png)

[00:54] Now, we can also adjust the fragment to add additional fields. If I wanted to grab the `photo` as well as the `thumbnail`, we can see that the thumbnail for the photo is being returned.

```graphql
fragment PetDetails on Pet {
  name
    weight
    category
    status
    photo {
      thumb
    }
}
```

![Photo & thumbnail returned as well with fragment](https://res.cloudinary.com/dg3gyk0gu/image/upload/v1563555708/transcript-images/reuse-graphql-selection-sets-with-fragments-photo-thumbnail.png)

 Let's add onto this query a little bit and add `petByID`.

[01:08] We're going to look for the pet `C-1`, and here, we can reuse `PetDetails` inside of the query so that we don't have to retype them.

```graphql
query {
  petById(id: "C-1") {
    ...PetDetails

  }
  allPets(category: RABBIT, status: AVAILABLE) {
    ...PetDetails
  }
}
```

 There we go, we get all of those fields for that pet. You can also add additional fields alongside your fragment.

[01:24] Let's add `inCareOf`, `name`, and `username`.

```graphql
query {
  petById(id: "C-1") {
    ...PetDetails
    inCareOf {
      name
      username
```

 We should see the person who has checked out this pet. Since we're having so much fun with fragments, we can create a fragment for `CustomerDetails`. We're going to specify that these fields come from the `customer` type, and we can add `name` and `username` from `inCareOf`.

[01:43] We can then push those fields into the query, hit prettify to get rid of that spacing issue again, and click play.

```graphql
query {
  petById(id: "C-1") {
    ...PetDetails
    inCareOf {
      ...CustomerDetails
    }
  }
  allPets(category: RABBIT, status: AVAILABLE) {
    ...PetDetails
  }
}

fragment CustomerDetails on Customer {
  name
  username
}
```

![All fields are returned](https://res.cloudinary.com/dg3gyk0gu/image/upload/v1563555708/transcript-images/reuse-graphql-selection-sets-with-fragments-all-fields-returned.png)

 We see all of those fields are being returned. We created a fragment called `PetDetails`. This will take all of these fields and put them into the query.

[01:57] Then we have another for `CustomerDetails`. All of the fields we want are going to be pushed into the query when we use that fragment syntax, ..., and then the name of the fragment.


## Explore Refactored GraphQL Queries
_**Description:** In this lesson, we’ll look at a refactored Pet Library which includes a range of new queries that aim to **minimize argument usage and naming collisions that require aliases**. To follow along with these queries, go to the Pet Library GraphQL Playground.._


Instructor: [00:00] The pet library just got some funding, some VC money, so we're going to open up our browser and head over to the new version of the app. We're going to go to `https://funded-pet-library.moonhighway.com`.

[00:13] You'll notice our new endpoint here at the center of the screen. With a larger budget comes more engineers and some enhancements to our API, one of which is that **we have some more specific queries that may be easier to track.**

[00:28] Let's write our `query` for `totalPets`.

```graphql
query {
  totalPets
}
```

 We'll see 25, but we have these new queries here, `availablePets`. We also have `checkedOutPet`, and we can access those values without having to use any filters or send any arguments.

```graphql
query {
  totalPets
  availablePets
  checkedOutPets
}
```

[00:43] We also have another query here called `customersWithPets`.

```graphql
query {
  customersWithPets {

  }
  totalPets
  availablePets
  checkedOutPets
}
```

 Now, if we look at this in the schema, we'll see that this query will return a list of all of the customers who currently have pets checked out.

[00:54] _This refactor gives us access to the same data, but we don't have to use as many arguments, and we've moved a lot of the logic of filtering, sorting to the server instead of having to handle this in the playground._

```graphql
query {
  customersWithPets {
    username
    name
  }
  totalPets
  availablePets
  checkedOutPets
}
```

![New query data returned](https://res.cloudinary.com/dg3gyk0gu/image/upload/v1563555709/transcript-images/explore-refactored-graphql-queries-query-data.png)


## Query GraphQL Interface Types in GraphQL Playground
_**Description:** Interfaces are similar to Unions in that they provide a mechanism for dealing with different types of data. However, an interface is more suited for data types that include many of the same fields. In this lesson, we will query different types of pets._

To follow along with these queries, go to the Pet Library GraphQL Playground._

Instructor: [00:00] If we sum the `allPets` query for `id` and `name`, we are going to see ID and name returned to us, just as we expect, but in the funded pet library, these data relationships are designed pretty differently.

[00:11] Let's open up the `AllPets` query in our schema and we'll see that the pet is no longer a type, but instead it's an **interface**.

[00:18] An **interface** is an abstract type that includes a set of fields. These fields must be used when creating new instances of that interface. We have an interface called `Pet`. This is the base class. It has certain fields on it.

[00:32] We have several different implementations of that interface. Remember, our numerator from before that had `Cat`, `Dog`, `Rabbit`, and `Stingray`, these are now implementations of this interface.

[00:44] If we scroll down to the `Cat`, we'll see that `Cat` is a separate type that implements the `Pet` interface. It has all of the different fields that are part of that interface, but then we can extent the `Cat` to have a couple of different ones, so sleep amount and curious are now available on that `Cat`.

[01:00] Let's click on `Stingray`. That's another type. We have a couple of extra fields defined on that as well. Same with the `Rabbit` and the `Dog`. `allPets` returns a list of pets, but all of these pets are different types, different implementations of the `Pet` interface.

[01:16] If I want to see which one is which, I can query the `_ _` `typename` field, this will give me the data about what type of pet we're querying.

```graphql
query {
  allPets {
    __typename
    id
    name
  }
}
```

 The way that we write a query for an interface is also a little bit different.

[01:29] Remember those extra fields that we had on the cat. I can use an inline fragment, `... on Cat`, and then I can query that field. Whenever there is a cat in the response, we're going to see a `sleepAmount` value for the cat. For all other types, we're not going to see that extra field.

```graphql
query {
  allPets {
    __typename
    id
    name
    ... on Cat {
      sleepAmount
    }
  }
}
```

![Sleep Amount for all cats shown](https://res.cloudinary.com/dg3gyk0gu/image/upload/v1563555708/transcript-images/query-graphql-interface-types-in-graphql-playground-sleep-amount.png)

[01:47] We can do this for any additional fields that we'd like to. I can say `on Stingray` and get how `chill` the stingray is on a scale of 1 to 10. This will be provided in the response only for stingrays.

```graphql
query {
  allPets {
    __typename
    id
    name
    ... on Cat {
      sleepAmount
    }
    ... on Stimgray {
      chill
    }
  }
}
```

[02:00] An interface gives us a little bit more flexibility when we're designing our domain's objects. We have a pet, that is an interface. Some base fields on that pet interface, and then we can create implementations of that interface and then create custom fields for each additional pet type.


## Query Lists of Multiple Types using a Union in GraphQL
_**Description:** Unions are used when we want a GraphQL field or list to handle multiple types of data. With a Union Type, we can define a field that could resolve completely different types of data. In this lesson, we will write a query that obtains a list of different pet types._

Instructor: [00:00] In the Funded Pet Library there are a couple of extra queries here that we haven't looked at yet. One is called FamilyPets and the other one is called ExoticPets.

[00:08] Let's click on FamilyPets. We should see that there is some helpful documentation here. It says this query returns a list of FamilyPets, either a cat or a dog.

[00:18] Another useful data structure when we're working with GraphQL is called a union. **We use unions when we want to return lists of multiple types.**

[00:26] Whenever we query the FamilyPets field, I want to return a list of FamilyPets, either a cat or a dog. Let's go ahead and write that `query`.

[00:35] We're going to say `familyPets`. Let's look for the `typename` here.

```graphql
query {
  familyPets {
    __typename
  }
}
```

 Now let me click Play. I should see Cat, Cat, Cat. As I scroll down, we should see Dog, Dog, Dog, etc.

![Family Pets types shown in returned data](https://res.cloudinary.com/dg3gyk0gu/image/upload/v1563555709/transcript-images/query-lists-of-multiple-types-using-a-union-in-graphql-family-pets.png)

[00:47]**Unions don't have to share any fields.** If I tried to query for the name field for each of these, we're going to see an error. It says cannot query field name on type FamilyPet. It's recommending that I use an inline fragment instead. Let's go ahead and do that.

[01:03] I'm going to use the inline fragment syntax to grab fields from the `Cat`. I'll say name and sleep amount. This should give us that data for just the cats. Dogs will still only have the type name.

```graphql
query {
  familyPets {
    __typename
    ... on Cat {
      name
      sleepAmount
    }
  }
}
```

![On Cat fragment shows sleepAmount for all cats](https://res.cloudinary.com/dg3gyk0gu/image/upload/v1563555709/transcript-images/query-lists-of-multiple-types-using-a-union-in-graphql-on-cat.png)

[01:17] If I wanted some dog-specific fields, I could get them with another **in-line fragment**. We'll use `... on Dog` and see their `name` and whether or not the dog is `good`.

```graphql
query {
  familyPets {
    __typename
    ... on Cat {
      name
      sleepAmount
    }
    ... on Dog {
      name
      good
    }
  }
}
```

[01:30] **We use unions whenever we want to return a list of multiple types.** These are really useful for things like this with `FamilyPets` being either a `Cat` or a `Dog`, `ExoticPets` with a `Rabbit` or a `Stingray`.

[01:43] You could use this for a number union that returns either an Int or a Float. A lot of different ways to use this but it's a useful data structure to know when you're working with GraphQL.


## Listen for Data Changes in Real-time with a GraphQL Subscription
_**Description:** A GraphQL API can push new data to the client with the Subscription Type. In this lesson, we’ll listen to checkIn and checkOut mutations in real time._

Instructor: [00:00] At this point, we've requested data with queries, we've changed data with mutations. There's one more operation type, though, with GraphQL. That is a GraphQL subscription. Let's say we wanted to set up a real-time listener for anytime a pet is returned.

[00:16] The way that we would do this is to use a subscription. Let's close the schema, and in order to check in a pet, we have to check out a pet. In order to do either of those things, we have to be logged in. Let's make sure that we're logged in.

[00:30] We're going to provide our username and our password, and now, I can log in.

```graphql
mutation {
  logIn(username: "ep123" password: "123") {
    customer {
      username
      name
    }
    token
  }
}
```

 The next thing I want to do is grab the token so that I can make myself an authorized user. I will add this to the HTTP headers under the authorization key. We'll add `Bearer` and paste the token.

#### HTTP Headers panel

```graphql
{
  "Authorization": "Bearer your-token-here"
}
```

[00:50] The nex

#### GraphQL playground
```graphql
subscription {
  petReturned {

  }
}
```

[01:03] The `petReturned` subscription returns something. What this returns is the checkout object. We have access to the pet, the checkout date, the check-in date, and whether or not the pet was late.

[01:16] Here, we want to add the `pet` query, and then we'll add `name`.

```graphql
subscription {
  petReturned {
    pet {
      name
    }
  }
}
```

 As soon as I click play, we're going to notice some different behavior. This is listening for any changes, so it's not a request and a response. Instead, we're listening over a web socket.

![GraphQL subscription listening for changes](https://res.cloudinary.com/dg3gyk0gu/image/upload/v1563555708/transcript-images/listen-for-data-changes-in-real-time-with-a-graphql-subscription-listening-changes.png)

[01:30] Now, let's open up one final tab here. We're going to add a `mutation` for checkout, and the `pet` that I want to check out is going to be `C-2`. I'll get the `pet` and their `name`, then I'll change this to checkin to check-in the same pet.

```graphql
mutation {
  checkIn(id: "C-2") {
    pet {
      name
    }
  }
}
```

[01:46] Now, if I switch back to our subscription, we see that the `petReturned` subscription has heard some new data.

![New Data returned after pet is checked in](https://res.cloudinary.com/dg3gyk0gu/image/upload/v1563555708/transcript-images/listen-for-data-changes-in-real-time-with-a-graphql-subscription-data.png)

 We've echoed back this checkout object. The GraphQL subscription is very useful. Whenever you have any real-time needs in your application, you're going to use a subscription. Then we can use a mutation to trigger that change.


## Query a GraphQL API's Types With Introspection Queries
_**Description:** Introspection is the ability to query information about a GraphQL API’s schema. In this lesson, we will write queries that will return information about the pet library schema.._

Instructor: [00:00] Throughout this process, we've used the schema tab to look at all the available queries and types on this API. There's also a way that we can use the query language itself to look at, or introspect, the schema.

[00:12] We're going to send some introspection queries to look at the schema for this API. The first thing we're going to `query` is `__schema`. This will show us the schema for this server. We can query all of the `types` on the types query, then let's look at the `name`, the `kind`, and the `description`.

```graphql
query {
  __schema {
    types {
      name
      kind
      description
    }
  }
}
```

[00:32] I'm going to go ahead and click play on this, and we should see all of the different types.

![All schema's types returned](https://res.cloudinary.com/dg3gyk0gu/image/upload/v1563555708/transcript-images/query-a-graphql-api-s-types-with-introspection-queries-all-schemas.png)

 Now, let's write a query for just the `customer` type. We're going to query `__type`. We're going to use the type name. Let's add `Customer` as a string.

[00:50] Then we if want to find out what fields are available on the customer type, we can query `fields`, `name`, and `description` for each.

```graphql
query Customer {
  __type(name: "Customer") {
    fields {
      name
      description
    }
  }
}

query AllTypes {
  __schema {
    types {
      name
      kind
      description
    }
  }
}
```

 Now, if I look at customer, we see `username`, `name`, `dateCreated`, `currentPets`, all of the fields that we're familiar with, along with their `descriptions`.

![Customer schema shown in the return panel](https://res.cloudinary.com/dg3gyk0gu/image/upload/v1563555709/transcript-images/query-a-graphql-api-s-types-with-introspection-queries-customer-schema.png)

[01:09] What if we wanted to ask our schema which queries are available on this API? Let's write another query for `AvailableQueries`. We'll look at the `schema`, and then we'll look at `queryType`. Query type, we want to look at the fields, so what `fields` are available in that query type.

[01:26] Then let's look at the `name` and the `description`.

```graphql
query AvailableQueries {
  __schema {
    queryType {
      fields {
        name
        description
      }
    }
  }
}
```

 We should see, if we click on `AvailableQueries`, `totalPets`, `availablePets`, `checkedOutPets`, all of the queries on this API.

 ![Available queries returned in right panel](https://res.cloudinary.com/dg3gyk0gu/image/upload/v1563555708/transcript-images/query-a-graphql-api-s-types-with-introspection-queries-available-queries.png)

The final query I want to send is for the pet interface.

[01:41] Let's write a query for `InterfaceTypes`. We're going to look at `__type`. We'll look for the `pet` interface. Then we can look for the `kind`.

```graphql
query InterfaceTypes {
  __type(name: "Pet") {
    kind
  }
}
```

 This will give us the kind, the interface. We're going to figure out the `name`, the `description`.

[02:01] This should give us the pet and the description for that pet interface. Then finally, if you want to see all of the different implementations of that interface, all you need to do is look at `possibleTypes`. Then we'll query `name`, `kind`, `description`. There we go. `Cat`, `Dog`, `Rabbit`, and `Stingray`.

```graphql
query InterfaceTypes {
  __type(name: "Pet") {
    kind
    name
    description
    possibleTypes {
      name
      kind
      description
    }
  }
}
```

![Query interface types shown](https://res.cloudinary.com/dg3gyk0gu/image/upload/v1563555708/transcript-images/query-a-graphql-api-s-types-with-introspection-queries-interface.png)
