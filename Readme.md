## Nightwatch custom commands and assertions [![Build Status](https://travis-ci.org/maxgalbu/nightwatch-custom-commands-assertions.svg?branch=master)](https://travis-ci.org/maxgalbu/nightwatch-custom-commands-assertions)

These are some commands and assertion I use when I'm testing with nightwatch.js and selenium.

### How to use these things?

Go into your `tests` folder (or where your nightwatch.json is) and do:

	git clone https://github.com/maxgalbu/nightwatch-custom-commands-assertions.git

(or download the zipped repository [here](https://github.com/maxgalbu/nightwatch-custom-commands-assertions/archive/master.zip) and extract it).

Then, open the nightwatch.json file in your editor (or [create it if it doesn't exist](http://nightwatchjs.org/guide#settings-file)) and edit the `custom_commands_path` and `custom_assertions_path` keys so they look like this:

```json
{
	... //your config
	
	"custom_commands_path" : "nightwatch-custom-commands-assertions/js/commands",
	"custom_assertions_path" : "nightwatch-custom-commands-assertions/js/assertions",
	
	... //your config again
}
```

Now you should be able to use these commands/assertions when you call `nightwatch --test`.

### Contributing

See [Contributing.md](https://github.com/maxgalbu/nightwatch-custom-commands-assertions/blob/master/Contributing.md).

### List of commands

- **jqueryClick**: clicks an element using jquery selectors
	
	```js
	browser.jqueryClick(".classname:first > input:checked")
	browser.jqueryClick("div:has(.classname):contains('something'):last")
	```
	 
- **jqueryElement**: returns an element using jquery selectors
	
	```js
	browser.jqueryElement(".classname:first > input:checked", function(element) {
		//Element is the DOM element
	})
	browser.jqueryElement("div:has(.classname):contains('something'):last", function(element) {
		//Element is the DOM element
	})
	```
	
- **setSelect2Data**: set a select2 value using select2("data", object)
	
	```js
	browser.setSelect2Data("input[type=hidden].has-select2", {id:1, text: "hello"})
	```
	
- **setSelect2Value**: set a select2 value using select2("val", string/number)
	
	```js
	browser.setSelect2Value("select.has-select2:hidden", "some value")
	```
	
- **setValueAndTrigger**: set a value on an `<input>` or a `<select>` and trigger a `change` event
	
	```js
	browser.setValueAndTrigger("#a-select-or-input", "some value")
	```
	
- **shell**: execute a command on the shell
	
	```js
	browser.shell("mysql -u root database_name < fakedata.sql")
	```

- **waitForAttribute**: wait for an element to have an attribute that equal some expression (thanks @dkoo761)
	
	```js
	browser.waitForAttribute("img", 'src', function(imageSrc) {
		return imageSrc === pathToSomeImage;
	});
	```

	The command waits by default for 5 seconds, and checks every 100 milliseconds.

- **waitForText**: wait for an element to have its inner HTML equal some expression (thanks @dkoo761)
	
	```js
	browser.waitForText("div", function(text) {
		return text === "something";
	});
	```

	The command waits by default for 5 seconds, and checks every 100 milliseconds.

- **waitForTitle**: wait for the `<title>` tag to equal some expression (thanks @dkoo761)
	
	```js
	browser.waitForTitle(function(title) {
		return title === "something";
	});
	```

	The command waits by default for 5 seconds, and checks every 100 milliseconds.

- **saveElementScreenshot**: take a screenshot of the requested element
	
	```js
	browser.saveElementScreenshot(".class", "screenshot-name.jpg");
	```

	This command requires [ImageMagick](http://www.imagemagick.org) installed on the system and [node-easyimage](https://github.com/hacksparrow/node-easyimage) installed as a npm module. You can install node-easyimage with:

	```
	npm install --production
	```

	And imagemagick with:

	```
	#Centos
	yum install ImageMagick

	#OSX
	brew install imagemagick --build-from-source

	#Ubuntu
	apt-get install ImageMagick
	```
	
### List of assertions

Some assertion requires a command: if you already included all the repository commands (following the instructions above) you should be fine, otherwise you need to tell nightwatch where are those commands (you need to change nightwatch.json).

- **urlMatch**: assert that the url matches the regex provided
	
	```js
	browser
		.url("http://www.google.com")
		.assert.urlMatch(/.com$/)
	```
	
	requires command: none

- **jqueryElementPresent**: assert that the element identified by the jquery selector exists in the DOM:
	
	```js
	browser
		.url("http://www.github.com")
		.assert.jqueryElementPresent("div:eq(2)")
	```
	
	requires command: jqueryElement
