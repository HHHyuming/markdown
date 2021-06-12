# channels WS

## 安装与配置

```
1.pip install channels

2. asgi.py
from channels.routing import ProtocolTypeRouter
application = ProtocolTypeRouter({
    "http": get_asgi_application()
})

3. settings.py
ASGI_APPLICATION = "ws_demo.asgi.application"
```



