1. Check /tmp mount
```
mount | grep /tmp
```

## 2. Check available space
```
df -h /tmp
```

## 3. Check /etc/fstab for /tmp entry
```
grep /tmp /etc/fstab
```

## 4. Mount /tmp with security options
```
# Remount existing /tmp with noexec, nodev, nosuid
sudo mount -o remount,noexec,nodev,nosuid /tmp

# Mount /tmp if not mounted
sudo mount -t tmpfs -o defaults,noexec,nodev,nosuid tmpfs /tmp
```

## 5. Make /tmp mount permanent
```
echo "tmpfs /tmp tmpfs defaults,noexec,nodev,nosuid 0 0" | sudo tee -a /etc/fstab
sudo mount -o remount /tmp
```

## 6. Check permissions and ownership
```
ls -ld /tmp
```

## 7. Check SELinux context
```
ls -ldZ /tmp
```
## 8. Test write access as web server user
```
sudo -u nginx touch /tmp/test_upload.txt
sudo -u nginx rm /tmp/test_upload.txt
```

## 9. Clean temporary directories
```
sudo systemd-tmpfiles --clean
```

## 10. Check tmpfiles rules
```
cat /usr/lib/tmpfiles.d/tmp.conf
```

## 11. Check open files in /tmp
```
lsof +D /tmp
```

#### 12. Restart web services
```
sudo systemctl restart nginx
sudo systemctl restart php-fpm
```
