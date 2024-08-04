---
layout: post
title: C{A}TF V 2.0 - Cryptograhy - Reveal.me Writeup
date: 2024-02-04 
thumbnail: /assets/images/threejavafiles.png
categories: [Writeups, CTF]
---
![reveal-me.png](/assets/images/reveal-me.png)

the attached zip file contains three files:

![threejavafiles](/assets/images/threejavafiles.png)

At first glance we think it’s very complicated, three files!, written in java! and maybe you haven’t used java before and maybe you have no knowledge how Android application build, but easy on yourself it is not that big deal!

We just need to analyze the code and try to figure out each class job.

<aside>
💡 Remember! You can use your research skill to learn about Android application development and Java syntax.

</aside>

First I opened **LoginActivity.java** I tried to figure out any related thing, scrolled up and down, read classes until I found something that can help me 

```java
protected void fillData() throws UnsupportedEncodingException, InvalidKeyException, NoSuchAlgorithmException, NoSuchPaddingException, InvalidAlgorithmParameterException, IllegalBlockSizeException, BadPaddingException {
        SharedPreferences settings = getSharedPreferences("mySharedPreferences", 0);
        String username = settings.getString("EncryptedUsername", null);
        String password = settings.getString("superSecurePassword", null);
        if (username != null && password != null) {
            byte[] usernameBase64Byte = Base64.decode(username, 0);
            try {
                this.usernameBase64ByteString = new String(usernameBase64Byte, "UTF-8");
            } catch (UnsupportedEncodingException e) {
                e.printStackTrace();
            }
            this.Username_Text = (EditText) findViewById(R.id.loginscreen_username);
            this.Password_Text = (EditText) findViewById(R.id.loginscreen_password);
            this.Username_Text.setText(this.usernameBase64ByteString);
            CryptoClass crypt = new CryptoClass();
            **String decryptedPassword = crypt.aesDeccryptedString(password);**
            this.Password_Text.setText(decryptedPassword);
        } else if (username == null || password == null) {
            Toast.makeText(this, "No stored credentials found!!", 1).show();
        } else {
            Toast.makeText(this, "No stored credentials found!!", 1).show();
        }
    }
```

![LoginActivity.java.png](/assets/images/loginactivity.png)

I found the Encryption Type is **AES.**

I opened **CryptoClass.java,** I found a key and its type is string

![CryptoClass.java.png](/assets/images/cryptoclass.png)


So What I know now is the Encryption type, the key and I have an encrypted password.

Next, I opened an online AES Decryption tool such as [https://anycript.com/](https://anycript.com/) then filled the fields with the provided encrypted password.

![anycrypt.png](/assets/images/anycrypt.png)

And Whoa! it’s the flag.

<aside>
💡 This is the easy way we didn’t write any code or needed a full Java environment.

</aside>