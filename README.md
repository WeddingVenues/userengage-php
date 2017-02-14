<p align="center"><a href="http://www.weddingvenues.com" title="WeddingVenues.com"><img src="https://static.weddingvenues.com/assets/img/brand-logo.png" title="WeddingVenues.com" /></a></p>

## Table of Contents

 1. [Installation](#installation)
 2. [Quick Start](#quickstart)
 3. [Usage](#usage)
 4. [Use Cases](#usecases)
 5. [Troubleshooting](#troubleshooting)
 6. [About](#about)

<a name="installation"></a>
## 1. Installation

#### Prerequisites:
- PHP version 5.4 or geater.
- Active subscription to the [UserEngage](https://userengage.io/) service.

Download the `UserEngage.php` file from the `lib/` folder in this repository, and then simply require it within your project:

	require 'path/to/UserEngage.php';
	
It should be noted that the SDK is namespaced with `UserEngage`. To learn more about namespacing with PHP, please refer to the [docs](http://php.net/manual/en/language.namespaces.php).

<a name="quickstart"></a>
## 2. Quick Start

With the main class file already included, a new instance of the `UserEngage` class should be initialised as follows:

	$ue = new UserEngage\UserEngage( 'YOUR_API_KEY' );
	
While it is not required to pass in your UserEngage API Key to the constructor, it is recommended if you wish to make API calls (it can always be set after instatiating though, using `setKey()`. To obtain your UserEngage API Key, follow [these instructions](https://userengage.io/en-gb/api/basic-usage/).

You're now all set to start making requests to the API. For the purposes of this documentation, we will assume you have instatiated a new UserEngage object to `$ue`.
	
<a name="usage"></a>
## 3. Usage

The following outlines the numerous methods exposed by the SDK. They should be called using `$ue->methodName()` (replacing `methodName` with the actual method name):

**`setKey( 'YOUR_API_KEY' )`**<br />
Set the API Key for the current object. Accepts a single `string` parameter containing the pubnlic API key associated with your UserEngage account.

----

**`setEndpoint( 'ENDPOINT' )`**<br />
Specifies the API endpoint for the current call. Accepts a single `string` parameter containing the API endpoint name. Exposed endpoints are available [here](https://userengage.io/en-gb/api/introduction/)

----

**`setMethod( 'POST' )`**<br />
Specifies the HTTP method to be used for the API call. It is necessary, as the object will be instantiated with a default method of `POST`.

----

**`addField( $name, $value )`**<br />
Adds a key-value field to the data that will be passed along with the API call. Useful when specifying data that should be sent.

----

**`debug( $header = true )`**<br />
This is a useful method for debugging the currently defined POST / GET fields that have been added to the request. The method will print out a JSON-encoded string of fields. The only optional parameter is a `boolean` which indicates whether PHP should send the `Content-Type` header to the browser when calling `debug()`; default is **`true`**. <br />
**Note:** The `Content-Type` header will not be sent to the browser if headers have already been sent (to avoid PHP notices), regardless of the value of `$header`.

----

**`send( $decode = true )`**<br />
Actually sends off the request to the UserEngage API. The only parameter (`$decode`) specifies whether the response from UserEngage should be parsed into a PHP object (rather than a JSON-encoded string); default is **`true`**.

---

**The SDK will throw exceptions when something is wrong, so it's recommended to wrap all requests in a `try { } catch() { }` block. 

<a name="usecases"></a>
## 4. Use Cases

The following code snippets show some basic use cases of the SDK in action.

**i. Triggering an event**<br />
We will trigger an event (which can be used inside an Action) called `card_expired`, and pass through some basic data with the event (that we can then use to filter the request, or send in an [email message](https://userengage.io/en/docs/event-data-in-emails):
	
	<?php 
	
	try
	{
		$ue = new UserEngage( 'YOUR_API_KEY' );
		$ue->setEndpoint( 'events' );
		$ue->addField( 'name', 'card_expired' );
	    
		// Add some event data about the expired card:
		$data = new stdClass();
		$data->last4Digits = '1234';
		$data->expiryDate  = '12/18';
	    
		// Now create the event
		// Pass through the timestamp of the event, and the client it should be raised against.
		$ue->addField( 'timestamp', time() );
		$ue->addField( 'client', 'ID_OF_CLIENT' );
		
		// Finally assign our data:
		$ue->addField( 'data', $data );
	    
		// Send the request, and print_r the response:
		print_r( $ue->send() );
	}
	
	// Catch any errors:
	catch( Exception $e )
	{
		echo $e->getMessage();
	}
	
----

**ii. Create a new User:**<br />
This example will create a new User in your UserEngage account using the [`users/`](https://userengage.io/en-gb/api/user/create/) endpoint:

	<?php
	
	try
	{
		$ue = new UserEngage( 'YOUR_API_KEY' );
		$ue->setEndpoint( 'users' );
		
		// Add the required fields:
		$ue->addField( 'email': 'someone@domain.com' );
		$ue->addField( 'first_name': 'John' );
		$ue->addField( 'last_name': 'Doe' );
		
		print_r( $ue->send() );
	}
	catch( Exception $e )
	{
		echo $e->getMessage();
	}

<a name="troubleshooting"></a>
## 5. Troubleshooting

For in-depth troubleshooting and information, please refer to UserEngage's excellent [Knowledge Base](https://userengage.io/en/knowledge-base/).

<a name="about"></a>
## 6. About

userengage-php is an independent software development kit (SDK) developed by @BenMajor (part of the [WeddingVenues.com](http://www.weddingvenues.com/) development team), initially for use in-house. 

Please note that unless otherwise stated, the SDK is licensed under the MIT License, and remains copyrighted to WeddingVenues.com. Any logos or references to UserEngage remain their copyright.

---

<p align="center"><a href="http://www.weddingvenues.com" title="WeddingVenues.com"><img src="https://static.weddingvenues.com/assets/img/brand-logo.png" title="WeddingVenues.com" /></a><br /><a href="https://userengage.io/" title="UserEngage"><img src="https://userengage.io/static/img/logo/logo-blue.png" title="UserEngage" /></a></p>
