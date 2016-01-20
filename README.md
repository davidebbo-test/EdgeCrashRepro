# Simple repro case for Edge.js crash

To repro the crash:

- clone this repo
- open it in VS as a WebSite over the repo root
- Ctrl-F5 to run it (no need to debug). It should run fine nd display "Node.js welcomes Node.js welcomes .NET"
- Touch web.config to cause an AppDomain shutdown
- Refresh the page in the browser

Result: IIS process crashes with this AV:

```
(2c3c.1d10): Access violation - code c0000005 (first chance)
First chance exceptions are reported before any exception handling.
This exception may be expected and handled.
*** WARNING: Unable to verify checksum for D:\code\GitHub\azure-webjobs-sdk-script\src\WebJobs.Script.WebHost\bin\edge\x86\node.dll
*** ERROR: Symbol file could not be found.  Defaulted to export symbols for D:\code\GitHub\azure-webjobs-sdk-script\src\WebJobs.Script.WebHost\bin\edge\x86\node.dll - 
eax=00000000 ebx=2483e8c0 ecx=0eab7553 edx=0adb46ad esi=0ad3e394 edi=0ad3e390
eip=00000000 esp=2483e850 ebp=2483e8a0 iopl=0         nv up ei pl zr na pe nc
cs=0023  ss=002b  ds=002b  es=002b  fs=0053  gs=002b             efl=00010246
00000000 ??              ???
0:046> kb
ChildEBP RetAddr  Args to Child              
WARNING: Frame IP not in any known module. Following frames may be wrong.
2483e84c 0ea1d623 0e78acbf 0ecc5a44 00000070 0x0
2483e8a0 73211705 00000002 0ad3f348 00000002 node!v8::Unlocker::~Unlocker+0x191633
2483e8d4 0dffa85f 00000002 0ad3f348 721ed588 clr!PInvokeStackImbalanceHelper+0x22 [f:\dd\ndp\clr\src\vm\i386\asmhelpers.asm @ 1829]
*** WARNING: Unable to verify checksum for image0c4c0000
*** ERROR: Module load completed but symbols could not be loaded for image0c4c0000
2483e984 0dffa757 00000000 00000000 00000000 image0c4c0000!DomainBoundILStubClass.IL_STUB_PInvoke(Int32, System.String[])+0xd7
2483e9e0 7210f1cc 00000000 00000000 00000000 image0c4c0000!EdgeJs.Edge.<Func>b__1()+0xcf
2483e9ec 72109ca4 00000000 00000000 00000000 mscorlib_ni!System.Threading.ThreadHelper.ThreadStart_Context(System.Object)+0x70 [f:\dd\ndp\clr\src\BCL\system\threading\thread.cs @ 74]
2483ea50 72109be6 00000000 00000000 00000000 mscorlib_ni!System.Threading.ExecutionContext.RunInternal(System.Threading.ExecutionContext, System.Threading.ContextCallback, System.Object, Boolean)+0xb4 [f:\dd\ndp\clr\src\BCL\system\threading\executioncontext.cs @ 954]
2483ea64 72109ba1 00000000 00000000 00000000 mscorlib_ni!System.Threading.ExecutionContext.Run(System.Threading.ExecutionContext, System.Threading.ContextCallback, System.Object, Boolean)+0x16 [f:\dd\ndp\clr\src\BCL\system\threading\executioncontext.cs @ 902]
2483ea80 7210f154 00000000 00000000 00000000 mscorlib_ni!System.Threading.ExecutionContext.Run(System.Threading.ExecutionContext, System.Threading.ContextCallback, System.Object)+0x41 [f:\dd\ndp\clr\src\BCL\system\threading\executioncontext.cs @ 891]
2483ea98 73211396 00000000 00000000 00000000 mscorlib_ni!System.Threading.ThreadHelper.ThreadStart()+0x44 [f:\dd\ndp\clr\src\BCL\system\threading\thread.cs @ 111]
```
