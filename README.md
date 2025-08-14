# Claude Code Proxy Docker

这个项目为 [Claude Code](https://github.com/anthropics/claude-code) 提供了一个Docker容器化的代理服务，使其能够与OpenAI兼容的API提供商协同工作。

## 功能特性

- 🐳 **Docker容器化** - 简单易用的容器部署
- 🔄 **自动构建** - GitHub Actions自动构建并推送镜像
- 🏗️ **多平台支持** - 支持linux/amd64和linux/arm64架构
- 🌐 **API代理** - 将Claude Code的请求转换为OpenAI兼容格式
- 🚀 **高性能** - 基于FastAPI构建，响应迅速

## 快速开始

### 1. 使用Docker运行

```bash
# 设置环境变量
export OPENAI_API_KEY="your-openai-api-key"
export OPENAI_BASE_URL="https://api.openai.com/v1"  # 可选，默认为OpenAI官方API

# 运行容器
docker run -d \
  --name claude-code-proxy \
  -p 8000:8000 \
  -e OPENAI_API_KEY="${OPENAI_API_KEY}" \
  -e OPENAI_BASE_URL="${OPENAI_BASE_URL}" \
  ghcr.io/moderras/claude-code-proxy-docker:latest
```

### 2. 使用Docker Compose

创建 `docker-compose.yml` 文件：

```yaml
version: '3.8'

services:
  claude-code-proxy:
    image: ghcr.io/moderras/claude-code-proxy-docker:latest
    container_name: claude-code-proxy
    ports:
      - "8000:8000"
    environment:
      - OPENAI_API_KEY=your-openai-api-key
      - OPENAI_BASE_URL=https://api.openai.com/v1
    restart: unless-stopped
```

启动服务：
```bash
docker-compose up -d
```

### 3. 配置Claude Code

在Claude Code中，你需要配置代理设置。通常，你需要：

1. 设置API端点为：`http://localhost:8000`
2. 设置API密钥为你的OpenAI API密钥

## 环境变量

| 变量名 | 必需 | 默认值 | 说明 |
|--------|------|--------|------|
| `OPENAI_API_KEY` | ✅ | - | OpenAI API密钥 |
| `OPENAI_BASE_URL` | ❌ | `https://api.openai.com/v1` | OpenAI API基础URL |
| `HOST` | ❌ | `0.0.0.0` | 服务器监听地址 |
| `PORT` | ❌ | `8000` | 服务器监听端口 |

## 支持的API提供商

本项目支持任何OpenAI兼容的API提供商：

- **OpenAI** - 官方OpenAI API
- **Azure OpenAI** - 微软Azure OpenAI服务
- **本地模型** - 通过Ollama等本地服务
- **第三方提供商** - 如Anthropic Claude API、Google Gemini等

### 示例配置

#### Azure OpenAI
```bash
export OPENAI_API_KEY="your-azure-api-key"
export OPENAI_BASE_URL="https://your-resource.openai.azure.com/openai/deployments/your-deployment"
```

#### Ollama
```bash
export OPENAI_API_KEY="not-needed"
export OPENAI_BASE_URL="http://localhost:11434/v1"
```

## 检查服务状态

```bash
# 检查容器状态
docker ps

# 查看日志
docker logs claude-code-proxy

# 健康检查
curl http://localhost:8000/health
```

## 构建镜像

### 本地构建

```bash
# 克隆仓库
git clone https://github.com/ModerRAS/claude-code-proxy-docker.git
cd claude-code-proxy-docker

# 初始化submodule
git submodule update --init --recursive

# 构建镜像
docker build -t claude-code-proxy-docker ./claude-code-proxy
```

### 自动构建

本项目配置了GitHub Actions，在以下情况下会自动构建并推送镜像到GitHub Container Registry：

- 推送到 `master` 或 `main` 分支
- 创建版本标签 (如 `v1.0.0`)
- 创建Pull Request

镜像地址：`ghcr.io/moderras/claude-code-proxy-docker:latest`

## 故障排除

### 常见问题

1. **容器启动失败**
   ```bash
   # 检查环境变量
   docker logs claude-code-proxy
   
   # 确保OPENAI_API_KEY已设置
   echo $OPENAI_API_KEY
   ```

2. **连接超时**
   - 检查端口是否正确映射：`-p 8000:8000`
   - 确认防火墙设置
   - 验证API提供商的网络连接

3. **API密钥错误**
   - 确认API密钥有效
   - 检查API密钥是否有足够权限
   - 验证API基础URL是否正确

### 调试模式

```bash
# 以调试模式运行容器
docker run --rm \
  --name claude-code-proxy-debug \
  -p 8000:8000 \
  -e OPENAI_API_KEY="${OPENAI_API_KEY}" \
  ghcr.io/moderras/claude-code-proxy-docker:latest \
  --log-level debug
```

## 贡献

欢迎提交Issue和Pull Request来改进这个项目。

## 许可证

本项目基于 [Claude Code Proxy](https://github.com/fuergaosi233/claude-code-proxy) 项目，遵循MIT许可证。

## 相关链接

- [Claude Code](https://github.com/anthropics/claude-code)
- [Claude Code Proxy](https://github.com/fuergaosi233/claude-code-proxy)
- [GitHub Container Registry](https://github.com/ModerRAS/claude-code-proxy-docker/pkgs/container/claude-code-proxy-docker)
- [Docker Hub](https://hub.docker.com/)