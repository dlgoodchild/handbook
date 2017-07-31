[ [Home](README.md) ]
# One Liner Webservers

### [PHP](http://php.net)
```
$ php -S 127.0.0.1:8000
```

### [Docker](https://www.docker.com)
```
docker run --rm -d -p 8000:80 -v "$PWD":/usr/local/apache2/htdocs/ httpd:2.4-alpine
```

### [Python](https://www.python.org) 2
```
$ python -m SimpleHTTPServer 8000
```

### [Python](https://www.python.org) 3
```
$ python -m http.server 8000
```

### [Ruby](https://www.ruby-lang.org)
```
$ brew install ruby
$ ruby -run -e httpd . -b 127.0.0.1 -p 8000
```
or
```
$ gem install serve && serve -p 8000 .
```

### [Node.js](https://nodejs.org)
```
$ npm install -g http-server && http-server -p 8000
```
