---
layout: post
title: Register/LOG IN
---

<!DOCTYPE html>
<html>
    <center>
<head>
<meta charset="utf-8">
<title>登录</title>
    <link href="testlogin.css" rel="stylesheet" type="text/css"/>
<style>
    #login {
    width: 290px;
    height: auto;
    overflow: hidden;
    border: solid 1px #CCCCCC;
}
#login_title {
    width: 100%;
    height: 40px;
    line-height: 40px;
    background-color: #F60;
    text-align: center;
}
.line {
    width: 250px;
    height: 30px;
    line-height: 30px;
    margin-left: 20px;
    text-align: center;
    font-family: 楷体;
}
.line input {
    width: 150px;
}
.line a {
    font-size: 14px;
    color: black;
}
.line span {
    color: #F00;
}
#log_submit {
    display: block;
    width: 200px;
    height: 30px;
    margin-left: 45px;
    margin-top: 15px;
    margin-bottom: 5px;
}
</style>
</head>


<body>
<form action="#" method="post">
  <div id="login">
    <div id="login_title">登&nbsp;录</div>

    <div class="line"><span id="msg"></span></div>
    <div class="line">账号&nbsp;&nbsp;
      <input name="username" type="text" placeholder="账号/手机/邮箱" />
    </div>
    <div class="line">密码&nbsp;&nbsp;
      <input name="password" type="password" placeholder="请输入密码" />
    </div>
    
    <input id="log_submit" type="button" value="登录" onclick="location.href='/'">
       <!--  注册按钮    -->
      <input id ="log_submit" type="button" value="创建新账号" onclick="location.href='/trying/register.html'">

    <div class="line"><a href="#">找回密码</a>&nbsp;&nbsp;&nbsp;&nbsp;</div>

  </div>
</form>
</body>
 </center>
</html>
