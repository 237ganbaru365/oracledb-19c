- Dockerを使ったOracle Databaseのインストールを試みるが失敗 
-- docker-imageのファイルをローカルにクローン
-- Oracleオフィシャルから`Linux x86-64` のバイナリファイルをインストール
-- zipファイルを指定の場所に移動させた後、ビルドを試みたが失敗　`./buildContainerImage.sh -v 19.3.0 -t oracle_1930 -e`

```
==========================
Building image 'oracle_1930' ...
[+] Building 21.5s (8/14)                                                                                          
 => [internal] load build definition from Dockerfile                                                          0.0s
 => => transferring dockerfile: 37B                                                                           0.0s
 => [internal] load .dockerignore                                                                             0.0s
 => => transferring context: 2B                                                                               0.0s
 => [internal] load metadata for docker.io/library/oraclelinux:7-slim                                         0.8s
 => CACHED [base 1/4] FROM docker.io/library/oraclelinux:7-slim@sha256:8a1041ef1238165425de696fae910aa36a16b  0.0s
 => [internal] load build context                                                                             0.0s
 => => transferring context: 941B                                                                             0.0s
 => [base 2/4] COPY setupLinuxEnv.sh checkSpace.sh /opt/install/                                              0.0s
 => [base 3/4] COPY runOracle.sh startDB.sh createDB.sh createObserver.sh dbca.rsp.tmpl setPassword.sh check  0.0s
 => ERROR [base 4/4] RUN chmod ug+x /opt/install/*.sh &&     sync &&     /opt/install/checkSpace.sh &&       20.5s
------                                                                                                             
 > [base 4/4] RUN chmod ug+x /opt/install/*.sh &&     sync &&     /opt/install/checkSpace.sh &&     /opt/install/setupLinuxEnv.sh &&     rm -rf /opt/install:                                                                         
#8 0.411 Loaded plugins: ovl                                                                                       
#8 19.26 No package oracle-database-preinstall-19c available.                                                      
#8 19.44 Resolving Dependencies                                                                                    
#8 19.44 --> Running transaction check
#8 19.44 ---> Package openssl.aarch64 1:1.0.2k-26.el7_9 will be installed
#8 19.45 --> Processing Dependency: make for package: 1:openssl-1.0.2k-26.el7_9.aarch64
#8 19.54 --> Running transaction check
#8 19.54 ---> Package make.aarch64 1:3.82-24.el7 will be installed
#8 19.58 --> Finished Dependency Resolution
#8 19.58 
#8 19.58 Dependencies Resolved
#8 19.58 
#8 19.58 ================================================================================
#8 19.58  Package        Arch           Version                  Repository         Size
#8 19.58 ================================================================================
#8 19.58 Installing:
#8 19.58  openssl        aarch64        1:1.0.2k-26.el7_9        ol7_latest        487 k
#8 19.58 Installing for dependencies:
#8 19.58  make           aarch64        1:3.82-24.el7            ol7_latest        415 k
#8 19.58 
#8 19.58 Transaction Summary
#8 19.58 ================================================================================
#8 19.58 Install  1 Package (+1 Dependent package)
#8 19.58 
#8 19.58 Total download size: 902 k
#8 19.58 Installed size: 1.9 M
#8 19.58 Downloading packages:
#8 19.91 --------------------------------------------------------------------------------
#8 19.91 Total                                              2.6 MB/s | 902 kB  00:00     
#8 19.92 Running transaction check
#8 19.93 Running transaction test
#8 19.94 Transaction test succeeded
#8 19.94 Running transaction
#8 20.06   Installing : 1:make-3.82-24.el7.aarch64                                   1/2 
#8 20.33   Installing : 1:openssl-1.0.2k-26.el7_9.aarch64                            2/2 
#8 20.35   Verifying  : 1:make-3.82-24.el7.aarch64                                   1/2 
#8 20.36   Verifying  : 1:openssl-1.0.2k-26.el7_9.aarch64                            2/2 
#8 20.37 
#8 20.37 Installed:
#8 20.37   openssl.aarch64 1:1.0.2k-26.el7_9                                             
#8 20.37 
#8 20.37 Dependency Installed:
#8 20.37   make.aarch64 1:3.82-24.el7                                                    
#8 20.37 
#8 20.37 Complete!
#8 20.50 ln: target '/home/oracle/' is not a directory: No such file or directory
------
executor failed running [/bin/sh -c chmod ug+x $INSTALL_DIR/*.sh &&     sync &&     $INSTALL_DIR/$CHECK_SPACE_FILE &&     $INSTALL_DIR/$SETUP_LINUX_FILE &&     rm -rf $INSTALL_DIR]: exit code: 1

ERROR: Oracle Database container image was NOT successfully created.
ERROR: Check the output and correct any reported problems with the build operation.
```
(resorce1)[https://registry.hub.docker.com/r/doctorkirk/oracle-19c]
(resorce2)[https://kita-note.com/oracle-install]

- 少し調べてみると、M1 MacではDockerを使用したOracle Databaseは使用できなさそう？　 *Jun 5, 2021
(resorce1)[https://github.com/oracle/docker-images/issues/2449]

-- 追加で調査。2つの問題点がありそう。
1. Oracle databaseはARMプロセスには対応しておらず、Intelのみとのこと
  - ARM プロセスとは？ : ARMホールディングスの事業部門であるARM Ltd.により設計・ライセンスされているプロセッサコアのアーキテクチャである。
主に組み込み機器やスマートデバイス用プロセッサに用いられている。　(source)[https://bablishe.com/what-is-macbook-based-on-arm/]
2. Oracle database Docker image は　ホスト OS として Oracle Linux 7 または Red Hat Enterprise Linux 7 でのみサポート
(resorce)[https://stackoverflow.com/questions/69069927/oracle-docker-container-not-working-properly-on-mac-m1-bigsur]




