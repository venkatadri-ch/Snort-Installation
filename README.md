### Snort-Installation 
# Note: The Snort 3 installation procedure also provided below
The Installation process for snort varies depending on the operating system. Given below is a general guide for installing snort on a Linux-based system.

![snort](https://github.com/user-attachments/assets/e87b4df1-e4aa-440d-b243-9877afd12b27)


## To update system packages, use the following code:
     sudo apt-get update
     sudo apt-get upgrade

## To install dependencies,type:

    sudo apt-get install build-essential libpcap-dev libpcre3-dev libdumbnet-dev bison flex zlibig.dev

# Now,download the latest version of snort from the official website or use wget to download it directly to your server.To compile and install, use the following code

    tar -xvzf snort-*.tar.gz
    cd snort-*
    ./configure
    make
    sudo make install

# update shared libraries by using:
        sudo ldconfig

# Create configuration directories as follows:
       sudo mkdir /etc/snort
       sudo mkdir /etc/snort/rules
       sudo mkdir /etc/snort/preproc_rules
       sudo mkdir /var/log/snort
       sudo mkdir /usr/local/lib/snort_dynamicrules

# Next,set permissions:

       sudo chmod -R 5775 /etc/snort
       sudo chmod -R 5775 /var/log/snort
       sudo chmod -R 5775 /usr/local/lib/snort_dynamicrules

# Create the custom rule file as follows:

       sudo touch /etc/snort/rules/local.rules

# To configure Snort,edit the snort.conf file to set up the network variables and include the local.rules file. To test the configuration,type:
       snort -T -c /etc/snort/snort.conf


### Snort3-instructions 
![snort3](https://github.com/user-attachments/assets/736eb6ea-c85b-480b-8eb6-be648542ad58)


# STEP 1 - INSTALL PRE-REQS
      sudo apt-get install -y build-essential autotools-dev libdumbnet-dev libluajit-5.1-dev libpcap-dev \ zlib1g-dev pkg-config libhwloc-dev cmake liblzma-dev openssl libssl-dev cpputest libsqlite3-dev \ libtool uuid-dev git autoconf bison flex libcmocka-dev libnetfilter-queue-dev libunwind-dev \ libmnl-dev ethtool libjemalloc-dev

# STEP 2 – Install pcre
       cd ~/snort wget https://sourceforge.net/projects/pcre/files/pcre/8.45/pcre-8.45.tar.gz
       tar -xzvf pcre-8.45.tar.gz
       cd pcre-8.45
       ./configure
       make
       sudo make install

# STEP 3 – Install gperftools

       cd ~/snort wget https://github.com/gperftools/gperftools/releases/download/gperftools-2.9.1/gperftools-2.9.1.tar.gz
       tar xzvf gperftools-2.9.1.tar.gz
       cd gperftools-2.9.1
       ./configure
       make
       sudo make install

# STEP 4 – Install Ragel

        cd ~/snort wget http://www.colm.net/files/ragel/ragel-6.10.tar.gz
        tar -xzvf ragel-6.10.tar.gz
        cd ragel-6.10
        ./configure
        make
        sudo make install

# STEP 5 - download (but don’t install) the Boost C++ Libraries:

        cd ~/snort wget https://boostorg.jfrog.io/artifactory/main/release/1.77.0/source/boost_1_77_0.tar.gz
        tar -xvzf boost_1_77_0.tar.gz

# STEP 6 – Install Hyperscan

        cd ~/snort wget https://github.com/intel/hyperscan/archive/refs/tags/v5.4.2.tar.gz
        tar -xvzf v5.4.2.tar.gz
        mkdir ~/snort/hyperscan-5.4.2-build
        cd hyperscan-5.4.2-build/
        cmake -DCMAKE_INSTALL_PREFIX=/usr/local -DBOOST_ROOT=~/snort/boost_1_77_0/ ../hyperscan-5.4.2
        make
        sudo make install

# STEP 7 – Install flatbuffers

        cd ~/snort
        wget https://github.com/google/flatbuffers/archive/refs/tags/v2.0.0.tar.gz -O flatbuffers-v2.0.0.tar.gz
        tar -xzvf flatbuffers-v2.0.0.tar.gz
        mkdir flatbuffers-build
        cd flatbuffers-build
        cmake ../flatbuffers-2.0.0
        make
        sudo make install

# STEP 8 - Install Data Acquistion (DAQ) from Snort

        cd ~/snort wget https://github.com/snort3/libdaq/archive/refs/tags/v3.0.13.tar.gz -O libdaq-3.0.13.tar.gz
        tar -xzvf libdaq-3.0.13.tar.gz
        cd libdaq-3.0.13
        ./bootstrap
        ./configure
        make
        sudo make install

# STEP 9 - Update shared libraries

        sudo ldconfig

# STEP 10 – Download latest version of Snort 3

        cd ~/snort
        wget https://github.com/snort3/snort3/archive/refs/tags/3.1.74.0.tar.gz -O snort3-3.1.74.0.tar.gz
        tar -xzvf snort3-3.1.74.0.tar.gz
        cd snort3-3.1.74.0
        ./configure_cmake.sh --prefix=/usr/local --enable-jemalloc
        cd build
        make
        sudo make install

# STEP 11 - Disable LRO & GRO using a service

 - [Unit]
- Description=Ethtool Configration for Network Interface
- [Service]
- Requires=network.target
- Type=oneshot
- ExecStart=/sbin/ethtool -K <network adapter> gro off
- ExecStart=/sbin/ethtool -K <network adapter> lro off
- [Install]
- WantedBy=multi-user.target

# STEP 12

      git clone https://github.com/shirkdog/pulledpork3.git
      cd ~/snort/pulledpork3
      sudo mkdir /usr/local/bin/pulledpork3
      sudo cp pulledpork.py /usr/local/bin/pulledpork3
      sudo cp -r lib/ /usr/local/bin/pulledpork3
      sudo chmod +x /usr/local/bin/pulledpork3/pulledpork.py
      sudo mkdir /usr/local/etc/pulledpork3
      sudo cp etc/pulledpork.conf /usr/local/etc/pulledpork3/
