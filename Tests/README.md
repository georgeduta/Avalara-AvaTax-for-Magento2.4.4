# General

This document assumes this extension has been installed via Composer and resides in `vendor/classyllama/module-avatax`.

# Test Coverage

The integration tests cover two things:

1. Build multiple quotes and request AvaTax tax rates for the quotes. The rates are then applied to the quotes and the values on the quote and quote items are verified against the "expected_results" arrays in the `vendor/classyllama/module-avatax/Tests/Integration/_files/scenarios/*.php` files to ensure they contain the proper values.
    
2. Since Magento may change how tax rates are applied to quotes and quote items, there are also tests in place that compare quotes/quote items after running through AvaTax tax calculation vs native Magento tax calculation. Scenarios with "compare_with_native_tax_calculation" set to true are compared to native Magento tax calculation. The following properties are using for determining which fields to compare:
    * `\ClassyLlama\AvaTax\Tests\Integration\Model\Tax\Sales\Total\Quote\TaxTest::$quoteAddressFieldsEnsureMatch`
    * `\ClassyLlama\AvaTax\Tests\Integration\Model\Tax\Sales\Total\Quote\TaxTest::$quoteAddressFieldsEnsureDiff`
    * `\ClassyLlama\AvaTax\Tests\Integration\Model\Tax\Sales\Total\Quote\TaxTest::$quoteItemFieldsEnsureMatch`

The following scenarios are tested:

* Quote with a single applied rate
* Quote with multiple applied rates
* Simple products
* Bundled products with dynamic pricing
* Bundled products with simple pricing
* Configurable products
* Currency conversion, including rounding accuracy
* Customer tax classes (customer use types)
* Product tax classes (tax codes)
* Discounts
* Tax on shipping

# Running Integration Tests

Follow these steps to run the integration tests:

1. Create a database for the integration tests (such as "magento_integration_tests")

    1. Update the `dev/tests/integration/etc/install-config-mysql.php.dist` file with your MySQL credentials.

1. You'll need an AvaTax development account setup with the rules specified in `Tests/Integration/credentials.php.dist`

    1. Copy the `Tests/Integration/credentials.php.dist` file to `Tests/Integration/credentials.php` and update the "Company Code", "Account Number", and "License Key"

1. Run the integration tests using this command: `vendor/bin/phpunit --debug -c vendor/classyllama/module-avatax/Tests/Integration/phpunit.xml`