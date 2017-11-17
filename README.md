# RPC, RMI与SOAP

RPC：（Remote Procedure Call）
　　被设计为在应用程序间通信的平台中立的方式，它不理会操作系统之间以及语言之间的差异。 支持多语言。

RMI：（Remote Method Invocation）
RPC 的Java版本，EJB的基础技术
RMI 采用JRMP（Java Remote Method Protocol）通讯协议，是构建在TCP/IP协议上的一种远程调用方法。
RMI 采用stubs和skeletons来进行远程对象的通讯。
　　stub充当远程对象的客户端代理，有着和远程对象相同的远程接口。
　　远程对象的调用实际是通过调用该对象的客户端代理对象stub来完成的。

    创建远程方法调用的5个步骤：
    1）定义一个扩展了Remote接口的接口，该接口中的每一个方法必须声明它将产生一个RemoteException异常；
    2）定义一个实现该接口的类；
    3）使用rmic程序生成远程实现所需的存根和框架；
    　　(例如，在demo.rmi.EchoServer.java所在目录运行: rmic demo.rmi.EchoServer)
    4）创建一个客户程序和服务器进行RMI调用；
    5）启动rmiregistry并运行自己的服务程序和客户程序。

RMI与RPC的区别在于：
1）方法是如何被调用的
　　对RMI来说，如果一个方法在服务器上执行，但是没有相匹配的签名被添加到这个远程接口上，那么这个新方法就不能被RMI客户方所调用。
　　而在RPC中，当一个请求到达RPC服务器时，请求包含一个参数集和一个文本值，通常为“classname.methodname”形式。
　　这表明，请求的方法在“classname”类中，名叫“methodname”。
　　然后，RPC服务器就去搜索与之相匹配的类和方法，并把它作为那种方法参数类型的输入。
　　这里的参数类型是与RPC请求中的类型匹配的。 一旦匹配成功，方法就被调用了，其结果被编码后返回客户方。
2）对传递信息的限制
　　RMI 调用远程对象方法，允许方法返回 Java 对象以及基本数据类型。
　　而RPC不允许传递对象，RPC服务的消息由外部数据表示（External Data Representation，XDR）语言来表示。
3）采用的协议不同
    RPC不支持对象，采用http协议。RMI支持传输对象，采用TCP/IP协议
4）RIM只限于Java语言，而RPC跨语言
   
另外，RMI优于RPC或SOAP的一点是：在程序开发过程中因为对象或方法不匹配造成的错误可以在编译期被发现，而不用等到运行期。

RPC, SOAP, WSDL的关系
============================================================================
RPC, SOAP, WSDL都是web service的关键词，这里描述一下他们的关系，下面的解释可能比较狭义，主要为了帮助理解这三者的关系。
 
1.RPC

如果要调用远端的一个方法，可以使用RMI和RPC，这是2种截然不同的风格。
RMI: (Remote Method Invocation) 直接获取远端方法的签名，进行调用。优点是强类型、编译期可检查错误；缺点是只限于java语言
RPC: (Remote Procedure Call) 采用客户端/服务器方式(请求/响应)，发送请求到服务器端，服务端执行方法后返回结果。优点是跨语言跨平台，缺点是编译期无法排错，只能在运行时检查。
 
2.SOAP

为了包装RPC的请求信息，推出了XML-RPC，但XML-RPC只能使用有限的数据类型种类和一些简单的数据结构。于是就出现了SOAP(Simple Object Access Protocol)。SOAP最主要的工作是使用标准的XML描述了RPC的请求信息(URI/类/方法/参数/返回值)。理论上，SOAP就是一段xml，你可以通过http,smtp等发送它(复制到软盘上，叫快递公司送去也行?)。同样SOAP也是跨语言的。
 
3.WSDL

WSDL(Web Services Description Language)是描述web服务的，是描述怎样访问web服务的。WSDL是用来描述SOAP的，换句话说，WSDL 文件告诉你调用 SOAP 所需要知道的一切。WSDL也是一段xml。现在各个语言对wsdl的支持都很成熟，可以根据同一份wsdl文件生成自己语言的客户端。