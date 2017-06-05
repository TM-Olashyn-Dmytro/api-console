# The API Console

MuleSoft's API Console is a fully-fledged API documentation tool that generates mobile-friendly web-documentation based on RAML (Restful API Modeling Language) documents. It also provides a mechanism to let consumer directly interact with an API.

![](images/new-console-header.png)

## Introduction

In this reporitory, you can find the source for a single HTML element that represents the API Console.

The HTML element is built on top of the [Web Components specifications](https://www.webcomponents.org/introduction) and powered by the [Polymer library](https://www.polymer-project.org/). Although, there is no additional knowledge about how Polymer works necessary to use the console.

The following sections give you a brief overview on how to build and use the console. For a much more in depth overview, please see the [docs](docs) directory in this repository. 

## Using the API console

The API Console comes in two flavors which you can choose from: as a **standalone web-application** and as an embeddable **HTML element**. 

### Run as a standalone web-application

API Console as a standalone application should be used to display the documentation for your API as a web page. The application supports [Deep linking][deep linking] which allows you to share a link to a particular part of you API documentation. You can find a basic example of the standalone application on our [demo application] web page.

To build the API Console as a standalone application use one of our [build tools].

### Embed as an HTML element

The API Console was build on top of new Web Components specification. When you include sources of the console into your web application it will register a new HTML element, the `<api-console>` element, that can be used as any other element on the page or web application. This means that you can embed the console into your blog post or as a part of a press release, and your users will be able to get know your API without being redirected to another web page.

Use [bower] to install the console and it's dependencies:

```bash
$ bower install --save mulesoft/api-console
```

Then include the element in you web page:

```html
<link rel="import" href="bower_components/api-console/api-console.html">
```

And finally use the HTML tag

```html
<body>
  <api-console raml="{...}"></api-console>
</body>
```

See full usage documentation and how to import sources into your web page in the [api console element docs]. Also, if you are a developer you can check out [demo application source code].

You can also build the API Console as a embeddable HTML element using one of our [build tools].

## Optimisation options

The API Console displays a documentation for RAML documents. It means that it must perform some heavy duty computations to transform RAML data into JavaScript object and this takes time. There are, however, few options to optimise loading time of the API Console. It depends on your use case.

### RAML data source

If your API is under active development and changes often, you may want to consider using the RAML file hosted on a server as a data source. The API Console application will then parse RAML file using RAML JavaScript parser and use the parser output as a data source. It will take some time during console's load time but you will always display your APIs newest version.

Because this use case requires to include more custom HTML elements it's not suitable for the standalone version. Other options would be a better fit.

### JSON data source

This is a good choice if your API doesn't change often or if you are using our [build tools] in your CI process. In this case you can generate a JSON file from the RAML and use it as a data input in the `<api-console>` element.

This option will significantly reduce the API Console load time. It is also suitable for both standalone application and the HTML element.

### JSON inlined in the page source

This option gives you the fastest load time but may increase initial page weight. It is the same option as the JSON data source but the JSON data are not kept in separate JSON file but the data are included in the page source as a JavaScript object.

You should use this if your API changes rarely or never. Every change to source RAML file would require regenerating the whole page. Though, it can be automated with our [build tools].

## API Console configuration options

Configuration options differs from previous version. Because the API Console is a (custom) HTML element it's configuration is based on HTML attributes. You can pass values as an attribute value or set boolean option by simply setting the attribute. Configuration from JavaScript code is based on setting a JavaScript property as the attribute name on the element. If the attribute name contains dashes then the property name should be [camel cased].

Example:

```html
<api-console append-headers="x-api-key: 1234" narrow></api-console>
```

which is equivalent of:

```javascript
var console = document.querySelector('api-console');
console.narrow = true;
console.appendHeaders = 'x-api-key: 1234';
```

Full list of the API Console configuration options can be found in the [configuring the api console] document.

## Build tools

The API Console comes with a set of build tools that will help you create the API Console from the RAML file. Build tools are configured to produce a production optimized version of the API Console. The build tools can generate both standalone and embeddable version of the console. You can also configure data source strategy (RAML, JSON or inline JSON as a data source).

Available build tools are the `api-console` CLI and node modules: `api-console-builder` and `raml-json-enhance-node`. Depending on your needs you can choose whether you want to use a CLI tool or a node module.

Build tools can be helpful in the CI process to automate documentation release cycle. See the [build tools] documentation for more information and build strategies.

## Theming

The API Console support theming. By default it comes with it's own styling but you can create your own theme and style the console to match your corporate style guide.

Theming is based on CSS variables and CSS mixins. Basic concepts of using the variables and mixins are described in [Polymer 1.0 styling] documentation. You can check the [api-console-styles.html](api-console-styles.html) file to see current theme definition and then read [theming documentation] to learn how to create your own theme.

## CORS

Cross-origin resource sharing (CORS) allows sharing resources from one domain with other domains. Browsers block all requests to other domains but with special set of headers authors can allow other domains to request a resource (Read more in [CORS Wiki]).

If your API is not allowing CORS for any reason then the API Console won't be able to make a request to an endpoint. API Console currently support 3 ways of dealing with this issue:

- by installing the **API Console Chrome extension**
- by setting up a **proxy server**
- by handling HTTP requests by hosting application

Read our [CORS guideline] for more information about each of this solutions.

## Preview and development

The API Console is a custom element that serves as a shell element for other custom web components. It means that to develop the API Console most probably you'd have to develop one of over a hundred other web components that creates the console. All the elements are described in [the elements catalogue].

1. Clone the element:
```
git clone https://github.com/mulesoft/api-console.git
cd api-console
```

2. Checkout latest version
```
git checkout release/4.0.0
```

3. Install [polymer-cli] and [Bower].
```
sudo npm install -g bower polymer-cli
```

4. Install dependencies
```
bower install
```

5. Serve the element
```
polymer serve --open -p 8080
```

Default page is the element's documentation. Switch to demo to see working example.

You can also append the `/demo/` to the URL to switch to demo page permanently.

## Reporting issues and features requests

The API Console and the Advanced REST client is open and we encourage the community to contribute in the project. However, it is very important to follow couple of simple rules when you create an issue report or send a pull request.

Please, see CONTRIBUTING.md for description of how to file issue report of feature request.

### Contributor's Agreement

To contribute source code to this repository, please read our [contributor's agreement](http://www.mulesoft.org/legal/contributor-agreement.html), and then execute it by running this notebook and following the instructions: https://api-notebook.anypoint.mulesoft.com/notebooks/#380297ed0e474010ff43

## License

The API Console is shared under Common Public Attribution License Version 1.0 (CPAL-1.0).

See the LICENSE.md file for more information.

[deep linking]: https://en.wikipedia.org/wiki/Deep_linking
[demo application]: https://mulesoft.github.io/api-console
[demo application source code]: demo/api.html
[api console element docs]: docs/api-console-element.md
[build tools]: docs/build-tools.md
[configuring the api console]: docs/configuring-api-console.md
[theming documentation]: docs/theming.md
[camel cased]: https://en.wikipedia.org/wiki/Camel_case
[polymer-cli]: https://www.polymer-project.org/1.0/docs/tools/polymer-cli
[Bower]: https://bower.io/
[Polymer 1.0 styling]: https://www.polymer-project.org/1.0/docs/devguide/styling
[the elements catalogue]: https://elements.advancedrestclient.com/
[bower]: https://bower.io/
[CORS Wiki]: https://en.wikipedia.org/wiki/Cross-origin_resource_sharing
[CORS guideline]: docs/cors.md
