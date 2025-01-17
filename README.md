```
git clone https://github.com/HaneeshDevops/mern_docker_demo.git
```
```
cd mern_docker_demo
docker network create library-mern-api
docker network ls
docker volume create mongodb-data
docker volume ls
```
```
docker run -dit -p 27017:27017 -e MONGO_INITDB_ROOT_USERNAME=admin -e MONGO_INITDB_ROOT_PASSWORD=password -e PWD=/ -v mongodb-data:/data/db --name mern_library_nginx_mongodb_1 --net library-mern-api mongo 
```
```
docker run -dit -p 8081:8081 -e ME_CONFIG_MONGODB_ADMINUSERNAME=admin -e ME_CONFIG_MONGODB_ADMINPASSWORD=password --net library-mern-api --name mern_library_nginx_mongo-express_1 -e ME_CONFIG_MONGODB_SERVER=mern_library_nginx_mongodb_1 -e ME_CONFIG_BASICAUTH_USERNAME=admin -e ME_CONFIG_BASICAUTH_PASSWORD=admin123456 mongo-express
```
## cd server
```
docker build -t mern_library_nginx_library-api .
```
```
docker run -dit -p 5000:5000 -e MONGO_URI=mongodb://admin:password@mern_library_nginx_mongodb_1 -e NODE_ENV=development -e PWD=/app -v "$(pwd)":/app -v "$(pwd)"/node_modules:/app/node_modules --net library-mern-api --name library_mern_nginx mern_library_nginx_library-api
```
## cd client
```
docker build -t mern_library_nginx_client .
```
```
docker run -d -v "$(pwd)":/app -v "$(pwd)"/node_modules:/app/node_modules --net library-mern-api --name library_mern_frontend mern_library_nginx_client
```
## cd nginx
```
docker build -t mern_library_nginx_nginx:latest .
```
```
docker run -d -p 8080:80 --name mern_library_nginx_nginx_1 --net library-mern-api mern_library_nginx_nginx
```
