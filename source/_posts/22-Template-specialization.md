---
title: 22. Template specialization
author: cyclezone
img: /medias/banner/3.jpeg
top: false
cover: false
coverImg: /medias/banner/1.jpeg
password: 8d969eef6
toc: false
mathjax: false
summary: Template specialization
categories: cycle zone
tags:
  - cycle
date: 2021-12-20 21:19:49
---


# 22. Template specialization

 1. we use template function which has the original operation 
    to cover some or several cases;

    but when template function  crash some special case,
	the original operation cannot cover  ,

    we may specialize the template function with the speicial operation  ;

 2. in common ,function name is same in different cases,
    
	but with different parameter type and different operation;

  ```
    template<typename T> bool i_FromString(T& t,std::string str)
    {
      istringstream is(str);
      is.imbue(LOC_CHS);
      is >> t;
      return true;
    }
    template<size_t N> inline bool i_FromString(char (&sz)[N],std::string str)
    {
      if (N>0)
      {
        memset(sz,0,sizeof(char)*N);
        strncpy(sz,str.c_str(),N-1);
        return true;
      }
      return false;
    }
    template<> inline bool i_FromString(char*& t,std::string str)
    {
      const char* pChVal = str.c_str();
      lstrcpyn(t,pChVal,strlen(pChVal)+1);
      return true;
    }

    template<> inline bool i_FromString(float& t,std::string str)
    {
      if(str != "")
      {
        float fVal = atof(str.c_str());
        memcpy(&t,&fVal,sizeof(float));
        return true;
      }
      return false;
    }

    template<> inline bool i_FromString(double& t,std::string str)
    {
      if(str != "")
      {
    #if defined(BUTTER_ANDROID)
        double dVal = atof(str.c_str());
        memcpy(&t,&dVal,sizeof(double));
    #else
        t = atof(str.c_str());
    #endif
        return true;
      }
      return false;
    }

    template<> inline bool i_FromString(GUID& t,std::string str)
    {
      CString   strTemp;
      if(str != "")
      {
        strTemp = str.c_str();
        CvtStringToGuid(strTemp,t);
        return true;
      }
      return false;
    }

    template<> inline bool i_FromString(string& t,std::string str)
    {
      t = str;
      return true;
    }
    template<> inline bool i_FromString(CString& t, std::string str)
    {
      t = str.c_str();
      return true;
    }

  ```

