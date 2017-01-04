---
title: Android时间、日期相关类和方法
date: 2017-01-04 14:31:36
categories: Android
tags: Android
---
# Android时间、日期相关类和方法 #
Android 系统有三种获取日期的方式：分别是Time、date、Calendar
## Time ##

	 android.text.format.Time time = new Time("GMT+8");
     time.setToNow();
     year = time.year;
     month = time.month;
     day = time.monthDay;
     minute = time.minute;
     hour = time.hour;
     sec = time.second;

## Date ##
	SimpleDateFormat formatter = new SimpleDateFormat("yyyy年MM月dd日 HH:mm:ss ");
	Date curDate = new Date(System.currentTimeMillis());//获取当前时间
	dateTime = formatter.format(curDate);

## Calendar ##

	Calendar calendar = Calendar.getInstance();
	Clenderyear = calendar.get(Calendar.YEAR);
	Clendermonth = calendar.get(Calendar.MONTH);
	Clenderday = calendar.get(Calendar.DAY_OF_MONTH);

## 获取当前系统的时间 ##

	System.currentTimeMillis()

注意：Canlendar 和Date 的月份是0~11，要比实际的月份少1。