# 由 Docker Hub 官方版本 node:12 映像檔為基礎
FROM node:12

# 建立應用程式所在目錄
WORKDIR /usr/src/app

# 複製目前程式
COPY . .

# 開放 Port
EXPOSE 9876

# 容器執行即自動執行 node index.js
CMD [ "node", "index.js" ]