#PHP源码

	基于版本5.5.9

##变量存储结构

路径：Zend/zend.h

```cpp
typedef struct _zval_struct zval;
...
struct _zval_struct {
    /* Variable information */
    zvalue_value value;     /* value */
    zend_uint refcount__gc;
    zend_uchar type;    /* active type */
    zend_uchar is_ref__gc;
};
```

| 属性名         | 含义          | 初始值  |
| :------------ | --------:    | :----: |
|  refcount__gc | 表示引用计数   |  1     |
| is_ref__gc    | 表示是否为引用 |  0      |
|value          | 存储变量的值   |         |
|type           | 变量具体的类型 |         |

##变量的值存储
```cpp
typedef union _zvalue_value {
    long lval;                  /* long value */
    double dval;                /* double value */
    struct {
        char *val;
        int len;
    } str;
    HashTable *ht;              /* hash table value */
    zend_object_value obj;
} zvalue_value;
```

结构体和联合体的区别

1. struct和union都是由多个不同的数据类型成员组成, 但在任何同一时刻, union中只存放了一个被选中的成员, 而struct的所有成员都存在。在struct中，各成员都占有自己的内存空间，它们是同时存在的。一个struct变量的总长度等于所有成员长度之和。在Union中，所有成员不能同时占用它的内存空间，它们不能同时存在。Union变量的长度等于最长的成员的长度。
2. 对于union的不同成员赋值, 将会对其它成员重写, 原来成员的值就不存在了, 而对于struct的不同成员赋值是互不影响的。

##哈希表结构

路径：Zend/zend_hash.c

###HashTable的数据结构
```cpp
typedef struct _hashtable { 
    uint nTableSize;        // hash Bucket的大小，最小为8，以2x增长。
    uint nTableMask;        // nTableSize-1 ， 索引取值的优化
    uint nNumOfElements;    // hash Bucket中当前存在的元素个数，count()函数会直接返回此值 
    ulong nNextFreeElement; // 下一个数字索引的位置
    Bucket *pInternalPointer;   // 当前遍历的指针（foreach比for快的原因之一）
    Bucket *pListHead;          // 存储数组头元素指针
    Bucket *pListTail;          // 存储数组尾元素指针
    Bucket **arBuckets;         // 存储hash数组
    dtor_func_t pDestructor;    // 在删除元素时执行的回调函数，用于资源的释放
    zend_bool persistent;       //指出了Bucket内存分配的方式。如果persisient为TRUE，则使用操作系统本身的内存分配函数为Bucket分配内存，否则使用PHP的内存分配函数。
    unsigned char nApplyCount; // 标记当前hash Bucket被递归访问的次数（防止多次递归）
    zend_bool bApplyProtection;// 标记当前hash桶允许不允许多次访问，不允许时，最多只能递归3次
#if ZEND_DEBUG //预处理指令，满足条件才编译
    int inconsistent; 
#endif
} HashTable;
```

###数据容器
```cpp

typedef struct bucket {
    ulong h;            // 对char *key进行hash后的值，或者是用户指定的数字索引值
    uint nKeyLength;    // hash关键字的长度，如果数组索引为数字，此值为0
    void *pData;        // 指向value，一般是用户数据的副本，如果是指针数据，则指向pDataPtr
    void *pDataPtr;     //如果是指针数据，此值会指向真正的value，同时上面pData会指向此值
    struct bucket *pListNext;   // 整个hash表的下一元素
    struct bucket *pListLast;   // 整个哈希表该元素的上一个元素
    struct bucket *pNext;       // 存放在同一个hash Bucket内的下一个元素
    struct bucket *pLast;       // 同一个哈希bucket的上一个元素
    // 保存当前值所对于的key字符串，这个字段只能定义在最后，实现变长结构体
    char arKey[1];              
} Bucket;
```

##PHP运行时的数据结构

```cpp
struct _zend_execute_data {
	struct _zend_op *opline;
	zend_function_state function_state;
	zend_op_array *op_array;
	zval *object;
	HashTable *symbol_table;
	struct _zend_execute_data *prev_execute_data;
	zval *old_error_reporting;
	zend_bool nested;
	zval **original_return_value;
	zend_class_entry *current_scope;
	zend_class_entry *current_called_scope;
	zval *current_this;
	struct _zend_op *fast_ret; /* used by FAST_CALL/FAST_RET (finally keyword) */
	call_slot *call_slots;
	call_slot *call;
};

```

```cpp
typedef struct _zend_function_state {
	zend_function *function;
	void **arguments;
} zend_function_state;
```
zend_function_state 保存了执行的函数指针，以及参数

参数采用了 void**


