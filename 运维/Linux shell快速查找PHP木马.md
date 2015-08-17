###Linux shell快速查找PHP木马

一句话查找PHP木马

    find ./ -name "*.php" |xargs egrep "phpspy|c99sh|milw0rm|eval\(gunerpress|eval\(base64_decoolcode|spider_bc"> /tmp/php.txt
     
    grep -r --include=*.php  '[^a-z]eval($_POST' . > /tmp/eval.txt
     
    grep -r --include=*.php  'file_put_contents(.*$_POST\[.*\]);' . > /tmp/file_put_contents.txt
     
    find ./ -name "*.php" -type f -print0 | xargs -0 egrep "(phpspy|c99sh|milw0rm|eval\(gzuncompress\(base64_decoolcode|eval\(base64_decoolcode|spider_bc|gzinflate)" | awk -F: '{print $1}' | sort | uniq

查找最近一天被修改的PHP文件

    find -mtime -1 -type f -name \*.php
    
修改网站的权限  

    find -type f -name \*.php -exec chmod 444 {} \;  
    find ./ -type d -exec chmod 555{} \;