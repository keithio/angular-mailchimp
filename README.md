# Installation

Install with `bower`:

```shell
bower install angular-mailchimp
```

Add a `<script>` to your `index.html`:

```html
<script src="/bower_components/angular-mailchimp/angular-mailchimp.js"></script>
```

And add `mailchimp` as a dependency for your app:

```javascript
angular.module('myApp', ['mailchimp']);
```

# Justification of Method

Instead of relying on a resource or a service, I chose to make this module as reusable as possible. This means that all configuration is done in the [HTML shown below](https://github.com/keithio/angular-mailchimp#usage). So, you can have one page with as many subscription forms as you'd like as long as you [configure](https://github.com/keithio/angular-mailchimp#configuration) everything properly.

# Configuration

1. Log into your Mailchimp Account
2. Click on "Lists" from the sidebar
3. Click on the list for which you want
4. Click "Signup forms"
5. Select "Embedded forms"
6. Select "Naked"

You must obtain the following information from the form action attribute code:

* `mailchimp.username` - Your MailChimp username, immediately follows `http://`
* `mailchimp.dc` - Your MailChimp distribution center, immediately follows your username
* `mailchimp.u` - Unique string that identifies your account. It is obtained from 
* `mailchimp.id` - A unique string that identifies your list.

Example::

    <form action="http://username.us1.list-manage.com/subscribe/post?u=a1b2c3d4e5f6g7h8i9j0&amp;id=aabb12" method="post" id="mc-embedded-subscribe-form" name="mc-embedded-subscribe-form" class="validate" target="_blank" novalidate>

Result:

* `mailchimp.username` - `username`
* `mailchimp.dc` - `us1`
* `mailchimp.u` - `a1b2c3d4e5f6g7h8i9j0`
* `mailchimp.id` - `aabb12`

# Usage

The results from the last step will be populated in your form via `ng-init`.

So, add a `<form>` to your template as follows and replace the values with
those you obtained:

```html
<form name="MailchimpSubscriptionForm" ng-controller="MailchimpSubscriptionCtrl">
  <div ng-hide="mailchimp.result === 'success'">
    <input class="hidden" type="hidden" ng-model="mailchimp.username" ng-init="mailchimp.username='username'">
    <input class="hidden" type="hidden" ng-model="mailchimp.dc" ng-init="mailchimp.dc='us1'">
    <input class="hidden" type="hidden" ng-model="mailchimp.u" ng-init="mailchimp.u='a1b2c3d4e5f6g7h8i9j0'">
    <input class="hidden" type="hidden" ng-model="mailchimp.id" ng-init="mailchimp.id='aabb12'">
    <input type="text" name="fname" ng-model="mailchimp.FNAME" placeholder="First name">
    <input type="text" name="lname" ng-model="mailchimp.LNAME" placeholder="Last name">
    <input type="email" name="email" ng-model="mailchimp.EMAIL" placeholder="Email address" required>
    <button ng-disabled="MailchimpSubscriptionForm.$invalid" ng-click="addSubscription(mailchimp)">Join</button>
  </div>

  <!-- Show error message if MailChimp unsuccessfully added the email to the list. -->
  <div ng-show="mailchimp.result === 'error'" ng-cloak>
    <span ng-bind-html="mailchimp.errorMessage"></span>
  </div>

  <!-- Show success message if MailChimp returned successfully. -->
  <div ng-show="mailchimp.result === 'success'" ng-cloak>
    <span ng-bind-html="mailchimp.successMessage"></span>
  </div>
</form>
```

# Custom Fields
You also have the option to include extra fields in your subscription form.
For example, you can pass a phone number to MailChimp by providing a value for
``mailchimp.phone``.

# License

The MIT License

Copyright (c) 2014 Keith Hall

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.
