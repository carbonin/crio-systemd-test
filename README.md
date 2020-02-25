# crio-systemd-test

1. `oc new-project test-httpd`
2. `oc apply -f buildconfigs.yaml`
3. `oc start-build manageiq-httpd; oc start-build test-httpd`
4. Wait for builds to finish
5. `oc adm policy add-scc-to-user anyuid system:serviceaccount:test-httpd:anyuid`
6. `oc apply -f <deploy-file>`
7. `oc exec <pod-name> -- systemctl status httpd`

Working step 6 output:
```
● httpd.service - The Apache HTTP Server
   Loaded: loaded (/usr/lib/systemd/system/httpd.service; enabled; vendor preset: disabled)
   Active: active (running) since Tue 2020-02-25 19:07:06 UTC; 33min ago
     Docs: man:httpd.service(8)
 Main PID: 26 (httpd)
   Status: "Running, listening on: port 80"
   CGroup: /kubepods.slice/kubepods-besteffort.slice/kubepods-besteffort-pod012a7478_5802_11ea_94d4_d094660d31fb.slice/crio-6caf43d729bee513a86918dd805ac4aae0db655ee5ac001f81033f774ce43ad1.scope/system.slice/httpd.service
           ├─26 /usr/sbin/httpd -DFOREGROUND
           ├─29 /usr/sbin/httpd -DFOREGROUND
           ├─30 /usr/sbin/httpd -DFOREGROUND
           ├─31 /usr/sbin/httpd -DFOREGROUND
           └─32 /usr/sbin/httpd -DFOREGROUND

Feb 25 19:07:06 test-httpd-75665c5445-thd55 systemd[1]: Starting The Apache HTTP Server...
Feb 25 19:07:06 test-httpd-75665c5445-thd55 httpd[26]: AH00558: httpd: Could not reliably determine the server's fully qualified domain name, using 10.129.1.49. Set the 'ServerName' directive globally to suppress this message
Feb 25 19:07:06 test-httpd-75665c5445-thd55 systemd[1]: Started The Apache HTTP Server.
Feb 25 19:07:06 test-httpd-75665c5445-thd55 httpd[26]: Server configured, listening on: port 80
```

Failing step 6 output:
```
System has not been booted with systemd as init system (PID 1). Can't operate.
Failed to connect to bus: Host is down
command terminated with exit code 1
```
