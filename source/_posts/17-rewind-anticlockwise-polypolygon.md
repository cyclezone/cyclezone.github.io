---
title: 17. rewind anticlockwise polypolygon
author: cyclezone
img: /medias/banner/3.jpeg
top: false
cover: false
coverImg: /medias/banner/1.jpeg
password: 8d969eef6
toc: false
mathjax: false
summary: rewind anticlockwise polypolygon
categories: cycle zone
tags:
  - cycle
date: 2021-12-19 21:10:23
---
   
# 17. rewind anticlockwise polypolygon

```
    
long ClockSpace::CheckPolyPolyGon(DOT* pDotBuf, long* ne, int na)
{
  if (na <= 0) return 0;
  else if (na == 1) return 1;

  long nDotIndex = 0; bool bClockwiseBase = false;
  for (int i = 0; i < na; ++i)
  {
    bool bClockwise = ClockSpace::IsClockwise(pDotBuf + nDotIndex, ne[i]);
    
    if (i == 0) 
      bClockwiseBase = bClockwise;
    else if (bClockwise == bClockwiseBase)
      ClockSpace::Rewind(pDotBuf + nDotIndex, ne[i]);
    nDotIndex += ne[i];
  }

  return na;
}

void   ClockSpace::Rewind(DOT *tp, long n)
{
  long i, mid;
  DOT tmp;
  mid = n / 2;
  for (i = 0; i < mid; i++)
  {
    tmp = tp[i];
    tp[i] = tp[n - i - 1];
    tp[n - i - 1] = tmp;
  }
}

bool ClockSpace::IsClockwise(DOT* pDotBuf, long ne) {
  double maxY = 0.0;
  int index = 0;

  //找到Y值最大的点及其前一点和后一点
  for (int i = 0; i < ne; i++) {
    DOT* pt = pDotBuf+i;
    if (i == 0) {
      maxY = pt->y;
      index = 0;
      continue;
    }
    if (maxY < pt->y) {
      maxY = pt->y;
      index = i;
    }
  }

  int front = index == 0 ? ne - 2 : index - 1;
  int middle = index;
  int after = index + 1;

  //利用矢量叉积判断是逆时针还是顺时针。设矢量P = ( x1, y1 )，Q = ( x2, y2 )，则矢量叉积定义为由(0,0)、p1、p2和p1+p2    
  //所组成的平行四边形的带符号的面积，即：P × Q = x1*y2 - x2*y1，其结果是一个标量。
  //显然有性质 P × Q = - ( Q × P ) 和 P × ( - Q ) = - ( P × Q )。
  //叉积的一个非常重要性质是可以通过它的符号判断两矢量相互之间的顺逆时针关系：
  //若 P × Q > 0 , 则P在Q的顺时针方向。
  //若 P × Q < 0 , 则P在Q的逆时针方向。
  //若 P × Q = 0 , 则P与Q共线，但可能同向也可能反向。
  //解释：
  //a×b=(ay * bz - by * az, az * bx - ax * bz, ax * by - ay * bx) 又因为az bz都为0，所以a×b=（0，0， ax * by - ay * bx）
  //根据右手系（叉乘满足右手系），若 P × Q > 0,ax * by - ay * bx>0,也就是大拇指指向朝上，所以P在Q的顺时针方向，一下同理。

  DOT* frontPt = (pDotBuf + front); 
  DOT* middlePt = (pDotBuf + middle); 
  DOT* afterPt =  (pDotBuf + after);

  return (afterPt->x - frontPt->x) * (middlePt->y - frontPt->y)
    - (middlePt->x - frontPt->x) * (afterPt->y - frontPt->y) > 0.0;
}

```
