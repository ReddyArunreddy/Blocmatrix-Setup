# Build and Run blocmatrixd

## 1. Build blocmatrixd

1. Update the list of packages that are available for apt-get to install or upgrade.
$sudo apt-get update

2. Upgrade currently installed packages.
sudo apt-get -y upgrade

3. Install dependencies.
sudo apt-get -y install git pkg-config protobuf-compiler libprotobuf-dev libssl-dev wget

4. Install CMake.
wget https://github.com/Kitware/CMake/releases/download/v3.13.3/cmake-3.13.3-Linux-x86_64.sh
sudo sh cmake-3.13.3-Linux-x86_64.sh --prefix=/usr/local --exclude-subdir

use cmake --version check.

5. compile Boost.
        1. Download Boost.
            wget https://dl.bintray.com/boostorg/release/1.70.0/source/boost_1_70_0.tar.gz
        
        2. Extract boost_1_70_0.tar.gz.
            tar xvzf boost_1_70_0.tar.gz
        
        3. Change to the new boost_1_70_0 directory.
            cd boost_1_70_0

        4. Prepare the Boost.Build system for use.
            ./bootstrap.sh

        5. Build the separately-compiled Boost libraries. This may take about 10 minutes, depending on your hardware specs.
            ./b2 -j 4

        6. Set the environment variable BOOST_ROOT to point to the new boost_1_70_0 directory.
        It's best to put this environment variable in your .profile, or equivalent, 
        file for your shell so it's automatically set when you log in. Add the following line to the file:
            export BOOST_ROOT=/home/my_user/boost_1_70_0 (boost installed path)

        7. Source your updated .profile file. For example:
            source ~/.profile

6. From a working directory, get the blocmatrixd source code. The master branch has the latest released version.
        git clone https://github.com/ReddyArunreddy/blocmatrixd.git
        cd blocmatrixd

7. Use CMake to build a blocmatrixd binary executable from source code.
    The result will be a blocmatrixd binary executable in the my_build directory.
        
        1. Generate the build system. Builds should be performed in a directory that is separate from the source tree root. In this example, we'll use a my_build directory that is a subdirectory of rippled.
            mkdir my_build
            cd my_build
            cmake ..
        2 . Build the blocmatrixd binary executable. This may take about 30 minutes, depending on your hardware specs.
            cmake --build .

8. (Optional) Run blocmatrixd unit tests. If there are no test failures,
    you can be fairly certain that your blocmatrixd executable compiled correctly.

    ./blocmatrixd -u


## 2. Configure blocmatrixd
Complete the following configurations that are required for blocmatrixd to start up successfully.
All other configuration is optional and can be tweaked after you have a working server.

1. Create a copy of the example config file (assumes you're in the blocmatrixd folder already).
Saving the config file to this location enables you to run blocmatrixd as a non-root user (recommended).
    mkdir -p ~/.config/blocmatrix
    cp cfg/blocmatrixd-example.cfg ~/.config/blocmatrix/blocmatrixd.cfg

2. Configuring for databases and log files.
    sudo mkdir /var/log/blocmatrixd
    sudo mkdir -p /var/lib/blocmatrixd/db
    sudo chown blocmatrix(your logged in username):blocmatrix(your logged in username) /var/log/blocmatrixd
    sudo chown -R blocmatrix(your logged in username):blocmatrix(your logged in username) /var/lib/blocmatrixd
    change the above "blocmatrix" to your logged in username.

3. Copy the example validators.txt file to the same folder as blocmatrixd.cfg:
    cp cfg/validators-example.txt ~/.config/blocmatrix/validators.txt


## 3. Running blocmatrixd.
1 . Enable Blocmatrixd on system startup
    sudo systemctl enable blocmatrixd.service
2. Start the blocmatrixd.
    sudo systemctl start blocmatrixd.service

3. Check the status of blocmatrixd.
    sudo systemctl status blocmatrixd.service
    you should be able to see something like below if it's working properly.
    [sudo] password for blocmatrix: 
    ● blocmatrixd.service - Blocmatrix Daemon
    Loaded: loaded (/etc/systemd/system/blocmatrixd.service; enabled; vendor preset: enabled)
    Active: active (running) since Fri 2019-12-20 11:57:33 IST; 38min ago
    Main PID: 18646 (blocmatrixd: #1)
    Tasks: 25 (limit: 4915)
    CGroup: /system.slice/blocmatrixd.service
           ├─18646 /usr/bin/blocmatrixd --net
           └─18647 /usr/bin/blocmatrixd --net

    Dec 20 11:57:34 blocmatrix-HP-Pavilion-Desktop-PC-570-p0xx blocmatrixd[18646]:         "severity" : "warning"
    Dec 20 11:57:34 blocmatrix-HP-Pavilion-Desktop-PC-570-p0xx blocmatrixd[18646]: }
    Dec 20 11:57:34 blocmatrix-HP-Pavilion-Desktop-PC-570-p0xx blocmatrixd[18646]: 2019-Dec-20 06:27:34.175447538 Application:FTL Result: {}
    Dec 20 11:57:36 blocmatrix-HP-Pavilion-Desktop-PC-570-p0xx blocmatrixd[18646]: 2019-Dec-20 06:27:36.175897055 LedgerConsensus:WRN View of consensus changed du
    Dec 20 11:57:36 blocmatrix-HP-Pavilion-Desktop-PC-570-p0xx blocmatrixd[18646]: 2019-Dec-20 06:27:36.175979675 LedgerConsensus:WRN 40595C6AE76FE084C3B2B38B612A
    Dec 20 11:57:36 blocmatrix-HP-Pavilion-Desktop-PC-570-p0xx blocmatrixd[18646]: 2019-Dec-20 06:27:36.176112721 LedgerConsensus:WRN {"accepted":true,"account_ha
    Dec 20 11:57:46 blocmatrix-HP-Pavilion-Desktop-PC-570-p0xx blocmatrixd[18646]: 2019-Dec-20 06:27:46.489394053 LoadMonitor:WRN Job: tryFill run: 4542ms wait: 0
    Dec 20 11:58:47 blocmatrix-HP-Pavilion-Desktop-PC-570-p0xx blocmatrixd[18646]: 2019-Dec-20 06:28:47.024264784 Peer:WRN [003] onReadMessage from n9LRcypGigsuCv
    Dec 20 12:00:28 blocmatrix-HP-Pavilion-Desktop-PC-570-p0xx blocmatrixd[18646]: 2019-Dec-20 06:30:28.553759438 Peer:WRN [002] onReadMessage from n94DccKV43dHtB
    Dec 20 12:07:53 blocmatrix-HP-Pavilion-Desktop-PC-570-p0xx blocmatrixd[18646]: 2019-Dec-20 06:37:53.097123172 Peer:WRN [001] onReadMessage from n94wMBuN4nx7Mg


