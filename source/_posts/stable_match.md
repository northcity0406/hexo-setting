---
title: 稳定匹配
date: 2017-10-23 21:25:25
tags: 
	- 算法设计
	- 计算机
copyright: true
categories: 算法设计
---
### 稳定匹配的两种方式
  +  ###完美匹配
>    集合M={m<sub>1</sub>,m<sub>2</sub>……m<sub>n</sub>}和集合W={w<sub>1</sub>,w<sub>2</sub>……w<sub>n</sub>},令M×W={(m<sub>i</sub>,w<sub>j</sub>)|m<sub>i</sub>∈M,w<sub>i</sub>∈W}
同时，S∈M×W,S是M×W的一个有序对的集合，且M的任意一个成员和W的任意一个成员都仅仅只会在S中一个对里。

  + ###稳定匹配
>    集合M={m<sub>1</sub>,m<sub>2</sub>……m<sub>n</sub>}和集合W={w<sub>1</sub>,w<sub>2</sub>……w<sub>n</sub>},∀m<sub>i</sub>∈M,m<sub>i</sub>对于W有一个偏好排序。同时，∀w<sub>j</sub>∈W,w<sub>j</sub>对于M也有一个偏好排序。
如果存在一种完美匹配，使得:(以下两者符合其一即可）
     ∀(m<sub>i</sub>,w<sub>j</sub>)∈S,对于m<sub>i</sub>来说，w<sub>j</sub>>(∀w<sub>k</sub>∈W - w<sub>j</sub>)
或者：
     ∀(m<sub>i</sub>,w<sub>j</sub>)∈S,对于w<sub>j</sub>来说，m<sub>i</sub>>(∀m<sub>k</sub>∈M - m<sub>i</sub>)
也就是说，无论对于m<sub>i</sub>还是w<sub>j</sub>来说，只要其中的一个得到了最偏好的那一个即可。

### G-S算法的伪代码

>∀m<sub>i</sub>∈M,∀w<sub>i</sub>∈W,m<sub>i</sub>和w<sub>i</sub>都是自由的。
While ∃m<sub>j</sub>∈M,且m<sub>j</sub>是自由的,∃w<sub>k</sub>∈W,m<sub>j</sub>还未向w<sub>k</sub>求过婚:
     选择这样的一个男人m<sub>j</sub>
     令w<sub>k</sub>是m<sub>j</sub>的优先表中还未求过婚的排名最高的女人
     If w<sub>k</sub>是自由的 then
          (m<sub>j</sub>,w<sub>k</sub>)变成约会状态
      Else  w<sub>k</sub>当前与m<sub>t</sub>约会:
>            If  w<sub>k</sub>更加偏爱m<sub>t</sub>而不爱m<sub>j</sub>  Then
                  m<sub>j</sub>  保持自由 
>            Else w<sub>k</sub>更加偏爱m<sub>j</sub>而不爱m<sub>t</sub>
                  (m<sub>j</sub>,w<sub>k</sub>)变成约会状态
                  m<sub>t</sub>  变为自由状态
          Endif
     Endif
EndWhile
### G-S算法的特点
>w选择的约会对象会越来越好。
>m选择的约会对象会越来越差。

 当w处于约会状态下，遇到一个更好的求婚对象时，她会选择抛弃原有的约会对象，从而选择更好的。因此，w的约会对象会越来越好，m的约会对象会越来越差。

>G-S算法在至多n<sup>2</sup>次While循环后终止。

>如果m是自由的，那么至少存在未被他求婚的女人。

>循环结束时返回的集合S是一个完美匹配。(反证法）

>G-S算法执行结束返回的集合S是一个稳定匹配。
证明：（反证法）

![](http://upload-images.jianshu.io/upload_images/3732745-7047a3bf71a73232.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![](http://upload-images.jianshu.io/upload_images/3732745-180ac9904885b533.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
>G-S算法的每次执行都得到同一个集合S。(即结果不变）

>在稳定匹配中S中，每个女人与她最差的有效伴侣配对。