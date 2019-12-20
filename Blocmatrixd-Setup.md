# Build and Run blocmatrixd

## 1. Build blocmatrixd

1. Update the list of packages that are available for apt-get to install or upgrade.

sudo apt-get update

2. Upgrade currently installed packages.

sudo apt-get -y upgrade

3. Install dependencies.

sudo apt-get -y install git pkg-config protobuf-compiler libprotobuf-dev libssl-dev wget

4. Install CMake.

    1. wget https://github.com/Kitware/CMake/releases/download/v3.13.3/cmake-3.13.3-Linux-x86_64.sh


    2. sudo sh cmake-3.13.3-Linux-x86_64.sh --prefix=/usr/local --exclude-subdir

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

6. From a working directory, get the blocmatrixd source code.
        
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
        
        3. Copy the blocmatrixd executable to bin directory.

            sudo cp blocmatrixd /usr/bin

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

4. sudo cp /your_installed_path/blocmatrixd/Builds/containers/shared/blocmatrixd.service /etc/systemd/system/blocmatrixd.service
change the user and group to your logged in username in the blocmatrixd.service file.


## 3. Running blocmatrixd.

1 . Enable Blocmatrixd on system startup
    sudo systemctl enable blocmatrixd.service

2. Start the blocmatrixd.
    sudo systemctl start blocmatrixd.service

3. Check the status of blocmatrixd.
    sudo systemctl status blocmatrixd.service

    you should be able to see something like below if it's working properly.

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


# Configure blocmatrixd as a Validator.

## 1. Manually build and run validator- keys tool.

1. Clone the validator-keys-tool Repository.

         git clone https://github.com/ReddyArunreddy/validator-keys-tool.git

2. cd validator-keys-tool.

3. mkdir -p build/gcc.debug

4. cd build/gcc.debug

5. cmake -DCMAKE_BUILD_TYPE=Release ../..

6. cmake --build .

## 2. Generate a validator key pair using the create_keys command.

1.  ./validator-keys create_keys

Sample output:
Validator keys stored in /home/my-user/.blocmatrix/validator-keys.json

This file should be stored securely and not shared.


## 3. Generate a validator token using the create_token command.

1. ./validator-keys create_token 

Sample output:

Update blocmatrixd.cfg file with these values:

  validator public key: nHUtNnLVx7odrz5dnfb2xpIgbEeJPbzJWfdicSkGyVw1eE5GpjQr

  [validator_token]
  eyJ2YWxpZGF0aW9uX3NlY3J|dF9rZXkiOiI5ZWQ0NWY4NjYyNDFjYzE4YTI3NDdiNT
  QzODdjMDYyNTkwNzk3MmY0ZTcxOTAyMzFmYWE5Mzc0NTdmYT|kYWY2IiwibWFuaWZl
  c3QiOiJKQUFBQUFGeEllMUZ0d21pbXZHdEgyaUNjTUpxQzlnVkZLaWxHZncxL3ZDeE
  hYWExwbGMyR25NaEFrRTFhZ3FYeEJ3RHdEYklENk9NU1l1TTBGREFscEFnTms4U0tG
  bjdNTzJmZGtjd1JRSWhBT25ndTlzQUtxWFlvdUorbDJWMFcrc0FPa1ZCK1pSUzZQU2
  hsSkFmVXNYZkFpQnNWSkdlc2FhZE9KYy9hQVpva1MxdnltR21WcmxIUEtXWDNZeXd1
  NmluOEhBU1FLUHVnQkQ2N2tNYVJGR3ZtcEFUSGxHS0pkdkRGbFdQWXk1QXFEZWRGdj
  VUSmEydzBpMjFlcTNNWXl3TFZKWm5GT3I3QzBrdzJBaVR6U0NqSXpkaXRROD0ifQ==


## 4. update the [validator_token] string in the blocmatrixd.cfg file.

## 5. restart the blocmatrixd.

    sudo systemctl restart blocmatrixd.service.
    Run the "blocmatrixd server_info" command you should able to see "server_state" parameter as "proposing".



# How to do trasactions on blocmatrixd network.
All the coins in the network are in the genisis account.

you can create the genisis account by following command.
$ blocmatrixd wallet_propose masterpassphrease

by using wallet_propose command you can create your account.
$ blocmatrixd wallet_propose.

Sample output:

account_id" : "bLwZHra3cgNJ59SSmTZfQ5HfJgWDTo3TFU",
      "key_type" : "secp256k1",
      "master_key" : "LAUD DOUG ORAL SLAT TINE CERN KNEW TALE AIRY DAWN BAR NAIL",
      "master_seed" : "shXT6dA3HNpsLdJQFWGiUu6DLXTSx",
      "master_seed_hex" : "836130B828E59CA7C9C43EEF24BF2EAB",
      "public_key" : "aBQYysUDw8DhutaEoH58MkWV6qFZRwJxRNSc8dCTKwAhUVXNKcH4",
      "public_key_hex" : "0355A6DBEB6823FAFCDF34E530C660016D24556F6D9BDD13CA139B087A847E1236",
      "status" : "success"

Using following command you can make a trasaction.
## 1. sign the transaction.

$ blocmatrixd sign snoPBbXtMeMyMHUVTgruqAfg1SUTr '{"TransactionType": "Payment", "Account": "bHr9CJAWyB4bj91VRWn96DkukG4rwdtyTh", "Destination": "ba7HgAEoz89poVv6TJ2BdqLoThe9x6BwEu", "Amount": "200000000", "Sequence": 4, "Fee": "100"}' offline

parameters:
1. snoPBbXtMeMyMHUVTgruqAfg1SUTr ----> is master seed of account is used sign the trasaction.(you have to keep it safe.)
2. Payment ----> It is a TransactionType.
3. Account ----> It is a source account from which you want to make a payment.
4. Destination ----> The destination whish to recieve payment.
5. Amount ----> Amount you wish to send.
6. Sequence ----> Is sequence number it indicates the (n-1)  transaction occurred on this source account.
    For sequence number type the below command.
    $blocmatrixd account_info bHr9CJAWyB4bj91VRWn96DkukG4rwdtyTh(can be any source account wish to make a payment)
    in the result you will the current sequence number to enter so use that in the above.

7. Fee ----> Fees for transaction to happen.

When you execute the above command.

Sample output:

result" : {
      "deprecated" : "This command has been deprecated and will be removed in a future version of the server. Please migrate to a standalone signing tool.",
      "status" : "success",
      "tx_blob" : "1200132280000000240000000368400000000000000A732102946398A614A41D1FFCBEDC9A2CD84FD2381DDCADDD9D50D66A75B5D0A990B98C74473045022100A97495711F41F7758A39A3DEFE91C2139ED7E94C0A39A92EB4B475C80B5E80D302201CBE94907E3C34AF44916039FC37A16CFD6744C6BB2745037DAFED50971A1B5681141BE0BFD6C04C38D41010C5490CA526AFAFAF485D85142AA699A44D05EEE878F8F8502EB3E30C470441CF",
      "tx_json" : {
         "Account" : "rsYQVsHjszqE6mMvkSdrzb7529u4ZpZ1RA",
         "Authorize" : "rhtWxen3p3waXsgCVdjC5MMbtxtPKRxSQe",
         "Fee" : "10",
         "Flags" : 2147483648,
         "Sequence" : 3,
         "SigningPubKey" : "02946398A614A41D1FFCBEDC9A2CD84FD2381DDCADDD9D50D66A75B5D0A990B98C",
         "TransactionType" : "DepositPreauth",
         "TxnSignature" : "3045022100A97495711F41F7758A39A3DEFE91C2139ED7E94C0A39A92EB4B475C80B5E80D302201CBE94907E3C34AF44916039FC37A16CFD6744C6BB2745037DAFED50971A1B56",
         "hash" : "BA87D0C01DE8E3790393C16C937CF84B577C98AA50B806D21D00F605A7CC1FEF"
      }
   }
}

## 2. submit the transaction.
by using the tx_blob parameter you submit the transaction to the network.

command $ blocmatrixd submit tx_blob

$ blocmatrixd submit 1200132280000000240000000368400000000000000A732102946398A614A41D1FFCBEDC9A2CD84FD2381DDCADDD9D50D66A75B5D0A990B98C74473045022100A97495711F41F7758A39A3DEFE91C2139ED7E94C0A39A92EB4B475C80B5E80D302201CBE94907E3C34AF44916039FC37A16CFD6744C6BB2745037DAFED50971A1B5681141BE0BFD6C04C38D41010C5490CA526AFAFAF485D85142AA699A44D05EEE878F8F8502EB3E30C470441CF

Sample output :


   "result" : {
      "engine_result" : "tesSUCCESS",
      "engine_result_code" : 0,
      "engine_result_message" : "The transaction was applied. Only final in a validated ledger.",
      "status" : "success",
      "tx_blob" : "1200132280000000240000000368400000000000000A732102946398A614A41D1FFCBEDC9A2CD84FD2381DDCADDD9D50D66A75B5D0A990B98C74473045022100A97495711F41F7758A39A3DEFE91C2139ED7E94C0A39A92EB4B475C80B5E80D302201CBE94907E3C34AF44916039FC37A16CFD6744C6BB2745037DAFED50971A1B5681141BE0BFD6C04C38D41010C5490CA526AFAFAF485D85142AA699A44D05EEE878F8F8502EB3E30C470441CF",

the result should be : "tesSUCCESS"

## 3. Check the transaction in the ledger.

Use the below command to check the transaction is added in the ledger or not.

$ blocmatrixd tx_history 0

In the response you should be able to see the transaction you made above.