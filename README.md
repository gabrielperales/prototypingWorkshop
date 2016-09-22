# Deployd-ngAdmin-workshop

Let's started! In this workshop we are going to create a small
application using [deployd](https://github.com/deployd/deployd) and
[ng-admin](https://github.com/marmelab/ng-admin). Deployd is a tool for
building backends with RESTful APIs very easy, and ng-admin is a
administration app done with angular which is going to consume the API
we are going to build with deployd.

## Install mongodb

```bash
$ brew install mongodb
```

## Step 1. Installing deployd

```bash
$ npm install -g deployd

$ dpd create prototypingWorkshop

$ cd prototypingWorkshop
```

Now that we are in the folder with our brand new app we can launch
deployd runing `dpd`

```bash
$ dpd
```

Now you can check [`localhost:2403`](http://localhost:2403) and you
should see a welcome page. Also, you can check
[`localhost:2403/dashboard`](http://localhost:2403/dashboard) and the
deployd dashboard will show up.

You can go directly to this step checking out the step1 branch of this
repository:

```bash
$ git checkout step1

$ dpd
```

## Step 2. Setting up ng-admin

Donwload the last version of ng-admin from
[ng-admin](https://github.com/marmelab/ng-admin/archive/v0.9.1.zip).
From the build folder take `ng-admin.min.css` and `ng-admin.min.js` and
copy them inside of the `public/` folder of our project. Open up the
`index.html` file with your favourite text editor and leave like this:

```html
<!DOCTYPE html>
<html>
<head>
  <link rel="stylesheet" href="ng-admin.min.css">
</head>
<body ng-app="prototype">
  <div ui-view></div>
  <script type="text/javascript"
src="http://code.jquery.com/jquery-latest.min.js"></script>
  <script type="text/javascript" src="dpd.js"></script>
  <script type="text/javascript" src="ng-admin.min.js"></script>
  <script type="text/javascript">
  var dashboard = angular.module('prototype', ['ng-admin']);
  dashboard.config(['NgAdminConfigurationProvider',
    function(NgAdminConfigurationProvider) {
        var nga = NgAdminConfigurationProvider;
        // create an admin application
        var admin = nga.application('Prototyping Workshop');
        // more configuration here later
        // attach the admin application to the DOM and run it
        nga.configure(admin);
    }]);
  </script>
</body>
</html>
```

As you can see in the 4th line we are adding a reference to our
ng-admin css, also we have added an attribute `ng-app="prototype"`
to our body tag in the line 6. `prototype` makes reference to an angular
module, to be more concrete that is the module that where we are going
to create our dashboard.

The definition of that module comes on the line 12.

You can go directly up to this point checking out the `step2` branch in
git as we did in the first step.

## Step 3. Creating our first collection

Now we are going to create our first collection. Deployd will for us create
a CRUD (*create*, *read*, *update* and *delete*) REST API to add, list,
modify or remove elements in that collection. We only have to define the
entity or model and the name of the collection.

To start, we have to go to the dashboard
[http://localhost:2403/dashboard](http://localhost:2403/dashboard) and
click on the green button with the `+` simbol. We should see somthing
like:

![dashboard](/.github/images/dashboard - Create collection.png)

now we are going to create a new collection with the name `contacts`

![dashboard - Create collection](/.github/images/dashboard - Naming collection.png)

Then we will have a new entry on the left menu with the name of that
collection. If we click on `properties` we could add some properties to
our contacts entity.

For this workshop we are going to add these:
- name - `String` - `required`
- lastName - `String` - `required`
- phone - `String` - `required`

Check that we can specify the attribute type between `String`, `Number`,
`Boolean`, `Object (JSON)` and `Array`.

![dashboard - Defining collection attributes](/.github/images/dashboard - Defining collection attributes.png)

Now we should be able to add some contacts if we go to the `data`
section of the `contacts` entry in the menu.

If we go to the `API` section also, we should see all the endpoints that
Deployd has created for us.

![dashboard - Contacts API details](/.github/images/dashboard - Contacts API details.png)

Now, if we click one the route
[`/contacts`](http://localhost:2403/contacts) we should see in the
browser a JSON object with the data of the contacts that we have added.

It's time to subscribe ng-admin to our collection, so open again
`index.html` and we are going to add some stuff:

```html
   // attach the admin application to the DOM and run it
   var contacts = nga.entity('contacts');

   var fields = [
     nga.field('name'),
     nga.field('lastName'),
     nga.field('phone')
   ];

   contacts.listView()
     .fields(fields);

   contacts.creationView()
     .fields(fields);

   contacts.editionView()
     .fields(fields);

   admin.addEntity(contacts);

   nga.configure(admin);
```

In the first line we are telling to ng-admin that we have a collection
in `/contacts` so it is going to know how to create, read, update and
delete elements from that collection because both are following the same
REST API convention.

Then, we are going to create a `listView`, `creationView` and
`editionView` on all of those views we are passing the `fields` variable
where we have deffined the attributes of our entity, so ng-admin will
render those fields in our views. In the last new line, we are
subscribing the new `contacts` entity to our administraton panel.
Finally, we can go to [http://localhost:2403](http://localshot:2403) and
we will see our new administration panel:

![frontend](/.github/images/frontend.png)

We can create, edit, and delete contacts from our panel:

![frontend - Creating a contact](/.github/images/frontend - Creating a contact.png)

Check in the devtool what is happening when you make a change in one
contact:

![frontend - Contact created - devtools](/.github/images/frontend - Contact created - devtools.png)

You can go directly up to this point checking out the `step3` branch.


## Resources
- [deployd](http://deployd.com/)
- [ng-admin](https://github.com/marmelab/ng-admin)
- [Jeff Cross - Rapid Prototyping with Angular & Deployd - NGConf](https://www.youtube.com/watch?v=0V8fQoqQLLA)
