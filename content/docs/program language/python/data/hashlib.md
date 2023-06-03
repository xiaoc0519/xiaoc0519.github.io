+++
title = "Hashlib"
date = 2023-05-23T11:49:19+08:00
menu = 'python'
+++



# **Hashlib**

```python
import hashlib
```

> 支持 `blake2b`, `sha1`, `sha3_224`, `sha256`, `md4`, `sha3_384`, `sm3`, `shake_128`, `sha384`, `sha512_224`, `shake_256`, `sha3_512`, `whirlpool`, `blake2s`, `ripemd160`, `sha3_256`, `sha512`, `md5`, `md5-sha1`, `mdc2`, `sha224`, `sha512_256`


## MD5

>  MD5是最常见的摘要算法，速度很快  
> 生成结果是固定的128 bit字节，通常用一个32位的16进制字符串表示。

```python
m = hashlib.md5() # 构建MD5对象

#设置编码格式 并将字符串添加到MD5对象中
m.update(password.encode(encoding='utf-8')) 

# hexdigest()将加密字符串 生成十六进制数据字符串值
password_md5 = m.hexdigest() # '4dec5d6a773de446285d2ac3b540dade'


# 在构建对象直接插入加密字符串
md = hashlib.md5('string double'.encode(encoding='utf-8'))
md.hexdigest() # '98b9e3a1f9c0aa9c10eeda9fdbbea340'

# 通过update方法 往MD5对象中增加字符串参数
md = hashlib.md5()
md.update('string double'.encode(encoding='utf-8'))
md.hexdigest() # '98b9e3a1f9c0aa9c10eeda9fdbbea340'

# 当数据量过大时，可以分块摘要
md = hashlib.md5()
md.update("string ".encode("utf-8"))  # 注意：分块是空格也要保持一致
md.update("double".encode("utf-8"))
md.hexdigest() # '98b9e3a1f9c0aa9c10eeda9fdbbea340'

# 加盐 
md.update('string'.encode('utf-8'))  # 添加 无意义 / 任何字符 ,增强md5安全
md.update(password.encode('utf-8'))
md.hexdigest() # '397e58d3844e56fc797a2dd3ceea19b8' 
```






