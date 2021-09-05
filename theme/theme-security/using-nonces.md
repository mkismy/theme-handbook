# Using Nonces

WordPress nonces are one-time use security tokens generated by WordPress to help protect URLs and forms from misuse.

If your theme allows users to submit data; be it in the Admin or the front-end; nonces can be used to **verify a user intends to perform an action**, and is instrumental in protecting against [Cross-Site Request Forgery(CSRF)](https://developer.wordpress.org/themes/theme-security/common-vulnerabilities/#cross-site-request-forgery-csrf).

An example is a WordPress site in which authorized users are allowed to upload videos. As an authorized user uploading videos is an intentional action and permitted. However, in a CSRF, a hacker can hijack (forge) the use of an authorized user and perform a fraudulent submission.

The one-time use hash generated by a nonce, prevents this type of forged attacks from being successful by validating the upload request is done by the current logged in user. Nonces are unique only to the current user’s session, so if an attempt is made to log in or out any nonces on the page become invalid.

## Creating a Nonce

*   [wp\_nonce\_url()](https://developer.wordpress.org/reference/functions/wp_nonce_url/) – Adding a nonce to an URL.
*   [wp\_nonce\_field()](https://developer.wordpress.org/reference/functions/wp_nonce_field/) – Adding a nonce to a form.
*   [wp\_create\_nonce()](https://developer.wordpress.org/reference/functions/wp_create_nonce/) – Using a nonce in a custom way; useful for processing AJAX requests.

## Verifying a Nonce

*   [check\_admin\_referer()](https://developer.wordpress.org/reference/functions/check_admin_referer/) – To verify a nonce that was passed in a URL or a form in an admin screen.
*   [check\_ajax\_referer()](https://developer.wordpress.org/reference/functions/check_ajax_referer/) – Checks the nonce (but not the referrer), and if the check fails then by default it terminates script execution.
*   [wp\_verify\_nonce()](https://developer.wordpress.org/reference/functions/wp_verify_nonce/) – To verify a nonce passed in some other context.

## Example

In this example, we have a basic submission form.

**Create the Nonce**

To secure your form with a nonce, create a hidden nonce field using [wp\_nonce\_field()](https://developer.wordpress.org/reference/functions/wp_nonce_field/) function:

<form method="post">
   <!-- some inputs here ... -->
   <?php wp\_nonce\_field( 'name\_of\_my\_action', 'name\_of\_nonce\_field' ); ?>
</form>

**Verify the Nonce**

In our example we first check if the nonce field is set since we do not want to run anything if the form has not been submitted. If the form has been submitted we use the nonce field value function. If nonce verification is successful the form will process.

Using wp\_verify\_nonce + wp\_nonce\_ays *(Are You Sure)*

if (
    ! isset( $\_POST\['name\_of\_nonce\_field'\] )
    || ! wp\_verify\_nonce( $\_POST\['name\_of\_nonce\_field'\], 'name\_of\_my\_action' )
) {
   wp\_nonce\_ays( '' );
} 
// process form data

or using check\_admin\_referer

 check\_admin\_referer( 'name\_of\_my\_action', 'name\_of\_nonce\_field' );
// process form data

In these examples the basic nonce process:

1.  Generates a nonce with the [wp\_nonce\_field()](https://developer.wordpress.org/reference/functions/wp_nonce_field/) function.
2.  The nonce is submitted with the form submission.
3.  The nonce is verified for validity using the [wp\_verify\_nonce()](https://developer.wordpress.org/reference/functions/wp_verify_nonce/) or [check\_admin\_referer()](https://developer.wordpress.org/reference/functions/check_admin_referer/) function. If not verified the request exits with a default error message (don’t precise the error message).