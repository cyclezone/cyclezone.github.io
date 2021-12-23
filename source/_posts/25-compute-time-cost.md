---
title: 25. Compute time cost
author: cyclezone
img: /medias/banner/3.jpeg
top: false
cover: false
coverImg: /medias/banner/1.jpeg
password: 8d969eef6
toc: false
mathjax: false
summary: compute time cost
categories: cycle zone
tags:
  - cycle
date: 2021-12-21 23:15:04
---

# 25. Compute time cost
```

int main()
{
	{
		CHTime timecost;
		fun1();
	}
	
	{
		CHTime timecost;
		fun2();
		timecost.getTime();
	}
}

////**********************CHTime.h************************

#ifdef WIN32
/// for windows
#include <windows.h>　
#else //BUTTER_LINUX
//// for linux
#include <sys/time.h>　
#endif

class CHTime
{
public:
	CHTime(){ 
		Begin();
	}
	~CHTime(){
		End();
	}
	void Begin()
	{
#ifdef WIN32
       QueryPerformanceFrequency(&m_freq);
       QueryPerformanceCounter(&m_queryBegin);
#else
       gettimeofday(&m_begin,NULL);
#endif
	}
	void End()
	{
#ifdef WIN32   
       QueryPerformanceCounter(&m_queryEnd);
       m_timeuse=(double)(m_queryEnd.QuadPart-m_queryBegin.QuadPart)/(double)m_freq.QuadPart * 1000.0; 
#else
       gettimeofday(&m_end,NULL);
       m_timeuse = (m_end.tv_sec - m_begin.tv_sec) + (double)(m_end.tv_usec - m_begin.tv_usec)/1000.0;
#endif
      cout<<"timecost = "<<m_timeuse<<endl;  //（unit：ms）
	}
	double getTime()
	{
		End(); 
		return m_timeuse;
	}
protected:	
    double m_timeuse;///ms

#ifdef WIN32
    LARGE_INTEGER m_queryBegin,m_queryEnd,m_freq;
#else
	struct timeval m_begin,m_end;
#endif

}


```