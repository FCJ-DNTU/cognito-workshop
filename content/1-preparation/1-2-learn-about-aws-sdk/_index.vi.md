+++
title = "Tìm hiểu qua về AWS SDK"
date = 2025
weight = 2
chapter = false
+++

### Software Development Kit (SDK) là gì?

Để bài workshop có thể thực hiện được hiệu quả cao khi thực hành, thì chúng ta sẽ cùng đi tìm hiểu về AWS SDK. Để xem nó là gì và đóng vai trò như thế nào trong dự án của chúng ta. Trước tiên thì SDK là gì? Theo định nghĩa trên Google, thì SDK là một bộ công cụ dùng để phát triển phần mềm, ứng dụng trên một nền tảng nào đó, mà nó giúp cho phần mềm và ứng dụng của mình có thể giao tiếp, thực hiện các công việc, tác vụ nhờ vào nền tảng đó. SDK thì mình sẽ chia nó ra làm 2 dạng:

- Native SDK: là các SDK sẽ được sử dụng phụ thuộc với nền tảng, ví dụ như Windows SDK, mình phải dùng chung nó với hệ điều hành Windows để phát triển các ứng dụng ASP, Windows Forms, C++, ...
- Seperated SDK: là các SDK tách rời với nền tảng. Nó sẽ giúp mình thao tác với REST API (hoặc các loại API khác) của nền tảng dễ dàng hơn. AWS SDK chính là dạng SDK này.

Tóm lại thì, về cơ bản thì các SDK đều sẽ giúp mình giao tiếp với API của nền tảng, có thể nó tách biệt với nền tảng hoặc không. Lưu ý là những cái tên trên kia là do mình tự đặt ra để mình dễ phân biệt và trong bài này chúng ta có thể hiểu được phần nào đó về AWS SDK.

![1.2.1](/images/1-preparation/1-2-learn-about-aws-sdk/1.2.1-sdk-types.png)

### AWS SDK

Và theo như định nghĩa thôi AWS SDK là bộ công cụ bao gồm các thư viện, hàm, được chuẩn hoá lại để giao tiếp với các dịch vụ của AWS dễ dàng hơn.

![1.2.2](/images/1-preparation/1-2-learn-about-aws-sdk/1.2.2-sdk-overview-v3.png)

Trong bài workshop này thì chúng ta sẽ đi tìm hiểu SDK cho Typescript và Python. Mỗi một ngôn ngữ sẽ có cách dùng khác nhau, nên cần lưu ý là bạn phải đọc tài liệu chính thức của họ nhé.

### AWS SDK for Javascript/Typescript

Khi thao tác với công cụ nào đó, thì mình sẽ cần phải vào trong tài liệu chính thức của công cụ đó, đây là một trong những kỹ năng cần có để chúng ta có thể debug sâu nhưng vấn đề đang gây lỗi (kể cả trong thời đại mà ai cũng dùng tới GenAI).

Đây là giao diện chính của trang JS SDK chính thức.

![1.2.3](/images/1-preparation/1-2-learn-about-aws-sdk/1.2.3-aws-sdk-js-homepage.png)

Giờ mình sẽ tìm thử DynamoDB. Đây là trang chủ của DynamoDB, bạn có thể thấy là họ hướng dẫn mình cài đặt SDK luôn ở đây.

![1.2.4](/images/1-preparation/1-2-learn-about-aws-sdk/1.2.4-aws-sdk-js-dynamodb-homepage.png)

Kéo xuống dưới nữa thì chúng ta có thể thấy được một số các Commands hiện tại đang được hỗ trợ. Khi mà bạn muốn thực hiện một việc gì đó với dịch vụ này, thì bạn sẽ cần phải định nghĩa yêu cầu (Command) của bạn và gửi nó đi, thì đấy là cách mà JS SDK nó hoạt động.

![1.2.5](/images/1-preparation/1-2-learn-about-aws-sdk/1.2.5-aws-sdk-js-dynamodb-commands.png)

Và để tìm bất kì Command nào thì bạn có thể vào đây luôn để tìm và đọc cách sử dụng của Command đó. Mình lấy ví dụ với **PutItemCommand**. Tìm trước và ấn chọn.

![1.2.6](/images/1-preparation/1-2-learn-about-aws-sdk/1.2.6-aws-sdk-js-dynamodb-command-search.png)

Trong này thì tài liệu có phần giới thiệu về Command.

![1.2.7](/images/1-preparation/1-2-learn-about-aws-sdk/1.2.7-aws-sdk-js-dynamodb-putitemcommand-1.png)

Kéo xuống dưới là phần hướng dẫn sử dụng.

![1.2.8](/images/1-preparation/1-2-learn-about-aws-sdk/1.2.8-aws-sdk-js-dynamodb-putitemcommand-2.png)

Và đây là các thông tin về input của command này.

![1.2.9](/images/1-preparation/1-2-learn-about-aws-sdk/1.2.9-aws-sdk-js-dynamodb-putitemcommand-3.png)

Ví dụ một command.

```ts
// This example adds a new item to the Music table.
const input = {
  Item: {
    AlbumTitle: {
      S: "Somewhat Famous"
    },
    Artist: {
      S: "No One You Know"
    },
    SongTitle: {
      S: "Call Me Today"
    }
  },
  ReturnConsumedCapacity: "TOTAL",
  TableName: "Music"
};
const command = new PutItemCommand(input);
const response = await client.send(command);
/* response is
{
  ConsumedCapacity: {
    CapacityUnits: 1,
    TableName: "Music"
  }
}
*/
```

Mình có thể mô hình hoá tổng quan mà cách SDK này hoạt động như sau

![1.2.10](/images/1-preparation/1-2-learn-about-aws-sdk/1.2.10-general-js-sdk-workflow.png)

### AWS SDK for Python (boto3)

Python thì khác rất nhiều, AWS không có trang document như của Typescript/Javascript, nhưng nhìn chung thì vẫn đảm bảo đầy đủ các thông tin mà chúng ta cần có dể thực hiện công việc. AWS SDK của Python được gọi là Boto3 (ý nghĩa của cái tên này là từ tên của một loài cá heo nước ngọt - Boto, sống ở sông Amazon - có vẻ như các bác viết thư viện Python thích đặt tên thư viện theo các con vật, loài vật).

Đây là trang giao diện chính của Boto3:

![1.2.11](/images/1-preparation/1-2-learn-about-aws-sdk/1.2.11-aws-sdk-py-homepage.png)

Tương tự với thao tác trước đó, nhưng mình sẽ tìm Cognito. Bạn có thể thấy có tới 3 cái, nhưng chúng ta sẽ tập chung vào cái **CognitoIdentityProvider**, vì mình đang cần sử dụng đúng tính năng từ **CognitoIdentityProvider**.

![1.2.12](/images/1-preparation/1-2-learn-about-aws-sdk/1.2.12-aws-sdk-py-cognito-search.png)

Đây là mô tả của lớp CognitoIdentityProvider dùng để tạo ra client cho dịch vụ này. JS SDK cũng có, nhưng mình sẽ nói ở trong lúc build luôn.

![1.2.13](/images/1-preparation/1-2-learn-about-aws-sdk/1.2.13-aws-sdk-py-cognito-homepage.png)

Nhưng khác biệt là Boto3 không có Commands, chỉ có các phương thức ở trong class.

![1.2.14](/images/1-preparation/1-2-learn-about-aws-sdk/1.2.14-aws-sdk-py-cognito-methods.png)

Giờ thì mình sẽ vào thư phương thức **admin_initiate_auth**, và các bạn cũng có thể thấy phần giới thiệu, phần input và output.

![1.2.15](/images/1-preparation/1-2-learn-about-aws-sdk/1.2.15-aws-sdk-py-cognito-admininitiateauth-homepage-1.png)

![1.2.16](/images/1-preparation/1-2-learn-about-aws-sdk/1.2.16-aws-sdk-py-cognito-admininitiateauth-homepage-2.png)

![1.2.17](/images/1-preparation/1-2-learn-about-aws-sdk/1.2.17-aws-sdk-py-cognito-admininitiateauth-homepage-3.png)

Đây là một đoạn mã mẫu hướng dẫn sử dụng.

```py
response = client.admin_initiate_auth(
    UserPoolId="string",
    ClientId="string",
    AuthFlow="USER_PASSWORD_AUTH",
    AuthParameters={
        "USERNAME": "string",
        "PASSWORD": "string"
    }
)
```

Và mình có thể mô hình hoá cách thức hoạt động của SDK này như sau:

![1.2.18](/images/1-preparation/1-2-learn-about-aws-sdk/1.2.18-general-boto3-sdk-workflow.png)

Tới đây thì mình nghĩ là bạn cũng đã phần nào nắm được sơ về AWS SDK là gì rồi, nếu vẫn chưa nắm được thì không sao vì trong phần thực hành chúng ta sẽ thực hiện thao tác với SDK cũng khá nhiều, nên mình tin chắc là sau bài workshop này bạn sẽ nắm được AWS SDK (nhớ luyện tập thêm nhiều để thành thạo nhé).