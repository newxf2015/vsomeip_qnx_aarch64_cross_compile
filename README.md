# vsomeip_qnx_aarch64_cross_compile
## cross compile for boost_1_70_0  
<font color="red">【compile only static boost lib】</font>  
1、source envsetup.sh  
2、download boost_1_70_0  
3、modify tools/build/src/tools/qcc.jam  
![image](https://user-images.githubusercontent.com/12552813/208382257-4f1f2478-96fa-4df0-b3cd-2a3f18492ff1.png)  
4、sh bootstrap.sh  
5、detail for   project-config.jam 
```shell
libraries = --without-python \
			--without-wave \
			--without-timer \
			--without-mpi \
			--without-log \
			--without-locale \
			--without-iostreams \
			--without-graph \
			--without-graph_parallel \
			--without-coroutine \
			--without-context \
			--without-atomic \
			--without-chrono \
			--without-test \
			--without-math \
			 ;
6、compile boost
./b2 install link=static toolset=qcc cxxflags="-Vgcc_ntoaarch64le -stdlib=libstdc++ -fPIC -std=c++11 -Wall" target-os=qnxnto --prefix=./3_qnx_boost 

7、vsomip can't cross compile for qnx, it can be running only on Linux/WIN32/Android. must be Shielding some futrue for qnx 
can download from github: https://github.com/lixiaolia/vsomeip-qnx
8、build vsomeip
prepare toolchain cmake file and put in vsomeip root path toolchain/qnx_7.0.0_linux_x86_64.cmake

set(Boost_FOUND TRUE)
set(Boost_DIR your_boost_install_dir/boost_1_70_0/3_qnx_boost)
set(Boost_INCLUDE_DIR ${Boost_DIR}/include)
set(Boost_LIBRARY_DIR ${Boost_DIR}/lib)
set(BOOST_LIBRARYDIR ${Boost_DIR}/lib)



modify your boost install dir
9、run build bash 
```shell
#!/bin/bash
if [ -d "build" ]; then
 rm -rf build
fi
mkdir build
cd build
rm -rf /your_vsomeip_install_path/vsomeip_qnx/vsomeip_qnx
cmake -DCMAKE_TOOLCHAIN_FILE=./toolchain/qnx_7.0.0_linux_x86_64.cmake -DCMAKE_POSITION_INDEPENDENT_CODE=ON \
	-DCMAKE_INSTALL_PREFIX=/your_vsomeip_install_path/vsomeip_qnx \
       ..
if [ $? != 0 ]; then
  echo "FAIL: cmake failed"
fi
make -j8
make install 
cp -rf /home/xiongfeng/workspace/github/vsomeip-qnx-master/build/examples/hello_world \
       	/home/xiongfeng/workspace/github/3rdparty/vsomeip_qnx/bin/
if [ $? != 0 ]; then
  echo "FAIL: make failed"
fi
```
10、bash build.sh compile vsomeip
![image](https://user-images.githubusercontent.com/12552813/208389231-14f2cb37-8e38-4223-a803-fa5523cebf1f.png)
l1、check .so and binary file
![image](https://user-images.githubusercontent.com/12552813/208389587-b9635c0e-7790-44c2-80fe-15b67edcf585.png)
![image](https://user-images.githubusercontent.com/12552813/208389669-9a44d45e-1917-49a0-b2fc-b912bb94d429.png)

12、run hello_world servic
![image](https://user-images.githubusercontent.com/12552813/208388506-42267419-20de-46dc-acc5-53246c63cc97.png)

13、run hello_world client
![image](https://user-images.githubusercontent.com/12552813/208388868-9d4e9bab-7368-4710-8a7e-29efea8dbedf.png)
