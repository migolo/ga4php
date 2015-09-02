#Using Other HOTP/TOTP Tokens

# Other Tokens #

Its possible (and quite easy) with this class to use other HOTP and TOTP capable tokens as its a quite well specified standard (including hardware based ones such as Vasco/Feitian/Safenet/etc). Most of these token providers provide their tokens with a secret key encoded in HEX. This is good cause internally I use HEX to calculate the key.

It all comes down to how you provision the user. When you call the setUser() method it has several variables:
```
function setUser($username, $ttype="HOTP", $key = "", $hexkey="")
```
the first is the username, the second is the token type (hotp/totp), the next two are the keys, $key is base32 format, $hexkey is hexformat. But, you should be able to convert from almost any provided key format back to HEX (ultimately a key is a variable length chunk of binary data in reality).

Its also possible to set token variables after the user has been provisioned with several methods:
```
// this allows you to change the key for the token (key in hex format)
function setUserKey($username, $key)

// this allows you to change the token type
function setTokenType($username, $tokentype)
```

I've only tested safenet (HOTP) hardware tokens myself, which have a much longer HEX key.

There are also other software tokens, Mobile OTP springs to mind which support mOTP, HOTP and TOTP and requires the user to enter the token details during token provisioning. This is where I need to do a bit more coding because its possible to have tokens that arent 6 digits long or dont use a 30second time base (though mobile otp allows you to set these things when you enter the token into your phone). At some point in the future i'll see how hard it is to support mOTP software tokens.

The plus to all this is that if you dont have an android/iphone/blackberry, you could still get your users to use this system as there are OTP tokens available for even the smaller J2ME phones (i.e. its possible to find a bit of token software for almost any phone) but you'll need to find a way of provisioning the user thats acceptable. My only real beef with some of the software tokens is entering that hex key into a phone which is not necessarily a user-friendly experience (i believe this is why GA uses a base32 key because its shorter for the same data AND harder to get wrong) where scanning a qrcode is very easy.

Lastly, my base32 routines are not bullet proof and designed specifically with the GA in mind. If your going to provide "other" tokens, dont try and use base32 format for entering them when calling setUser() as it'll probably just break. GA uses an 80-bit key and thats what the helperb322hex() and helperhex2b32() methods expect to see.