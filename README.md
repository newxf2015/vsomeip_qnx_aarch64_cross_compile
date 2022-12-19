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

7、vsomip can't cross compile for qnx, it can be running only on Linux/WIN32/Android. must be Shielding some future for qnx 
can download from github: https://github.com/lixiaolia/vsomeip-qnx
8、build vsomeip
prepare toolchain cmake 
