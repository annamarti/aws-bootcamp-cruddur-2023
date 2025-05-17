# Week 1 — App Containerization

## Run locally
1. Add env vars
```
export FRONTEND_URL="*"
export BACKEND_URL="*"
```
2. Run Python in backend's folder, then open 4567 port 
```
cd backend-flask
pip3 install -r requirements.txt
python3 -m flask run --host=0.0.0.0 --port=4567
cd ..
```
3. Open url and add 'api/activities/home' at the end, should recieve json file.


## Dockerize backend
1. Add Dockerfile
2. Build Iimage (in root dir, not in backend-flask)
```
docker build -t  backend-flask ./backend-flask
```
3. Run Image 
Option 1:
```
docker run --rm -p 4567:4567 -it -e FRONTEND_URL=* -e BACKEND_URL=*  backend-flask 
```

Option 2: 
```
export FRONTEND_URL="*"
export BACKEND_URL="*"
docker run --rm -p 4567:4567 -it  -e FRONTEND_URL -e BACKEND_URL backend-flask
unset FRONTEND_URL="*"
unset BACKEND_URL="*"
```
`-e FRONTEND_URL` is equivalent `-e FRONTEND_URL=(ENV | grip FRONTEND_URL)`

4. Test Server
```
curl -X GET http://localhost:4567/api/activities/home -H "Accept: application/json" -H "Content-Type: application/json"
```
Or open url and add 'api/activities/home' at the end, should recieve json file.

## Dockerize frontend
1. Build fron
```
cd frontend-react-js
npm i
cd ..
```
2. Add Dcokerfile:
```
FROM node:16.18

ENV PORT=3000

COPY . /frontend-react-js
WORKDIR /frontend-react-js
RUN npm install
EXPOSE ${PORT}
CMD ["npm", "start"]
```

3. Build Image (from root, not from frontend):
```
docker build -t frontend-react-js ./frontend-react-js
```
4. Run Image
```
docker run -p 3000:3000 -d frontend-react-js
```
5. Test frontend

## Notes
Add env vars:
```
export FRONTEND_URL="*"
export BACKEND_URL="*"
```
Remove env vars
```
unset BACKEND_URL
unset FRONTEND_URL
```
Search env vars with _URL postfix:
```
env | grep _URL
```
Some docker commands:
```
docker ps
docker images

docker logs CONTAINER_ID -f
docker logs backend-flask -f
docker logs $CONTAINER_ID -f

docker exec CONTAINER_ID -it /bin/bash

docker image rm backend-flask --force
```