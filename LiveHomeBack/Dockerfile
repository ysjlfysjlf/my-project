# 使用官方Go镜像作为构建环境
FROM golang:1.19-alpine AS builder

# 设置工作目录
WORKDIR /app

# 复制go mod和sum文件
COPY go.mod go.sum ./

# 在 RUN go mod download 前加上环境变量设置
RUN GOPROXY=https://goproxy.cn,direct go mod download

# 下载依赖
RUN go mod download

# 复制源代码
COPY . .

# 构建应用
RUN CGO_ENABLED=0 GOOS=linux go build -o main .

# 使用轻量级alpine作为运行环境
FROM alpine:latest

WORKDIR /app

# 从builder阶段复制编译好的应用
COPY --from=builder /app/main .

# 暴露端口
EXPOSE 8080

# 运行应用
CMD ["./main"] 
