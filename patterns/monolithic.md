# Pattern: Monolithic Architecture

## Context
Khi bạn đang phát triển một ứng dụng phía server dành cho doanh nghiệp, nó phải hỗ trợ một vài thiết bị cuối như trình duyệt cho máy tính để bàn hoặc trình duyệt cũng như ứng dụng cho điện thoại di động. Ngoài ra ứng dụng còn cung cấp các API cho các ứng dụng bên thứ 3 khác. Nó cũng có thể tích hợp với những ứng dụng khác thông qua web serivces hoặc message broker. Ứng dụng có thể xử lý (HTTP request và messages) bằng các thực thi các nghiệp vụ, truy cập database, trao đổi tin nhắn với những hệ thống khác và trả HTML/XML/JSON responce. Các thành phần nghiệp vụ tương ứng với các vùng chức năng khác nhau của ứng dụng.

You are developing a server-side enterprise application. It must support a variety of different clients including desktop browsers, mobile browsers and native mobile applications. The application might also expose an API for 3rd parties to consume. It might also integrate with other applications via either web services or a message broker. The application handles requests (HTTP requests and messages) by executing business logic; accessing a database; exchanging messages with other systems; and returning a HTML/JSON/XML response. There are logical components corresponding to different functional areas of the application.

## Problem
Kiến trúc triển khai của ứng dụng là gì?

## Forces 

Có một nhóm phát triển đang làm việc trên một ứng dụng.
Người mới có thể gia nhập nhanh hơn hiệu quả hơn.
Ứng dụng dễ hiểu và dễ điều chỉnh.
Thực hiện `Continuous deployment` cho ứng dụng.
Phải chạy song song nhiều instance của ứng dụng trên nhiều máy để đảm bỏ tính khả dụng và tính mở rộng.
Muốn có được các lợi thế của các công nghệ mới (frameworks, ngôn ngữ lập trình, v.v...)

## Solution
Để xây dựng một ứng dụng với kiến trúc nguyên khối ví dụ như:

Một Java WAR file.
Một thư mục phân cấp của Rails hoặc NodeJs.

## Example
Hãy tưởng tượng bạn muốn xây dựng một ứng dựng tương mại điện tử có các chức năng như nhân đặt hàng tự khác hàng, xác nhận nhận tồn kho và tính dụng có hiệu lực (đã thanh toán hoặc còn dư nợ) sau đó vận chuyển hàng hóa. Ứng dụng bao gồm 1 vài thành phần bao gồm giao diện kho, kèm theo đó là `backend services` để kiểm tra tính dụng, bảo trì kho và chuyển đơn hàng.

Ưng dụng được triển khai như một ứng dụng nguyên khối duy nhất. Ví dụ như, một ứng dụng web Java bao gồm 1 tập tin WAR dùng để chạy trên một `container` như Tomcat. Một ứng dụng Rails bao gồm tập hợp các thư mục kế thừa, như là Phusion Passenger on Apache/Nginx hoặc JRuby trên Tomcat. Bạn có thể chạy nhiều `instance` của ứng dụng đằng sau `load balancer` để có thể mở rộng cũng như tăng tính khả dụng.

## Resulting context
Giải pháp này có một số các lợi ích:

- Phát triển đơn giản - mục tiêu của các công cụ phát triển và IDEs là đê hổ trợ việc phát triển các ứng dụng nguyên khối.
- Dễ dàng triển khai - chỉ cần triển khai file WAR (hoặc thư mục) trên các `runtime` phù hợp.
- Dễ dàng mở rộng - chỉ cần mở rộng bằng cách chạy nhiều bản sao của ứng dụng đằng sau `laod balancer`.
Nhưng, một ứng dụng trở nên rất lớn và đội ngũ phát triên cũng tăng lên, phương pháp tiếp cận này có một số hạn chế ngày càng trở nên nghiệm trọng hơn:



This solution has a number of benefits:

Simple to develop - the goal of current development tools and IDEs is to support the development of monolithic applications
Simple to deploy - you simply need to deploy the WAR file (or directory hierarchy) on the appropriate runtime
Simple to scale - you can scale the application by running multiple copies of the application behind a load balancer
However, once the application becomes large and the team grows in size, this approach has a number of drawbacks that become increasingly significant:

The large monolithic code base intimidates developers, especially ones who are new to the team. The application can be difficult to understand and modify. As a result, development typically slows down. Also, because there are not hard module boundaries, modularity breaks down over time. Moreover, because it can be difficult to understand how to correctly implement a change the quality of the code declines over time. It’s a downwards spiral.

Overloaded IDE - the larger the code base the slower the IDE and the less productive developers are.

Overloaded web container - the larger the application the longer it takes to start up. This had have a huge impact on developer productivity because of time wasted waiting for the container to start. It also impacts deployment too.

Continuous deployment is difficult - a large monolithic application is also an obstacle to frequent deployments. In order to update one component you have to redeploy the entire application. This will interrupt background tasks (e.g. Quartz jobs in a Java application), regardless of whether they are impacted by the change, and possibly cause problems. There is also the chance that components that haven’t been updated will fail to start correctly. As a result, the risk associated with redeployment increases, which discourages frequent updates. This is especially a problem for user interface developers, since they usually need to iterative rapidly and redeploy frequently.

Scaling the application can be difficult - a monolithic architecture is that it can only scale in one dimension. On the one hand, it can scale with an increasing transaction volume by running more copies of the application. Some clouds can even adjust the number of instances dynamically based on load. But on the other hand, this architecture can’t scale with an increasing data volume. Each copy of application instance will access all of the data, which makes caching less effective and increases memory consumption and I/O traffic. Also, different application components have different resource requirements - one might be CPU intensive while another might memory intensive. With a monolithic architecture we cannot scale each component independently

Obstacle to scaling development - A monolithic application is also an obstacle to scaling development. Once the application gets to a certain size its useful to divide up the engineering organization into teams that focus on specific functional areas. For example, we might want to have the UI team, accounting team, inventory team, etc. The trouble with a monolithic application is that it prevents the teams from working independently. The teams must coordinate their development efforts and redeployments. It is much more difficult for a team to make a change and update production.

Requires a long-term commitment to a technology stack - a monolithic architecture forces you to be married to the technology stack (and in some cases, to a particular version of that technology) you chose at the start of development . With a monolithic application, can be difficult to incrementally adopt a newer technology. For example, let’s imagine that you chose the JVM. You have some language choices since as well as Java you can use other JVM languages that inter-operate nicely with Java such as Groovy and Scala. But components written in non-JVM languages do not have a place within your monolithic architecture. Also, if your application uses a platform framework that subsequently becomes obsolete then it can be challenging to incrementally migrate the application to a newer and better framework. It’s possible that in order to adopt a newer platform framework you have to rewrite the entire application, which is a risky undertaking.

## Related patterns
The microservice architecture is an alternative pattern that addresses the limitations of the monolithic architecture.

## Known uses
Well known internet services such as Netflix, Amazon.com and eBay initially had a monolithic architecture. Most web applications developed by the author had a monolithic architecture.

## Variations