# SELinux

SELinux Commands and Documentation

1. Overview

SELinux (Security-Enhanced Linux) is a kernel security module that provides access control security policies, including mandatory access controls (MAC). It helps prevent unauthorized access and limits potential damage from compromised services.

2. SELinux Status Commands

Check current SELinux status

sestatus

Shows whether SELinux is enabled, current mode, and loaded policy.

Check SELinux mode only

getenforce

Outputs: Enforcing, Permissive, or Disabled.

Set SELinux mode temporarily

setenforce 1   # Enforcing
setenforce 0   # Permissive

Changes mode until next reboot.

Set SELinux mode permanently

Edit /etc/selinux/config:

SELINUX=enforcing
# or
SELINUX=permissive
# or
SELINUX=disabled

3. File Contexts

View current file context

ls -Z /path/to/file

Shows SELinux security context of files/directories.

Change context temporarily

chcon -t type_t /path/to/file
chcon -R -t httpd_sys_rw_content_t /var/www/html/storage

-R = recursive

Types: httpd_sys_content_t, httpd_sys_rw_content_t

Add/modify persistent context

semanage fcontext -a -t httpd_sys_rw_content_t "/var/www/html/storage(/.*)?"

Use -m to modify existing context.

Apply context changes

restorecon -Rv /var/www/html/storage

-R = recursive, -v = verbose

4. Audit Logs and AVC Messages

Search SELinux denials

ausearch -m avc -ts today
ausearch -m avc -ts today | grep httpd_t
ausearch -m avc -c sh -c php -c nginx -ts today

-m avc = audit messages of SELinux denials

-ts today = start searching from today

Interpret AVC messages

scontext = source context (process)

tcontext = target context (file/dir)

tclass = object class (file, dir, etc.)

permissive=0 = denial enforced

5. Boolean Settings

List SELinux Booleans

getsebool -a

Enable/disable booleans

setsebool httpd_can_network_connect on
setsebool -P httpd_can_network_connect on

-P = persistent across reboots

Common httpd-related booleans

httpd_can_network_connect → allow Apache/Nginx to make network connections

httpd_can_network_connect_db → allow web server to connect to databases

httpd_enable_homedirs → allow web access to user home directories

6. Troubleshooting Tips

If AVC denies write to web directory:

chcon -R -t httpd_sys_rw_content_t /var/www/html/storage
restorecon -Rv /var/www/html/storage

Check running processes context

ps -eZ | grep httpd

Check real-time AVC denials

tail -f /var/log/audit/audit.log | grep AVC

7. References

SELinux Project Wiki

Red Hat SELinux Guide

man selinux, man chcon, man restorecon, man semanage

