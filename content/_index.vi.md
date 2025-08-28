+++
title = "Cognito Workshop"
date = 2025
weight = 1
chapter = false
+++

# Authentication and Authorization with Cognito Workshop

![cognito-service-icon](/images/cognito.png)

### Tổng quan

**Amazon Cognito** là một dịch vụ quản lý bởi AWS (fully-managed service) mà cho phép chúng ta có thể định danh và quản lý truy vập của người dùng, nghe giống với IAM đúng không, nhưng thực chất thì đây là 2 dịch vụ khác nhau. Với Amazon IAM thì mình có thể quản lý những người dùng sử dụng trực tiếp các dịch vụ AWS, có thể kể tới như là _Solution Architect_, _Cloud Engineer_, _Backend Developer_, _DevOps Engineer_, ... Còn với **Amazon Cognito** thì có thể giúp cho chúng ta quản lý những người dùng ở "bên ngoài", là những người dùng không sử dụng các dịch vụ AWS hoặc sử dụng các dịch vụ AWS gián tiếp.

Thử tưởng tượng nếu như bạn có một ứng dụng và giờ bạn muốn phân quyền người dùng của bạn, thì việc đầu tiên bạn cần làm đó chính là nhận diện được họ có phải là người dùng của bạn không ? Sau đó là tới bước tạo một "bằng chứng" để cho họ giữ, khi họ yêu cầu gì đó thì họ có thể đưa "bằng chứng" này ra và nói là "Tôi là người dùng của ứng dụng này", khi đó máy chủ sẽ quyết định xem là người dùng này có thể truy cập hay là không. Thay vì bạn cần phải xây dựng một máy chủ riêng để làm việc này, thì giờ chúng ta có thể dùng tới Cognito để định danh người dùng trong ứng dụng của bạn.

### Nội dung

Trong bài thực hành này, chúng ta sẽ tìm hiểu về Cognito trước để đảm bảo là chúng ta hiểu được cơ bản về dịch vụ này, sau đó sẽ lấy những kiến thức đã có để áp dụng vào bài thực hành. Trước khi có thể thực hành với Cognito, thì chúng ta sẽ cần phải xây dựng một ứng dụng mẫu, có chức năng cơ bản để có thể demo được. Với ứng dụng này thì mình sẽ build nó ở 2 ngôn ngữ khác nhau (mình sẽ giải thích rõ hơn trong phần 4. Xây dựng các dự án mẫu), bao gồm **Javascript** và **Python**, bạn có thể chọn ngôn ngữ mà mình thích để thực hành.

Sau khi hoàn thành việc xây dựng một ứng dụng nhỏ, thì chúng ta sẽ tiến hành thử nghiệm kết quả khi đã tích hợp với Cognito. Cuối cùng, sau khi kiếm tra kết quả xong, chúng ta sẽ viết tiếp mã nguồn để tiển khai module này với công nghệ server, bằng Lambda và tích hợp với API Gateway.

Để cho bạn có cài nhìn tổng quan hơn về bài thực hành này, thì hãy theo dõi kiến trúc dữ án ở bên dưới:

![architecture](/images/architecture.png)

Chúng ta sẽ có 2 người dùng thuộc hai nhóm (team) người dùng khác nhau là **Marketing** và **Sales**. Với mỗi yêu cầu được gửi từ những người dùng này, thì hệ thống sẽ kiểm tra định danh và phần quyền chức năng cho 2 nhóm người dùng này dựa trên những gì mà chúng ta sẽ khai báo.

Vậy thì sau bài thực hành này thì chúng ta sẽ biết thêm được gì?

- Cách setup một dự án với Typescript hoặc Python và thích hợp AWS SDK..
- Cách sử dụng AWS CLI.
- Triển khai một chức năng với AWS Lambda.
- Tích hợp API Server với API Gateway cũng như là Lambda Function.

Mình tin là sau bài thực hành này, bạn có thể nắm được cách mà Authentication và Authorization được triển khai trong một ứng dụng sẽ như thế nào. Và từ đó có thể áp dụng vào trong chính dự án của bạn.

### Mục lục

Trong bài này thì chúng ta sẽ thực hiện các phần sau:

1. [Các bước chuẩn bị](/1-preparation)
2. [Thiết lập Cognito User Pool](/2-setup-cognito-user-pool)
3. [Thiệp lập DynamoDB table](/3-setup-dynamodb-table)
4. [Xây dựng các ứng dụng mẫu](/4-build-example-applications)
5. [Triển khai các ứng dụng mẫu](/5-deploy-applications)
6. [Kiểm tra kết quả](/6-test-result)
7. [Xây dựng các hàm lambda](/7-build-lambda-functions)
8. [Tích hợp với API Gateway](/8-integrate-with-api-gateway)
9. [Kiểm tra kết quả](/9-test-result)
10. [Dọn dẹp tài nguyên](/10-clean-up-resources)
