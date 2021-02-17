# Install

- Linux
    - Docker
        - Pull image

            ```cmd
            docker pull redis
            ```

        - Run container

            ```cmd
            docker run --name redis-test -p 6379:6379 -d redis
            ```

        - Enter Redis container

            ```cmd
            docker exec -it container-id bash
            ```

        - Redis cli

            ```cmd
            redis-cli
            ```

        - Test

            ```cmd
            ping
            
            // PONG
            ```

    - apt-get
        - Get Redis server

            ```cmd
            apt-get update
            apt-get install redis-server
            ```

        - Start Redis server

            ```cmd
            redis-server

            // redis-server --daemonize yes
            ```

        - Redus cli

            ```cmd
            redis-cli
            ```

        - Test

            ```cmd
            ping
            
            // PONG
            ```

    - wget
        - Get tar file

            ```cmd
            wget http://download.redis.io/releases/redis-3.2.6.tar.gz
            ```

        - Unzip tar file

            ```cmd
            tar -zxvf redis-3.2.6.tar.gz
            ```

        - Install gcc

            ```cmd
            apt-get install gcc
            ```

        - Make Redis

            ```cmd
            // Enter Redis folder

            make
            make install
            ```

        - Start Redis server

            ```cmd
            redis-server

            // redis-server --daemonize yes
            ```

- Windows
    - WSL ( Windows Subsystem for Linux )
        - Enable WSL

            ```cmd
            dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
            ```

        - Install WSL from Microsoft store

        - Install Redis in WSL

    - Docker for Windows
        - Using Hyper-v or WSL2
        - https://docs.docker.com/docker-for-windows/install/

    - Install Redis on Windows
        - https://github.com/MicrosoftArchive/redis

- Redis-cli 中文編碼
    - 使用 redis-cli --raw 啟動，強制輸出原始資料