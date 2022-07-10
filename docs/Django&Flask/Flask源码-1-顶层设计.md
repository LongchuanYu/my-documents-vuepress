大纲：
1. 顶层设计
2. 详细设计
3. 技术知识点
4. 最小化框架模型


app = Flask()
app.run() -> run_simple() -> make_server() -> serve_forever()

request ->  WSGIRequestHandler() -> self.setup() -> self.handle() -> self.finish()