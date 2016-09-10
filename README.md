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
deployd will show up.

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
