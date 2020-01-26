*Mostly Used  Kerberos Commands*

```
# kinit
# kinit -kt <keytab> <princiapl>
# kinit -S [ServiceName] <principal>
# kinit -r7d
#_keytab=/etc/security/keytabs/hdfs.headless.keytab; kinit -kt $_keytab $(klist -kt $_keytab |sed -n "4p"|cut -d ' ' -f7)
```
```
# klist
# klist -ef
# klist -kte <keytab>
```
```
# kdestroy
```
```
# kvno <principal>
```
```
# listprics
# addprincs
# getprinc <principal>
# delprinc
# cpw
# getpol
```
```
# kadmin -p 'admin/admin' -q 'listprincs'
```

```
# modprinc <flag> <principal> # https://web.mit.edu/kerberos/krb5-1.5/krb5-1.5.4/doc/krb5-admin/Adding-or-Modifying-Principals.html
# modprinc -maxrenewlife 90day krbtgt/YOUR_REALM.COM
# modprinc -maxrenewlife 90day +allow_renewable hue/<hostname>@REALM
# modprinc -maxrenewlife 90day +requires_preauth zk/fqdn@REALM
```

**Create Service Principal & keytab:**
```
kadmin: addprinc -randkey sname/fqdn
kadmin: xst -norandkey -k file.keytab sname/fqdn
```
**Create keytabs:**
MIT : Use "kadmin" OR an alternative ["kadmin.local"](http://kadmin.local/) interface to create keytabs in MIT. (use [ktadd](http://web.mit.edu/kerberos/krb5-1.5/krb5-1.5.3/doc/krb5-admin/Adding-Principals-to-Keytabs.html) or xst)
```
kadmin: ktadd -k <keytab_path> <principal>
kadmin: xst -k nn.service.keytab nn/$namenode-host 
kadmin: xst -k spnego.service.keytab HTTP/$namenode-host
```

[FreeIPA](https://linux.die.net/man/1/ipa-getkeytab)
```
# ipa-getkeytab -s <ipaserver.example.com> -p <principal> -k <keytab_path>
```

**Windows AD Powershell:**
```
# ktpass /princ hive/sandbox.hortonworks.com@REALM /pass <password> /mapuser hiveservice /pType KRB5_NT_PRINCIPAL /crypto ALL /out c:\temp\hive.service.keytab
```
**From LINUX using ktutil:**
```
ktutil 
ktutil: add_entry -password -p HTTP/loadbalancerhost@REALM -k 1 -e aes128-cts-hmac-sha1-96
ktutil: add_entry -password -p HTTP/loadbalancerhost@REALM -k 1 -e aes256-cts-hmac-sha1-96
ktutil: add_entry -password -p HTTP/loadbalancerhost@REALM -k 1 -e arcfour-hmac-md5
ktutil: add_entry -password -p HTTP/loadbalancerhost@REALM -k 1 -e des3-cbc-sha1
ktutil: add_entry -password -p HTTP/loadbalancerhost@REALM -k 1 -e des-cbc-md5
ktutil: write_kt spenego.service.keytab
ktutil: exit
```


**Remove/Delete principal from keytab :** [Ref](https://docs.oracle.com/cd/E19683-01/806-4078/6jd6cjs1p/index.html)
```
kadmin: ktremove [-k keytab] [-q] principal [kvno | all | old ]
Delete Entry from keytab
ktutil: delent <slot>
Merge Keytabs
ktutil: read_kt <keytab1>
ktutil: read_kt <keytab2>
ktutil: list
ktutil: write_kt <keytab_new>
```



**Verify auth_to_local RULE's**
```
# hadoop org.apache.hadoop.security.HadoopKerberosName hdfs@REALM

```
**Check JCE files**
```
# source /etc/hadoop/conf/hadoop-env.sh && zipgrep CryptoAllPermission $JAVA_HOME/jre/lib/security/US_export_policy.jar
# source /etc/hadoop/conf/hadoop-env.sh && zipgrep CryptoAllPermission $JAVA_HOME/jre/lib/security/local_policy.jar 
default_local.policy:    permission javax.crypto.CryptoAllPermission; 
```

*If 'zipgrep' is unavailable, please use the following two commands to check the same:*
```
$ jar xf $JAVA_HOME/jre/lib/security/local_policy.jar default_local.policy 
grep CryptoAllPermission default_local.policy
```

**Collect Info like krb5, java/os version, env Variables, keytab/principal details**
```
# hadoop org.apache.hadoop.security.Kdiag
```
```
# _keytab=/etc/security/keytabs/nn.service.keytab; _principal=nn/`hostname -f`; hadoop org.apache.hadoop.security.KDiag --keytab $_keytab --principal $_principal
```

**Backup KDC database**
```
kdb5_util dump -verbose /var/tmp/kdc.dump
```
**Restore KDC database from backup**
```
kdb5_util load /var/tmp/kdc.dump
```
**Create new database**
```
kdb5_util create -s
```
**Delete database**
```
kdb5_util destroy
```
