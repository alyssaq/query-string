# query-string [![Build Status](https://travis-ci.org/sindresorhus/query-string.svg?branch=master)](https://travis-ci.org/sindresorhus/query-string)

> Parse and stringify URL [query strings](http://en.wikipedia.org/wiki/Query_string)


## Install

```
$ npm install --save query-string
```


## Usage

```js
const queryString = require('query-string');

console.log(location.search);
//=> '?foo=bar'

const parsed = queryString.parse(location.search);
console.log(parsed);
//=> {foo: 'bar'}

console.log(location.hash);
//=> '#token=bada55cafe'

const parsedHash = queryString.parse(location.hash);
console.log(parsedHash);
//=> {token: 'bada55cafe'}

parsed.foo = 'unicorn';
parsed.ilike = 'pizza';

location.search = queryString.stringify(parsed);

console.log(location.search);
//=> 'foo=unicorn&ilike=pizza'
```


## API

### .parse(*string*)

Parse a query string into an object. Leading `?` or `#` are ignored, so you can pass `location.search` or `location.hash` directly.

The returned object is created with [`Object.create(null)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/create) and thus does not have a `prototype`.

### .stringify(*object*, *[options]*)

Stringify an object into a query string, sorting the keys.

#### strict

Type: `boolean`<br />
Default: `true`

Strictly encode URI components with [strict-uri-encode](https://github.com/kevva/strict-uri-encode). It uses [encodeURIComponent](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/encodeURIComponent)
if set to false. You probably [don't care](https://github.com/sindresorhus/query-string/issues/42) about this option.

#### encode

Type: `boolean`<br>
Default: `true`

[URL encode](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/encodeURIComponent) the keys and values.

### .extract(*string*)

Extract a query string from a URL that can be passed into `.parse()`.


## Nesting

This module intentionally doesn't support nesting as it's not spec'd and varies between implementations, which causes a lot of [edge cases](https://github.com/visionmedia/node-querystring/issues).

You're much better off just converting the object to a JSON string:

```js
queryString.stringify({
	foo: 'bar',
	nested: JSON.stringify({
		unicorn: 'cake'
	})
});
//=> 'foo=bar&nested=%7B%22unicorn%22%3A%22cake%22%7D'
```

However, there is support for multiple instances of the same key:

```js
queryString.parse('likes=cake&name=bob&likes=icecream');
//=> {likes: ['cake', 'icecream'], name: 'bob'}

queryString.stringify({color: ['taupe', 'chartreuse'], id: '515'});
//=> 'color=chartreuse&color=taupe&id=515'
```


## License

MIT © [Sindre Sorhus](https://sindresorhus.com)
