### Webview là gì?
- Hiểu một cách đơn giản thì Webview là một đối tượng view trong Android cho phép lập trình viên có thể hiển thị nội dung của một Website vào trong ứng dụng của mình thông qua 1 đường dẫn hoặc bạn có thể định nghĩa 1 file HTML trong project của bạn và sử dụng Webview để đọc file đó. Nói 1 cách đơn giản hơn thì Webview giúp biến ứng dụng của bạn trở thành 1 Web Application ( "turns your application to a web application" )

### Sử dụng Webview như thế nào?
- **Để sử dụng Webview ta có 2 trường hợp có thể cần đến:**
  - Nạp Website từ 1 HTML trong project
  - Nạp Website từ 1 đường dẫn của website đó.

## iOS
### Tìm Hiểu Về WebView Trong iOS  

**iOS** hỗ trợ **WebView** thông qua hai lớp chính trong framework **WebKit**:

  - **UIWebView:** Được sử dụng trong các phiên bản iOS cũ hơn (trước iOS 8). Tuy nhiên, UIWebView đã bị Apple deprecate và không nên sử dụng trong các ứng dụng mới.

  - **WKWebView:** Được giới thiệu từ iOS 8 trở lên, là phiên bản cải tiến của UIWebView. WKWebView có hiệu suất cao hơn và hỗ trợ nhiều tính năng hiện đại như:
    - Tăng tốc phần cứng khi hiển thị nội dung web.
    - Tương tác tốt hơn với JavaScript thông qua WKScriptMessageHandler.
    - Hỗ trợ tải các trang web từ URL hoặc chuỗi HTML.
    - Tính năng bảo mật và quản lý bộ nhớ tốt hơn so với UIWebView.

#### 1.1 **Nạp Website từ một file HTML trong ứng dụng**  
Đầu tiên, bạn cần thêm một `WKWebView` vào giao diện của ứng dụng bằng cách tạo nó trong code hoặc thêm vào storyboard.  

Ví dụ tạo `WKWebView` trong code:  
```swift
import UIKit
import WebKit

class ViewController: UIViewController {
    var webView: WKWebView!

    override func viewDidLoad() {
        super.viewDidLoad()
        
        webView = WKWebView(frame: self.view.frame)
        self.view.addSubview(webView)
        
        if let filePath = Bundle.main.path(forResource: "yourfile", ofType: "html") {
            let url = URL(fileURLWithPath: filePath)
            webView.loadFileURL(url, allowingReadAccessTo: url)
        }
    }
}
```

#### 1.2 **Nạp Website từ một URL**  
Để nạp một trang web từ URL, bạn chỉ cần gọi phương thức `load()` của `WKWebView` với một đối tượng `URLRequest`.  

Ví dụ:  
```swift
let url = URL(string: "https://www.apple.com")!
let request = URLRequest(url: url)
webView.load(request)
```

Nếu trang web cần chạy JavaScript, bạn phải bật JavaScript bằng cách cấu hình `WKWebViewConfiguration`:  
```swift
let config = WKWebViewConfiguration()
config.preferences.javaScriptEnabled = true
webView = WKWebView(frame: self.view.frame, configuration: config)
```

### 2: **Xử lý sự kiện trong WKWebView**  
Sau khi bạn đã hiển thị thành công một trang web trong ứng dụng, bạn có thể muốn xử lý các sự kiện khi người dùng tương tác với web (ví dụ: nhấn vào một đường dẫn, tải trang mới, hoặc xử lý lỗi khi tải). Để làm điều này, bạn cần sử dụng `WKNavigationDelegate`.  

Ví dụ:  
```swift
extension ViewController: WKNavigationDelegate {
    func webView(_ webView: WKWebView, didStartProvisionalNavigation navigation: WKNavigation!) {
        print("Bắt đầu tải trang")
    }

    func webView(_ webView: WKWebView, didFinish navigation: WKNavigation!) {
        print("Hoàn tất tải trang")
    }

    func webView(_ webView: WKWebView, didFail navigation: WKNavigation!, withError error: Error) {
        print("Lỗi khi tải trang: \(error.localizedDescription)")
    }
}
```

Đừng quên gán delegate cho `WKWebView`:  
```swift
webView.navigationDelegate = self
```

### 3: **Xử lý BackStack**  
Khi người dùng nhấn nút back trong ứng dụng, bạn có thể muốn quay lại trang web trước đó thay vì thoát ứng dụng. Để làm điều này, bạn cần kiểm tra stack của `WKWebView` và xử lý thủ công trong `viewWillDisappear` hoặc khi người dùng nhấn nút back.  

Ví dụ:  
```swift
override func viewWillDisappear(_ animated: Bool) {
    if webView.canGoBack {
        webView.goBack()
    } else {
        super.viewWillDisappear(animated)
    }
}
```
## Android

### 1.1 Nạp Website từ 1 file HTML

Đầu tiên, ta sẽ cần thiết lập 1 Webview trong layout của bạn bằng 1 thẻ Webview trong file
  
![image](https://github.com/user-attachments/assets/f63aff9b-2b75-4824-b119-4708d5b6cb44)

**MainActivity.xml:**
  
![image](https://github.com/user-attachments/assets/4939c868-1d77-48e7-b93a-5c197593ce5a)

Tiếp đến ta cần thiết lập 1 file **yourfile.html** với định dạng HTML trong project của bạn ở thư mục **assets**.

![image](https://github.com/user-attachments/assets/c51ed90f-a18e-4068-b782-4a70d14dfa2c)

Và sau đó là thực hiện load file HTML đã định nghĩa trong **MainActivity.kt** sử dụng phương thức **loadUrl()** của WebView.
  
![image](https://github.com/user-attachments/assets/8633c92e-43d4-439d-98d9-5da77110f5a4)

### 1.2 Nạp Website từ 1 đường dẫn đến website.

Với trường hợp load website từ 1 đường dẫn của web thì đầu tiên bạn cần cấp quyền Internet cho ứng dụng.
  
![image](https://github.com/user-attachments/assets/f365a619-c5d2-46b7-8ac6-31f3746775ce)

Sau đó tiến hành truyền Url của Web vào phương thức **loadUrl()** của Webview. Thiết lập webview trong file xml bạn vẫn sẽ làm như với 2.1.

![image](https://github.com/user-attachments/assets/7507616a-03b4-4369-89be-77dd7e499962)
  
Trường hợp website có sử dụng Javascripts thì bạn có thể thêm dòng lệnh bên dưới
  
![image](https://github.com/user-attachments/assets/2784c376-c48f-4aae-ab1b-d42ac36fb647)

Tiến hành chạy ứng dụng và bạn sẽ hiển thị được web trong ứng dụng của bạn

![image](https://github.com/user-attachments/assets/64cab848-11c4-4c61-bb6b-2d90b2a5f547)

### 2. Xử lý sự kiện trong Webview.

Vậy là bạn đã thành công load được 1 website lên ứng dụng của mình rồi 💯

Tuy nhiên, sau khi website được hiển thị trong app thì làm thế nào lập trình viên có thể xử lí được sự kiện khi người dùng sử dụng website đó? Ví dụ như khi họ nhấn vào 1 đường dẫn hay khi họ nhấn back trên web. Lập trình viên sẽ lắng nghe và xử lý sự kiện đó như thế nào ?

Để giải quyết vấn đề này thì Webview có 1 giải pháp được gọi là ****WebViewClient****.

**WebViewClient** được hiểu là nơi xử lý khi người dùng tương tác với website được load lên trong ứng dụng của bạn. Bình thường nếu không sử dụng **WebViewClient** thì khi bạn tương tác với web(nhấn vào 1 đường dẫn trong web, ...) thì đường dẫn mà bạn nhấn sẽ được load lên bởi trình duyệt mặc định của điện thoại chứ không phải bởi Webview nữa. Lúc này sẽ rất khó để kiểm soát các tương tác với website đó.

Với Kotlin bạn chỉ cần setup bằng hàm webViewClient() sau đó truyền 1 object WebviewClient và tiến hành ghi đè phương thức shouldOverrideUrlLoading() để xử lý mỗi khi người dùng nhấn vào bất kỳ link nào trên website đồng thời mở link đó với Webview chứ không phải sử dụng trình duyệt của thiết bị.

Hàm **shouldOverrideUrlLoading()** sẽ chạy ngay sau khi hàm **loadUrl()** được thực thi. Tại đây bạn có thể kiểm tra Url được click và có thể ghi đè Url truyền vào

![image](https://github.com/user-attachments/assets/42b4cfe3-1ab8-42eb-ba8a-672758fc7e67)

### 3. Các Callback Quan Trọng Khi Sử Dụng WebView

1. **`onPageStarted()`**  
   Hàm này sẽ được gọi khi trang web **bắt đầu load**, và nó được gọi **sau khi hàm `loadUrl()`** được thực thi.

2. **`onPageCommitVisible()`**  
   Hàm này sẽ được gọi khi nội dung của trang web **bắt đầu hiển thị lên giao diện** người dùng.

3. **`onPageFinished()`**  
   Hàm này sẽ được gọi khi trang web đã **hoàn tất việc load** và **hiển thị đầy đủ** trên màn hình.

4. **`onReceivedError()`**  
   Hàm này sẽ được gọi khi có lỗi xảy ra trong quá trình **load trang web**.



