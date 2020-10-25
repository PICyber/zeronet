# zeronet
ZeroNet
Example configuration for /etc/nginx/sites-enabled/zeronet:

server {

    server_name yourhost.name;

    location / {
        proxy_pass http://127.0.0.1:43110;
        proxy_set_header Host $host;
        proxy_http_version 1.1;
        proxy_read_timeout 1h;  # for long live websocket connetion
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
    }

    listen 80;
}
[global]
data_dir = my-data-dir
log_dir = my-log-dir
ui_restrict =
 1.2.3.4
 2.3.4.5
 [global]
tor_controller = 127.0.0.1:9151
tor_proxy = 127.0.0.1:9150
{
...
    "zeronet": "1EU1tbG9oC1A8jz2ouVwGZyQ5asrNsE4Vr",
    "map": {
        "blog": { "zeronet": "1BLogC9LN4oPDcruNz3qo1ysa133E9AGg8" },
        "talk": { "zeronet": "1TaLk3zM7ZRskJvrh3ZNCDVGXvkJusPKQ" }
    },
...
}
 - name: Test
      run: |
        catchsegv python3 -m pytest src/Test --cov=src --cov-config src/Test/coverage.ini
        export ZERONET_LOG_DIR="log/CryptMessage"; catchsegv python3 -m pytest -x plugins/CryptMessage/Test
        export ZERONET_LOG_DIR="log/Bigfile"; catchsegv python3 -m pytest -x plugins/Bigfile/Test
        export ZERONET_LOG_DIR="log/AnnounceLocal"; catchsegv python3 -m pytest -x plugins/AnnounceLocal/Test
        export ZERONET_LOG_DIR="log/OptionalManager"; catchsegv python3 -m pytest -x plugins/OptionalManager/Test
        export ZERONET_LOG_DIR="log/Multiuser"; mv plugins/disabled-Multiuser plugins/Multiuser && catchsegv python -m pytest -x plugins/Multiuser/Test
        export ZERONET_LOG_DIR="log/Bootstrapper"; mv plugins/disabled-Bootstrapper plugins/Bootstrapper && catchsegv python -m pytest -x plugins/Bootstrapper/Test
        find src -name "*.json" | xargs -n 1 python3 -c "import json, sys; print(sys.argv[1], end=' '); json.load(open(sys.argv[1])); print('[OK]')"
        find plugins -name "*.json" | xargs -n 1 python3 -c "import json, sys; print(sys.argv[1], end=' '); json.load(open(sys.argv[1])); print('[OK]')"
        flake8 . --count --select=E9,F63,F72,F82 --show-source --statistics --exclude=src/lib/pyaes/
        
