 # NOTE
 * [Intel®SGX Data Center Attestation Primitives (Intel®SGX DCAP)  ](https://download.01.org/intel-sgx/dcap-1.1/linux/docs/Intel_SGX_DCAP_ECDSA_Orientation.pdf)

 * https://chenju2k6.github.io/blog/2018/11/driver

 * [Intel SGX入门（三）——SGX保护框架、软件层改进篇](https://blog.csdn.net/clh14281055/article/details/106960650/)

 # driver

 * [Linux内核中经典链表 list_head 常见使用方法解析](https://blog.csdn.net/wanshilun/article/details/79747710)


 # Overview of Intel® Software Guard Extensions Instructions and Data Structures

 * [Overview of Intel® Software Guard Extensions Instructions and Data Structures](https://software.intel.com/content/www/us/en/develop/blogs/overview-of-intel-software-guard-extensions-instructions-and-data-structures.html)

 * Paging Crypto MetaData (PCMD)
 
 # others

 * [Open Enclave SDK](https://github.com/openenclave/openenclave)

 * EDMM（Enclave Dynamic Memory Management）

----

ref [《Intel® 64 and IA-32 Architectures Software Developer’s Manual Volume 3》SGX部分【按需翻译】](https://blog.csdn.net/clh14281055/article/details/108229461)

> 在支持SGX2的处理器上，作为EMODT（硬件指令）的特殊用法，Intel SGX支持删除Enclave页。页类型PT_TRIM意味着该页已从Enclave的地址空间中裁剪，并且该页不再可以访问。PT_TRIM状态的页不允许被做任何修改；该页必须先删除，当Enclave再次使用该页面之前由OS重新分配。可以批量化的进行页面重新分配操作，以提高其效率。
> 
>裁剪Enclave页的协议如下：
> 1. Enclave发信号通知OS某个页不再使用。
> 2. OS对该页调用EMODT，请求将页的类型更改为PT_TRIM。
> 2.1   SECS和VA页面无法用这种方式裁剪，因此要求页面的初始类型必须为PT_REG或PT_TCS
> 2.2. EMODT只能作用于VALID页面
> 3. OS执行ETRACK指令，从所有处理器（CPU核）中删除TLB地址
> 4. Enclave内发出EACCEPT指令。（由可信的Enclave表示接受此次修改操作）
> 5. OS现在可以将该页永久删除（通过调用EREMOVE）。