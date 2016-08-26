# nginx
## 設定雛形
```
# 動作ユーザー／グループの設定
# user www-data;

# ワーカープロセス数
worker_processes auto;
#worker_processes 1;

# pid
pid /run/nginx.pid;

# ワーカーの扱う最大ファイルディスクリプター数.
# OSレベルで扱えるディスクリプター総数である、/proc/sys/fs/file-maxを参考に設定する.
worker_rlimit_nofile 9000;

http {
  log_format  isucon '$time_local $msec\t$status\treqtime:$request_time\t'
                       'in:$request_length\tout:$bytes_sent\trequest:$request\t'
                       'acceptencoding:$http_accept_encoding\treferer:$http_referer\t'
                       'ua:$http_user_agent';
  access_log  /var/log/nginx/access.log isucon;
  # access_log  /var/log/nginx/access.log off;

  # カーネル空間内で静的ファイルの読み込みと送信ができるようになる.
  sendfile on;

  # 0以外の値が設定された場合、一つのsendfile() コールで転送することができるデータの量を制限します。
  # 制限が無い場合、一つの高速な接続がworkerプロセスを完全に掴むかもしれません。
  sendfile_max_chunk 0;

  # TCP_CORK TCP_NOPSUH を使用する. 
  tcp_nopush on;
  # TCP_NODELAY を使用する. tcp_nopush onの場合offでいいと思う.
  # tcp_nodelay on;

  # Connection: keep-alive がレスポンスヘッダに付加される. 
  # 数値はサーバ側でTCPコネクションを開きっぱなしにする時間(sec).
  keepalive_timeout 60;
  # 同一keep-alive connection でサービスするリクエストの最大値.
  keepalive_requests 100000;

  # コンテンツをgzip圧縮する. CPUと帯域と相談して on/offをきめる事.
  gzip on;
  # Vary: Accept-Encoding をレスポンスヘッダに付加する.
  gzip_vary on;
  # デフォルトだと nginxは text/html しかgzip圧縮しないので明示的に追加する.
  gzip_types text/css text/javascript;
  # Viaヘッダ(前段にキャッシュサーバやCDNが存在する場合に付く)が付いていてもgzip圧縮する
  gzip_proxied any;
	
	
  location ~* \.(css|js)$ {
    gzip_static always;
    gunzip on;
  }
  
  server {
  }
}
```

# mysql

# golang