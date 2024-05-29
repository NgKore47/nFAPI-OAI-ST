# nFAPI OAI 

## Build procedure:
### For USRP support:

```shell
sudo apt install -y autoconf automake build-essential ccache cmake cpufrequtils doxygen ethtool g++ git inetutils-tools libboost-all-dev libncurses5 libncurses5-dev libusb-1.0-0 libusb-1.0-0-dev libusb-dev python3-dev python3-mako python3-numpy python3-requests python3-scipy python3-setuptools python3-ruamel.yaml

git clone https://github.com/EttusResearch/uhd.git ~/uhd
cd ~/uhd
git checkout v4.6.0.0
cd host
mkdir build
cd build
cmake ../
make -j $(nproc)
make test # This step is optional
sudo make install
sudo ldconfig
sudo uhd_images_downloader
```

```shell
./build_oai -I
./build_oai -w USRP --ninja --nrUE --gNB --build-lib "nrscope" -C
```

Build for ORAN O-RU also

```shell
./build_oai --gNB --ninja -t oran_fhlib_5g --cmake-opt -Dxran_LOCATION=$HOME/phy/fhi_lib/lib
```

**Use the conf files from the `oai_conf_files` directory**

## rfSIM:

```shell
sudo NFAPI_TRACE_LEVEL=info ./nr-softmodem -O b210_l2_vnf.conf --nfapi VNF --sa --log_config.global_log_options wall_clock  --emulate-l1
```

```shell
sudo NFAPI_TRACE_LEVEL=info ./nr-softmodem -O b210_l1_pnf.conf --nfapi PNF --rfsim --rfsimulator.serveraddr server --log_config.global_log_options wall_clock --sa
```

You can test with simulated `UE` using the below command:

```shell
sudo ./nr-uesoftmodem -r 106 --numerology 1 --band 78 -C 3619200000 --sa --uicc0.imsi 001010000000001 --rfsim
```

## USRP B210:

```shell
sudo NFAPI_TRACE_LEVEL=info ./nr-softmodem -O b210_l2_vnf.conf --nfapi VNF --sa --log_config.global_log_options wall_clock  --emulate-l1
```

```shell
sudo NFAPI_TRACE_LEVEL=info ./nr-softmodem -O b210_l1_pnf.conf --nfapi PNF --log_config.global_log_options wall_clock --sa -E --continuous-tx
```

## LiteOn Flex-FI O-RU:

```shell
sudo NFAPI_TRACE_LEVEL=info ./nr-softmodem -O final_liteon_l2_vnf.conf --nfapi VNF --sa --log_config.global_log_options wall_clock  --emulate-l1
```


```shell
sudo NFAPI_TRACE_LEVEL=info ./nr-softmodem -O liteon_l1_pnf.conf --nfapi PNF --log_config.global_log_options wall_clock --sa --reorder-thread-disable 1 --thread-pool 10,11,12,13,14
```
