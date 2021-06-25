# di.xml

**di.xml**  (***Dependency Injection***) là một tập tin quan trọng trong Magento 2, được dùng để khai báo các phụ thuộc của module trong Magento 2.

Magento 2 sử dụng Dependency Injection để thay thế chức năng được cung cấp bởi Mage class trong Magento 1.x.

*Để hiểu được Dependency Injection là gì có thể tham khảo bài viết* https://viblo.asia/p/dependency-injection-hoat-dong-the-nao-trong-laravel-3Q75wD3JKWb

## File di.xml nằm ở đâu ?
Mỗi module sẽ có 1 tập tine di.xml ở gobal và area. Magento đọc tất cả các file di.xml khai báo trong hệ thống vàmerges tất cả chúng lại với nhau bằng cách nối tất cả các nodes. Các nodes ở đây chính là các thẻ như `preference`, `type`, `virtualType`, `Plugin`

Magento sẽ load cấu hình của những tập tin di.xml có trong cái đường dẫn sau:

1. Tập tin khởi tạo (Initial) (`app/etc/di.xml`)
1. Tập tin chung (global) của các modules (`<moduleDir>/etc/di.xml`)
1. Tập tin riêng cho mỗi thành phần con (`<moduleDir>/etc/<area>/di.xml`)

## Khi nào chúng ta có thể sử dụng di.xml ?
* Để viết lại, ghi đè 1 class với  preference node
* Thay thế các đối số  trong constructor của class hiện có với arguments.
* Sử dụng các plugin dể thực hiện 1 số công việc before, after and around của function.
* Sử dụng virtualTypes tạo 1 class con của class khác  với virtualType node.

## `<preference>`
```php
<config  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:ObjectManager/etc/config.xsd">
    <preference for="Magento\Checkout\Block\Onepage\Success" type="ExampleMagento\OverrideExample\Block\Onepage\CheckoutSuccess" />
</config>
```

**preference** dùng để ghi đè(override) một class cụ thể. Ở đoạn code trên chúng ta có thể hiểu class `CheckoutSuccess` sẽ ghi đè lên class `Success`. Điều này nói cho Magento biết rằng những nơi nào có khởi tạo class `Success` thì hãy thay thế bằng class `CheckoutSuccess`.

*preference không chỉ được dùng cho* `Interface`. Chúng có thể sử dụng cho bất cứ class nào mà bạn muốn. Nhưng cách này thì không được khuyến khích. Cách làm này có thể gây ra xung đột trong hệ thống và làm giảm khả năng nâng cấp của hệ thống . Nên thường sử dụng `plugin` hoặc `events`, nếu các cách còn lại không được thì mới sử dụng `preference`.

Ví dụ dưới đây ghi đè method `isVisible` từ  class` Magento\Checkout\Block\Onepage\Success`
```php
namespace ExampleMagento\OverrideExample\Block\Onepage;

class CheckoutSuccess extends \Magento\Checkout\Block\Onepage\Success
{
    /**
     * Constructor Modification
     *
     * @param \Magento\Framework\View\Element\Template\Context $context
     * @param \Magento\Checkout\Model\Session $checkoutSession
     * @param \Magento\Sales\Model\Order\Config $orderConfig
     * @param \Magento\Framework\App\Http\Context $httpContext
     * @param array $data
     */
    public function __construct(
        \Magento\Framework\View\Element\Template\Context $context,
        \Magento\Checkout\Model\Session $checkoutSession,
        \Magento\Sales\Model\Order\Config $orderConfig,
        \Magento\Framework\App\Http\Context $httpContext,
        array $data = []
    ) {
        parent::__construct(
            $context,
            $checkoutSession,
            $orderConfig,
            $httpContext,
            $data
        );
    }

    /**
     * Is order visible
     *
     * @param Order $order
     * @return bool
     */
    protected function isVisible(Order $order)
    {
        # Write your custom logic here.
        return !in_array(
            $order->getStatus(),
            $this->_orderConfig->getInvisibleOnFrontStatuses()
        );
    }
}
```
## Plugin
```php
<type name="Magento\RelatedProductGraphQl\Model\Resolver\Batch\CrossSellProducts">
        <plugin name="not_show_product_disable_plugin" type="ExampleMagento\OrderAPI\Plugin\FilterEnabledCrossSellProducts" sortOrder="1" disabled="false" />
    </type>
```
Plugin thường được sử dụng nhiều. Sử dụng để tác động đến hành vi của một class hay method bằng cách chặn lệnh gọi đến method ban đầu và run code của chúng ta. Có ba loại plugin là
* **before - beforeDispatch()**
* **around - aroundDispatch()**
* **after - afterDispatch()**

Ví dụ như trong đoạn code trên mình sẽ ghi đè lại method `resolve` trong class `Magento\RelatedProductGraphQl\Model\Resolver\Batch\CrossSellProducts` để không lấy ra những  `product` có *status là disable.*
```php
<?php

namespace ExampleMagento\OrderAPI\Plugin;

use Magento\Framework\GraphQl\Query\Resolver\BatchResponse;
use Magento\Catalog\Model\Product\Attribute\Source\Status;

class FilterEnabledCrossSellProducts
{
    public function afterResolve(
        \Magento\RelatedProductGraphQl\Model\Resolver\Batch\CrossSellProducts $subject,
        BatchResponse $result,
        $context,
        $field,
        $requests
    ) {
        $newResult = new BatchResponse();
        /** @var \Magento\Framework\GraphQl\Query\Resolver\BatchRequestItemInterface $request */
        foreach ($requests as $request) {
            $products = $result->findResponseFor($request);

            foreach ($products as $key => $productData) {
                if ($productData['status'] == Status::STATUS_DISABLED) {
                    unset($products[$key]);
                }
            }

            $newResult->addResponse($request, $products);
        }

        return $newResult;
    }
}
```
## `<type>`
```php
<type name="Magento\Customer\Model\ResourceModel\Group" shared="false">
    <arguments>
        <argument name="groupManagement" xsi:type="object">Magento\Customer\Api\GroupManagementInterface\Proxy</argument>
    </arguments>
</type>

```
 Ở đây chúng ta có thể hiểu rằng chúng ta đang tạo mới một đối tượng có tên là `groupManagement` với class là `Proxy` như là một đối số, và chúng ta sẽ truyền cái đối số này vào contructor của class `Group`.

Ở đây chúng ta cũng có thể thay thế một đối số trong contructor của một class. Để các bạn có một cái nhìn tổng quan hơn thì chúng ta cùng xem ví dụ đơn giản như sau:
Giả sử tôi có một class có tên là ViMagento\HelloWorld\Block\Index và trong contructor của tôi đang có một biến $test = 1 như đoạn code bên dưới:
```php
protected $test;

    public function __construct($test= 1)
    {
        $this->test = $test;
    }
```
Biến test đang bằng 1 và sử dụng thẻ type trong di.xml để thay thế đối số này thành 2 chẳng hạn thì chúng ta làm như thế nào phải làm sao.

```php
 <type name="ViMagento\HelloWorld\Block\Index">
    <arguments>
        <argument name="test" xsi:type="string">2</argument>
    </arguments>
</type>
```
Chỉ cần khai báo một file di.xml,  sử dụng thẻ type và truyền vào class mà mình muốn thay đổi đối số. Sau đó sử dụng arguments khai báo một biến là test trùng với biến ở contructor mà mình muốn thay thế có giá trị là 2 và kiểu dữ liệu là string. Đó là cách mà thẻ type trong tập tin di.xml hoạt động.

## `<virtualType>`
virtualType tạo một class ảo cũng giống như tạo một lớp con cho một lớp hiện có. Hay nói cách khác virtualType có thể dùng để tạo một class con của một class khác mà không làm ảnh hưởng đến class đó.
```php
<virtualType name="Magento\Core\Model\Session\Storage" type="Magento\Framework\Session\Storage">
    <arguments>
        <argument name="namespace" xsi:type="string">core</argument>
    </arguments>
</virtualType>
```
Đoạn code trên đã sử dụng virtualType để tạo một class con Magento\Core\Model\Session\Storage của class cha là Magento\Framework\Session\Storage. Giống như class Magento\Core\Model\Session\Storage kế thừa từ class Magento\Framework\Session\Storage và nó tất cả những đặc tính của class cha vậy.
