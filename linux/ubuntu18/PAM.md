Remediation:

```
$ sudo vi /etc/security/pwquality.conf
minlen = 14
minclass = 4
dcredit = -1
ucredit = -1
ocredit = -1
lcredit = -1

$ sudo vi /etc/login.defs
PASS_MAX_DAYS   30
PASS_MIN_DAYS   1
PASS_WARN_AGE   7

$ sudo suseradd -D -f 30
```

Verrify

```
grep '^\s*minlen\s*' /etc/security/pwquality.conf \
&& grep '^\s*minclass\s*' /etc/security/pwquality.conf \
&& grep -E '^\s*[duol]credit\s*' /etc/security/pwquality.conf \
&& grep -E '^\s*password\s+(requisite|required)\s+pam_pwquality\.so\s+(\S+\s+)*retry=[1-3]\s*(\s+\S+\s*)*(\s+#.*)?$' /etc/pam.d/common-password

grep PASS_MAX_DAYS /etc/login.defs \
&& grep PASS_MIN_DAYS /etc/login.defs \
&& grep PASS_WARN_AGE /etc/login.defs

useradd -D | grep INACTIVE \
&& grep "^root:" /etc/passwd | cut -f4 -d:
```
