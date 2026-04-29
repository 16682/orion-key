# 第一阶段：构建环境（安装了 Java 22 和 Maven 3.9+）
FROM maven:3.9.6-eclipse-temurin-22 AS builder
WORKDIR /build

# 拷贝 pom.xml 和 src 源码
COPY pom.xml .
COPY src ./src

# 执行 Maven 打包，跳过烦人的测试用例
RUN mvn clean package -DskipTests

# 第二阶段：运行环境（只保留 JRE 运行库，让容器变小变快）
FROM eclipse-temurin:22-jre
WORKDIR /app

# 把第一阶段打好的 jar 包捞过来重命名
COPY --from=builder /build/target/*.jar app.jar

# 声明项目端口
EXPOSE 8083

# 注入时区并启动程序
ENV TZ=Asia/Shanghai
ENTRYPOINT ["java", "-jar", "app.jar"]
