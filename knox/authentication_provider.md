### ShiroProvider (LDAP)

```
<provider>
	<role>authentication</role>
	<name>ShiroProvider</name>
	<enabled>true</enabled>
	<param>
		<name>sessionTimeout</name>
		<value>30</value>
	</param>
	<param>
		<name>main.ldapRealm</name>
		<value>org.apache.hadoop.gateway.shirorealm.KnoxLdapRealm</value>
	</param>
	<!-- changes for AD/user sync -->
	<param>
		<name>main.ldapContextFactory</name>
		<value>org.apache.hadoop.gateway.shirorealm.KnoxLdapContextFactory</value>
	</param>
	<param>
		<name>main.ldapRealm.contextFactory</name>
		<value>$ldapContextFactory</value>
	</param>
	<param>
		<name>main.ldapRealm.contextFactory.url</name>
		<value>ldap://ad.cloudera.com:389</value>
	</param>
	<param>
		<name>main.ldapRealm.contextFactory.systemUsername</name>
		<value>CN=ldapadmin,DC=HORTONWORKS,DC=COM</value>
	</param>
	<param>
		<name>main.ldapRealm.contextFactory.systemPassword</name>
		<value>hadoop123</value>
	</param>
	<param>
		<name>main.ldapRealm.contextFactory.authenticationMechanism</name>
		<value>simple</value>
	</param>
	<param>
		<name>main.ldapRealm.searchBase</name>
		<value>DC=HORTONWORKS,DC=COM</value>
	</param>
	<param>
		<name>main.ldapRealm.groupSearchBase</name>
		<value>DC=HORTONWORKS,DC=COM</value>
	</param>
	<param>
		<name>main.ldapRealm.userObjectClass</name>
		<value>posixAccount</value>
	</param>
	<param>
		<name>main.ldapRealm.userSearchAttributeName</name>
		<value>uid</value>
	</param>
	<param>
		<name>main.ldapRealm.groupObjectClass</name>
		<value>posixGroup</value>
	</param>
	<param>
		<name>main.ldapRealm.groupIdAttribute</name>
		<value>cn</value>
	</param>
	<param>
		<name>main.ldapRealm.memberAttribute</name>
		<value>memberUid</value>
	</param>
	<param>
		<name>main.ldapRealm.authorizationEnabled</name>
		<value>true</value>
	</param>
	<param>
		<name>urls./**</name>
		<value>authcBasic</value>
	</param>
</provider>
```

### HadoopAuth (Kerberos)
```
		<provider>
			<role>authentication</role>
			<name>HadoopAuth</name>
			<enabled>true</enabled>
			<param name="config.prefix" value="hadoop.auth.config" />
			<param name="hadoop.auth.config.type" value="kerberos" />
			<param name="hadoop.auth.config.signature.secret" value="E7UcS9D7" />
			<param name="hadoop.auth.config.token.validity" value="36000" />
			<param name="hadoop.auth.config.cookie.domain" value="coelab.cloudera.com" />
			<param name="hadoop.auth.config.cookie.path" value="gateway/kdefault/hive" />
			<param name="hadoop.auth.config.simple.anonymous.allowed" value="false" />
			<param name="hadoop.auth.config.kerberos.principal" value="HTTP/c3230-node2.coelab.cloudera.com@COELAB.CLOUDERA.COM" />
			<param name="hadoop.auth.config.kerberos.keytab" value="/etc/security/keytabs/spnego.service.keytab" />
			<param name="hadoop.auth.config.kerberos.name.rules" value="DEFAULT" />
		</provider>

```
