---
title: spider
date: 2017-10-23 18:10:04
tags: [spider,python,js]
copyright: true
categories: "千里码"
---
[千里码](http://www.qlcoder.com/home) 
[豆瓣评分爬取](http://www.qlcoder.com/task/7560)
[豆瓣电影Top250](https://movie.douban.com/top250)收录了至今为止，大家最喜欢的250部电影。
该列表呈现了每部电影的评分，年份等基本信息。
这题的答案很简单，就是这个榜单的前166部电影的评分总和。
# 千里码：
### 豆瓣评分爬取
```#!/usr/bin/env python
# -*- coding: UTF-8 -*-
import urllib2
import requests , re
import sys , MySQLdb
import random

reload ( sys )
sys.setdefaultencoding ( 'utf-8' )
Type = sys.getfilesystemencoding ()
# 数据库设置
MYSQL_HOST = 'localhost'
MYSQL_DBNAME = 'ip'
MYSQL_USER = 'mark'
MYSQL_PASSWD = '****'
MYSQL_PORT = 3306

# 此处修改伪造的头字段,
headers = {
    'User-Agent': 'Mozilla/5.0 (Windows NT 6.1) AppleWebKit/537.11 (KHTML, like Gecko) Chrome/23.0.1271.64 Safari/537.11'}

#获取代理
def getProxy ( conn , cur , columns_data ):
    flag = random.choice ( columns_data )
    sql = "select proxy_ip,proxy_port from proxy where id =%d" % (flag)
    cur.execute ( sql )
    proxy = {}
    for each in cur.fetchall ():
        proxy[ 'http' ] = "http://%s:%s" % (each[ 0 ] , each[ 1 ])
    try:
        requests.get ( 'https://movie.douban.com/top250' , proxies=proxy )
    except:
        print "proxy error"
        getProxy ( conn , cur , columns_data )
    else:
        print "proxy success"
        return proxy

#获取请求的HTML页面
def get_request ( url , headers , conn , cur , columns_data ):
    proxy = getProxy ( conn , cur , columns_data )
    proxy_s = urllib2.ProxyHandler ( proxy )
    opener = urllib2.build_opener ( proxy_s )
    urllib2.install_opener ( opener )
    req = urllib2.Request ( url , headers=headers )
    r = urllib2.urlopen ( req )
    return r.read ()


if __name__ == "__main__":
    conn = MySQLdb.connect ( host=MYSQL_HOST , user=MYSQL_USER , passwd=MYSQL_PASSWD , db=MYSQL_DBNAME ,
                             port=MYSQL_PORT , charset='utf8' )
    cur = conn.cursor ()

    columns = "select id from proxy"
    cur.execute ( columns )
    columns_data = [ ]
    for each in cur.fetchall ():
        columns_data.append ( each[ 0 ] )

    url_total = [ 'https://movie.douban.com/top250?start=%d&filter=' % (each * 25) for each in range ( 7 ) ]
    print url_total
    data = [ ]
    for each in url_total:
        print each
        html = get_request ( each , headers , conn , cur , columns_data )
        re_str = r'<span class="rating_num" property="v:average">(\S+)</span>'
        for ebch in re.findall ( re_str , html ):
            data.append ( float ( str ( ebch ) ) )
    print data
    data = sorted ( data )
    print sum ( data[ :166 ] )
```
# 上述代码中对应的代理IP地址的SQL文件
```
SET FOREIGN_KEY_CHECKS=0;

-- ----------------------------
-- Table structure for proxy
-- ----------------------------
DROP TABLE IF EXISTS `proxy`;
CREATE TABLE `proxy` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `proxy_ip` varchar(255) NOT NULL,
  `proxy_port` int(11) DEFAULT NULL,
  `proxy_country` varchar(255) DEFAULT NULL,
  `proxy_type` varchar(255) DEFAULT NULL,
  `addtime` varchar(255) DEFAULT NULL,
  `last_test_time` varchar(255) DEFAULT NULL,
  `proxy_status` varchar(255) DEFAULT NULL,
  `Remarks` varchar(255) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=351 DEFAULT CHARSET=utf8;

-- ----------------------------
-- Records of proxy
-- ----------------------------
INSERT INTO `proxy` VALUES ('126', '219.238.124.221', '3128', 'China', 'Transparent', 'Tue Sep 26 21:06:32 2017', '11 28', '1', 'ly');
INSERT INTO `proxy` VALUES ('127', '61.153.67.110', '9999', 'China', 'Elite', 'Tue Sep 26 21:06:32 2017', '11 26', '1', 'ly');
INSERT INTO `proxy` VALUES ('128', '47.93.63.47', '8888', 'China', 'Elite', 'Tue Sep 26 21:06:32 2017', '11 26', '1', 'ly');
INSERT INTO `proxy` VALUES ('129', '101.37.79.125', '3128', 'China', 'Transparent', 'Tue Sep 26 21:06:32 2017', '10 43', '1', 'ly');
INSERT INTO `proxy` VALUES ('130', '203.91.121.76', '3128', 'China', 'Transparent', 'Tue Sep 26 21:06:32 2017', '7 14', '1', 'ly');
INSERT INTO `proxy` VALUES ('131', '61.136.163.245', '3128', 'China', 'Transparent', 'Tue Sep 26 21:06:32 2017', '6 29', '1', 'ly');
INSERT INTO `proxy` VALUES ('132', '58.22.61.211', '3128', 'China', 'Transparent', 'Tue Sep 26 21:06:32 2017', '6 25', '1', 'ly');
INSERT INTO `proxy` VALUES ('133', '42.51.26.79', '3128', 'China', 'Transparent', 'Tue Sep 26 21:06:32 2017', '6 24', '1', 'ly');
INSERT INTO `proxy` VALUES ('134', '61.153.108.142', '80', 'China', 'Transparent', 'Tue Sep 26 21:06:32 2017', '6 2', '1', 'ly');
INSERT INTO `proxy` VALUES ('135', '110.53.202.169', '808', 'China', 'Transparent', 'Tue Sep 26 21:06:32 2017', '5 53', '1', 'ly');
INSERT INTO `proxy` VALUES ('136', '117.141.18.70', '65205', 'China', 'Elite', 'Tue Sep 26 21:06:32 2017', '5 45', '1', 'ly');
INSERT INTO `proxy` VALUES ('137', '122.226.183.145', '80', 'China', 'Transparent', 'Tue Sep 26 21:06:32 2017', '5 40', '1', 'ly');
INSERT INTO `proxy` VALUES ('138', '43.241.10.206', '8080', 'China', 'Elite', 'Tue Sep 26 21:06:32 2017', '5 40', '1', 'ly');
INSERT INTO `proxy` VALUES ('139', '123.118.34.110', '9000', 'China', 'Transparent', 'Tue Sep 26 21:06:32 2017', '5 26', '1', 'ly');
INSERT INTO `proxy` VALUES ('140', '124.238.235.135', '81', 'China', 'Elite', 'Tue Sep 26 21:06:32 2017', '5 11', '1', 'ly');
INSERT INTO `proxy` VALUES ('141', '222.185.137.204', '808', 'China', 'Elite', 'Tue Sep 26 21:06:32 2017', '5 1', '1', 'ly');
INSERT INTO `proxy` VALUES ('142', '101.4.136.34', '80', 'China', 'Elite', 'Tue Sep 26 21:06:32 2017', '4 59', '1', 'ly');
INSERT INTO `proxy` VALUES ('143', '122.72.99.104', '80', 'China', 'Transparent', 'Tue Sep 26 21:06:32 2017', '4 55', '1', 'ly');
INSERT INTO `proxy` VALUES ('144', '114.215.103.121', '8081', 'China', 'Elite', 'Tue Sep 26 21:06:32 2017', '4 52', '1', 'ly');
INSERT INTO `proxy` VALUES ('145', '159.226.249.93', '8080', 'China', 'Transparent', 'Tue Sep 26 21:06:32 2017', '4 48', '1', 'ly');
INSERT INTO `proxy` VALUES ('146', '106.14.51.145', '8118', 'China', 'Elite', 'Tue Sep 26 21:06:32 2017', '4 41', '1', 'ly');
INSERT INTO `proxy` VALUES ('147', '122.72.18.35', '80', 'China', 'Transparent', 'Tue Sep 26 21:06:32 2017', '4 39', '1', 'ly');
INSERT INTO `proxy` VALUES ('148', '123.7.82.20', '3128', 'China', 'Transparent', 'Tue Sep 26 21:06:32 2017', '3 35', '1', 'ly');
INSERT INTO `proxy` VALUES ('149', '122.228.179.178', '80', 'China', 'Elite', 'Tue Sep 26 21:06:32 2017', '3 28', '1', 'ly');
INSERT INTO `proxy` VALUES ('150', '123.147.165.144', '8080', 'China', 'Transparent', 'Tue Sep 26 21:06:32 2017', '2 58', '1', 'ly');
INSERT INTO `proxy` VALUES ('251', '203.88.210.121', '138', 'China', 'Elite', '17-09-26 21:04', '200天', '1', 'ly');
INSERT INTO `proxy` VALUES ('252', '117.84.205.140', '8118', 'China', 'Elite', '17-09-26 21:02', '1天', '1', 'ly');
INSERT INTO `proxy` VALUES ('253', '122.72.32.74', '80', 'China', 'Elite', '17-09-26 21:02', '335天', '1', 'ly');
INSERT INTO `proxy` VALUES ('254', '49.77.210.116', '27475', 'China', 'Elite', '17-09-26 21:01', '1分钟', '1', 'ly');
INSERT INTO `proxy` VALUES ('255', '121.204.165.35', '8118', 'China', 'Elite', '17-09-26 21:01', '265天', '1', 'ly');
INSERT INTO `proxy` VALUES ('256', '49.64.103.196', '44074', 'China', 'Elite', '17-09-26 21:01', '1分钟', '1', 'ly');
INSERT INTO `proxy` VALUES ('257', '60.169.220.213', '28481', 'China', 'Elite', '17-09-26 21:01', '1分钟', '1', 'ly');
INSERT INTO `proxy` VALUES ('258', '61.135.217.7', '80', 'China', 'Elite', '17-09-26 21:01', '501天', '1', 'ly');
INSERT INTO `proxy` VALUES ('259', '123.160.26.198', '32721', 'China', 'Elite', '17-09-26 21:00', '1分钟', '1', 'ly');
INSERT INTO `proxy` VALUES ('260', '123.162.201.88', '39428', 'China', 'Elite', '17-09-26 21:00', '1分钟', '1', 'ly');
INSERT INTO `proxy` VALUES ('261', '140.255.255.169', '8118', 'China', 'Elite', '17-09-26 20:55', '15小时', '1', 'ly');
INSERT INTO `proxy` VALUES ('262', '171.39.29.221', '8123', 'China', 'Elite', '17-09-26 20:55', '62天', '1', 'ly');
INSERT INTO `proxy` VALUES ('263', '27.159.125.151', '8118', 'China', 'Elite', '17-09-26 20:53', '2小时', '1', 'ly');
INSERT INTO `proxy` VALUES ('264', '60.255.186.169', '8888', 'China', 'Elite', '17-09-26 20:52', '43天', '1', 'ly');
INSERT INTO `proxy` VALUES ('265', '36.251.248.76', '80', 'China', 'Elite', '17-09-26 20:51', '20天', '1', 'ly');
INSERT INTO `proxy` VALUES ('266', '121.12.42.181', '61234', 'China', 'Elite', '17-09-26 20:50', '25天', '1', 'ly');
INSERT INTO `proxy` VALUES ('267', '120.78.15.63', '80', 'China', 'Elite', '17-09-26 20:46', '9天', '1', 'ly');
INSERT INTO `proxy` VALUES ('268', '121.12.42.10', '61234', 'China', 'Elite', '17-09-26 20:45', '19天', '1', 'ly');
INSERT INTO `proxy` VALUES ('269', '115.218.83.192', '80', 'China', 'Elite', '17-09-26 20:44', '1天', '1', 'ly');
INSERT INTO `proxy` VALUES ('270', '49.81.250.174', '8118', 'China', 'Elite', '17-09-26 20:43', '14小时', '1', 'ly');
INSERT INTO `proxy` VALUES ('271', '36.102.62.45', '80', 'China', 'Elite', '17-09-26 20:36', '6天', '1', 'ly');
INSERT INTO `proxy` VALUES ('272', '101.68.73.54', '53281', 'China', 'Elite', '17-09-26 20:33', '41天', '1', 'ly');
INSERT INTO `proxy` VALUES ('273', '180.118.243.174', '61234', 'China', 'Elite', '17-09-26 20:33', '1分钟', '1', 'ly');
INSERT INTO `proxy` VALUES ('274', '117.69.2.252', '20037', 'China', 'Elite', '17-09-26 20:33', '1分钟', '1', 'ly');
INSERT INTO `proxy` VALUES ('275', '1.195.10.101', '34401', 'China', 'Elite', '17-09-26 20:33', '1分钟', '1', 'ly');
INSERT INTO `proxy` VALUES ('276', '110.73.15.7', '8123', 'China', 'Elite', '17-09-26 20:33', '16天', '1', 'ly');
INSERT INTO `proxy` VALUES ('277', '222.163.214.90', '8118', 'China', 'Elite', '17-09-26 20:30', '18小时', '1', 'ly');
INSERT INTO `proxy` VALUES ('278', '113.120.132.122', '8118', 'China', 'Elite', '17-09-26 20:23', '1天', '1', 'ly');
INSERT INTO `proxy` VALUES ('279', '110.73.50.141', '8123', 'China', 'Elite', '17-09-26 20:22', '552天', '1', 'ly');
INSERT INTO `proxy` VALUES ('280', '110.73.42.169', '8123', 'China', 'Elite', '17-09-26 20:15', '21天', '1', 'ly');
INSERT INTO `proxy` VALUES ('281', '120.42.125.1', '28771', 'China', 'Elite', '17-09-26 20:11', '1分钟', '1', 'ly');
INSERT INTO `proxy` VALUES ('282', '115.217.254.189', '30134', 'China', 'Elite', '17-09-26 20:11', '1分钟', '1', 'ly');
INSERT INTO `proxy` VALUES ('283', '110.182.50.43', '8118', 'China', 'Elite', '17-09-26 20:04', '2天', '1', 'ly');
INSERT INTO `proxy` VALUES ('284', '111.155.116.245', '8123', 'China', 'Elite', '17-09-26 20:04', '212天', '1', 'ly');
INSERT INTO `proxy` VALUES ('285', '122.72.32.88', '80', 'China', 'Elite', '17-09-26 20:01', '335天', '1', 'ly');
INSERT INTO `proxy` VALUES ('286', '180.122.155.128', '33103', 'China', 'Elite', '17-09-26 20:00', '1分钟', '1', 'ly');
INSERT INTO `proxy` VALUES ('287', '144.12.30.69', '4382', 'China', 'Elite', '17-09-26 20:00', '1天', '1', 'ly');
INSERT INTO `proxy` VALUES ('288', '122.242.94.63', '38006', 'China', 'Elite', '17-09-26 20:00', '1分钟', '1', 'ly');
INSERT INTO `proxy` VALUES ('289', '180.120.200.240', '43902', 'China', 'Elite', '17-09-26 20:00', '1分钟', '1', 'ly');
INSERT INTO `proxy` VALUES ('290', '1.194.118.164', '22850', 'China', 'Elite', '17-09-26 20:00', '1分钟', '1', 'ly');
INSERT INTO `proxy` VALUES ('291', '110.73.52.34', '8123', 'China', 'Elite', '17-09-26 19:55', '705天', '1', 'ly');
INSERT INTO `proxy` VALUES ('292', '222.141.15.186', '8118', 'China', 'Elite', '17-09-26 19:50', '12小时', '1', 'ly');
INSERT INTO `proxy` VALUES ('293', '123.161.157.78', '43588', 'China', 'Elite', '17-09-26 19:33', '2分钟', '1', 'ly');
INSERT INTO `proxy` VALUES ('294', '113.121.250.165', '23785', 'China', 'Elite', '17-09-26 19:31', '1分钟', '1', 'ly');
INSERT INTO `proxy` VALUES ('295', '111.155.116.215', '8123', 'China', 'Elite', '17-09-26 19:30', '168天', '1', 'ly');
INSERT INTO `proxy` VALUES ('296', '113.124.92.36', '25869', 'China', 'Elite', '17-09-26 19:30', '1分钟', '1', 'ly');
INSERT INTO `proxy` VALUES ('297', '113.107.161.44', '808', 'China', 'Elite', '17-09-26 19:22', '1分钟', '1', 'ly');
INSERT INTO `proxy` VALUES ('298', '117.78.37.198', '8000', 'China', 'Elite', '17-09-26 19:21', '56天', '1', 'ly');
INSERT INTO `proxy` VALUES ('299', '180.172.217.173', '34599', 'China', 'Elite', '17-09-26 19:00', '1分钟', '1', 'ly');
INSERT INTO `proxy` VALUES ('300', '42.231.1.1', '80', 'China', 'Elite', '17-09-26 19:00', '1分钟', '1', 'ly');
INSERT INTO `proxy` VALUES ('301', '115.221.127.232', '41445', 'China', 'Elite', '17-09-26 19:00', '1分钟', '1', 'ly');
INSERT INTO `proxy` VALUES ('302', '115.221.127.232', '41445', 'China', 'Elite', '17-09-26 19:00', '1分钟', '1', 'ly');
INSERT INTO `proxy` VALUES ('303', '121.31.100.194', '8123', 'China', 'Elite', '17-09-26 18:55', '1分钟', '1', 'ly');
INSERT INTO `proxy` VALUES ('304', '49.73.100.36', '8118', 'China', 'Elite', '17-09-26 18:51', '10小时', '1', 'ly');
INSERT INTO `proxy` VALUES ('305', '121.31.103.233', '8123', 'China', 'Elite', '17-09-26 18:33', '375天', '1', 'ly');
INSERT INTO `proxy` VALUES ('306', '183.63.101.62', '53281', 'China', 'Elite', '17-09-26 18:33', '40天', '1', 'ly');
INSERT INTO `proxy` VALUES ('307', '123.55.191.158', '808', 'China', 'Elite', '17-09-26 18:30', '80天', '1', 'ly');
INSERT INTO `proxy` VALUES ('308', '113.128.27.212', '29016', 'China', 'Elite', '17-09-26 18:30', '1分钟', '1', 'ly');
INSERT INTO `proxy` VALUES ('309', '60.24.153.214', '8118', 'China', 'Elite', '17-09-26 18:27', '10小时', '1', 'ly');
INSERT INTO `proxy` VALUES ('310', '113.249.159.135', '8118', 'China', 'Elite', '17-09-26 18:25', '3小时', '1', 'ly');
INSERT INTO `proxy` VALUES ('311', '27.184.124.175', '8118', 'China', 'Elite', '17-09-26 18:15', '7小时', '1', 'ly');
INSERT INTO `proxy` VALUES ('312', '114.95.184.20', '65205', 'China', 'Elite', '17-09-26 18:12', '4小时', '1', 'ly');
INSERT INTO `proxy` VALUES ('313', '183.63.223.2', '63000', 'China', 'Elite', '17-09-26 17:55', '294天', '1', 'ly');
INSERT INTO `proxy` VALUES ('314', '115.46.122.35', '8123', 'China', 'Elite', '17-09-26 17:44', '1分钟', '1', 'ly');
INSERT INTO `proxy` VALUES ('315', '110.182.239.81', '53281', 'China', 'Elite', '17-09-26 17:33', '6天', '1', 'ly');
INSERT INTO `proxy` VALUES ('316', '110.72.218.113', '8123', 'China', 'Elite', '17-09-26 17:33', '516天', '1', 'ly');
INSERT INTO `proxy` VALUES ('317', '121.204.165.172', '8118', 'China', 'Elite', '17-09-26 17:30', '282天', '1', 'ly');
INSERT INTO `proxy` VALUES ('318', '114.89.157.106', '8118', 'China', 'Elite', '17-09-26 17:30', '11小时', '1', 'ly');
INSERT INTO `proxy` VALUES ('319', '125.78.104.243', '37656', 'China', 'Elite', '17-09-26 17:30', '1分钟', '1', 'ly');
INSERT INTO `proxy` VALUES ('320', '171.13.37.235', '808', 'China', 'Elite', '17-09-26 17:30', '159天', '1', 'ly');
INSERT INTO `proxy` VALUES ('321', '117.25.190.25', '808', 'China', 'Elite', '17-09-26 17:30', '112天', '1', 'ly');
INSERT INTO `proxy` VALUES ('322', '180.122.154.118', '22512', 'China', 'Elite', '17-09-26 17:30', '1分钟', '1', 'ly');
INSERT INTO `proxy` VALUES ('323', '121.12.42.50', '61234', 'China', 'Elite', '17-09-26 17:27', '6天', '1', 'ly');
INSERT INTO `proxy` VALUES ('324', '115.211.59.117', '8118', 'China', 'Elite', '17-09-26 17:24', '4小时', '1', 'ly');
INSERT INTO `proxy` VALUES ('325', '121.31.195.82', '8123', 'China', 'Elite', '17-09-26 17:22', '1分钟', '1', 'ly');
INSERT INTO `proxy` VALUES ('326', '180.127.149.107', '808', 'China', 'Elite', '17-09-26 17:15', '4小时', '1', 'ly');
INSERT INTO `proxy` VALUES ('327', '49.77.210.67', '39862', 'China', 'Elite', '17-09-26 17:11', '1分钟', '1', 'ly');
INSERT INTO `proxy` VALUES ('328', '60.182.236.230', '37150', 'China', 'Elite', '17-09-26 17:11', '1分钟', '1', 'ly');
INSERT INTO `proxy` VALUES ('329', '121.31.102.45', '8123', 'China', 'Elite', '17-09-26 17:11', '207天', '1', 'ly');
INSERT INTO `proxy` VALUES ('330', '60.23.40.20', '80', 'China', 'Elite', '17-09-26 17:00', '3天', '1', 'ly');
INSERT INTO `proxy` VALUES ('331', '115.63.86.209', '80', 'China', 'Elite', '17-09-26 16:44', '1分钟', '1', 'ly');
INSERT INTO `proxy` VALUES ('332', '1.63.107.198', '80', 'China', 'Elite', '17-09-26 16:41', '2天', '1', 'ly');
INSERT INTO `proxy` VALUES ('333', '220.166.243.246', '8118', 'China', 'Elite', '17-09-26 16:40', '4天', '1', 'ly');
INSERT INTO `proxy` VALUES ('334', '119.5.177.133', '80', 'China', 'Elite', '17-09-26 16:22', '52天', '1', 'ly');
INSERT INTO `proxy` VALUES ('335', '119.5.177.155', '4386', 'China', 'Elite', '17-09-26 16:22', '1分钟', '1', 'ly');
INSERT INTO `proxy` VALUES ('336', '117.95.105.151', '36355', 'China', 'Elite', '17-09-26 16:01', '1分钟', '1', 'ly');
INSERT INTO `proxy` VALUES ('337', '59.40.51.253', '8010', 'China', 'Elite', '17-09-26 15:44', '16天', '1', 'ly');
INSERT INTO `proxy` VALUES ('338', '140.250.158.95', '40601', 'China', 'Elite', '17-09-26 15:33', '2分钟', '1', 'ly');
INSERT INTO `proxy` VALUES ('339', '49.88.168.166', '46914', 'China', 'Elite', '17-09-26 15:30', '1分钟', '1', 'ly');
INSERT INTO `proxy` VALUES ('340', '123.55.3.169', '24281', 'China', 'Elite', '17-09-26 15:30', '1分钟', '1', 'ly');
INSERT INTO `proxy` VALUES ('341', '36.25.27.202', '25065', 'China', 'Elite', '17-09-26 15:30', '1分钟', '1', 'ly');
INSERT INTO `proxy` VALUES ('342', '110.87.4.118', '27023', 'China', 'Elite', '17-09-26 15:22', '1分钟', '1', 'ly');
INSERT INTO `proxy` VALUES ('343', '221.205.61.105', '80', 'China', 'Elite', '17-09-26 15:22', '6小时', '1', 'ly');
INSERT INTO `proxy` VALUES ('344', '49.87.177.104', '32659', 'China', 'Elite', '17-09-26 15:11', '1分钟', '1', 'ly');
INSERT INTO `proxy` VALUES ('345', '59.49.129.60', '8998', 'China', 'Elite', '17-09-26 15:09', '219天', '1', 'ly');
INSERT INTO `proxy` VALUES ('346', '218.5.161.215', '35216', 'China', 'Elite', '17-09-26 15:00', '1分钟', '1', 'ly');
INSERT INTO `proxy` VALUES ('347', '27.40.143.209', '61234', 'China', 'Elite', '17-09-26 14:56', '6小时', '1', 'ly');
INSERT INTO `proxy` VALUES ('348', '110.73.8.214', '8123', 'China', 'Elite', '17-09-26 14:45', '208天', '1', 'ly');
INSERT INTO `proxy` VALUES ('349', '180.118.240.167', '61234', 'China', 'Elite', '17-09-26 14:34', '2小时', '1', 'ly');
INSERT INTO `proxy` VALUES ('350', '61.142.240.132', '808', 'China', 'Elite', '17-09-26 14:34', '102天', '1', 'ly');

```