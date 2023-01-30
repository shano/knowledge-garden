
## Upgrading from 5.7 to 8.0

- Looks like this isn't as scary as it would seem
- Affects authentication, replication and administrative tasks more(e.g. user management)
- To roll out MySQL with [[Docker]] with 5.7 compatibility this is likely a good start, lets see if [[AWS]] supports this configuration in [[RDS]].

```
mysql:
	image: circleci/mysql:8.0
	command: >
		mysql --explicit_defaults_for_timestamp=1
		--log_bin_trust_function_creators=1
		--default-authentication-plugin=mysql_native_password
	ports:
	- 3306:3306
```

