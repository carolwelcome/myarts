---
title: emailJs使用
date: 2019-04-18 15:38:22
categories:  #分类
    - attendance
tags:   #标签
    - ARTS
    - emailJs 
description: 
    emailJs-无需部署服务器，使用简单的JavaScript API 发送邮件。太方便了
---

emailJs的注册什么，帮不了你，只能自己去注册。这里直接上代码



```Java
<!DOCTYPE html>
<html>
<head>
    <title>Contact Form</title>
    <script type="text/javascript" src="https://cdn.emailjs.com/sdk/2.3.2/email.min.js"></script>
    <script type="text/javascript">
        (function(){
           emailjs.init("user_wiSKXtFObSd0yPdeiV0sQ");
        })();
    </script>
    <script type="text/javascript">
        window.onload = function() {
            document.getElementById('contact-form').addEventListener('submit', function(event) {
                event.preventDefault();
				let date = new Date();
				var template_params = {
				   "reply_to": "158642752@qq.com,lihaiming_2012@163.com",
				   "mail_date": date.getFullYear()+'-'+(date.getMonth()+1)+'-'+date.getDate()+" "+date.getHours()+":" +date.getMinutes()+":"+date.getSeconds(),
				   "rec_recom":this.rec_recom.value,
				   "rec_name":this.rec_name.value,
				   "rec_tel":this.rec_tel.value,
				   "rec_loc":this.rec_loc.value,
				   "rec_mail":this.rec_mail.value,
				   "rec_posts":this.rec_posts.value,
				   "message_html":this.message_html.value
				}

				var service_id = "default_service";
				var template_id = "jstech_recruit";
				emailjs.send(service_id, template_id, template_params)
				.then(function(response) {
					   console.log('SUCCESS!', response.status, response.text);
					}, function(error) {
					   console.log('FAILED...', error);
					});
				 		
            });
        }
    </script>
</head>
<body>
    <form id="contact-form">
        <label>姓名：</label>
        <input type="text" name="rec_name">
        <label>手机号：</label>
        <input type="number" name="rec_tel">
		<label>学历：</label>
        <input type="text" name="rec_loc">
		<label>邮箱：</label>
        <input type="email" name="rec_mail">
        <label>应聘岗位</label>
		<input type="text" name="rec_posts">
		<label>个人简介</label>
        <textarea name="message_html"></textarea>
		<label>推荐人信息</label>
		<input type="text" name="rec_recom">
		
        <input type="submit" value="Send">
    </form>
</body>
</html>

```