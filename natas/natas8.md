# natas8 -> natas9

We're once again presented with an input field and a secret. This time, though, the source code contains an encoded secret:

```php
$encodedSecret = "3d3d516343746d4d6d6c315669563362";

function encodeSecret($secret) {
    return bin2hex(strrev(base64_encode($secret)));
}

if(array_key_exists("submit", $_POST)) {
    if(encodeSecret($_POST['secret']) == $encodedSecret) {
    print "Access granted. The password for natas9 is <censored>";
    } else {
    print "Wrong secret";
    }
}
```

So all we really need is to decode the secret. It's not encrypted, but rather just encoded, so we need to reverse the operations. Using the PHP interactive shell `php -a` we can feed it the secret and use:

```php
echo base64_decode(strrev(hex2bin($encodedSecret)));
```

This gets us the password `oubWYf2kBq`, and the flag.

## Problems

* [CWE-261: Weak Cryptography for Passwords](https://cwe.mitre.org/data/definitions/261.html)

## Remediation

Encoding is not encryption, and it is also not hashing. It was very trivial to reverse this weird scheme. Strongly hash passwords to make them harder to reverse.

## The flag

`W0mMhUcRRnG8dcghE4qvk3JA9lGt8nDl`