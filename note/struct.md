


```c
enum sgx_page_type {
	SGX_PAGE_TYPE_SECS	= 0x00,
	SGX_PAGE_TYPE_TCS	= 0x01,
	SGX_PAGE_TYPE_REG	= 0x02,
	SGX_PAGE_TYPE_VA	= 0x03,
	SGX_PAGE_TYPE_TRIM	= 0x04,
};
```


```c
struct sgx_secs {
	uint64_t size;
	uint64_t base;
	uint32_t ssaframesize;
	uint32_t miscselect;
	uint8_t reserved1[SGX_SECS_RESERVED1_SIZE];

	uint64_t attributes;//[63:0]
	uint64_t xfrm;      //xsave feature request mask

	uint32_t mrenclave[8];
	uint8_t reserved2[SGX_SECS_RESERVED2_SIZE];
	uint32_t mrsigner[8];
	uint8_t	reserved3[SGX_SECS_RESERVED3_SIZE];
	uint32_t configid[16];
	uint16_t isvvprodid;
	uint16_t isvsvn;
	uint16_t configsvn;
	uint8_t reserved4[SGX_SECS_RESERVED4_SIZE];
};
```





```c
struct sgx_encl_page {
	unsigned long addr;
	unsigned int flags;
	struct sgx_epc_page *epc_page;
    //sgx_free_list链表 中 的一项
    
	struct sgx_va_page *va_page;
	unsigned int va_offset;
};
```





```c
struct sgx_encl {
	unsigned int flags;

	//来自secs
	uint64_t attributes;
	uint64_t xfrm;


	unsigned int secs_child_cnt;
	struct mutex lock;
	struct mm_struct *mm;
    
	struct file *backing;
	struct file *pcmd;
    
	struct list_head load_list;  //链表
	struct kref refcount;            //一个引用计数器
    
    
	unsigned long base;
	unsigned long size;
	unsigned long ssaframesize;
    
    
	struct list_head va_pages;  //链表
	struct radix_tree_root page_tree;
	struct list_head add_page_reqs;  //链表
	struct work_struct add_page_work;//内核工作队列
   
	struct sgx_encl_page secs;
    
	struct sgx_tgid_ctx *tgid_ctx;
	struct list_head encl_list; //链表
	struct mmu_notifier mmu_notifier;
	unsigned int shadow_epoch;
};
```

