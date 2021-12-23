---
title: 23. Howt to access unaligned memory
author: cyclezone
img: /medias/banner/3.jpeg
top: false
cover: false
coverImg: /medias/banner/1.jpeg
password: 8d969eef6
toc: false
mathjax: false
summary: Howt to access unaligned memory
categories: cycle zone
tags:
  - cycle
date: 2021-12-20 23:11:39
---

# 23. Howt to access unaligned memory

  * Unaligned memory accesses occur when you try to read N bytes of data starting from an address 
    
	that is not evenly divisible by N (i.e. addr % N != 0).
  
    For example, reading 4 bytes of data from address 0x10004 is fine, 
	
	but reading 4 bytes of data from address 0x10005 would be an unaligned memory access.
   
* when access the unligned memory directly,OS may sent SIGBUS error;
   
* if read the unligned memory, use function 'get_unaligned(ptr)'
  
  if write the unligned  memory,use memcpy();

* for more information ,please visit https://www.kernel.org/doc/html/latest/core-api/unaligned-memory-access.html


##  case 1: both dt and st is aligned
 ```
## for case: both dt and st is aligned
template<typename T> void i_AssignValue(T& dt, T& st)
{
	i_FromString(dt, i_ToString(st));
}
 ```
##  case 2: either dt or st is aligned or both are unligned


 ```
/************************************************************
 *  for case: either dt or st is unaligned;
 *  if read the unligned value, use function 'get_unaligned(ptr)'
 *  if write the unligned value,use memcpy();
*************************************************************/

#ifdef BUTTER_ANDROID
#ifndef __GET_UNALIGNED_
#define __GET_UNALIGNED_

#define __get_unaligned_t(type, ptr) ({						\
	const struct { type x; } __packed *__pptr = (__typeof(__pptr))(ptr);	\
	__pptr->x;								\
})

#define get_unaligned(ptr)	__get_unaligned_t(__typeof(*(ptr)), (ptr))

#endif /* __GET_UNALIGNED_ */

/// for case: Ether dt or st is unaligned
template<> inline void i_AssignValue(float& dt, float& st)
{
	float val;
	const void* ptr = (void*)&st;
	//uint16_t temp16 = get_unaligned((uint16_t *) ptr); //OK
	float temp16 = get_unaligned((float *) ptr); /// read the unligned memory
	std::string strVal = i_ToString((double)temp16);
	i_FromString<float>(val,strVal);
	memcpy(&dt, &val, sizeof(float)); /// write the unligned memory
}

#endif //BUTTER_ANDROID

```
