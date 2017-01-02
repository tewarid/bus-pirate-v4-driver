# How to create a self-signed driver for Bus Priate v4.0

These instructions explain how to self-sign Bus Pirate v4.0 driver to use on Windows 7/8. Windows 10 comes with built-in support for virtual serial ports - no driver installation is required.

## Instructions to self-sign

Requires Windows Driver Kit 7.1. All commands located under bin\amd64\ except if indicated otherwise. 

Execute:

makecert.exe -r -pe -sv BusPirateV4(Test).pvk -n CN=BusPirateV4(Test) BusPirateV4(Test).cer

pvk2pfx.exe -pvk BusPirateV4(Test).pvk -spc BusPirateV4(Test).cer -pfx BusPirateV4(Test).pfx

Inf2cat.exe /driver:. /os:7_x64,7_X86
(command located under bin\selfsign\)

Signtool.exe sign /v /f BusPirateV4(Test).pfx /n BusPirateV4(Test) /t http://timestamp.verisign.com/scripts/timstamp.dll mchpcdc.cat

certmgr.exe /add BusPirateV4(Test).cer /s /r localMachine root
(Need to run this with administrator privilege)


## Instructions to Install

Add BusPirateV4(Test).cer to local machine using certmgr.exe (last command above) or through Windows Explorer. In Device Manager, choose CDC Test under Other devices. Right click, choose Update Driver, choose to install driver manually from known location. Specify path to mchpcdc.inf file.
