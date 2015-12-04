##php密码哈希
php5.5版本以后，新出的API之一就是Password Hashing API。它包含4个函数：
password_get_info(),password_hash(),password_needs_rehash()和password_verify()。

###password_hash函数
password_hash用作创建一个新的密码的哈希值。

用法

```php
//$password  需要加密的密码
//$algo  哈希加密算法  
//	PASSWORD_DEFAULT  通过bcrypt算法加密
//	PASSWORD_BCRYPT   通过CRYPT_BLOWFISH算法加密
//$options  
//	$opeions = array['cost'=>num , 'salt'=>string];
//		cost:密码递归的层级 (详细可以查看crypt函数)
//		salt:散列密码加盐(干扰字符串)
password_hash(string $password, integer $algo, [array $options]);
```

###password_get_info
通过password_get_info查看新建哈希值的相关信息

```php
var_dump(password_get_info($hash));
/**
 * array(3){
 * ['algo']=>int(1),
 * ['algoName']=>string(6) "bcrypt"
 * ['options']=>array(2){['cost']=>int(10),['salt']=>string(4) 'hash'}
 * }
 */
```

###password_verify
验证密码

```php
boolean password_verify($password, $password_hash);
``` 

###password_needs_rehash
当加密算法或者参数需要调整时，可以通过password_needs_rehash验证hash是否需要重新生成。

比如数据库泄露，数据库中的密码需要进行新的加密。当用户输入登录密码后，就可以通过该函数，判断用户密码是否需要新的加密算法。

```php
boolean password_needs_rehash ( string $hash , integer $algo [, array $options ] )
```