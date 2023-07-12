# Custom email verification - DisplayControls

The set of policies that demonstrates how to use a Custom email verification solution is well documented here:
https://github.com/azure-ad-b2c/samples/tree/master/policies/custom-email-verifcation-displaycontrol. These allow you to send your own custom email verification during sign-up or during the password reset user journeys.

However, I did run into an issue when trying to disable the **Continue** button while the e-mail verification code had not been clicked on the **Send Verification Code** button as shown below.

[Send verification code.](images/email-verification.png)

To fix this, I used a simple JQuery solution:

```csharp
$("#continue").hide();
var observer = new MutationObserver(function (mutations) {
    mutations.forEach(function (mutationRecord) {
        $("#continue").show();
    });
});
var target = document.getElementById('email_success');
observer.observe(target, { attributes: true, attributeFilter: ['style'] });
```

What this is doing:

1. First, it's hiding the Continue button.
2. Then, it's using Javascript's MutationObserver to observe when the style attribute of div tag for the email_success label change. This will only change once a user has successfully verified their email with the emailed code.
3. Reveal the Continue button.

The email_success div tag is the ID of the div tag for a successfully verified code label. This changes based on the outclaim name in your b2c policy. So in the case of the Self Service Password Reset (SSPR) flow, the code would have been:

```csharp
$("#continue").hide();
var observer = new MutationObserver(function (mutations) {
    mutations.forEach(function (mutationRecord) {
        $("#continue").show();
    });
});
var target = document.getElementById('readOnlyEmail_success');
observer.observe(target, { attributes: true, attributeFilter: ['style'] });
```
