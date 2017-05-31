---
title: Kernel
permalink: wiki/Kernel/
layout: wiki
---

The Xbox kernel is called xboxkrnl.exe. It is closely related to the
Windows NT [ntoskrnl.exe](/wiki/Wikipedia:Ntoskrnl.exe "wikilink"). Its image
base address is always 0x80010000.

Header modifications
--------------------

xboxkrnl.exe is a mostly standard exe file. However, the MS-DOS header
was patched to contain Xbox specific data in the reserved 20 byte block
starting at offset 40:

<table>
<thead>
<tr class="header">
<th><p>Offset</p></th>
<th><p>Meaning</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>40</p></td>
<td><p>Size of uninitialized portion of the .data section</p></td>
</tr>
<tr class="even">
<td><p>44</p></td>
<td><p>Size of initialized portion of the .data section</p></td>
</tr>
<tr class="odd">
<td><p>48</p></td>
<td><p>Memory address of initialized portion of the .data section (usually in <a href="Flash" class="uri" title="wikilink">Flash</a>).<br />
Used to re-initialize the data section pointed to by the next field.<br />
Note that the pointer might be invalid during normal execution as the Flash might not be mapped at all times.</p></td>
</tr>
<tr class="even">
<td><p>52</p></td>
<td><p>Memory address where the .data section is stored (usually the same as in the section header + image base).</p></td>
</tr>
</tbody>
</table>

Sections
--------

All sections are identity mapped (meaning file offsets and offsets in
RAM match). This is because the kernel is not loaded through a
traditional PE / exe loader, but just unpacked into memory.

### .text

The .text section contains the kernel exports.

### .data

The .data section stores initialized and uninitialized data. A copy of
the initialized portion of this section is usually stored in the
[BIOS](/wiki/BIOS "wikilink").

### STICKY

Stores variables which must be preserved across a quick-reboot.

### IDEXPRDT

A Physical Region Descriptor Table (PRDT) for the IDE bus. This section
serves as a memory allocation only, it does not have to be initialized
when loading the kernel.

### INIT

This section is always the last one. It contains the entrypoint of the
kernel. This does all the cold-boot kernel initialization as described
[here](/wiki/Boot_Process#Initialization "wikilink"). Later kernels will
discard this section after initialization. INIT also contains the [Boot
Animation](/wiki/Boot_Animation "wikilink"), so once the kernel has finished
booting it can't do a full hardware re-initialization or play the boot
animation anymore.

Kernel exports
--------------

| Name                                                                                                | Ordinal | Calling Convention | Notes         |
|-----------------------------------------------------------------------------------------------------|---------|--------------------|---------------|
| [Kernel/AvGetSavedDataAddress](/wiki/Kernel/AvGetSavedDataAddress "wikilink")                             | 1       | stdcall            |               |
| [Kernel/AvSendTVEncoderOption](/wiki/Kernel/AvSendTVEncoderOption "wikilink")                             | 2       | stdcall            |               |
| [Kernel/AvSetDisplayMode](/wiki/Kernel/AvSetDisplayMode "wikilink")                                       | 3       | stdcall            |               |
| [Kernel/AvSetSavedDataAddress](/wiki/Kernel/AvSetSavedDataAddress "wikilink")                             | 4       | stdcall            |               |
| [Kernel/DbgBreakPoint](/wiki/Kernel/DbgBreakPoint "wikilink")                                             | 5       | stdcall            |               |
| [Kernel/DbgBreakPointWithStatus](/wiki/Kernel/DbgBreakPointWithStatus "wikilink")                         | 6       | stdcall            |               |
| [Kernel/DbgLoadImageSymbols](/wiki/Kernel/DbgLoadImageSymbols "wikilink")                                 | 7       | stdcall            | Devkits only! |
| [Kernel/DbgPrint](/wiki/Kernel/DbgPrint "wikilink")                                                       | 8       | stdcall            |               |
| [Kernel/HalReadSMCTrayState](/wiki/Kernel/HalReadSMCTrayState "wikilink")                                 | 9       | stdcall            |               |
| [Kernel/DbgPrompt](/wiki/Kernel/DbgPrompt "wikilink")                                                     | 10      | stdcall            |               |
| [Kernel/DbgUnLoadImageSymbols](/wiki/Kernel/DbgUnLoadImageSymbols "wikilink")                             | 11      | stdcall            | Devkits only! |
| [Kernel/ExAcquireReadWriteLockExclusive](/wiki/Kernel/ExAcquireReadWriteLockExclusive "wikilink")         | 12      | stdcall            |               |
| [Kernel/ExAcquireReadWriteLockShared](/wiki/Kernel/ExAcquireReadWriteLockShared "wikilink")               | 13      | stdcall            |               |
| [Kernel/ExAllocatePool](/wiki/Kernel/ExAllocatePool "wikilink")                                           | 14      | stdcall            |               |
| [Kernel/ExAllocatePoolWithTag](/wiki/Kernel/ExAllocatePoolWithTag "wikilink")                             | 15      | stdcall            |               |
| [Kernel/ExEventObjectType](/wiki/Kernel/ExEventObjectType "wikilink")                                     | 16      | stdcall            | Variable?     |
| [Kernel/ExFreePool](/wiki/Kernel/ExFreePool "wikilink")                                                   | 17      | stdcall            |               |
| [Kernel/ExInitializeReadWriteLock](/wiki/Kernel/ExInitializeReadWriteLock "wikilink")                     | 18      | stdcall            |               |
| [Kernel/ExInterlockedAddLargeInteger](/wiki/Kernel/ExInterlockedAddLargeInteger "wikilink")               | 19      | stdcall            |               |
| [Kernel/ExInterlockedAddLargeStatistic](/wiki/Kernel/ExInterlockedAddLargeStatistic "wikilink")           | 20      | fastcall           |               |
| [Kernel/ExInterlockedCompareExchange64](/wiki/Kernel/ExInterlockedCompareExchange64 "wikilink")           | 21      | fastcall           |               |
| [Kernel/ExMutantObjectType](/wiki/Kernel/ExMutantObjectType "wikilink")                                   | 22      | stdcall            | Variable?     |
| [Kernel/ExQueryPoolBlockSize](/wiki/Kernel/ExQueryPoolBlockSize "wikilink")                               | 23      | stdcall            |               |
| [Kernel/ExQueryNonVolatileSetting](/wiki/Kernel/ExQueryNonVolatileSetting "wikilink")                     | 24      | stdcall            |               |
| [Kernel/ExReadWriteRefurbInfo](/wiki/Kernel/ExReadWriteRefurbInfo "wikilink")                             | 25      | stdcall            |               |
| [Kernel/ExRaiseException](/wiki/Kernel/ExRaiseException "wikilink")                                       | 26      | stdcall            |               |
| [Kernel/ExRaiseStatus](/wiki/Kernel/ExRaiseStatus "wikilink")                                             | 27      | stdcall            |               |
| [Kernel/ExReleaseReadWriteLock](/wiki/Kernel/ExReleaseReadWriteLock "wikilink")                           | 28      | stdcall            |               |
| [Kernel/ExSaveNonVolatileSetting](/wiki/Kernel/ExSaveNonVolatileSetting "wikilink")                       | 29      | stdcall            |               |
| [Kernel/ExSemaphoreObjectType](/wiki/Kernel/ExSemaphoreObjectType "wikilink")                             | 30      | stdcall            | Variable?     |
| [Kernel/ExTimerObjectType](/wiki/Kernel/ExTimerObjectType "wikilink")                                     | 31      | stdcall            | Variable?     |
| [Kernel/ExfInterlockedInsertHeadList](/wiki/Kernel/ExfInterlockedInsertHeadList "wikilink")               | 32      | stdcall            |               |
| [Kernel/ExfInterlockedInsertTailList](/wiki/Kernel/ExfInterlockedInsertTailList "wikilink")               | 33      | fastcall           |               |
| [Kernel/ExfInterlockedRemoveHeadList](/wiki/Kernel/ExfInterlockedRemoveHeadList "wikilink")               | 34      | fastcall           |               |
| [Kernel/FscGetCacheSize](/wiki/Kernel/FscGetCacheSize "wikilink")                                         | 35      | stdcall            |               |
| [Kernel/FscInvalidateIdleBlocks](/wiki/Kernel/FscInvalidateIdleBlocks "wikilink")                         | 36      | stdcall            |               |
| [Kernel/FscSetCacheSize](/wiki/Kernel/FscSetCacheSize "wikilink")                                         | 37      | stdcall            |               |
| [Kernel/HalClearSoftwareInterrupt](/wiki/Kernel/HalClearSoftwareInterrupt "wikilink")                     | 38      | fastcall           |               |
| [Kernel/HalDisableSystemInterrupt](/wiki/Kernel/HalDisableSystemInterrupt "wikilink")                     | 39      | stdcall            |               |
| [Kernel/HalDiskCachePartitionCount](/wiki/Kernel/HalDiskCachePartitionCount "wikilink")                   | 40      | stdcall            | Variable?     |
| [Kernel/HalDiskModelNumber](/wiki/Kernel/HalDiskModelNumber "wikilink")                                   | 41      | stdcall            | Variable?     |
| [Kernel/HalDiskSerialNumber](/wiki/Kernel/HalDiskSerialNumber "wikilink")                                 | 42      | stdcall            | Variable?     |
| [Kernel/HalEnableSystemInterrupt](/wiki/Kernel/HalEnableSystemInterrupt "wikilink")                       | 43      | stdcall            |               |
| [Kernel/HalGetInterruptVector](/wiki/Kernel/HalGetInterruptVector "wikilink")                             | 44      | stdcall            |               |
| [Kernel/HalReadSMBusValue](/wiki/Kernel/HalReadSMBusValue "wikilink")                                     | 45      | stdcall            |               |
| [Kernel/HalReadWritePCISpace](/wiki/Kernel/HalReadWritePCISpace "wikilink")                               | 46      | stdcall            |               |
| [Kernel/HalRegisterShutdownNotification](/wiki/Kernel/HalRegisterShutdownNotification "wikilink")         | 47      | stdcall            |               |
| [Kernel/HalRequestSoftwareInterrupt](/wiki/Kernel/HalRequestSoftwareInterrupt "wikilink")                 | 48      | fastcall           |               |
| [Kernel/HalReturnToFirmware](/wiki/Kernel/HalReturnToFirmware "wikilink")                                 | 49      | stdcall            |               |
| [Kernel/HalWriteSMBusValue](/wiki/Kernel/HalWriteSMBusValue "wikilink")                                   | 50      | stdcall            |               |
| [Kernel/InterlockedCompareExchange](/wiki/Kernel/InterlockedCompareExchange "wikilink")                   | 51      | fastcall           |               |
| [Kernel/InterlockedDecrement](/wiki/Kernel/InterlockedDecrement "wikilink")                               | 52      | fastcall           |               |
| [Kernel/InterlockedIncrement](/wiki/Kernel/InterlockedIncrement "wikilink")                               | 53      | fastcall           |               |
| [Kernel/InterlockedExchange](/wiki/Kernel/InterlockedExchange "wikilink")                                 | 54      | fastcall           |               |
| [Kernel/InterlockedExchangeAdd](/wiki/Kernel/InterlockedExchangeAdd "wikilink")                           | 55      | fastcall           |               |
| [Kernel/InterlockedFlushSList](/wiki/Kernel/InterlockedFlushSList "wikilink")                             | 56      | fastcall           |               |
| [Kernel/InterlockedPopEntrySList](/wiki/Kernel/InterlockedPopEntrySList "wikilink")                       | 57      | fastcall           |               |
| [Kernel/InterlockedPushEntrySList](/wiki/Kernel/InterlockedPushEntrySList "wikilink")                     | 58      | fastcall           |               |
| [Kernel/IoAllocateIrp](/wiki/Kernel/IoAllocateIrp "wikilink")                                             | 59      | stdcall            |               |
| [Kernel/IoBuildAsynchronousFsdRequest](/wiki/Kernel/IoBuildAsynchronousFsdRequest "wikilink")             | 60      | stdcall            |               |
| [Kernel/IoBuildDeviceIoControlRequest](/wiki/Kernel/IoBuildDeviceIoControlRequest "wikilink")             | 61      | stdcall            |               |
| [Kernel/IoBuildSynchronousFsdRequest](/wiki/Kernel/IoBuildSynchronousFsdRequest "wikilink")               | 62      | stdcall            |               |
| [Kernel/IoCheckShareAccess](/wiki/Kernel/IoCheckShareAccess "wikilink")                                   | 63      | stdcall            |               |
| [Kernel/IoCompletionObjectType](/wiki/Kernel/IoCompletionObjectType "wikilink")                           | 64      | stdcall            | Variable?     |
| [Kernel/IoCreateDevice](/wiki/Kernel/IoCreateDevice "wikilink")                                           | 65      | stdcall            |               |
| [Kernel/IoCreateFile](/wiki/Kernel/IoCreateFile "wikilink")                                               | 66      | stdcall            |               |
| [Kernel/IoCreateSymbolicLink](/wiki/Kernel/IoCreateSymbolicLink "wikilink")                               | 67      | stdcall            |               |
| [Kernel/IoDeleteDevice](/wiki/Kernel/IoDeleteDevice "wikilink")                                           | 68      | stdcall            |               |
| [Kernel/IoDeleteSymbolicLink](/wiki/Kernel/IoDeleteSymbolicLink "wikilink")                               | 69      | stdcall            |               |
| [Kernel/IoDeviceObjectType](/wiki/Kernel/IoDeviceObjectType "wikilink")                                   | 70      | stdcall            | Variable?     |
| [Kernel/IoFileObjectType](/wiki/Kernel/IoFileObjectType "wikilink")                                       | 71      | stdcall            | Variable?     |
| [Kernel/IoFreeIrp](/wiki/Kernel/IoFreeIrp "wikilink")                                                     | 72      | stdcall            |               |
| [Kernel/IoInitializeIrp](/wiki/Kernel/IoInitializeIrp "wikilink")                                         | 73      | stdcall            |               |
| [Kernel/IoInvalidDeviceRequest](/wiki/Kernel/IoInvalidDeviceRequest "wikilink")                           | 74      | stdcall            |               |
| [Kernel/IoQueryFileInformation](/wiki/Kernel/IoQueryFileInformation "wikilink")                           | 75      | stdcall            |               |
| [Kernel/IoQueryVolumeInformation](/wiki/Kernel/IoQueryVolumeInformation "wikilink")                       | 76      | stdcall            |               |
| [Kernel/IoQueueThreadIrp](/wiki/Kernel/IoQueueThreadIrp "wikilink")                                       | 77      | stdcall            |               |
| [Kernel/IoRemoveShareAccess](/wiki/Kernel/IoRemoveShareAccess "wikilink")                                 | 78      | stdcall            |               |
| [Kernel/IoSetIoCompletion](/wiki/Kernel/IoSetIoCompletion "wikilink")                                     | 79      | stdcall            |               |
| [Kernel/IoSetShareAccess](/wiki/Kernel/IoSetShareAccess "wikilink")                                       | 80      | stdcall            |               |
| [Kernel/IoStartNextPacket](/wiki/Kernel/IoStartNextPacket "wikilink")                                     | 81      | stdcall            |               |
| [Kernel/IoStartNextPacketByKey](/wiki/Kernel/IoStartNextPacketByKey "wikilink")                           | 82      | stdcall            |               |
| [Kernel/IoStartPacket](/wiki/Kernel/IoStartPacket "wikilink")                                             | 83      | stdcall            |               |
| [Kernel/IoSynchronousDeviceIoControlRequest](/wiki/Kernel/IoSynchronousDeviceIoControlRequest "wikilink") | 84      | stdcall            |               |
| [Kernel/IoSynchronousFsdRequest](/wiki/Kernel/IoSynchronousFsdRequest "wikilink")                         | 85      | stdcall            |               |
| [Kernel/IofCallDriver](/wiki/Kernel/IofCallDriver "wikilink")                                             | 86      | fastcall           |               |
| [Kernel/IofCompleteRequest](/wiki/Kernel/IofCompleteRequest "wikilink")                                   | 87      | fastcall           |               |
| [Kernel/KdDebuggerEnabled](/wiki/Kernel/KdDebuggerEnabled "wikilink")                                     | 88      | stdcall            | Variable?     |
| [Kernel/KdDebuggerNotPresent](/wiki/Kernel/KdDebuggerNotPresent "wikilink")                               | 89      | stdcall            | Variable?     |
| [Kernel/IoDismountVolume](/wiki/Kernel/IoDismountVolume "wikilink")                                       | 90      | stdcall            |               |
| [Kernel/IoDismountVolumeByName](/wiki/Kernel/IoDismountVolumeByName "wikilink")                           | 91      | stdcall            |               |
| [Kernel/KeAlertResumeThread](/wiki/Kernel/KeAlertResumeThread "wikilink")                                 | 92      | stdcall            |               |
| [Kernel/KeAlertThread](/wiki/Kernel/KeAlertThread "wikilink")                                             | 93      | stdcall            |               |
| [Kernel/KeBoostPriorityThread](/wiki/Kernel/KeBoostPriorityThread "wikilink")                             | 94      | stdcall            |               |
| [Kernel/KeBugCheck](/wiki/Kernel/KeBugCheck "wikilink")                                                   | 95      | stdcall            |               |
| [Kernel/KeBugCheckEx](/wiki/Kernel/KeBugCheckEx "wikilink")                                               | 96      | stdcall            |               |
| [Kernel/KeCancelTimer](/wiki/Kernel/KeCancelTimer "wikilink")                                             | 97      | stdcall            |               |
| [Kernel/KeConnectInterrupt](/wiki/Kernel/KeConnectInterrupt "wikilink")                                   | 98      | stdcall            |               |
| [Kernel/KeDelayExecutionThread](/wiki/Kernel/KeDelayExecutionThread "wikilink")                           | 99      | stdcall            |               |
| [Kernel/KeDisconnectInterrupt](/wiki/Kernel/KeDisconnectInterrupt "wikilink")                             | 100     | stdcall            |               |
| [Kernel/KeEnterCriticalRegion](/wiki/Kernel/KeEnterCriticalRegion "wikilink")                             | 101     | stdcall            |               |
| [Kernel/MmGlobalData](/wiki/Kernel/MmGlobalData "wikilink")                                               | 102     | stdcall            | Variable?     |
| [Kernel/KeGetCurrentIrql](/wiki/Kernel/KeGetCurrentIrql "wikilink")                                       | 103     | stdcall            |               |
| [Kernel/KeGetCurrentThread](/wiki/Kernel/KeGetCurrentThread "wikilink")                                   | 104     | stdcall            |               |
| [Kernel/KeInitializeApc](/wiki/Kernel/KeInitializeApc "wikilink")                                         | 105     | stdcall            |               |
| [Kernel/KeInitializeDeviceQueue](/wiki/Kernel/KeInitializeDeviceQueue "wikilink")                         | 106     | stdcall            |               |
| [Kernel/KeInitializeDpc](/wiki/Kernel/KeInitializeDpc "wikilink")                                         | 107     | stdcall            |               |
| [Kernel/KeInitializeEvent](/wiki/Kernel/KeInitializeEvent "wikilink")                                     | 108     | stdcall            |               |
| [Kernel/KeInitializeInterrupt](/wiki/Kernel/KeInitializeInterrupt "wikilink")                             | 109     | stdcall            |               |
| [Kernel/KeInitializeMutant](/wiki/Kernel/KeInitializeMutant "wikilink")                                   | 110     | stdcall            |               |
| [Kernel/KeInitializeQueue](/wiki/Kernel/KeInitializeQueue "wikilink")                                     | 111     | stdcall            |               |
| [Kernel/KeInitializeSemaphore](/wiki/Kernel/KeInitializeSemaphore "wikilink")                             | 112     | stdcall            |               |
| [Kernel/KeInitializeTimerEx](/wiki/Kernel/KeInitializeTimerEx "wikilink")                                 | 113     | stdcall            |               |
| [Kernel/KeInsertByKeyDeviceQueue](/wiki/Kernel/KeInsertByKeyDeviceQueue "wikilink")                       | 114     | stdcall            |               |
| [Kernel/KeInsertDeviceQueue](/wiki/Kernel/KeInsertDeviceQueue "wikilink")                                 | 115     | stdcall            |               |
| [Kernel/KeInsertHeadQueue](/wiki/Kernel/KeInsertHeadQueue "wikilink")                                     | 116     | stdcall            |               |
| [Kernel/KeInsertQueue](/wiki/Kernel/KeInsertQueue "wikilink")                                             | 117     | stdcall            |               |
| [Kernel/KeInsertQueueApc](/wiki/Kernel/KeInsertQueueApc "wikilink")                                       | 118     | stdcall            |               |
| [Kernel/KeInsertQueueDpc](/wiki/Kernel/KeInsertQueueDpc "wikilink")                                       | 119     | stdcall            |               |
| [Kernel/KeInterruptTime](/wiki/Kernel/KeInterruptTime "wikilink")                                         | 120     | stdcall            | Variable?     |
| [Kernel/KeIsExecutingDpc](/wiki/Kernel/KeIsExecutingDpc "wikilink")                                       | 121     | stdcall            |               |
| [Kernel/KeLeaveCriticalRegion](/wiki/Kernel/KeLeaveCriticalRegion "wikilink")                             | 122     | stdcall            |               |
| [Kernel/KePulseEvent](/wiki/Kernel/KePulseEvent "wikilink")                                               | 123     | stdcall            |               |
| [Kernel/KeQueryBasePriorityThread](/wiki/Kernel/KeQueryBasePriorityThread "wikilink")                     | 124     | stdcall            |               |
| [Kernel/KeQueryInterruptTime](/wiki/Kernel/KeQueryInterruptTime "wikilink")                               | 125     | stdcall            |               |
| [Kernel/KeQueryPerformanceCounter](/wiki/Kernel/KeQueryPerformanceCounter "wikilink")                     | 126     | stdcall            |               |
| [Kernel/KeQueryPerformanceFrequency](/wiki/Kernel/KeQueryPerformanceFrequency "wikilink")                 | 127     | stdcall            |               |
| [Kernel/KeQuerySystemTime](/wiki/Kernel/KeQuerySystemTime "wikilink")                                     | 128     | stdcall            |               |
| [Kernel/KeRaiseIrqlToDpcLevel](/wiki/Kernel/KeRaiseIrqlToDpcLevel "wikilink")                             | 129     | stdcall            |               |
| [Kernel/KeRaiseIrqlToSynchLevel](/wiki/Kernel/KeRaiseIrqlToSynchLevel "wikilink")                         | 130     | stdcall            |               |
| [Kernel/KeReleaseMutant](/wiki/Kernel/KeReleaseMutant "wikilink")                                         | 131     | stdcall            |               |
| [Kernel/KeReleaseSemaphore](/wiki/Kernel/KeReleaseSemaphore "wikilink")                                   | 132     | stdcall            |               |
| [Kernel/KeRemoveByKeyDeviceQueue](/wiki/Kernel/KeRemoveByKeyDeviceQueue "wikilink")                       | 133     | stdcall            |               |
| [Kernel/KeRemoveDeviceQueue](/wiki/Kernel/KeRemoveDeviceQueue "wikilink")                                 | 134     | stdcall            |               |
| [Kernel/KeRemoveEntryDeviceQueue](/wiki/Kernel/KeRemoveEntryDeviceQueue "wikilink")                       | 135     | stdcall            |               |
| [Kernel/KeRemoveQueue](/wiki/Kernel/KeRemoveQueue "wikilink")                                             | 136     | stdcall            |               |
| [Kernel/KeRemoveQueueDpc](/wiki/Kernel/KeRemoveQueueDpc "wikilink")                                       | 137     | stdcall            |               |
| [Kernel/KeResetEvent](/wiki/Kernel/KeResetEvent "wikilink")                                               | 138     | stdcall            |               |
| [Kernel/KeRestoreFloatingPointState](/wiki/Kernel/KeRestoreFloatingPointState "wikilink")                 | 139     | stdcall            |               |
| [Kernel/KeResumeThread](/wiki/Kernel/KeResumeThread "wikilink")                                           | 140     | stdcall            |               |
| [Kernel/KeRundownQueue](/wiki/Kernel/KeRundownQueue "wikilink")                                           | 141     | stdcall            |               |
| [Kernel/KeSaveFloatingPointState](/wiki/Kernel/KeSaveFloatingPointState "wikilink")                       | 142     | stdcall            |               |
| [Kernel/KeSetBasePriorityThread](/wiki/Kernel/KeSetBasePriorityThread "wikilink")                         | 143     | stdcall            |               |
| [Kernel/KeSetDisableBoostThread](/wiki/Kernel/KeSetDisableBoostThread "wikilink")                         | 144     | stdcall            |               |
| [Kernel/KeSetEvent](/wiki/Kernel/KeSetEvent "wikilink")                                                   | 145     | stdcall            |               |
| [Kernel/KeSetEventBoostPriority](/wiki/Kernel/KeSetEventBoostPriority "wikilink")                         | 146     | stdcall            |               |
| [Kernel/KeSetPriorityProcess](/wiki/Kernel/KeSetPriorityProcess "wikilink")                               | 147     | stdcall            |               |
| [Kernel/KeSetPriorityThread](/wiki/Kernel/KeSetPriorityThread "wikilink")                                 | 148     | stdcall            |               |
| [Kernel/KeSetTimer](/wiki/Kernel/KeSetTimer "wikilink")                                                   | 149     | stdcall            |               |
| [Kernel/KeSetTimerEx](/wiki/Kernel/KeSetTimerEx "wikilink")                                               | 150     | stdcall            |               |
| [Kernel/KeStallExecutionProcessor](/wiki/Kernel/KeStallExecutionProcessor "wikilink")                     | 151     | stdcall            |               |
| [Kernel/KeSuspendThread](/wiki/Kernel/KeSuspendThread "wikilink")                                         | 152     | stdcall            |               |
| [Kernel/KeSynchronizeExecution](/wiki/Kernel/KeSynchronizeExecution "wikilink")                           | 153     | stdcall            |               |
| [Kernel/KeSystemTime](/wiki/Kernel/KeSystemTime "wikilink")                                               | 154     | stdcall            | Variable?     |
| [Kernel/KeTestAlertThread](/wiki/Kernel/KeTestAlertThread "wikilink")                                     | 155     | stdcall            |               |
| [Kernel/KeTickCount](/wiki/Kernel/KeTickCount "wikilink")                                                 | 156     | stdcall            | Variable?     |
| [Kernel/KeTimeIncrement](/wiki/Kernel/KeTimeIncrement "wikilink")                                         | 157     | stdcall            | Variable?     |
| [Kernel/KeWaitForMultipleObjects](/wiki/Kernel/KeWaitForMultipleObjects "wikilink")                       | 158     | stdcall            |               |
| [Kernel/KeWaitForSingleObject](/wiki/Kernel/KeWaitForSingleObject "wikilink")                             | 159     | stdcall            |               |
| [Kernel/KfRaiseIrql](/wiki/Kernel/KfRaiseIrql "wikilink")                                                 | 160     | fastcall           |               |
| [Kernel/KfLowerIrql](/wiki/Kernel/KfLowerIrql "wikilink")                                                 | 161     | fastcall           |               |
| [Kernel/KiBugCheckData](/wiki/Kernel/KiBugCheckData "wikilink")                                           | 162     | stdcall            | Variable?     |
| [Kernel/KiUnlockDispatcherDatabase](/wiki/Kernel/KiUnlockDispatcherDatabase "wikilink")                   | 163     | fastcall           |               |
| [Kernel/LaunchDataPage](/wiki/Kernel/LaunchDataPage "wikilink")                                           | 164     | stdcall            | Variable?     |
| [Kernel/MmAllocateContiguousMemory](/wiki/Kernel/MmAllocateContiguousMemory "wikilink")                   | 165     | stdcall            |               |
| [Kernel/MmAllocateContiguousMemoryEx](/wiki/Kernel/MmAllocateContiguousMemoryEx "wikilink")               | 166     | stdcall            |               |
| [Kernel/MmAllocateSystemMemory](/wiki/Kernel/MmAllocateSystemMemory "wikilink")                           | 167     | stdcall            |               |
| [Kernel/MmClaimGpuInstanceMemory](/wiki/Kernel/MmClaimGpuInstanceMemory "wikilink")                       | 168     | stdcall            |               |
| [Kernel/MmCreateKernelStack](/wiki/Kernel/MmCreateKernelStack "wikilink")                                 | 169     | stdcall            |               |
| [Kernel/MmDeleteKernelStack](/wiki/Kernel/MmDeleteKernelStack "wikilink")                                 | 170     | stdcall            |               |
| [Kernel/MmFreeContiguousMemory](/wiki/Kernel/MmFreeContiguousMemory "wikilink")                           | 171     | stdcall            |               |
| [Kernel/MmFreeSystemMemory](/wiki/Kernel/MmFreeSystemMemory "wikilink")                                   | 172     | stdcall            |               |
| [Kernel/MmGetPhysicalAddress](/wiki/Kernel/MmGetPhysicalAddress "wikilink")                               | 173     | stdcall            |               |
| [Kernel/MmIsAddressValid](/wiki/Kernel/MmIsAddressValid "wikilink")                                       | 174     | stdcall            |               |
| [Kernel/MmLockUnlockBufferPages](/wiki/Kernel/MmLockUnlockBufferPages "wikilink")                         | 175     | stdcall            |               |
| [Kernel/MmLockUnlockPhysicalPage](/wiki/Kernel/MmLockUnlockPhysicalPage "wikilink")                       | 176     | stdcall            |               |
| [Kernel/MmMapIoSpace](/wiki/Kernel/MmMapIoSpace "wikilink")                                               | 177     | stdcall            |               |
| [Kernel/MmPersistContiguousMemory](/wiki/Kernel/MmPersistContiguousMemory "wikilink")                     | 178     | stdcall            |               |
| [Kernel/MmQueryAddressProtect](/wiki/Kernel/MmQueryAddressProtect "wikilink")                             | 179     | stdcall            |               |
| [Kernel/MmQueryAllocationSize](/wiki/Kernel/MmQueryAllocationSize "wikilink")                             | 180     | stdcall            |               |
| [Kernel/MmQueryStatistics](/wiki/Kernel/MmQueryStatistics "wikilink")                                     | 181     | stdcall            |               |
| [Kernel/MmSetAddressProtect](/wiki/Kernel/MmSetAddressProtect "wikilink")                                 | 182     | stdcall            |               |
| [Kernel/MmUnmapIoSpace](/wiki/Kernel/MmUnmapIoSpace "wikilink")                                           | 183     | stdcall            |               |
| [Kernel/NtAllocateVirtualMemory](/wiki/Kernel/NtAllocateVirtualMemory "wikilink")                         | 184     | stdcall            |               |
| [Kernel/NtCancelTimer](/wiki/Kernel/NtCancelTimer "wikilink")                                             | 185     | stdcall            |               |
| [Kernel/NtClearEvent](/wiki/Kernel/NtClearEvent "wikilink")                                               | 186     | stdcall            |               |
| [Kernel/NtClose](/wiki/Kernel/NtClose "wikilink")                                                         | 187     | stdcall            |               |
| [Kernel/NtCreateDirectoryObject](/wiki/Kernel/NtCreateDirectoryObject "wikilink")                         | 188     | stdcall            |               |
| [Kernel/NtCreateEvent](/wiki/Kernel/NtCreateEvent "wikilink")                                             | 189     | stdcall            |               |
| [Kernel/NtCreateFile](/wiki/Kernel/NtCreateFile "wikilink")                                               | 190     | stdcall            |               |
| [Kernel/NtCreateIoCompletion](/wiki/Kernel/NtCreateIoCompletion "wikilink")                               | 191     | stdcall            |               |
| [Kernel/NtCreateMutant](/wiki/Kernel/NtCreateMutant "wikilink")                                           | 192     | stdcall            |               |
| [Kernel/NtCreateSemaphore](/wiki/Kernel/NtCreateSemaphore "wikilink")                                     | 193     | stdcall            |               |
| [Kernel/NtCreateTimer](/wiki/Kernel/NtCreateTimer "wikilink")                                             | 194     | stdcall            |               |
| [Kernel/NtDeleteFile](/wiki/Kernel/NtDeleteFile "wikilink")                                               | 195     | stdcall            |               |
| [Kernel/NtDeviceIoControlFile](/wiki/Kernel/NtDeviceIoControlFile "wikilink")                             | 196     | stdcall            |               |
| [Kernel/NtDuplicateObject](/wiki/Kernel/NtDuplicateObject "wikilink")                                     | 197     | stdcall            |               |
| [Kernel/NtFlushBuffersFile](/wiki/Kernel/NtFlushBuffersFile "wikilink")                                   | 198     | stdcall            |               |
| [Kernel/NtFreeVirtualMemory](/wiki/Kernel/NtFreeVirtualMemory "wikilink")                                 | 199     | stdcall            |               |
| [Kernel/NtFsControlFile](/wiki/Kernel/NtFsControlFile "wikilink")                                         | 200     | stdcall            |               |
| [Kernel/NtOpenDirectoryObject](/wiki/Kernel/NtOpenDirectoryObject "wikilink")                             | 201     | stdcall            |               |
| [Kernel/NtOpenFile](/wiki/Kernel/NtOpenFile "wikilink")                                                   | 202     | stdcall            |               |
| [Kernel/NtOpenSymbolicLinkObject](/wiki/Kernel/NtOpenSymbolicLinkObject "wikilink")                       | 203     | stdcall            |               |
| [Kernel/NtProtectVirtualMemory](/wiki/Kernel/NtProtectVirtualMemory "wikilink")                           | 204     | stdcall            |               |
| [Kernel/NtPulseEvent](/wiki/Kernel/NtPulseEvent "wikilink")                                               | 205     | stdcall            |               |
| [Kernel/NtQueueApcThread](/wiki/Kernel/NtQueueApcThread "wikilink")                                       | 206     | stdcall            |               |
| [Kernel/NtQueryDirectoryFile](/wiki/Kernel/NtQueryDirectoryFile "wikilink")                               | 207     | stdcall            |               |
| [Kernel/NtQueryDirectoryObject](/wiki/Kernel/NtQueryDirectoryObject "wikilink")                           | 208     | stdcall            |               |
| [Kernel/NtQueryEvent](/wiki/Kernel/NtQueryEvent "wikilink")                                               | 209     | stdcall            |               |
| [Kernel/NtQueryFullAttributesFile](/wiki/Kernel/NtQueryFullAttributesFile "wikilink")                     | 210     | stdcall            |               |
| [Kernel/NtQueryInformationFile](/wiki/Kernel/NtQueryInformationFile "wikilink")                           | 211     | stdcall            |               |
| [Kernel/NtQueryIoCompletion](/wiki/Kernel/NtQueryIoCompletion "wikilink")                                 | 212     | stdcall            |               |
| [Kernel/NtQueryMutant](/wiki/Kernel/NtQueryMutant "wikilink")                                             | 213     | stdcall            |               |
| [Kernel/NtQuerySemaphore](/wiki/Kernel/NtQuerySemaphore "wikilink")                                       | 214     | stdcall            |               |
| [Kernel/NtQuerySymbolicLinkObject](/wiki/Kernel/NtQuerySymbolicLinkObject "wikilink")                     | 215     | stdcall            |               |
| [Kernel/NtQueryTimer](/wiki/Kernel/NtQueryTimer "wikilink")                                               | 216     | stdcall            |               |
| [Kernel/NtQueryVirtualMemory](/wiki/Kernel/NtQueryVirtualMemory "wikilink")                               | 217     | stdcall            |               |
| [Kernel/NtQueryVolumeInformationFile](/wiki/Kernel/NtQueryVolumeInformationFile "wikilink")               | 218     | stdcall            |               |
| [Kernel/NtReadFile](/wiki/Kernel/NtReadFile "wikilink")                                                   | 219     | stdcall            |               |
| [Kernel/NtReadFileScatter](/wiki/Kernel/NtReadFileScatter "wikilink")                                     | 220     | stdcall            |               |
| [Kernel/NtReleaseMutant](/wiki/Kernel/NtReleaseMutant "wikilink")                                         | 221     | stdcall            |               |
| [Kernel/NtReleaseSemaphore](/wiki/Kernel/NtReleaseSemaphore "wikilink")                                   | 222     | stdcall            |               |
| [Kernel/NtRemoveIoCompletion](/wiki/Kernel/NtRemoveIoCompletion "wikilink")                               | 223     | stdcall            |               |
| [Kernel/NtResumeThread](/wiki/Kernel/NtResumeThread "wikilink")                                           | 224     | stdcall            |               |
| [Kernel/NtSetEvent](/wiki/Kernel/NtSetEvent "wikilink")                                                   | 225     | stdcall            |               |
| [Kernel/NtSetInformationFile](/wiki/Kernel/NtSetInformationFile "wikilink")                               | 226     | stdcall            |               |
| [Kernel/NtSetIoCompletion](/wiki/Kernel/NtSetIoCompletion "wikilink")                                     | 227     | stdcall            |               |
| [Kernel/NtSetSystemTime](/wiki/Kernel/NtSetSystemTime "wikilink")                                         | 228     | stdcall            |               |
| [Kernel/NtSetTimerEx](/wiki/Kernel/NtSetTimerEx "wikilink")                                               | 229     | stdcall            |               |
| [Kernel/NtSignalAndWaitForSingleObjectEx](/wiki/Kernel/NtSignalAndWaitForSingleObjectEx "wikilink")       | 230     | stdcall            |               |
| [Kernel/NtSuspendThread](/wiki/Kernel/NtSuspendThread "wikilink")                                         | 231     | stdcall            |               |
| [Kernel/NtUserIoApcDispatcher](/wiki/Kernel/NtUserIoApcDispatcher "wikilink")                             | 232     | stdcall            |               |
| [Kernel/NtWaitForSingleObject](/wiki/Kernel/NtWaitForSingleObject "wikilink")                             | 233     | stdcall            |               |
| [Kernel/NtWaitForSingleObjectEx](/wiki/Kernel/NtWaitForSingleObjectEx "wikilink")                         | 234     | stdcall            |               |
| [Kernel/NtWaitForMultipleObjectsEx](/wiki/Kernel/NtWaitForMultipleObjectsEx "wikilink")                   | 235     | stdcall            |               |
| [Kernel/NtWriteFile](/wiki/Kernel/NtWriteFile "wikilink")                                                 | 236     | stdcall            |               |
| [Kernel/NtWriteFileGather](/wiki/Kernel/NtWriteFileGather "wikilink")                                     | 237     | stdcall            |               |
| [Kernel/NtYieldExecution](/wiki/Kernel/NtYieldExecution "wikilink")                                       | 238     | stdcall            |               |
| [Kernel/ObCreateObject](/wiki/Kernel/ObCreateObject "wikilink")                                           | 239     | stdcall            |               |
| [Kernel/ObDirectoryObjectType](/wiki/Kernel/ObDirectoryObjectType "wikilink")                             | 240     | stdcall            | Variable?     |
| [Kernel/ObInsertObject](/wiki/Kernel/ObInsertObject "wikilink")                                           | 241     | stdcall            |               |
| [Kernel/ObMakeTemporaryObject](/wiki/Kernel/ObMakeTemporaryObject "wikilink")                             | 242     | stdcall            |               |
| [Kernel/ObOpenObjectByName](/wiki/Kernel/ObOpenObjectByName "wikilink")                                   | 243     | stdcall            |               |
| [Kernel/ObOpenObjectByPointer](/wiki/Kernel/ObOpenObjectByPointer "wikilink")                             | 244     | stdcall            |               |
| [Kernel/ObpObjectHandleTable](/wiki/Kernel/ObpObjectHandleTable "wikilink")                               | 245     | stdcall            | Variable?     |
| [Kernel/ObReferenceObjectByHandle](/wiki/Kernel/ObReferenceObjectByHandle "wikilink")                     | 246     | stdcall            |               |
| [Kernel/ObReferenceObjectByName](/wiki/Kernel/ObReferenceObjectByName "wikilink")                         | 247     | stdcall            |               |
| [Kernel/ObReferenceObjectByPointer](/wiki/Kernel/ObReferenceObjectByPointer "wikilink")                   | 248     | stdcall            |               |
| [Kernel/ObSymbolicLinkObjectType](/wiki/Kernel/ObSymbolicLinkObjectType "wikilink")                       | 249     | stdcall            | Variable?     |
| [Kernel/ObfDereferenceObject](/wiki/Kernel/ObfDereferenceObject "wikilink")                               | 250     | fastcall           |               |
| [Kernel/ObfReferenceObject](/wiki/Kernel/ObfReferenceObject "wikilink")                                   | 251     | fastcall           |               |
| [Kernel/PhyGetLinkState](/wiki/Kernel/PhyGetLinkState "wikilink")                                         | 252     | stdcall            |               |
| [Kernel/PhyInitialize](/wiki/Kernel/PhyInitialize "wikilink")                                             | 253     | stdcall            |               |
| [Kernel/PsCreateSystemThread](/wiki/Kernel/PsCreateSystemThread "wikilink")                               | 254     | stdcall            |               |
| [Kernel/PsCreateSystemThreadEx](/wiki/Kernel/PsCreateSystemThreadEx "wikilink")                           | 255     | stdcall            |               |
| [Kernel/PsQueryStatistics](/wiki/Kernel/PsQueryStatistics "wikilink")                                     | 256     | stdcall            |               |
| [Kernel/PsSetCreateThreadNotifyRoutine](/wiki/Kernel/PsSetCreateThreadNotifyRoutine "wikilink")           | 257     | stdcall            |               |
| [Kernel/PsTerminateSystemThread](/wiki/Kernel/PsTerminateSystemThread "wikilink")                         | 258     | stdcall            |               |
| [Kernel/PsThreadObjectType](/wiki/Kernel/PsThreadObjectType "wikilink")                                   | 259     | stdcall            | Variable?     |
| [Kernel/RtlAnsiStringToUnicodeString](/wiki/Kernel/RtlAnsiStringToUnicodeString "wikilink")               | 260     | stdcall            |               |
| [Kernel/RtlAppendStringToString](/wiki/Kernel/RtlAppendStringToString "wikilink")                         | 261     | stdcall            |               |
| [Kernel/RtlAppendUnicodeStringToString](/wiki/Kernel/RtlAppendUnicodeStringToString "wikilink")           | 262     | stdcall            |               |
| [Kernel/RtlAppendUnicodeToString](/wiki/Kernel/RtlAppendUnicodeToString "wikilink")                       | 263     | stdcall            |               |
| [Kernel/RtlAssert](/wiki/Kernel/RtlAssert "wikilink")                                                     | 264     | stdcall            |               |
| [Kernel/RtlCaptureContext](/wiki/Kernel/RtlCaptureContext "wikilink")                                     | 265     | stdcall            |               |
| [Kernel/RtlCaptureStackBackTrace](/wiki/Kernel/RtlCaptureStackBackTrace "wikilink")                       | 266     | stdcall            |               |
| [Kernel/RtlCharToInteger](/wiki/Kernel/RtlCharToInteger "wikilink")                                       | 267     | stdcall            |               |
| [Kernel/RtlCompareMemory](/wiki/Kernel/RtlCompareMemory "wikilink")                                       | 268     | stdcall            |               |
| [Kernel/RtlCompareMemoryUlong](/wiki/Kernel/RtlCompareMemoryUlong "wikilink")                             | 269     | stdcall            |               |
| [Kernel/RtlCompareString](/wiki/Kernel/RtlCompareString "wikilink")                                       | 270     | stdcall            |               |
| [Kernel/RtlCompareUnicodeString](/wiki/Kernel/RtlCompareUnicodeString "wikilink")                         | 271     | stdcall            |               |
| [Kernel/RtlCopyString](/wiki/Kernel/RtlCopyString "wikilink")                                             | 272     | stdcall            |               |
| [Kernel/RtlCopyUnicodeString](/wiki/Kernel/RtlCopyUnicodeString "wikilink")                               | 273     | stdcall            |               |
| [Kernel/RtlCreateUnicodeString](/wiki/Kernel/RtlCreateUnicodeString "wikilink")                           | 274     | stdcall            |               |
| [Kernel/RtlDowncaseUnicodeChar](/wiki/Kernel/RtlDowncaseUnicodeChar "wikilink")                           | 275     | stdcall            |               |
| [Kernel/RtlDowncaseUnicodeString](/wiki/Kernel/RtlDowncaseUnicodeString "wikilink")                       | 276     | stdcall            |               |
| [Kernel/RtlEnterCriticalSection](/wiki/Kernel/RtlEnterCriticalSection "wikilink")                         | 277     | stdcall            |               |
| [Kernel/RtlEnterCriticalSectionAndRegion](/wiki/Kernel/RtlEnterCriticalSectionAndRegion "wikilink")       | 278     | stdcall            |               |
| [Kernel/RtlEqualString](/wiki/Kernel/RtlEqualString "wikilink")                                           | 279     | stdcall            |               |
| [Kernel/RtlEqualUnicodeString](/wiki/Kernel/RtlEqualUnicodeString "wikilink")                             | 280     | stdcall            |               |
| [Kernel/RtlExtendedIntegerMultiply](/wiki/Kernel/RtlExtendedIntegerMultiply "wikilink")                   | 281     | stdcall            |               |
| [Kernel/RtlExtendedLargeIntegerDivide](/wiki/Kernel/RtlExtendedLargeIntegerDivide "wikilink")             | 282     | stdcall            |               |
| [Kernel/RtlExtendedMagicDivide](/wiki/Kernel/RtlExtendedMagicDivide "wikilink")                           | 283     | stdcall            |               |
| [Kernel/RtlFillMemory](/wiki/Kernel/RtlFillMemory "wikilink")                                             | 284     | stdcall            |               |
| [Kernel/RtlFillMemoryUlong](/wiki/Kernel/RtlFillMemoryUlong "wikilink")                                   | 285     | stdcall            |               |
| [Kernel/RtlFreeAnsiString](/wiki/Kernel/RtlFreeAnsiString "wikilink")                                     | 286     | stdcall            |               |
| [Kernel/RtlFreeUnicodeString](/wiki/Kernel/RtlFreeUnicodeString "wikilink")                               | 287     | stdcall            |               |
| [Kernel/RtlGetCallersAddress](/wiki/Kernel/RtlGetCallersAddress "wikilink")                               | 288     | stdcall            |               |
| [Kernel/RtlInitAnsiString](/wiki/Kernel/RtlInitAnsiString "wikilink")                                     | 289     | stdcall            |               |
| [Kernel/RtlInitUnicodeString](/wiki/Kernel/RtlInitUnicodeString "wikilink")                               | 290     | stdcall            |               |
| [Kernel/RtlInitializeCriticalSection](/wiki/Kernel/RtlInitializeCriticalSection "wikilink")               | 291     | stdcall            |               |
| [Kernel/RtlIntegerToChar](/wiki/Kernel/RtlIntegerToChar "wikilink")                                       | 292     | stdcall            |               |
| [Kernel/RtlIntegerToUnicodeString](/wiki/Kernel/RtlIntegerToUnicodeString "wikilink")                     | 293     | stdcall            |               |
| [Kernel/RtlLeaveCriticalSection](/wiki/Kernel/RtlLeaveCriticalSection "wikilink")                         | 294     | stdcall            |               |
| [Kernel/RtlLeaveCriticalSectionAndRegion](/wiki/Kernel/RtlLeaveCriticalSectionAndRegion "wikilink")       | 295     | stdcall            |               |
| [Kernel/RtlLowerChar](/wiki/Kernel/RtlLowerChar "wikilink")                                               | 296     | stdcall            |               |
| [Kernel/RtlMapGenericMask](/wiki/Kernel/RtlMapGenericMask "wikilink")                                     | 297     | stdcall            |               |
| [Kernel/RtlMoveMemory](/wiki/Kernel/RtlMoveMemory "wikilink")                                             | 298     | stdcall            |               |
| [Kernel/RtlMultiByteToUnicodeN](/wiki/Kernel/RtlMultiByteToUnicodeN "wikilink")                           | 299     | stdcall            |               |
| [Kernel/RtlMultiByteToUnicodeSize](/wiki/Kernel/RtlMultiByteToUnicodeSize "wikilink")                     | 300     | stdcall            |               |
| [Kernel/RtlNtStatusToDosError](/wiki/Kernel/RtlNtStatusToDosError "wikilink")                             | 301     | stdcall            |               |
| [Kernel/RtlRaiseException](/wiki/Kernel/RtlRaiseException "wikilink")                                     | 302     | stdcall            |               |
| [Kernel/RtlRaiseStatus](/wiki/Kernel/RtlRaiseStatus "wikilink")                                           | 303     | stdcall            |               |
| [Kernel/RtlTimeFieldsToTime](/wiki/Kernel/RtlTimeFieldsToTime "wikilink")                                 | 304     | stdcall            |               |
| [Kernel/RtlTimeToTimeFields](/wiki/Kernel/RtlTimeToTimeFields "wikilink")                                 | 305     | stdcall            |               |
| [Kernel/RtlTryEnterCriticalSection](/wiki/Kernel/RtlTryEnterCriticalSection "wikilink")                   | 306     | stdcall            |               |
| [Kernel/RtlUlongByteSwap](/wiki/Kernel/RtlUlongByteSwap "wikilink")                                       | 307     | fastcall           |               |
| [Kernel/RtlUnicodeStringToAnsiString](/wiki/Kernel/RtlUnicodeStringToAnsiString "wikilink")               | 308     | stdcall            |               |
| [Kernel/RtlUnicodeStringToInteger](/wiki/Kernel/RtlUnicodeStringToInteger "wikilink")                     | 309     | stdcall            |               |
| [Kernel/RtlUnicodeToMultiByteN](/wiki/Kernel/RtlUnicodeToMultiByteN "wikilink")                           | 310     | stdcall            |               |
| [Kernel/RtlUnicodeToMultiByteSize](/wiki/Kernel/RtlUnicodeToMultiByteSize "wikilink")                     | 311     | stdcall            |               |
| [Kernel/RtlUnwind](/wiki/Kernel/RtlUnwind "wikilink")                                                     | 312     | stdcall            |               |
| [Kernel/RtlUpcaseUnicodeChar](/wiki/Kernel/RtlUpcaseUnicodeChar "wikilink")                               | 313     | stdcall            |               |
| [Kernel/RtlUpcaseUnicodeString](/wiki/Kernel/RtlUpcaseUnicodeString "wikilink")                           | 314     | stdcall            |               |
| [Kernel/RtlUpcaseUnicodeToMultiByteN](/wiki/Kernel/RtlUpcaseUnicodeToMultiByteN "wikilink")               | 315     | stdcall            |               |
| [Kernel/RtlUpperChar](/wiki/Kernel/RtlUpperChar "wikilink")                                               | 316     | stdcall            |               |
| [Kernel/RtlUpperString](/wiki/Kernel/RtlUpperString "wikilink")                                           | 317     | stdcall            |               |
| [Kernel/RtlUshortByteSwap](/wiki/Kernel/RtlUshortByteSwap "wikilink")                                     | 318     | fastcall           |               |
| [Kernel/RtlWalkFrameChain](/wiki/Kernel/RtlWalkFrameChain "wikilink")                                     | 319     | stdcall            |               |
| [Kernel/RtlZeroMemory](/wiki/Kernel/RtlZeroMemory "wikilink")                                             | 320     | stdcall            |               |
| [Kernel/XboxEEPROMKey](/wiki/Kernel/XboxEEPROMKey "wikilink")                                             | 321     | stdcall            | Variable?     |
| [Kernel/XboxHardwareInfo](/wiki/Kernel/XboxHardwareInfo "wikilink")                                       | 322     | stdcall            | Variable?     |
| [Kernel/XboxHDKey](/wiki/Kernel/XboxHDKey "wikilink")                                                     | 323     | stdcall            | Variable?     |
| [Kernel/XboxKrnlVersion](/wiki/Kernel/XboxKrnlVersion "wikilink")                                         | 324     | stdcall            | Variable?     |
| [Kernel/XboxSignatureKey](/wiki/Kernel/XboxSignatureKey "wikilink")                                       | 325     | stdcall            | Variable?     |
| [Kernel/XeImageFileName](/wiki/Kernel/XeImageFileName "wikilink")                                         | 326     | stdcall            | Variable?     |
| [Kernel/XeLoadSection](/wiki/Kernel/XeLoadSection "wikilink")                                             | 327     | stdcall            |               |
| [Kernel/XeUnloadSection](/wiki/Kernel/XeUnloadSection "wikilink")                                         | 328     | stdcall            |               |
| [Kernel/READ\_PORT\_BUFFER\_UCHAR](/wiki/Kernel/READ_PORT_BUFFER_UCHAR "wikilink")                        | 329     | stdcall            |               |
| [Kernel/READ\_PORT\_BUFFER\_USHORT](/wiki/Kernel/READ_PORT_BUFFER_USHORT "wikilink")                      | 330     | stdcall            |               |
| [Kernel/READ\_PORT\_BUFFER\_ULONG](/wiki/Kernel/READ_PORT_BUFFER_ULONG "wikilink")                        | 331     | stdcall            |               |
| [Kernel/WRITE\_PORT\_BUFFER\_UCHAR](/wiki/Kernel/WRITE_PORT_BUFFER_UCHAR "wikilink")                      | 332     | stdcall            |               |
| [Kernel/WRITE\_PORT\_BUFFER\_USHORT](/wiki/Kernel/WRITE_PORT_BUFFER_USHORT "wikilink")                    | 333     | stdcall            |               |
| [Kernel/WRITE\_PORT\_BUFFER\_ULONG](/wiki/Kernel/WRITE_PORT_BUFFER_ULONG "wikilink")                      | 334     | stdcall            |               |
| [Kernel/XcSHAInit](/wiki/Kernel/XcSHAInit "wikilink")                                                     | 335     | stdcall            |               |
| [Kernel/XcSHAUpdate](/wiki/Kernel/XcSHAUpdate "wikilink")                                                 | 336     | stdcall            |               |
| [Kernel/XcSHAFinal](/wiki/Kernel/XcSHAFinal "wikilink")                                                   | 337     | stdcall            |               |
| [Kernel/XcRC4Key](/wiki/Kernel/XcRC4Key "wikilink")                                                       | 338     | stdcall            |               |
| [Kernel/XcRC4Crypt](/wiki/Kernel/XcRC4Crypt "wikilink")                                                   | 339     | stdcall            |               |
| [Kernel/XcHMAC](/wiki/Kernel/XcHMAC "wikilink")                                                           | 340     | stdcall            |               |
| [Kernel/XcPKEncPublic](/wiki/Kernel/XcPKEncPublic "wikilink")                                             | 341     | stdcall            |               |
| [Kernel/XcPKDecPrivate](/wiki/Kernel/XcPKDecPrivate "wikilink")                                           | 342     | stdcall            |               |
| [Kernel/XcPKGetKeyLen](/wiki/Kernel/XcPKGetKeyLen "wikilink")                                             | 343     | stdcall            |               |
| [Kernel/XcVerifyPKCS1Signature](/wiki/Kernel/XcVerifyPKCS1Signature "wikilink")                           | 344     | stdcall            |               |
| [Kernel/XcModExp](/wiki/Kernel/XcModExp "wikilink")                                                       | 345     | stdcall            |               |
| [Kernel/XcDESKeyParity](/wiki/Kernel/XcDESKeyParity "wikilink")                                           | 346     | stdcall            |               |
| [Kernel/XcKeyTable](/wiki/Kernel/XcKeyTable "wikilink")                                                   | 347     | stdcall            |               |
| [Kernel/XcBlockCrypt](/wiki/Kernel/XcBlockCrypt "wikilink")                                               | 348     | stdcall            |               |
| [Kernel/XcBlockCryptCBC](/wiki/Kernel/XcBlockCryptCBC "wikilink")                                         | 349     | stdcall            |               |
| [Kernel/XcCryptService](/wiki/Kernel/XcCryptService "wikilink")                                           | 350     | stdcall            |               |
| [Kernel/XcUpdateCrypto](/wiki/Kernel/XcUpdateCrypto "wikilink")                                           | 351     | stdcall            |               |
| [Kernel/RtlRip](/wiki/Kernel/RtlRip "wikilink")                                                           | 352     | stdcall            |               |
| [Kernel/XboxLANKey](/wiki/Kernel/XboxLANKey "wikilink")                                                   | 353     | stdcall            |               |
| [Kernel/XboxAlternateSignatureKeys](/wiki/Kernel/XboxAlternateSignatureKeys "wikilink")                   | 354     | stdcall            | Variable?     |
| [Kernel/XePublicKeyData](/wiki/Kernel/XePublicKeyData "wikilink")                                         | 355     | stdcall            | Variable?     |
| [Kernel/HalBootSMCVideoMode](/wiki/Kernel/HalBootSMCVideoMode "wikilink")                                 | 356     | stdcall            | Variable?     |
| [Kernel/IdexChannelObject](/wiki/Kernel/IdexChannelObject "wikilink")                                     | 357     | stdcall            | Variable?     |
| [Kernel/HalIsResetOrShutdownPending](/wiki/Kernel/HalIsResetOrShutdownPending "wikilink")                 | 358     | stdcall            |               |
| [Kernel/IoMarkIrpMustComplete](/wiki/Kernel/IoMarkIrpMustComplete "wikilink")                             | 359     | stdcall            |               |
| [Kernel/HalInitiateShutdown](/wiki/Kernel/HalInitiateShutdown "wikilink")                                 | 360     | stdcall            |               |
| [Kernel/RtlSnprintf](/wiki/Kernel/RtlSnprintf "wikilink")                                                 | 361     | stdcall            | Unused?       |
| [Kernel/RtlSprintf](/wiki/Kernel/RtlSprintf "wikilink")                                                   | 362     | stdcall            | Unused?       |
| [Kernel/RtlVsnprintf](/wiki/Kernel/RtlVsnprintf "wikilink")                                               | 363     | stdcall            | Unused?       |
| [Kernel/RtlVsprintf](/wiki/Kernel/RtlVsprintf "wikilink")                                                 | 364     | stdcall            | Unused?       |
| [Kernel/HalEnableSecureTrayEject](/wiki/Kernel/HalEnableSecureTrayEject "wikilink")                       | 365     | stdcall            |               |
| [Kernel/HalWriteSMCScratchRegister](/wiki/Kernel/HalWriteSMCScratchRegister "wikilink")                   | 366     | stdcall            |               |
|                                                                                                     | 367     |                    | Unused?       |
|                                                                                                     | 368     |                    | Unused?       |
|                                                                                                     | 369     |                    | Unused?       |
|                                                                                                     | 370     |                    | Unused?       |
|                                                                                                     | 371     |                    | Unused?       |
|                                                                                                     | 372     |                    | Unused?       |
|                                                                                                     | 373     |                    | Unused?       |
| [Kernel/MmDbgAllocateMemory](/wiki/Kernel/MmDbgAllocateMemory "wikilink")                                 | 374     | stdcall            | Devkits only! |
| [Kernel/MmDbgFreeMemory](/wiki/Kernel/MmDbgFreeMemory "wikilink")                                         | 375     | stdcall            | Devkits only! |
| [Kernel/MmDbgQueryAvailablePages](/wiki/Kernel/MmDbgQueryAvailablePages "wikilink")                       | 376     | stdcall            | Devkits only! |
| [Kernel/MmDbgReleaseAddress](/wiki/Kernel/MmDbgReleaseAddress "wikilink")                                 | 377     | stdcall            | Devkits only! |
| [Kernel/MmDbgWriteCheck](/wiki/Kernel/MmDbgWriteCheck "wikilink")                                         | 378     | stdcall            | Devkits only! |


