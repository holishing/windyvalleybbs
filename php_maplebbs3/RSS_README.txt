########### Maple-XDBBS RSS Reader 說明 ###########
動作環境: PHP 5.1 以上
[Step 0.] 確定檔案在正確位置
rss.php 位於 (bbs家目錄)/src/util/ 
必須 chmod +x 

[Step 1.]
cd (bbs家目錄)
mkdir etc/rss
touch etc/rss/feed.list

[Step 2.] 準備 PHP 模組 maplebbs3.so
這個 PHP 模組目標是提供 PHP 存取 MapleBBS 3.x 資料之能力
目前只有新增文章的功能，供 RSS 功能使用
由於仍在初期開發階段，尚無 Makefile 供各位使用
敬請手動編譯安裝

首先，因為連結的過程需要 src/lib/*.o ，
如果上次編譯時已經清掉了，請在 src/ 下再 make 一次 (不要加clean)
cd src/php_maplebbs3
gcc -fpic -DCOMPILE_DL_FIRST_MODULE=1 -I/usr/local/include -I. -I/usr/local/include/php -I/usr/local/include/php/Zend -I/usr/local/include/php//main -I/usr/local/include/php/TSRM -I../include -c -o maplebbs3.o maplebbs3.c

gcc -shared -L/usr/local/lib -rdynamic -o maplebbs3.so maplebbs3.o ../lib/*.o

安裝則是複製到 PHP 的模組目錄，通常是在類似 /usr/local/lib/php/xxxxx/
可以試著用下面的 PHP ，觀察錯誤訊息，以知道其之目錄
<?php
dl('idoesnotexist');
?>

ps. 有可能在 /usr/lib/php/extensions/

開啟 PHP 的錯誤偵測的方法: /etc/php.ini 設定 display_errors = On 

記得看 which php 的結果來修改 rss.php 第一行
Ex. #!/usr/local/bin/php -q

[Step 3.] 設定 Crontab 
crontab -e
加入
# 每小時抓 RSS一次
15 * * * * src/util/run_rss > /dev/null 2>&1


[Step 4.] 站台個別設定
請手動編輯 rss.php ，前幾行的 define
