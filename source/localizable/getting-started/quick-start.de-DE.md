In diesem Tutorial lernst du, wie du eine einfache Ember-App von Grund auf neu erstellen kannst.

Folgende Schritte werden wir beschreiben:

  1. Ember installieren.
  2. Eine neue Anwendung erstellen.
  3. Eine Route definieren.
  4. Eine UI-Komponente schreiben.
  5. Die App zu kompilieren, damit sie auf einen Produktionsserver deployt werden kann.

## Ember installieren

Ember lässt sich mit einem npm-Befehl installieren (npm ist der Node.js-Paketmanager). Gib folgendes in dein Terminal ein:

```sh
npm install -g ember-cli
```

Hast du npm noch nicht? [Finde hier heraus, wie du Node.js und npm installieren kannst.](https://docs.npmjs.com/getting-started/installing-node).

## Eine neue Anwendung erstellen

Sobald du Ember CLI via npm installiert hast, hast du in deinem Terminal Zugriff auf einen neuen Befehl `ember`. Der Befehl `ember new` erstellt eine neue Anwendung.

```sh
ember new ember-quickstart
```

Dieser Befehl erstellt ein neues Verzeichnis mit dem Namen `ember-quickstart`, das das Grundgerüst für eine neue Ember-Anwendung enthält. In diesem Grundgerüst sind schon einige Features enthalten:

* Ein Development-Server.
* Kompilieren von Templates.
* Minifizierung von JavaScript und CSS.
* ES2015-Features via Babel.

Ember macht es dir sehr einfach, neue Projekte zu erstellen, indem es dir in einem integrierten Paket alles zur Verfügung stellt, was du brauchst, um produktionsfähige Webanwendungen zu erstellen.

Wir wollen nun sicherstellen, dass alles korrekt funktioniert. Wechsel mit `cd` in das Anwendungsverzeichnis `ember-quickstart` und starte den Development-Server, indem du folgendes eingibst:

```sh
cd ember-quickstart
ember server
```

Nach wenigen Sekunden solltest du eine Ausgabe erhalten, die ungefähr so aussieht:

```text
Livereload server on http://localhost:49152
Serving on http://localhost:4200/
```

(Um zu einem beliebigen Zeitpunkt den Server zu stoppen, kannst du Strg-C in deinem Terminal drücken.)

Öffne nun in deinem Lieblingsbrowser die URL [http://localhost:4200/](http://localhost:4200). Du solltest nun eine Webseite sehen, die "Welcome to Ember" (und nicht viel mehr) beinhaltet. Herzlichen Glückwunsch! Du hast soeben deine erste Ember-Anwendung erstellt und gestartet.

Öffne in deinem Editor die Datei `app/templates/application.hbs`. Hierbei handelt es sich um das `application`-Template, das stets angezeigt wird, wenn der Benutzer deine Anwendung geöffnet hat.

Ändere in deinem Editor den Text innerhalb des `<h2>`-Tags von `Welcome to Ember` zu `Personentracker` und speichere die Datei. Du wirst feststellen, dass Ember deine Änderung mitbekommt und die Webseite für dich im Hintergrund automatisch neu lädt. Nun solltest du sehen, dass der Text "Welcome to Ember" durch "Personentracker" ersetzt wurde.

## Define a Route

Let's build an application that shows a list of scientists. To do that, the first step is to create a route. For now, you can think of routes as being the different pages that make up your application.

Ember comes with *generators* that automate the boilerplate code for common tasks. To generate a route, type this in your terminal:

```sh
ember generate route scientists
```

You'll see output like this:

```text
installing route
  create app/routes/scientists.js
  create app/templates/scientists.hbs
updating router
  add route scientists
installing route-test
  create tests/unit/routes/scientists-test.js
```

That's Ember telling you that it has created:

  1. A template to be displayed when the user visits `/scientists`.
  2. A `Route` object that fetches the model used by that template.
  3. An entry in the application's router (located in `app/router.js`).
  4. A unit test for this route.

Open the newly-created template in `app/templates/scientists.hbs` and add the following HTML:

```app/templates/scientists.hbs 

## List of Scientists

    <br />In your browser, open
    [http://localhost:4200/scientists](http://localhost:4200/scientists). You should
    see the `<h2>` you put in the `scientists.hbs` template, right below the
    `<h2>` from our `application.hbs` template.
    
    Now that we've got the `scientists` template rendering, let's give it some
    data to render. We do that by specifying a _model_ for that route, and
    we can specify a model by editing `app/routes/scientists.js`.
    
    We'll take the code created for us by the generator and add a `model()`
    method to the `Route`:
    
    ```app/routes/scientists.js{+4,+5,+6}
    import Ember from 'ember';
    
    export default Ember.Route.extend({
      model() {
        return ['Marie Curie', 'Mae Jemison', 'Albert Hofmann'];
      }
    });
    

(This code example uses the latest features in JavaScript, some of which you may not be familiar with. Learn more with this [overview of the newest JavaScript features](https://ponyfoo.com/articles/es6).)

In a route's `model()` method, you return whatever data you want to make available to the template. If you need to fetch data asynchronously, the `model()` method supports any library that uses [JavaScript Promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise).

Now let's tell Ember how to turn that array of strings into HTML. Open the `scientists` template and add some Handlebars code to loop through the array and print it:

```app/templates/scientists.hbs{+3,+4,+5,+6,+7} 

## List of Scientists

{{#each model as |scientist|}} 

* {{scientist}} {{/each}} 

    <br />Here, we use the `each` helper to loop over each item in the array we
    provided from the `model()` hook and print it inside an `<li>` element.
    
    ## Create a UI Component
    
    As your application grows and you notice you are sharing UI elements
    between multiple pages (or using them multiple times on the same page),
    Ember makes it easy to refactor your templates into reusable components.
    
    Let's create a `people-list` component that we can use
    in multiple places to show a list of people.
    
    As usual, there's a generator that makes this easy for us. Make a new
    component by typing:
    
    ```sh
    ember generate component people-list
    

Copy and paste the `scientists` template into the `people-list` component's template and edit it to look as follows:

```app/templates/components/people-list.hbs 

## {{title}}

{{#each people as |person|}} 

* {{person}} {{/each}} 

    <br />Note that we've changed the title from a hard-coded string ("List of
    Scientists") to a dynamic property (`{{title}}`). We've also renamed
    `scientist` to the more-generic `person`, decreasing the coupling of our
    component to where it's used.
    
    Save this template and switch back to the `scientists` template. Replace all
    our old code with our new componentized version. Components look like
    HTML tags but instead of using angle brackets (`<tag>`) they use double
    curly braces (`{{component}}`). We're going to tell our component:
    
    1. What title to use, via the `title` attribute.
    2. What array of people to use, via the `people` attribute. We'll
       provide this route's `model` as the list of people.
    
    ```app/templates/scientists.hbs{-1,-2,-3,-4,-5,-6,-7,+8}
    <h2>List of Scientists</h2>
    
    <ul>
      {{#each model as |scientist|}}
        <li>{{scientist}}</li>
      {{/each}}
    </ul>
    {{people-list title="List of Scientists" people=model}}
    

Go back to your browser and you should see that the UI looks identical. The only difference is that now we've componentized our list into a version that's more reusable and more maintainable.

You can see this in action if you create a new route that shows a different list of people. As an exercise for the reader, you may try to create a `programmers` route that shows a list of famous programmers. By re-using the `people-list` component, you can do it in almost no code at all.

## Building For Production

Now that we've written our application and verified that it works in development, it's time to get it ready to deploy to our users. To do so, run the following command:

```sh
ember build --env production
```

The `build` command packages up all of the assets that make up your application&mdash;JavaScript, templates, CSS, web fonts, images, and more.

In this case, we told Ember to build for the production environment via the `--env` flag. This creates an optimized bundle that's ready to upload to your web host. Once the build finishes, you'll find all of the concatenated and minified assets in your application's `dist/` directory.

The Ember community values collaboration and building common tools that everyone relies on. If you're interested in deploying your app to production in a fast and reliable way, check out the [Ember CLI Deploy](http://ember-cli-deploy.github.io/ember-cli-deploy/) addon.

If you deploy your application to an Apache web server, first create a new virtual host for the application. To make sure all routes are handled by index.html, add the following directive to the application's virtual host configuration

    FallbackResource index.html