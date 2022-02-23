# General

## General

### 6. Assignment

```
-----BEGIN PRIVATE KEY-----
MIIEuwIBADANBgkqhkiG9w0BAQEFAASCBKUwggShAgEAAoIBAQCIvP5eMLAz1gp0hvZwF8nikYveT26jcF+x8AhekOV6Ac/n2Cfe3gNJffOo9Y/wTmOuhXvKH6gIR15Jq/dj7cX+DGcuuwxMbhXAbtaiG07vfItiwMhYdO65ojXJWfKjsqYGGk3K0VXLseotg9mPmLePUEblt5Fyx9mWeW+Gbtt0TY/Q+SPXginmCgLr5mPg6p5RJ8kbH3bdMjRZSDgN1AtSuGy8codVvajcjEfxLPLqKqnqRAkInRlqy1QFeTNF5wiW57ROyAzGrDbLu30ZR5Oce9Le4sHhqvtj2h5nn0lj825mUi2aUki2qDM1QJsXxQqKt7s03xjkN1LVBQhKIyvdAgERAoIBAADN6Wb0PUWTVGQS3U73Gsmdd8mXDjQpUeFsVGDuyCBXWhHtse28CQ38OSakFGxUyl/lreeWxkJOucc1t6rApU5LXfi+12ncgaJC6SseRz2k37DE5hKXRqzDNoc1PCgdqaKr00f4MjLkb06S2J72W27FviiBFG6axozupMwVvAUzpcwAmtuF/582U8azVMmjogw110184zgyWGxOjXlFu6+EqGQzgYH5Tu32Oc0aI/YM1mjzfD7E96KZ0tIaNKq6k+l1fH7kIXLnAB7FKvIGkiGw4yUDAWJNQ8/vsEcD5xd4xnWGz7fq8nxgci3D3dNUxXYNTEsDMgNvgUxmr+JXmdECgYEAwIiTOecpMyLWUTK44diKgJ2xPaNcYx+yp2A1WLp1+g1gyEZ9MRBH6BEjLoBk8iqGOrUSrUdhR38n8kBsjDRULx0vazFEza/Gp/vbr1MnImB/TL9ZPM0LE0wIbvW+OSZ5wRJC7hlO0qzLCoXABKCbaFcLcn/VEYvKEGRaMTSe9A0CgYEAtc//FCldNwkgD8YXNCG2gZe1kgLUMGwQ1Qzo7MsPbrc/0evBVfXnVsXmtz1sH1seDYOA2hJvX/F9Yj+lrbGsoHGznNoFCMl/emDxSQ3DZtyFr4uKmc1Q4AKmoElJmqdF40iJT17+b3HRVktvSCDegc7XnuTq0RBhZbR6CsdYExECgYB8lJt/0c9dUsbpPvAZqjuAZglGAErWuihOLzGTw/H8JsYnPKtb+3nSZXEeFtfn/WXpwHV/Li/i9+yrz1VLqWOmA9NjejuUJnF7wRWtrkZ/p9nmXbI2Zo6yIiOTF3sV67gxomeLAVEe6Eck7SHk4GSOzulKFnrPHjd0BLLUi3XpNQKBgBVjw6gE3co9TxDqINj06Et7Qoml+tiFLygfou6ueklCvDbQcRkr/RlEdX74KtaDMLZLtM5chZLRHc9SyDKNX5pnuscotUxT8OE4lNtrB+37034Qaqiuvjh4yE8Xrk5uCDjbW3K/4bLCGKC9lJ8S7QA2c7hXKq8vGoRvleMmgtURAoGBAJR8zwjIezaXSt6I7yJ+T3YvOw/xZjCuirIzaB4s3EhQktEk+ZGgivWmDtj2BUvDouNAetc8YD0/ri6FU0lqiCKoEaW5Ybm8UbLiLMQbTdb8vLN/IPAbx8R71FiFQcwezrmKyDPSzambEBI1IzlYCJH5/gslSBOWL48Sds3gPqqm
-----END PRIVATE KEY-----
```

Mình có một đoạn private key như vậy, lưu vào `priv.pem` và chạy `openssl` để lấy public key:

```console
$ openssl rsa -in priv.pem -pubout > pub.pem
```

Từ public key `pub.pem` mình in ra modulus:

```console
$ openssl rsa -in pub.pem -pubin -modulus -noout
Modulus=88BCFE5E30B033D60A7486F67017C9E2918BDE4F6EA3705FB1F0085E90E57A01CFE7D827DEDE03497DF3A8F58FF04E63AE857BCA1FA808475E49ABF763EDC5FE0C672EBB0C4C6E15C06ED6A21B4EEF7C8B62C0C85874EEB9A235C959F2A3B2A6061A4DCAD155CBB1EA2D83D98F98B78F5046E5B79172C7D996796F866EDB744D8FD0F923D78229E60A02EBE663E0EA9E5127C91B1F76DD32345948380DD40B52B86CBC728755BDA8DC8C47F12CF2EA2AA9EA4409089D196ACB5405793345E70896E7B44EC80CC6AC36CBBB7D1947939C7BD2DEE2C1E1AAFB63DA1E679F4963F36E66522D9A5248B6A83335409B17C50A8AB7BB34DF18E43752D505084A232BDD
```

Vậy là mình đã có được modulus. Tiếp theo là lấy đoạn modulus trên để sign `priv.pem`:

```console
$ echo -n "88BCFE5E30B033D60A7486F67017C9E2918BDE4F6EA3705FB1F0085E90E57A01CFE7D827DEDE03497DF3A8F58FF04E63AE857BCA1FA808475E49ABF763EDC5FE0C672EBB0C4C6E15C06ED6A21B4EEF7C8B62C0C85874EEB9A235C959F2A3B2A6061A4DCAD155CBB1EA2D83D98F98B78F5046E5B79172C7D996796F866EDB744D8FD0F923D78229E60A02EBE663E0EA9E5127C91B1F76DD32345948380DD40B52B86CBC728755BDA8DC8C47F12CF2EA2AA9EA4409089D196ACB5405793345E70896E7B44EC80CC6AC36CBBB7D1947939C7BD2DEE2C1E1AAFB63DA1E679F4963F36E66522D9A5248B6A83335409B17C50A8AB7BB34DF18E43752D505084A232BDD" | openssl dgst -sign priv.pem -sha256 | base64 -w0
FdhlRCRK6vGxli9N0mTXVx7TRCTNHb2E3YbT2VvaFOMrBOATFNjVU5d9nOhx7zVg3pYnrNCb2RTKbNNLs2tnFyZ65w6VrSTe/hbWXYCwoH8kkCPU3CXS3pa1a5SENK0M5TyeBz0mHuf1rmiiirPZLgmj33PRcOivstjbRRoJtYGo44IscL7afS4wZriEtUfn7J66Vgg6NsjeHZh6t8IcmXIPQ0+kQcw7rYuC172ldbl+nxehA7OvqkR/aSsbjtVlMMDRvvhZGRwSCkil3pKaduptgNTbQev1UzScVZY9FjOZvP84zd8t4wd9kVrco6zPQZV3zNE5Ptys6gx4lGEMlw==
```

Submit modulus và đoạn base64 trên là xong.

### 8. Assignment

Đầu tiên là chạy docker:

```console
$ docker run -d webgoat/assignments:findthesecret
Unable to find image 'webgoat/assignments:findthesecret' locally
findthesecret: Pulling from webgoat/assignments
5e6ec7f28fb7: Pull complete 
1cf4e4a3f534: Pull complete 
5d9d21aca480: Pull complete 
0a126fb8ec28: Pull complete 
1904df324545: Pull complete 
e6d9d96381c8: Pull complete 
d6419a981ec6: Pull complete 
4cf180de4a1f: Pull complete 
ff2e10214d79: Pull complete 
Digest: sha256:3fba41f35dbfac1daf7465ce0869c076d3cdef017e710dbec6d273cc9334d4a6
Status: Downloaded newer image for webgoat/assignments:findthesecret
512f169ac22d7a75cf40a0e55477b032a4474930771c5452b608cc179b1b244b
```

Tiếp theo mình mở 1 cái shell trên docker có quyền root và tìm file password:

```console
$ docker exec -it -u 0 512f169ac22d7a75cf40a0e55477b032a4474930771c5452b608cc179b1b244b bash
root@6b11f198002a:/# cd root
root@6b11f198002a:~# ls
default_secret
root@6b11f198002a:~# cat default_secret 
ThisIsMySecretPassw0rdF0rY0u
```

Vậy là mình đã tìm thấy password. Tiếp theo dùng password này để decrypt đoạn message ban đầu:

```console
$ echo ThisIsMySecretPassw0rdF0rY0u > key.txt
$ echo "U2FsdGVkX199jgh5oANElFdtCxIEvdEvciLi+v+5loE+VCuy6Ii0b+5byb5DXp32RPmT02Ek1pf55ctQN+DHbwCPiVRfFQamDmbHBUpD7as=" | openssl enc -aes-256-cbc -d -a -kfile key.txt
Leaving passwords in docker images is not so secure
```

Submit lên là xong.

Unencrypted message:
```
Leaving passwords in docker images is not so secure
```

Name of the file that stored the password:
```
default_secret
```