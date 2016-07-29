---
layout: post
title: "VB Series Communicate Delay Problems"
date: 2016-07-29
---

```
Public Function ConnectPort(Optional hPort As Integer = 0) As Boolean
      On Error Resume Next
      MSComm1.PortOpen = False
      If Err.Number <> 0 Then
         Err.Clear
      End If
      On Error GoTo 打开端口错误
      MSComm1.CommPort = hPort
      MSComm1.Settings = "9600,N,8,1"
      MSComm1.PortOpen = True
      MSComm1.RThreshold = 1
      MSComm1.SThreshold = 1
      MSComm1.InputMode = comInputModeBinary
      ReturnModelName = ReadModelName()
      If UCase(ReturnModelName) = "DRM-RICM0903A" Then
         ConnectPort = True
         m_Port = hPort
         m_State = 1
      Else
         ConnectPort = False
         m_Port = 0
         m_State = 0
      End If
      Exit Function
打开端口错误:
      RaiseEvent ErrorEvent(Err.Number, Err.Description)
      Err.Clear
      ConnectPort = False
End Function
Private Sub MSComm1_OnComm()
   Dim ReadBytes() As Byte
   If MSComm1.CommEvent = comEvReceive Then
      ReadBytes = MSComm1.Input
      ...
   ENd If
End Sub
```

>还有，你的上位机发送命令时，最好计算一下命令发送所需要的时间，发送完命令之后再发下一个命令。 

>如果你的上一个命令还没有发送完就开始发送下一个命令，很容易出现问题的。 

如果用的是9600Hz进行通讯，那么就是说 9600 bit/每秒 ，也就是 9600/8=1200 byte/秒 

如果你发送的是 20 个字节的数据，理论上所需要的时间是 1000毫秒/1200字节*20字节= 16.67 毫秒 

通常再预个十来二十毫秒出来再发送下一条信息，这样通讯会比较稳定一点。 

还有就是单片机上的程序也很重要，如果处理不好，也会跑死掉的，所以也千万要注意，最好在通讯时加校验码和包头 

包尾的数据包来通讯，安全可靠，网络数据也是这么个通讯法的，只是网络数据包格式多点MAC地址之类的东西，其实 

你看看TCP/IP协议来模仿数据包发送，即使是9600，也可以有不错的效果，何况还可以使用更高的频率。 

还有一点，弹片机上的晶振采用11.0592MHz的进行串口通讯比较好，特别是9600的，用11.0592MHz的最为合适。 

如果使用12.000MHz或24.000MHz的晶振，在理论上说，误差是比较大的，所以建议还是使用11.0592MHz的晶振来做9600的串口通讯。

### Reference

-[VB 串口接收延迟严重的问题](http://share.freesion.com/438288/)
