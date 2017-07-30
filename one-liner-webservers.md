[ [Home](README.md) ]
# One Liner Webservers

### PHP
```
$ php -S 127.0.0.1:8000
```

### Docker
```
docker run --rm -d -p 8000:80 -v "$PWD":/usr/local/apache2/htdocs/ httpd:2.4-alpine
```

### Python 2
```
$ python -m SimpleHTTPServer 8000
```

### Python 3
```
$ python -m http.server 8000
```

### Ruby
```
$ ruby -run -e httpd . -b 127.0.0.1 -p 8000
```
or
```
$ gem install serve && serve -p 8000 .
```

### NodeJS
```
$ npm install -g http-server && http-server -p 8000
```
