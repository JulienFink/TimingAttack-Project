# TimingAttack-Project

Overview and POC? of how timing attacks work in a network environment as well as in the context of hardware security. 💻

* ## Introduction
A timing attack is a rather sophisticated way to circumvent the security mechanisms of an application. In a timing attack, the attacker gains information that is indirectly leaked by the application. This information is then used for malicious purposes, such as guessing the password of a user.

The only information needed by the attacker is the timing information that is revealed by the algorithms of the application. By supplying various inputs to the application, timing the processing and statistically analyzing this information, the attacker can guess valid inputs.

For hardware applications, timing attacks require high quality equipments since the timing differences to guess a key, a password, etc. will be in the nanosecond/microsecond range.
</br>For network applications, timing attacks can go up to milliseconds differences.

* ## Example of vulnerable code
The following java code is vulnerable to a timing attack :
```java
public static boolean check_password(String user_password)
  {
    String correct_password = "Qsp7Na1GracrUom";

    if(correct_password.equals(user_password))
    {
      return true;
    }

    return false;
  }
```

Despite its simplicity, the code snippet above is vulnerable.
</br>The reason is that the code that compares two string is equivalent to this one :
</br>`Source for java.lang.String`
```java
public boolean equals(Object anObject)
{
  if (! (anObject instanceof String))
    return false;
  String str2 = (String) anObject;
  if (count != str2.count)
    return false;
  if (value == str2.value && offset == str2.offset)
      return true;
  int i = count;
  int x = offset;
  int y = str2.offset;
  while (--i >= 0)
    if (value[x++] != str2.value[y++])
      return false;
  return true;
 }
```

The `java.lang.String.equals` function iterates over each character of the two strings and returns `false` as soon as two characters differ. This means that comparing two strings can take different amounts of time depending on the location of the first difference.

* ## Strengthen your code against timing attack vulnerabilities
To prevent having a timing attack vulnerability in your code, the solution is to use “constant time string comparison”.

To successfully protect against timing attack vulnerabilities, the algorithm must :

➤ Compare all of the characters of the string before returning true or false 
</br> ➤ Compare strings of equal length

</br>

Depending on your environment, there is likely a function you can use for making safe comparisons. Here are some constant time string comparison functions for differents languages : 
</br> ➤ Ruby : `secure_compare`
</br> ➤ Rust : `constant_time_eq`
</br> ➤ Java : `none ; code it yourself`
</br> ➤ Python (Django) : `constant_time_compare`

* ## Sources
https://ropesec.com/articles/timing-attacks/
</br>https://blog.sqreen.com/developer-security-best-practices-protecting-against-timing-attacks/
</br>https://developer.classpath.org/doc/java/lang/String-source.html
</br>https://apidock.com/rails/ActiveSupport/MessageVerifier/secure_compare
</br>https://lib.rs/crates/constant_time_eq
</br>https://github.com/django/django/blob/main/django/utils/crypto.py
