#Running

1) `docker build -t application-name .`
2) `docker run -dp 9000:9000 -v "$(pwd)/..:/var/www" --name application-name application-name`
