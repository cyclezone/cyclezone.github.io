---
title: 18. Parse Segments
author: cyclezone
img: /medias/banner/3.jpeg
top: false
cover: false
coverImg: /medias/banner/1.jpeg
password: 8d969eef6
toc: false
mathjax: false
summary: Parse Segments
categories: cycle zone
tags:
  - cycle
date: 2021-12-19 21:11:42
---

# 18. Parse Segments 


    woid testDemo()
    {
      std::vector<std::string>  values;
      std::string strInfo = "id;name;property";
      ParseSegments(strInfo, ';', values);
    }
    //////////////////////////////////////////
    int ParseSegments(std::string strValue, char chPartition, std::vector<std::string> & values)
    {
      size_t idx = 0;
      values.clear();
      std::vector<size_t> indexs;
      for (; idx < strValue.size(); idx++)
      {
        if (strValue[idx] == chPartition)
        {
          indexs.push_back(idx);
        }
        else if (idx == strValue.size() - 1)
        {
          indexs.push_back(idx + 1);
        }
      }
      size_t nPosBegin = 0;
      for (idx = 0; idx < indexs.size(); idx++)
      {
        values.push_back(strValue.substr(nPosBegin, indexs[idx] - nPosBegin));
        nPosBegin = indexs[idx] + 1;
      }
      return idx;
    }
