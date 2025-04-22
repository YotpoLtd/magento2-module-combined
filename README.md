# README #

This library includes the files of the Yotpo extension. The directories hierarchy is as positioned in a standard magento 2 project library  
This library will also include different version packages as magento 2 extensions

## Requirements ##

Magento 2.3.3+ (Module version 4.0.0 and above)

Magento 2.4.8 (Module version 4.3.3 and above)

### Install via composer ###

Run the following command under your Magento 2 root dir:
```bash
composer require yotpo/module-yotpo-combined
php bin/magento maintenance:enable
php bin/magento setup:upgrade
php bin/magento setup:di:compile
php bin/magento setup:static-content:deploy
php bin/magento maintenance:disable
php bin/magento cache:flush
```

## Usage ##

After the installation, Go to The Magento 2 admin panel  
Go to Stores -> Settings -> Configuration, change store view (not to be default config) and click on Yotpo Configuration on the left sidebar  
Insert Your account app key and secret

## Advanced ##

To insert the widget manually on your product page add the following code in one of your product .phtml files
```php
<?= $this->helper('Yotpo\Reviews\Helper\Data')->showWidget($block) ?>
```

To insert the bottomline manually on your catalog page add the following code in Magento\Catalog\view\frontend\templates\product\list.phtml
```php
<?= $this->helper('Yotpo\Reviews\Helper\Data')->showBottomline($block, $_product) ?>
```
