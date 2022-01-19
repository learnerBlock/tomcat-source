# tomcat-source
learn tomcat source code 
目前tomcat系统通过组件形式搭建起来，核心组件通过生命周期Lifecycle管理，包含init(),start(),stop()三个生命周期函数，init()主要是初始化功能，初期实例化各种类，start()作为启动函数，其中：
步骤一：connector的endpoint类内聚了acceptor线程，poller线程，acceptor线程即为tcp监听线程，使用accept（）函数《BIO模式》 ，获取到socket封装成pollerEvent事件，添加到poller的synchronousQueue队列中，实现事件触发机制。
步骤二：poller线程获取配对队列数据成功后 标志着客户端socket获取成功，然后将socket封装成Runable任务，提交到线程池中执行。
步骤三：线程池中对socket处理，包含processor分支socket到Request,Adapter转换request为ServletRequest同时将请求转发给container，具体转发过程就是函数调用过程，adapter调用connector.getService().getContainer().getPipeline().getFirst().invoke(request,response)将请求传递到servlet容器中。
