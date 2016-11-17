Stripe API Drupal module
------------------------
Maintained by: Bonify, LLC (http://bonify.io)

This module provides a simple abstraction of the [Stripe](https://stripe.com/) PHP SDK. It does not (and will not) provide any additional functionality. This module is designed to be required by other contrib/custom modules.

You must download the [Stripe PHP library from GitHub](https://github.com/stripe/stripe-php).

Download the library in `/libraries/stripe/` such that `init.php` is at `/libraries/stripe/init.php`.

You can download the library using Drush: 
`drush stripe-api-download --version=3.2.0`

The main way to make Stripe API calls is using the <code>stripe_api_call</code> function.
```
<?php
/**
 * Makes a call to the Stripe API.
 *
 * @param string $obj
 *   Stripe object. Can be a Charge, Refund, Customer, Subscription, Card, Plan,
 *   Coupon, Discount, Invoice, InvoiceItem, Dispute, Transfer, TransferReversal,
 *   Recipient, BankAccount, ApplicationFee, FeeRefund, Account, Balance, Event,
 *   Token, BitcoinReceiver, FileUpload.
 *
 * @param string $method
 *   Stripe object method. Common operations include retrieve, all, create,
 *
 * @param mixed $params
 *   Additional params to pass to the method. Can be an array, string.
 *
 * @return Stripe\ApiResource
 *   Returns the ApiResource or NULL on error.
 */
stripe_api_call($obj, $method = NULL, $params = NULL) { ... }

// Examples.

// Get a customer object.
$customer = stripe_api_call('customer', 'retrieve', 'cus_id123123123');
$customer->email = NEW_EMAIL;
$customer->save();

// Create a customer object.
$customer = stripe_api_call('customer', 'create', array(
  'email' => $user->mail,
));

// Delete a customer.
$customer = stripe_api_call('customer', 'retrieve', 'cus_id123123123');
$customer->delete();

// List customers.
$list = stripe_api_call('customer', 'all', array('limit' => 5));
?>
```

--------------------------------------------------------------------------------

This module provides a secure Stripe webhook (events are validated) and provides two hooks for you to implement in your custom module.

```
<?php
/**
 * Interact with all incoming Stripe webhooks.
 *
 * @param string $type
 *   Webhook type, such as customer.created, charge.captured, etc.
 *
 * @param object $data
 *   Incoming data object.
 *
 * @param Stripe\Event $event
 *   The verified Stripe Event that is being sent.
 *   Only available in live mode with real events (not test events).
 *
 * @throws \Exception
 */
function hook_stripe_api_webhook($type, $data, Stripe\Event $event = NULL) { ... }

/**
 * Interact with a specific incoming webhook type.
 *
 * @param object $data
 *   Incoming data object.
 *
 * @param Stripe\Event $event
 *   The verified Stripe Event that is being sent.
 *   Only available in live mode with real events (not test events).
 *
 * @throws \Exception
 */
function hook_stripe_api_webhook_EVENT_TYPE($data, Stripe\Event $event = NULL) { ... }
?>
```

You can use Drush to download the stripe library. [View valid Stripe PHP releases](https://github.com/stripe/stripe-php/releases) on the GitHub page.
```
drush stripe-api-download --version=3.2.0
Copied the Stripe PHP Library version 3.2.0 to sites/all/libraries/stripe.
```

You can use Drush to make Stripe API calls:
`drush stripe-api customer retrieve cus_id123123123`
 
`drush stripe-api account retrieve`
 
`drush stripe-api customer create --params="email=test@example.com&description=Testing"`
 
`drush stripe-api customer retrieve cus_id123123123 --delete`
 
`drush stripe-api customer retrieve cus_id123123123 --update="email=new@email.com"`

--------------------------------------------------------------------------------

This is an *unofficial* module and is no way associated with Stripe.
