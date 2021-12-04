# RUN BELOW COMMANDS 
`vi nginx.conf` Add privateips and ports

`docker build --no-cache -t lb .`

`docker run -d --name contlb -p 8080:8080 lb`
