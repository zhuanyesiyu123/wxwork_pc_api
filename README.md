介绍
=============================
wxwork_pc_api 使用HOOK技术将核心功能封装成dll，并提供简易的接口给支持调用dll的语言使用。

你可以通过扩展 wxwork_pc_api 来实现：

* 监控或收集企业微信消息
* 自动消息推送
* 聊天机器人
* 通过企业微信远程控制你的设备

目前测试可以使用语言有C/C++，C#，易语言，Python, Java, Go, NodeJs, PHP, VB, Delphi。




功能清单
-----------------------------------
- 接收用户登录消息
- 接收用户注销消息
- 发送文本
- 发送文件
- 发送视频
- 发送图片
- 发送名片
- 发送图文卡片
- 接收文本消息
- 接收图片消息
- 接收语音消息
- 接收名片消息
- 接收视频消息
- 接收表情消息
- 接收位置消息
- 接收图文卡片消息
- 接收文件消息
- 接收红包消息
- 接收小程序消息



具体使用可以暂时参考samples/python/demo.py, 如下是python封装后的调用

```python
import wxwork
import json
import time
from wxwork import WxWorkManager,MessageType

wxwork_manager = WxWorkManager(libs_path='../../libs')

# 这里测试函数回调
@wxwork.CONNECT_CALLBACK(in_class=False)
def on_connect(client_id):
    print('[on_connect] client_id: {0}'.format(client_id))

@wxwork.RECV_CALLBACK(in_class=False)
def on_recv(client_id, message_type, message_data):
    print('[on_recv] client_id: {0}, message_type: {1}, message:{2}'.format(client_id, 
    message_type, json.dumps(message_data)))

@wxwork.CLOSE_CALLBACK(in_class=False)
def on_close(client_id):
    print('[on_close] client_id: {0}'.format(client_id))


class EchoBot(wxwork.CallbackHandler):


    @wxwork.RECV_CALLBACK(in_class=True)
    def on_message(self, client_id, message_type, message_data):

        # 如果是文本消息，就回复一条消息
        if message_type == MessageType.MT_RECV_TEXT_MSG:
            reply_content = '😂😂😂你发过来的消息是：{0}'.format(message_data['content'])
            time.sleep(2)
            wxwork_manager.send_text(client_id, message_data['conversation_id'], reply_content)


if __name__ == "__main__":
    echoBot = EchoBot()

    # 添加回调实例对象
    wxwork_manager.add_callback_handler(echoBot)
    wxwork_manager.manager_wxwork(smart=True)

    # 阻塞主线程
    while True:
        time.sleep(0.5)

```

企业微信：目前已经实现了大部分功能，运行稳定，比如：发各种消息，接收各种消息，外部群内部群管理，下载文件，加好友等等功能，可提供接口，方便各种语言二次开发，欢迎技术交流

如有需要可直接邮箱联系：
zhongtouke92684502@163.com


帮助&支持
-------------------------
如有需要可直接邮箱联系：
zhongtouke92684502@163.com


