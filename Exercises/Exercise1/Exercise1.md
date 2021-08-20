## Dockerfile with an example

Learn with an example. We create a web server with our source code.

First of all, create a html file with the following content:

```html
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="utf-8">
  <title>Docker example</title>
</head>
<body>
  Hello!
</body>
</html>
```

Now, we can create a Dockerfile:
```
FROM httpd:2.4
COPY index.html /usr/local/apache2/htdocs/.
```

Finally, build and run it:
```sh
docker build -t image_name .
docker run -p 80:80 image_name
```