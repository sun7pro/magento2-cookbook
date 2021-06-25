# 1. Magento là gì ?
Magento là một mã nguồn mở tạo web thương mại điện tử đã được ra mắt vào ngày 31 tháng 3 năm 2008. Magento được phát triển bởi Varien, với sự giúp đỡ từ các lập trình viên trong cộng đồng mã nguồn mở, nhưng chỉ được sở hữu bởi Magento Inc. Magento được xây dựng trên nền tảng Zend Framework. Nó sử dụng các mô hình thực thể thuộc tính giá trị cơ sở dữ liệu (EAV – entity-attribute-value) để lưu trữ dữ liệu.

Magento Community Edition (viết tắt là Magento CE) là phiên bản miễn phí của Magento có sẵn.

Theo ước tính của W3Techs vào tháng 4 năm 2014, Magento được sử dụng trên 1.0% tổng số website[3], tính đến năm 2016, con số tổng lượng website sử dụng Magento đã lên tới hơn 250,000 websites

Theo wiki.
Ở Việt Nam hiện nay có khá nhiều website thương mại điện tử lớn tại Việt Nam như, PNJ Shopping, Trung nguyên ... tính đến 15/2/2021 cơ khoảng 480  website Magento cả lớn nhỏ tại Việt Nam theo [Builtwith.com](https://trends.builtwith.com/websitelist/Magento/Vietnam)

## Các tính năng chính có sẵn của Magento ở phiên bản Open Source
* Các tính năng quản lý toàn bộ site Magento ecommerce
* Chức năng checkout, thanh toán, vận chuyển
* Tối ưu hóa tìm kiếm trên search engine
* Phân tích và báo cáo
* Các chức năng hỗ trợ tiếp thị, quảng cáo
* Chức năng quản lý bán hàng
* Quản lý khách hàng, dịch vụ khách hàng
* Quản lý danh mục sản phẩm, quản lý tồn kho
* Quản lý sản phẩm

Trên thực tế để có thể đưa vào sử dụng phù hợp với từng doanh nghiệp , lập trình viên cần phải custom rất nhiều như Phát triển, tích hợp, tùy biến thêm, tích hợp thanh toán....
# 2. Module
Module là một nhóm logic - 1 thư mục chứa các blocks, controllers, helpers, models -  có liên quan đến một tính năng cụ thể . Để phù hợp với cam kết của Magento về module  tối ưu, một module chỉ nên gói gọn một tính năng và ít phụ thuộc vào module khác nhất có thể.

Các module và theme là các đơn vị customization trong Magento. Module cung cấp các tính năng kinh doanh, với logic hỗ trợ, trong khi theme ảnh hưởng để UI,UX . Cả hai thành phần đều có vòng đời cho phép chúng được cài đặt, xóa và vô hiệu hóa. Từ quan điểm của cả người bán và nhà phát triển phần mở rộng , Module được xem như  đơn vị hạt nhân của Magento.

### Chức năng của module

Mục đích củamodule là cung cấp các tính năng sản phẩm cụ thể bằng cách triển khai chức năng mới hoặc mở rộng chức năng của các module khác. Mỗi module được thiết kế để hoạt động độc lập, do đó, việc bao gồm hoặc loại trừ một module cụ thể thường không ảnh hưởng đến chức năng của các module khác.

### Vị trí của Module
Các Module thường nằm trong  thư mục vendor của cài đặt Magento, trong thư mục có định dạng tuân thủ PSR-0 sau:, vendor/<vendor>/<type>-<module-name>trong đó <type>có thể là một trong các giá trị sau:
*  *module* - for modules (module-customer-import-export)
* *theme* - for frontend and admin themes (theme-frontend-luma or theme-adminhtml-backend)
* *language* - for language packs (language-de_de)
    
Chúng ta có thể xem thử 1 module Catalog là các chứng năng liên quan đến product:
    ![](https://images.viblo.asia/0ebf50c5-2c50-4a9b-8387-6c3983217600.png)
https://devdocs.magento.com/guides/v2.4/architecture/archi_perspectives/components/modules/mod_intro.html?itm_source=devdocs&itm_medium=search_page&itm_campaign=federated_search&itm_term=module
    

#  Thành phần của một module
  
### Các thư mục thường có trong 1 module
    
    
**registration.php**:  mỗi module phải có 1 file *registration.php* trong thư mục gốc của module để khai báo module.


**Block**: bao gồm các views classes PHP như một phần module logic implementation mô hình Model - View - Controller (MVC) triển khai dọc.
     `Block` là một trong những class elements đầu tiên trong cấu trúc của layouts Magento. Mỗi trang trong Magento được trang trí bởi `Layouts`  files và nội dung được lấp đầy bởi **Blocks** của các module khác nhau. Các Magento blocks là rất mạnh mẽ và linh hoạt để thêm các content của bạn vào các layout đã có. Mặt khác, bạn cũng có thể sử dụng các file layout XML để dễ dàng loại bỏ hoặc định vị lại bất kỳ blocks hiện có.
    
 Magento chia block thành hai loại : **structure block** và **content block.**
    
* ***Structure Block***: đây là những block được tạo ra để định dạng vị trí cho những block khác trong một page . Cùng xem ví dụ với trang chủ của magento default sử dụng three column layout.
    
Ví dụ, `Header`, `Footer`, `Left` và `Right` là các structural blocks trong Magento. **Content blocks** được gán cho các vị trí cấu trúc khác nhau của layout của trang Magento, từ đó hiển thị nội dung thực tế của các khối content blocks.
Footer
    
* ***Content Block***: đây là những block nội dung , chúng được hiển thị bên trong những structure block . Mỗi content block thường hiển thị một khối nội dung nào đó thông qua những file template và chèn vào những block cha là structure block .
   
Hãy xem right column có những content block nào.
    
`minicart`, `Recently`,` viewed product`, `Newsletter subscription`, `block`, `Poll`
     
Khi nhận được một request từ người dùng để hiển thị một page :
    
Magento sẽ load những vùng cấu trúc
Mỗi vùng cấu trúc có những content block . Magento sẽ tập hợp những block này lại và gán cho các structure block tương ứng theo layout để xử lý đầu ra .
Cuối cùng hệ thống sẽ trả lại kết quả sau cùng cho trình duyệt phần nội dung đã được định dạng vị trí .

Chúng ta cũng có thể tự custom Block.
    
**Controller**:includes các controller classes  PHP như là một phần của việc implementation  MVC logic 
    
  `Controller` là một phần quan trọng trong module nói riêng và mô hình MVC nói chung. Nhiệm vụ của `controller` là nhận request, xử lý và render page. Có thể có một hoặc nhiều file trong thư mục `controller` của module. Có hai loại `controller` là `frontend controller` và` backend controller`. Về bản chất thì 2 loại này giống nhau chỉ khác là ở `backend controller` sẽ có thêm một method để kiểm tra quyền truy cập. 
   
có thể tham khảo tại đây chi tiết hơn về cách tạo vào overide controller https://www.mageplaza.com/magento-2-module-development/how-to-create-controllers-magento-2.html
    
**etc**: includes  các file configuration; 

*  **module.xml**:   là file bắt buộc cần có xác định thông tin cơ bản về component, ví dụ:  component dependencies and version number.   Version sẽ được sử dụng để xác định schema  và cập nhật dữ liệu sau khi chúng ta update dữ liệu.
    
*  **di.xml**  : config các  dependencies  được inject bởi các object manager . Chúng ta cũng có thể chỉ định cài đặt cấu hình sensitive sử dụng di.xml.

**Model**: includes các classes model PHP như một phần của việc triển khai theo chiều dọc MVC logic mô-đun.
    
Trong Magento 2 Models sẽ chịu trách nhiệm làm việc với database, và được chia thành ba phần là Model, Resource Model và Collection. Được sử dụng để làm việc với các hoạt động CRUD(create, read, update, delete), bạn không cần phải viết bất kỳ một dòng code SQL nào nữa.
    
**Setup**: bao gồm các lớp cho  module database structure  và setup data. Những dữ liệu này được gọi khi installing hoặc upgrading.
    
 Trong một vài trường hợp có lẽ cần phải  cập nhật cơ sở dữ liệu, thêm một bảng mới hoặc thêm một vài field trong product form… Để làm được điều này, các setup script  sẽ giúp chúng ta.
    
*  **InstallSchema**: Tập lệnh này sẽ chạy khi lần đầu cài đặt module được dùng để thiết lập cấu trúc database.
    
*  **InstallData**: Tập lệnh này sẽ chạy khi lần đầu cài đặt module được dùng để khởi tạo dữ liệu cho một bảng trong database.
    
*  **UpgradeSchema**: Tập lệnh này sẽ chạy khi nâng cấp module được dùng để thiết lập cấu trúc database.
    
* **UpgradeData**: Tập lệnh này sẽ chạy khi nâng cấp module được dùng để khởi tạo dữ liệu cho một bảng trong database.
   
*  **Recurring**
    
*  **Uninstall**
    
###  Các thư mục bổ sung
 Các thư mục này có thể được thêm vào để configuration hoặc các chức năng hỗ trợ trợ của các mục khác như bản localization,plugin-ins, and layout files.
    
**Api**: bao gồm bất kỳ class PHP nào được tiếp xúc với API.
    
   Nó Chứa tập hợp code mà các nhà phát triển bên thứ ba có thể gọi, implement, or build as a plug-in.. Magento đảm bảo rằng code này sẽ không thay đổi trong các bản phát hành tiếp theo mà không có sự thay đổi phiên bản lớn.
Các giao diện công khai cho một module được đánh dấu bằng chú thích @api.

Các phương thức lưu trữ của Magento 2 có thể dễ dàng được hiển thị dưới dạng API resources, làm cho chúng có giá trị để tích hợp với hệ thống của bên thứ ba hoặc các phiên bản headless Magento instances

**Console**: bao gồm các lệnh CLI.

**Cron**: define các cron job.

**CustomerData**: các section files.

**Helper**: bao gồm chức năng tổng hợp.

**i18n**:  lưu các file translate đa ngôn ngữ

**Observer**:  nơi exec các commands và listener.

**Plugin**: includes bất kì plugin nào cần

**UI**: generation file. 

**view**: bao gồm các file view, containing static view files, email templates, design templates, and layout files.

Trên đây mình đã trình  bày sơ qua về cấu trúc 1 module magento để hiểu rõ hơn trong phần sau mình sẽ giới thiệu đến các bạn cách tạo 1 module trong magento .
    
###  Nguồn tham khảo
https://devdocs.magento.com/ https://www.mageplaza.com/magento-2-module-development/