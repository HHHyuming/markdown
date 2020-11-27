# DRF

## serializers

```python
序列化继承树

1.BaseSerializer 继承自django Field
2.Serializer 基础类
3.ListSerializer
4.ModelSerializer
5.HyperlinkedModelSerializer

from rest_framework.renderers import JSONRenderer
from rest_framework.parsers import JSONParser

JSONRenderer().render(serializer.data)


序列化过程
1. serializer(模型).data  拿到序列化数据
2. JSONRenderer().render(serializer.data) 序列化成python原生数据类型 --》 bytes
3. from io import BytesIO  
stream = BytesIO(JSONRenderer().render(serializer.data))
data = JSONParser().parse(stream)  反序列化成dict
```



## view

```
基础视图
from rest_framework import api_view ==》 APIView 为api_view 装饰器的元类
from rest_framework.views import  APIView   =======> 继承自django的 View


# 集成视图
1.form rest_framework.generic  
GenericAPIView restful 核心实现类 ---》 继承APIView  
2. from rest_framework.viewsets import ModelViewSet
============= class ================
ModelViewSet(mixins.CreateModelMixin,
                   mixins.RetrieveModelMixin,
                   mixins.UpdateModelMixin,
                   mixins.DestroyModelMixin,
                   mixins.ListModelMixin,
                   GenericViewSet)
                   
GenericViewSet(ViewSetMixin, generics.GenericAPIView)


viewsets 视图需要显示的绑定 url 与 function对应的关系
snippet_detail = SnippetViewSet.as_view({
    'get': 'retrieve',
    'put': 'update',
    'patch': 'partial_update',
    'delete': 'destroy'
})

# 混合视图
from rest_framework.mixins 
为 restful 风格数据的实现
CreateModelMixin
ListModelMixin
RetrieveModelMixin
UpdateModelMixin
DestroyModelMixin

# 高层路由
from rest_framework.decorators import detail_route, list_route
```

## 权限 permission

```
from rest_framework import permissions


class IsOwnerOrReadOnly(permissions.BasePermission):
    """
    自定义权限只允许对象的所有者编辑它。
    """

    def has_object_permission(self, request, view, obj):
        # 读取权限允许任何请求，
        # 所以我们总是允许GET，HEAD或OPTIONS请求。
        if request.method in permissions.SAFE_METHODS:
            return True

        # 只有该snippet的所有者才允许写权限。
        return obj.owner == request.user
        
1.局部使用
permission_classes = (permissions.IsAuthenticatedOrReadOnly,)
```

## API 策略

```
renderer_classes
parser_classes
authentication_classes
throttle_classes
permission_classes
.content_negotiation_class

下面这些方法被REST framework用来实例化各种可拔插的API策略。你通常不需要重写这些方法。

.get_renderers(self)
.get_parsers(self)
.get_authenticators(self)
.get_throttles(self)
.get_permissions(self)
.get_content_negotiator(self)
.get_exception_handler(self)


.initial(self, request, *args, **kwargs)
在处理方法调用之前进行任何需要的动作。 这个方法用于执行权限认证和限制，并且执行内容协商。 你通常不需要重写此方法。



============================= 装饰策略 =========================
from rest_framework.decorators import api_view, throttle_classes
from rest_framework.throttling import UserRateThrottle

@renderer_classes(...)
@parser_classes(...)
@authentication_classes(...)
@throttle_classes(...)
@permission_classes(...)

class OncePerDayUserThrottle(UserRateThrottle):
        rate = '1/day'

@api_view(['GET'])
@throttle_classes([OncePerDayUserThrottle])
def view(request):
    return Response({"message": "Hello for today! See you tomorrow!"})
```







