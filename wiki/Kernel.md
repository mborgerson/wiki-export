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
kernel. Later kernels will discard this section after initialization.

Kernel exports
--------------

| Name                                                                                                | Ordinal | Notes         |
|-----------------------------------------------------------------------------------------------------|---------|---------------|
| [Kernel/AvGetSavedDataAddress](/wiki/Kernel/AvGetSavedDataAddress "wikilink")                             | 1       |               |
| [Kernel/AvSendTVEncoderOption](/wiki/Kernel/AvSendTVEncoderOption "wikilink")                             | 2       |               |
| [Kernel/AvSetDisplayMode](/wiki/Kernel/AvSetDisplayMode "wikilink")                                       | 3       |               |
| [Kernel/AvSetSavedDataAddress](/wiki/Kernel/AvSetSavedDataAddress "wikilink")                             | 4       |               |
| [Kernel/DbgBreakPoint](/wiki/Kernel/DbgBreakPoint "wikilink")                                             | 5       |               |
| [Kernel/DbgBreakPointWithStatus](/wiki/Kernel/DbgBreakPointWithStatus "wikilink")                         | 6       |               |
| [Kernel/DbgLoadImageSymbols](/wiki/Kernel/DbgLoadImageSymbols "wikilink")                                 | 7       | Devkits only! |
| [Kernel/DbgPrint](/wiki/Kernel/DbgPrint "wikilink")                                                       | 8       |               |
| [Kernel/HalReadSMCTrayState](/wiki/Kernel/HalReadSMCTrayState "wikilink")                                 | 9       |               |
| [Kernel/DbgPrompt](/wiki/Kernel/DbgPrompt "wikilink")                                                     | 10      |               |
| [Kernel/DbgUnLoadImageSymbols](/wiki/Kernel/DbgUnLoadImageSymbols "wikilink")                             | 11      | Devkits only! |
| [Kernel/ExAcquireReadWriteLockExclusive](/wiki/Kernel/ExAcquireReadWriteLockExclusive "wikilink")         | 12      |               |
| [Kernel/ExAcquireReadWriteLockShared](/wiki/Kernel/ExAcquireReadWriteLockShared "wikilink")               | 13      |               |
| [Kernel/ExAllocatePool](/wiki/Kernel/ExAllocatePool "wikilink")                                           | 14      |               |
| [Kernel/ExAllocatePoolWithTag](/wiki/Kernel/ExAllocatePoolWithTag "wikilink")                             | 15      |               |
| [Kernel/ExEventObjectType](/wiki/Kernel/ExEventObjectType "wikilink")                                     | 16      | Variable?     |
| [Kernel/ExFreePool](/wiki/Kernel/ExFreePool "wikilink")                                                   | 17      |               |
| [Kernel/ExInitializeReadWriteLock](/wiki/Kernel/ExInitializeReadWriteLock "wikilink")                     | 18      |               |
| [Kernel/ExInterlockedAddLargeInteger](/wiki/Kernel/ExInterlockedAddLargeInteger "wikilink")               | 19      |               |
| [Kernel/ExInterlockedAddLargeStatistic](/wiki/Kernel/ExInterlockedAddLargeStatistic "wikilink")           | 20      |               |
| [Kernel/ExInterlockedCompareExchange64](/wiki/Kernel/ExInterlockedCompareExchange64 "wikilink")           | 21      |               |
| [Kernel/ExMutantObjectType](/wiki/Kernel/ExMutantObjectType "wikilink")                                   | 22      | Variable?     |
| [Kernel/ExQueryPoolBlockSize](/wiki/Kernel/ExQueryPoolBlockSize "wikilink")                               | 23      |               |
| [Kernel/ExQueryNonVolatileSetting](/wiki/Kernel/ExQueryNonVolatileSetting "wikilink")                     | 24      |               |
| [Kernel/ExReadWriteRefurbInfo](/wiki/Kernel/ExReadWriteRefurbInfo "wikilink")                             | 25      |               |
| [Kernel/ExRaiseException](/wiki/Kernel/ExRaiseException "wikilink")                                       | 26      |               |
| [Kernel/ExRaiseStatus](/wiki/Kernel/ExRaiseStatus "wikilink")                                             | 27      |               |
| [Kernel/ExReleaseReadWriteLock](/wiki/Kernel/ExReleaseReadWriteLock "wikilink")                           | 28      |               |
| [Kernel/ExSaveNonVolatileSetting](/wiki/Kernel/ExSaveNonVolatileSetting "wikilink")                       | 29      |               |
| [Kernel/ExSemaphoreObjectType](/wiki/Kernel/ExSemaphoreObjectType "wikilink")                             | 30      | Variable?     |
| [Kernel/ExTimerObjectType](/wiki/Kernel/ExTimerObjectType "wikilink")                                     | 31      | Variable?     |
| [Kernel/ExfInterlockedInsertHeadList](/wiki/Kernel/ExfInterlockedInsertHeadList "wikilink")               | 32      |               |
| [Kernel/ExfInterlockedInsertTailList](/wiki/Kernel/ExfInterlockedInsertTailList "wikilink")               | 33      |               |
| [Kernel/ExfInterlockedRemoveHeadList](/wiki/Kernel/ExfInterlockedRemoveHeadList "wikilink")               | 34      |               |
| [Kernel/FscGetCacheSize](/wiki/Kernel/FscGetCacheSize "wikilink")                                         | 35      |               |
| [Kernel/FscInvalidateIdleBlocks](/wiki/Kernel/FscInvalidateIdleBlocks "wikilink")                         | 36      |               |
| [Kernel/FscSetCacheSize](/wiki/Kernel/FscSetCacheSize "wikilink")                                         | 37      |               |
| [Kernel/HalClearSoftwareInterrupt](/wiki/Kernel/HalClearSoftwareInterrupt "wikilink")                     | 38      |               |
| [Kernel/HalDisableSystemInterrupt](/wiki/Kernel/HalDisableSystemInterrupt "wikilink")                     | 39      |               |
| [Kernel/HalDiskCachePartitionCount](/wiki/Kernel/HalDiskCachePartitionCount "wikilink")                   | 40      | Variable?     |
| [Kernel/HalDiskModelNumber](/wiki/Kernel/HalDiskModelNumber "wikilink")                                   | 41      | Variable?     |
| [Kernel/HalDiskSerialNumber](/wiki/Kernel/HalDiskSerialNumber "wikilink")                                 | 42      | Variable?     |
| [Kernel/HalEnableSystemInterrupt](/wiki/Kernel/HalEnableSystemInterrupt "wikilink")                       | 43      |               |
| [Kernel/HalGetInterruptVector](/wiki/Kernel/HalGetInterruptVector "wikilink")                             | 44      |               |
| [Kernel/HalReadSMBusValue](/wiki/Kernel/HalReadSMBusValue "wikilink")                                     | 45      |               |
| [Kernel/HalReadWritePCISpace](/wiki/Kernel/HalReadWritePCISpace "wikilink")                               | 46      |               |
| [Kernel/HalRegisterShutdownNotification](/wiki/Kernel/HalRegisterShutdownNotification "wikilink")         | 47      |               |
| [Kernel/HalRequestSoftwareInterrupt](/wiki/Kernel/HalRequestSoftwareInterrupt "wikilink")                 | 48      |               |
| [Kernel/HalReturnToFirmware](/wiki/Kernel/HalReturnToFirmware "wikilink")                                 | 49      |               |
| [Kernel/HalWriteSMBusValue](/wiki/Kernel/HalWriteSMBusValue "wikilink")                                   | 50      |               |
| [Kernel/InterlockedCompareExchange](/wiki/Kernel/InterlockedCompareExchange "wikilink")                   | 51      |               |
| [Kernel/InterlockedDecrement](/wiki/Kernel/InterlockedDecrement "wikilink")                               | 52      |               |
| [Kernel/InterlockedIncrement](/wiki/Kernel/InterlockedIncrement "wikilink")                               | 53      |               |
| [Kernel/InterlockedExchange](/wiki/Kernel/InterlockedExchange "wikilink")                                 | 54      |               |
| [Kernel/InterlockedExchangeAdd](/wiki/Kernel/InterlockedExchangeAdd "wikilink")                           | 55      |               |
| [Kernel/InterlockedFlushSList](/wiki/Kernel/InterlockedFlushSList "wikilink")                             | 56      |               |
| [Kernel/InterlockedPopEntrySList](/wiki/Kernel/InterlockedPopEntrySList "wikilink")                       | 57      |               |
| [Kernel/InterlockedPushEntrySList](/wiki/Kernel/InterlockedPushEntrySList "wikilink")                     | 58      |               |
| [Kernel/IoAllocateIrp](/wiki/Kernel/IoAllocateIrp "wikilink")                                             | 59      |               |
| [Kernel/IoBuildAsynchronousFsdRequest](/wiki/Kernel/IoBuildAsynchronousFsdRequest "wikilink")             | 60      |               |
| [Kernel/IoBuildDeviceIoControlRequest](/wiki/Kernel/IoBuildDeviceIoControlRequest "wikilink")             | 61      |               |
| [Kernel/IoBuildSynchronousFsdRequest](/wiki/Kernel/IoBuildSynchronousFsdRequest "wikilink")               | 62      |               |
| [Kernel/IoCheckShareAccess](/wiki/Kernel/IoCheckShareAccess "wikilink")                                   | 63      |               |
| [Kernel/IoCompletionObjectType](/wiki/Kernel/IoCompletionObjectType "wikilink")                           | 64      | Variable?     |
| [Kernel/IoCreateDevice](/wiki/Kernel/IoCreateDevice "wikilink")                                           | 65      |               |
| [Kernel/IoCreateFile](/wiki/Kernel/IoCreateFile "wikilink")                                               | 66      |               |
| [Kernel/IoCreateSymbolicLink](/wiki/Kernel/IoCreateSymbolicLink "wikilink")                               | 67      |               |
| [Kernel/IoDeleteDevice](/wiki/Kernel/IoDeleteDevice "wikilink")                                           | 68      |               |
| [Kernel/IoDeleteSymbolicLink](/wiki/Kernel/IoDeleteSymbolicLink "wikilink")                               | 69      |               |
| [Kernel/IoDeviceObjectType](/wiki/Kernel/IoDeviceObjectType "wikilink")                                   | 70      | Variable?     |
| [Kernel/IoFileObjectType](/wiki/Kernel/IoFileObjectType "wikilink")                                       | 71      | Variable?     |
| [Kernel/IoFreeIrp](/wiki/Kernel/IoFreeIrp "wikilink")                                                     | 72      |               |
| [Kernel/IoInitializeIrp](/wiki/Kernel/IoInitializeIrp "wikilink")                                         | 73      |               |
| [Kernel/IoInvalidDeviceRequest](/wiki/Kernel/IoInvalidDeviceRequest "wikilink")                           | 74      |               |
| [Kernel/IoQueryFileInformation](/wiki/Kernel/IoQueryFileInformation "wikilink")                           | 75      |               |
| [Kernel/IoQueryVolumeInformation](/wiki/Kernel/IoQueryVolumeInformation "wikilink")                       | 76      |               |
| [Kernel/IoQueueThreadIrp](/wiki/Kernel/IoQueueThreadIrp "wikilink")                                       | 77      |               |
| [Kernel/IoRemoveShareAccess](/wiki/Kernel/IoRemoveShareAccess "wikilink")                                 | 78      |               |
| [Kernel/IoSetIoCompletion](/wiki/Kernel/IoSetIoCompletion "wikilink")                                     | 79      |               |
| [Kernel/IoSetShareAccess](/wiki/Kernel/IoSetShareAccess "wikilink")                                       | 80      |               |
| [Kernel/IoStartNextPacket](/wiki/Kernel/IoStartNextPacket "wikilink")                                     | 81      |               |
| [Kernel/IoStartNextPacketByKey](/wiki/Kernel/IoStartNextPacketByKey "wikilink")                           | 82      |               |
| [Kernel/IoStartPacket](/wiki/Kernel/IoStartPacket "wikilink")                                             | 83      |               |
| [Kernel/IoSynchronousDeviceIoControlRequest](/wiki/Kernel/IoSynchronousDeviceIoControlRequest "wikilink") | 84      |               |
| [Kernel/IoSynchronousFsdRequest](/wiki/Kernel/IoSynchronousFsdRequest "wikilink")                         | 85      |               |
| [Kernel/IofCallDriver](/wiki/Kernel/IofCallDriver "wikilink")                                             | 86      |               |
| [Kernel/IofCompleteRequest](/wiki/Kernel/IofCompleteRequest "wikilink")                                   | 87      |               |
| [Kernel/KdDebuggerEnabled](/wiki/Kernel/KdDebuggerEnabled "wikilink")                                     | 88      | Variable?     |
| [Kernel/KdDebuggerNotPresent](/wiki/Kernel/KdDebuggerNotPresent "wikilink")                               | 89      | Variable?     |
| [Kernel/IoDismountVolume](/wiki/Kernel/IoDismountVolume "wikilink")                                       | 90      |               |
| [Kernel/IoDismountVolumeByName](/wiki/Kernel/IoDismountVolumeByName "wikilink")                           | 91      |               |
| [Kernel/KeAlertResumeThread](/wiki/Kernel/KeAlertResumeThread "wikilink")                                 | 92      |               |
| [Kernel/KeAlertThread](/wiki/Kernel/KeAlertThread "wikilink")                                             | 93      |               |
| [Kernel/KeBoostPriorityThread](/wiki/Kernel/KeBoostPriorityThread "wikilink")                             | 94      |               |
| [Kernel/KeBugCheck](/wiki/Kernel/KeBugCheck "wikilink")                                                   | 95      |               |
| [Kernel/KeBugCheckEx](/wiki/Kernel/KeBugCheckEx "wikilink")                                               | 96      |               |
| [Kernel/KeCancelTimer](/wiki/Kernel/KeCancelTimer "wikilink")                                             | 97      |               |
| [Kernel/KeConnectInterrupt](/wiki/Kernel/KeConnectInterrupt "wikilink")                                   | 98      |               |
| [Kernel/KeDelayExecutionThread](/wiki/Kernel/KeDelayExecutionThread "wikilink")                           | 99      |               |
| [Kernel/KeDisconnectInterrupt](/wiki/Kernel/KeDisconnectInterrupt "wikilink")                             | 100     |               |
| [Kernel/KeEnterCriticalRegion](/wiki/Kernel/KeEnterCriticalRegion "wikilink")                             | 101     |               |
| [Kernel/MmGlobalData](/wiki/Kernel/MmGlobalData "wikilink")                                               | 102     | Variable?     |
| [Kernel/KeGetCurrentIrql](/wiki/Kernel/KeGetCurrentIrql "wikilink")                                       | 103     |               |
| [Kernel/KeGetCurrentThread](/wiki/Kernel/KeGetCurrentThread "wikilink")                                   | 104     |               |
| [Kernel/KeInitializeApc](/wiki/Kernel/KeInitializeApc "wikilink")                                         | 105     |               |
| [Kernel/KeInitializeDeviceQueue](/wiki/Kernel/KeInitializeDeviceQueue "wikilink")                         | 106     |               |
| [Kernel/KeInitializeDpc](/wiki/Kernel/KeInitializeDpc "wikilink")                                         | 107     |               |
| [Kernel/KeInitializeEvent](/wiki/Kernel/KeInitializeEvent "wikilink")                                     | 108     |               |
| [Kernel/KeInitializeInterrupt](/wiki/Kernel/KeInitializeInterrupt "wikilink")                             | 109     |               |
| [Kernel/KeInitializeMutant](/wiki/Kernel/KeInitializeMutant "wikilink")                                   | 110     |               |
| [Kernel/KeInitializeQueue](/wiki/Kernel/KeInitializeQueue "wikilink")                                     | 111     |               |
| [Kernel/KeInitializeSemaphore](/wiki/Kernel/KeInitializeSemaphore "wikilink")                             | 112     |               |
| [Kernel/KeInitializeTimerEx](/wiki/Kernel/KeInitializeTimerEx "wikilink")                                 | 113     |               |
| [Kernel/KeInsertByKeyDeviceQueue](/wiki/Kernel/KeInsertByKeyDeviceQueue "wikilink")                       | 114     |               |
| [Kernel/KeInsertDeviceQueue](/wiki/Kernel/KeInsertDeviceQueue "wikilink")                                 | 115     |               |
| [Kernel/KeInsertHeadQueue](/wiki/Kernel/KeInsertHeadQueue "wikilink")                                     | 116     |               |
| [Kernel/KeInsertQueue](/wiki/Kernel/KeInsertQueue "wikilink")                                             | 117     |               |
| [Kernel/KeInsertQueueApc](/wiki/Kernel/KeInsertQueueApc "wikilink")                                       | 118     |               |
| [Kernel/KeInsertQueueDpc](/wiki/Kernel/KeInsertQueueDpc "wikilink")                                       | 119     |               |
| [Kernel/KeInterruptTime](/wiki/Kernel/KeInterruptTime "wikilink")                                         | 120     | Variable?     |
| [Kernel/KeIsExecutingDpc](/wiki/Kernel/KeIsExecutingDpc "wikilink")                                       | 121     |               |
| [Kernel/KeLeaveCriticalRegion](/wiki/Kernel/KeLeaveCriticalRegion "wikilink")                             | 122     |               |
| [Kernel/KePulseEvent](/wiki/Kernel/KePulseEvent "wikilink")                                               | 123     |               |
| [Kernel/KeQueryBasePriorityThread](/wiki/Kernel/KeQueryBasePriorityThread "wikilink")                     | 124     |               |
| [Kernel/KeQueryInterruptTime](/wiki/Kernel/KeQueryInterruptTime "wikilink")                               | 125     |               |
| [Kernel/KeQueryPerformanceCounter](/wiki/Kernel/KeQueryPerformanceCounter "wikilink")                     | 126     |               |
| [Kernel/KeQueryPerformanceFrequency](/wiki/Kernel/KeQueryPerformanceFrequency "wikilink")                 | 127     |               |
| [Kernel/KeQuerySystemTime](/wiki/Kernel/KeQuerySystemTime "wikilink")                                     | 128     |               |
| [Kernel/KeRaiseIrqlToDpcLevel](/wiki/Kernel/KeRaiseIrqlToDpcLevel "wikilink")                             | 129     |               |
| [Kernel/KeRaiseIrqlToSynchLevel](/wiki/Kernel/KeRaiseIrqlToSynchLevel "wikilink")                         | 130     |               |
| [Kernel/KeReleaseMutant](/wiki/Kernel/KeReleaseMutant "wikilink")                                         | 131     |               |
| [Kernel/KeReleaseSemaphore](/wiki/Kernel/KeReleaseSemaphore "wikilink")                                   | 132     |               |
| [Kernel/KeRemoveByKeyDeviceQueue](/wiki/Kernel/KeRemoveByKeyDeviceQueue "wikilink")                       | 133     |               |
| [Kernel/KeRemoveDeviceQueue](/wiki/Kernel/KeRemoveDeviceQueue "wikilink")                                 | 134     |               |
| [Kernel/KeRemoveEntryDeviceQueue](/wiki/Kernel/KeRemoveEntryDeviceQueue "wikilink")                       | 135     |               |
| [Kernel/KeRemoveQueue](/wiki/Kernel/KeRemoveQueue "wikilink")                                             | 136     |               |
| [Kernel/KeRemoveQueueDpc](/wiki/Kernel/KeRemoveQueueDpc "wikilink")                                       | 137     |               |
| [Kernel/KeResetEvent](/wiki/Kernel/KeResetEvent "wikilink")                                               | 138     |               |
| [Kernel/KeRestoreFloatingPointState](/wiki/Kernel/KeRestoreFloatingPointState "wikilink")                 | 139     |               |
| [Kernel/KeResumeThread](/wiki/Kernel/KeResumeThread "wikilink")                                           | 140     |               |
| [Kernel/KeRundownQueue](/wiki/Kernel/KeRundownQueue "wikilink")                                           | 141     |               |
| [Kernel/KeSaveFloatingPointState](/wiki/Kernel/KeSaveFloatingPointState "wikilink")                       | 142     |               |
| [Kernel/KeSetBasePriorityThread](/wiki/Kernel/KeSetBasePriorityThread "wikilink")                         | 143     |               |
| [Kernel/KeSetDisableBoostThread](/wiki/Kernel/KeSetDisableBoostThread "wikilink")                         | 144     |               |
| [Kernel/KeSetEvent](/wiki/Kernel/KeSetEvent "wikilink")                                                   | 145     |               |
| [Kernel/KeSetEventBoostPriority](/wiki/Kernel/KeSetEventBoostPriority "wikilink")                         | 146     |               |
| [Kernel/KeSetPriorityProcess](/wiki/Kernel/KeSetPriorityProcess "wikilink")                               | 147     |               |
| [Kernel/KeSetPriorityThread](/wiki/Kernel/KeSetPriorityThread "wikilink")                                 | 148     |               |
| [Kernel/KeSetTimer](/wiki/Kernel/KeSetTimer "wikilink")                                                   | 149     |               |
| [Kernel/KeSetTimerEx](/wiki/Kernel/KeSetTimerEx "wikilink")                                               | 150     |               |
| [Kernel/KeStallExecutionProcessor](/wiki/Kernel/KeStallExecutionProcessor "wikilink")                     | 151     |               |
| [Kernel/KeSuspendThread](/wiki/Kernel/KeSuspendThread "wikilink")                                         | 152     |               |
| [Kernel/KeSynchronizeExecution](/wiki/Kernel/KeSynchronizeExecution "wikilink")                           | 153     |               |
| [Kernel/KeSystemTime](/wiki/Kernel/KeSystemTime "wikilink")                                               | 154     | Variable?     |
| [Kernel/KeTestAlertThread](/wiki/Kernel/KeTestAlertThread "wikilink")                                     | 155     |               |
| [Kernel/KeTickCount](/wiki/Kernel/KeTickCount "wikilink")                                                 | 156     | Variable?     |
| [Kernel/KeTimeIncrement](/wiki/Kernel/KeTimeIncrement "wikilink")                                         | 157     | Variable?     |
| [Kernel/KeWaitForMultipleObjects](/wiki/Kernel/KeWaitForMultipleObjects "wikilink")                       | 158     |               |
| [Kernel/KeWaitForSingleObject](/wiki/Kernel/KeWaitForSingleObject "wikilink")                             | 159     |               |
| [Kernel/KfRaiseIrql](/wiki/Kernel/KfRaiseIrql "wikilink")                                                 | 160     |               |
| [Kernel/KfLowerIrql](/wiki/Kernel/KfLowerIrql "wikilink")                                                 | 161     |               |
| [Kernel/KiBugCheckData](/wiki/Kernel/KiBugCheckData "wikilink")                                           | 162     | Variable?     |
| [Kernel/KiUnlockDispatcherDatabase](/wiki/Kernel/KiUnlockDispatcherDatabase "wikilink")                   | 163     |               |
| [Kernel/LaunchDataPage](/wiki/Kernel/LaunchDataPage "wikilink")                                           | 164     | Variable?     |
| [Kernel/MmAllocateContiguousMemory](/wiki/Kernel/MmAllocateContiguousMemory "wikilink")                   | 165     |               |
| [Kernel/MmAllocateContiguousMemoryEx](/wiki/Kernel/MmAllocateContiguousMemoryEx "wikilink")               | 166     |               |
| [Kernel/MmAllocateSystemMemory](/wiki/Kernel/MmAllocateSystemMemory "wikilink")                           | 167     |               |
| [Kernel/MmClaimGpuInstanceMemory](/wiki/Kernel/MmClaimGpuInstanceMemory "wikilink")                       | 168     |               |
| [Kernel/MmCreateKernelStack](/wiki/Kernel/MmCreateKernelStack "wikilink")                                 | 169     |               |
| [Kernel/MmDeleteKernelStack](/wiki/Kernel/MmDeleteKernelStack "wikilink")                                 | 170     |               |
| [Kernel/MmFreeContiguousMemory](/wiki/Kernel/MmFreeContiguousMemory "wikilink")                           | 171     |               |
| [Kernel/MmFreeSystemMemory](/wiki/Kernel/MmFreeSystemMemory "wikilink")                                   | 172     |               |
| [Kernel/MmGetPhysicalAddress](/wiki/Kernel/MmGetPhysicalAddress "wikilink")                               | 173     |               |
| [Kernel/MmIsAddressValid](/wiki/Kernel/MmIsAddressValid "wikilink")                                       | 174     |               |
| [Kernel/MmLockUnlockBufferPages](/wiki/Kernel/MmLockUnlockBufferPages "wikilink")                         | 175     |               |
| [Kernel/MmLockUnlockPhysicalPage](/wiki/Kernel/MmLockUnlockPhysicalPage "wikilink")                       | 176     |               |
| [Kernel/MmMapIoSpace](/wiki/Kernel/MmMapIoSpace "wikilink")                                               | 177     |               |
| [Kernel/MmPersistContiguousMemory](/wiki/Kernel/MmPersistContiguousMemory "wikilink")                     | 178     |               |
| [Kernel/MmQueryAddressProtect](/wiki/Kernel/MmQueryAddressProtect "wikilink")                             | 179     |               |
| [Kernel/MmQueryAllocationSize](/wiki/Kernel/MmQueryAllocationSize "wikilink")                             | 180     |               |
| [Kernel/MmQueryStatistics](/wiki/Kernel/MmQueryStatistics "wikilink")                                     | 181     |               |
| [Kernel/MmSetAddressProtect](/wiki/Kernel/MmSetAddressProtect "wikilink")                                 | 182     |               |
| [Kernel/MmUnmapIoSpace](/wiki/Kernel/MmUnmapIoSpace "wikilink")                                           | 183     |               |
| [Kernel/NtAllocateVirtualMemory](/wiki/Kernel/NtAllocateVirtualMemory "wikilink")                         | 184     |               |
| [Kernel/NtCancelTimer](/wiki/Kernel/NtCancelTimer "wikilink")                                             | 185     |               |
| [Kernel/NtClearEvent](/wiki/Kernel/NtClearEvent "wikilink")                                               | 186     |               |
| [Kernel/NtClose](/wiki/Kernel/NtClose "wikilink")                                                         | 187     |               |
| [Kernel/NtCreateDirectoryObject](/wiki/Kernel/NtCreateDirectoryObject "wikilink")                         | 188     |               |
| [Kernel/NtCreateEvent](/wiki/Kernel/NtCreateEvent "wikilink")                                             | 189     |               |
| [Kernel/NtCreateFile](/wiki/Kernel/NtCreateFile "wikilink")                                               | 190     |               |
| [Kernel/NtCreateIoCompletion](/wiki/Kernel/NtCreateIoCompletion "wikilink")                               | 191     |               |
| [Kernel/NtCreateMutant](/wiki/Kernel/NtCreateMutant "wikilink")                                           | 192     |               |
| [Kernel/NtCreateSemaphore](/wiki/Kernel/NtCreateSemaphore "wikilink")                                     | 193     |               |
| [Kernel/NtCreateTimer](/wiki/Kernel/NtCreateTimer "wikilink")                                             | 194     |               |
| [Kernel/NtDeleteFile](/wiki/Kernel/NtDeleteFile "wikilink")                                               | 195     |               |
| [Kernel/NtDeviceIoControlFile](/wiki/Kernel/NtDeviceIoControlFile "wikilink")                             | 196     |               |
| [Kernel/NtDuplicateObject](/wiki/Kernel/NtDuplicateObject "wikilink")                                     | 197     |               |
| [Kernel/NtFlushBuffersFile](/wiki/Kernel/NtFlushBuffersFile "wikilink")                                   | 198     |               |
| [Kernel/NtFreeVirtualMemory](/wiki/Kernel/NtFreeVirtualMemory "wikilink")                                 | 199     |               |
| [Kernel/NtFsControlFile](/wiki/Kernel/NtFsControlFile "wikilink")                                         | 200     |               |
| [Kernel/NtOpenDirectoryObject](/wiki/Kernel/NtOpenDirectoryObject "wikilink")                             | 201     |               |
| [Kernel/NtOpenFile](/wiki/Kernel/NtOpenFile "wikilink")                                                   | 202     |               |
| [Kernel/NtOpenSymbolicLinkObject](/wiki/Kernel/NtOpenSymbolicLinkObject "wikilink")                       | 203     |               |
| [Kernel/NtProtectVirtualMemory](/wiki/Kernel/NtProtectVirtualMemory "wikilink")                           | 204     |               |
| [Kernel/NtPulseEvent](/wiki/Kernel/NtPulseEvent "wikilink")                                               | 205     |               |
| [Kernel/NtQueueApcThread](/wiki/Kernel/NtQueueApcThread "wikilink")                                       | 206     |               |
| [Kernel/NtQueryDirectoryFile](/wiki/Kernel/NtQueryDirectoryFile "wikilink")                               | 207     |               |
| [Kernel/NtQueryDirectoryObject](/wiki/Kernel/NtQueryDirectoryObject "wikilink")                           | 208     |               |
| [Kernel/NtQueryEvent](/wiki/Kernel/NtQueryEvent "wikilink")                                               | 209     |               |
| [Kernel/NtQueryFullAttributesFile](/wiki/Kernel/NtQueryFullAttributesFile "wikilink")                     | 210     |               |
| [Kernel/NtQueryInformationFile](/wiki/Kernel/NtQueryInformationFile "wikilink")                           | 211     |               |
| [Kernel/NtQueryIoCompletion](/wiki/Kernel/NtQueryIoCompletion "wikilink")                                 | 212     |               |
| [Kernel/NtQueryMutant](/wiki/Kernel/NtQueryMutant "wikilink")                                             | 213     |               |
| [Kernel/NtQuerySemaphore](/wiki/Kernel/NtQuerySemaphore "wikilink")                                       | 214     |               |
| [Kernel/NtQuerySymbolicLinkObject](/wiki/Kernel/NtQuerySymbolicLinkObject "wikilink")                     | 215     |               |
| [Kernel/NtQueryTimer](/wiki/Kernel/NtQueryTimer "wikilink")                                               | 216     |               |
| [Kernel/NtQueryVirtualMemory](/wiki/Kernel/NtQueryVirtualMemory "wikilink")                               | 217     |               |
| [Kernel/NtQueryVolumeInformationFile](/wiki/Kernel/NtQueryVolumeInformationFile "wikilink")               | 218     |               |
| [Kernel/NtReadFile](/wiki/Kernel/NtReadFile "wikilink")                                                   | 219     |               |
| [Kernel/NtReadFileScatter](/wiki/Kernel/NtReadFileScatter "wikilink")                                     | 220     |               |
| [Kernel/NtReleaseMutant](/wiki/Kernel/NtReleaseMutant "wikilink")                                         | 221     |               |
| [Kernel/NtReleaseSemaphore](/wiki/Kernel/NtReleaseSemaphore "wikilink")                                   | 222     |               |
| [Kernel/NtRemoveIoCompletion](/wiki/Kernel/NtRemoveIoCompletion "wikilink")                               | 223     |               |
| [Kernel/NtResumeThread](/wiki/Kernel/NtResumeThread "wikilink")                                           | 224     |               |
| [Kernel/NtSetEvent](/wiki/Kernel/NtSetEvent "wikilink")                                                   | 225     |               |
| [Kernel/NtSetInformationFile](/wiki/Kernel/NtSetInformationFile "wikilink")                               | 226     |               |
| [Kernel/NtSetIoCompletion](/wiki/Kernel/NtSetIoCompletion "wikilink")                                     | 227     |               |
| [Kernel/NtSetSystemTime](/wiki/Kernel/NtSetSystemTime "wikilink")                                         | 228     |               |
| [Kernel/NtSetTimerEx](/wiki/Kernel/NtSetTimerEx "wikilink")                                               | 229     |               |
| [Kernel/NtSignalAndWaitForSingleObjectEx](/wiki/Kernel/NtSignalAndWaitForSingleObjectEx "wikilink")       | 230     |               |
| [Kernel/NtSuspendThread](/wiki/Kernel/NtSuspendThread "wikilink")                                         | 231     |               |
| [Kernel/NtUserIoApcDispatcher](/wiki/Kernel/NtUserIoApcDispatcher "wikilink")                             | 232     |               |
| [Kernel/NtWaitForSingleObject](/wiki/Kernel/NtWaitForSingleObject "wikilink")                             | 233     |               |
| [Kernel/NtWaitForSingleObjectEx](/wiki/Kernel/NtWaitForSingleObjectEx "wikilink")                         | 234     |               |
| [Kernel/NtWaitForMultipleObjectsEx](/wiki/Kernel/NtWaitForMultipleObjectsEx "wikilink")                   | 235     |               |
| [Kernel/NtWriteFile](/wiki/Kernel/NtWriteFile "wikilink")                                                 | 236     |               |
| [Kernel/NtWriteFileGather](/wiki/Kernel/NtWriteFileGather "wikilink")                                     | 237     |               |
| [Kernel/NtYieldExecution](/wiki/Kernel/NtYieldExecution "wikilink")                                       | 238     |               |
| [Kernel/ObCreateObject](/wiki/Kernel/ObCreateObject "wikilink")                                           | 239     |               |
| [Kernel/ObDirectoryObjectType](/wiki/Kernel/ObDirectoryObjectType "wikilink")                             | 240     | Variable?     |
| [Kernel/ObInsertObject](/wiki/Kernel/ObInsertObject "wikilink")                                           | 241     |               |
| [Kernel/ObMakeTemporaryObject](/wiki/Kernel/ObMakeTemporaryObject "wikilink")                             | 242     |               |
| [Kernel/ObOpenObjectByName](/wiki/Kernel/ObOpenObjectByName "wikilink")                                   | 243     |               |
| [Kernel/ObOpenObjectByPointer](/wiki/Kernel/ObOpenObjectByPointer "wikilink")                             | 244     |               |
| [Kernel/ObpObjectHandleTable](/wiki/Kernel/ObpObjectHandleTable "wikilink")                               | 245     | Variable?     |
| [Kernel/ObReferenceObjectByHandle](/wiki/Kernel/ObReferenceObjectByHandle "wikilink")                     | 246     |               |
| [Kernel/ObReferenceObjectByName](/wiki/Kernel/ObReferenceObjectByName "wikilink")                         | 247     |               |
| [Kernel/ObReferenceObjectByPointer](/wiki/Kernel/ObReferenceObjectByPointer "wikilink")                   | 248     |               |
| [Kernel/ObSymbolicLinkObjectType](/wiki/Kernel/ObSymbolicLinkObjectType "wikilink")                       | 249     | Variable?     |
| [Kernel/ObfDereferenceObject](/wiki/Kernel/ObfDereferenceObject "wikilink")                               | 250     |               |
| [Kernel/ObfReferenceObject](/wiki/Kernel/ObfReferenceObject "wikilink")                                   | 251     |               |
| [Kernel/PhyGetLinkState](/wiki/Kernel/PhyGetLinkState "wikilink")                                         | 252     |               |
| [Kernel/PhyInitialize](/wiki/Kernel/PhyInitialize "wikilink")                                             | 253     |               |
| [Kernel/PsCreateSystemThread](/wiki/Kernel/PsCreateSystemThread "wikilink")                               | 254     |               |
| [Kernel/PsCreateSystemThreadEx](/wiki/Kernel/PsCreateSystemThreadEx "wikilink")                           | 255     |               |
| [Kernel/PsQueryStatistics](/wiki/Kernel/PsQueryStatistics "wikilink")                                     | 256     |               |
| [Kernel/PsSetCreateThreadNotifyRoutine](/wiki/Kernel/PsSetCreateThreadNotifyRoutine "wikilink")           | 257     |               |
| [Kernel/PsTerminateSystemThread](/wiki/Kernel/PsTerminateSystemThread "wikilink")                         | 258     |               |
| [Kernel/PsThreadObjectType](/wiki/Kernel/PsThreadObjectType "wikilink")                                   | 259     | Variable?     |
| [Kernel/RtlAnsiStringToUnicodeString](/wiki/Kernel/RtlAnsiStringToUnicodeString "wikilink")               | 260     |               |
| [Kernel/RtlAppendStringToString](/wiki/Kernel/RtlAppendStringToString "wikilink")                         | 261     |               |
| [Kernel/RtlAppendUnicodeStringToString](/wiki/Kernel/RtlAppendUnicodeStringToString "wikilink")           | 262     |               |
| [Kernel/RtlAppendUnicodeToString](/wiki/Kernel/RtlAppendUnicodeToString "wikilink")                       | 263     |               |
| [Kernel/RtlAssert](/wiki/Kernel/RtlAssert "wikilink")                                                     | 264     |               |
| [Kernel/RtlCaptureContext](/wiki/Kernel/RtlCaptureContext "wikilink")                                     | 265     |               |
| [Kernel/RtlCaptureStackBackTrace](/wiki/Kernel/RtlCaptureStackBackTrace "wikilink")                       | 266     |               |
| [Kernel/RtlCharToInteger](/wiki/Kernel/RtlCharToInteger "wikilink")                                       | 267     |               |
| [Kernel/RtlCompareMemory](/wiki/Kernel/RtlCompareMemory "wikilink")                                       | 268     |               |
| [Kernel/RtlCompareMemoryUlong](/wiki/Kernel/RtlCompareMemoryUlong "wikilink")                             | 269     |               |
| [Kernel/RtlCompareString](/wiki/Kernel/RtlCompareString "wikilink")                                       | 270     |               |
| [Kernel/RtlCompareUnicodeString](/wiki/Kernel/RtlCompareUnicodeString "wikilink")                         | 271     |               |
| [Kernel/RtlCopyString](/wiki/Kernel/RtlCopyString "wikilink")                                             | 272     |               |
| [Kernel/RtlCopyUnicodeString](/wiki/Kernel/RtlCopyUnicodeString "wikilink")                               | 273     |               |
| [Kernel/RtlCreateUnicodeString](/wiki/Kernel/RtlCreateUnicodeString "wikilink")                           | 274     |               |
| [Kernel/RtlDowncaseUnicodeChar](/wiki/Kernel/RtlDowncaseUnicodeChar "wikilink")                           | 275     |               |
| [Kernel/RtlDowncaseUnicodeString](/wiki/Kernel/RtlDowncaseUnicodeString "wikilink")                       | 276     |               |
| [Kernel/RtlEnterCriticalSection](/wiki/Kernel/RtlEnterCriticalSection "wikilink")                         | 277     |               |
| [Kernel/RtlEnterCriticalSectionAndRegion](/wiki/Kernel/RtlEnterCriticalSectionAndRegion "wikilink")       | 278     |               |
| [Kernel/RtlEqualString](/wiki/Kernel/RtlEqualString "wikilink")                                           | 279     |               |
| [Kernel/RtlEqualUnicodeString](/wiki/Kernel/RtlEqualUnicodeString "wikilink")                             | 280     |               |
| [Kernel/RtlExtendedIntegerMultiply](/wiki/Kernel/RtlExtendedIntegerMultiply "wikilink")                   | 281     |               |
| [Kernel/RtlExtendedLargeIntegerDivide](/wiki/Kernel/RtlExtendedLargeIntegerDivide "wikilink")             | 282     |               |
| [Kernel/RtlExtendedMagicDivide](/wiki/Kernel/RtlExtendedMagicDivide "wikilink")                           | 283     |               |
| [Kernel/RtlFillMemory](/wiki/Kernel/RtlFillMemory "wikilink")                                             | 284     |               |
| [Kernel/RtlFillMemoryUlong](/wiki/Kernel/RtlFillMemoryUlong "wikilink")                                   | 285     |               |
| [Kernel/RtlFreeAnsiString](/wiki/Kernel/RtlFreeAnsiString "wikilink")                                     | 286     |               |
| [Kernel/RtlFreeUnicodeString](/wiki/Kernel/RtlFreeUnicodeString "wikilink")                               | 287     |               |
| [Kernel/RtlGetCallersAddress](/wiki/Kernel/RtlGetCallersAddress "wikilink")                               | 288     |               |
| [Kernel/RtlInitAnsiString](/wiki/Kernel/RtlInitAnsiString "wikilink")                                     | 289     |               |
| [Kernel/RtlInitUnicodeString](/wiki/Kernel/RtlInitUnicodeString "wikilink")                               | 290     |               |
| [Kernel/RtlInitializeCriticalSection](/wiki/Kernel/RtlInitializeCriticalSection "wikilink")               | 291     |               |
| [Kernel/RtlIntegerToChar](/wiki/Kernel/RtlIntegerToChar "wikilink")                                       | 292     |               |
| [Kernel/RtlIntegerToUnicodeString](/wiki/Kernel/RtlIntegerToUnicodeString "wikilink")                     | 293     |               |
| [Kernel/RtlLeaveCriticalSection](/wiki/Kernel/RtlLeaveCriticalSection "wikilink")                         | 294     |               |
| [Kernel/RtlLeaveCriticalSectionAndRegion](/wiki/Kernel/RtlLeaveCriticalSectionAndRegion "wikilink")       | 295     |               |
| [Kernel/RtlLowerChar](/wiki/Kernel/RtlLowerChar "wikilink")                                               | 296     |               |
| [Kernel/RtlMapGenericMask](/wiki/Kernel/RtlMapGenericMask "wikilink")                                     | 297     |               |
| [Kernel/RtlMoveMemory](/wiki/Kernel/RtlMoveMemory "wikilink")                                             | 298     |               |
| [Kernel/RtlMultiByteToUnicodeN](/wiki/Kernel/RtlMultiByteToUnicodeN "wikilink")                           | 299     |               |
| [Kernel/RtlMultiByteToUnicodeSize](/wiki/Kernel/RtlMultiByteToUnicodeSize "wikilink")                     | 300     |               |
| [Kernel/RtlNtStatusToDosError](/wiki/Kernel/RtlNtStatusToDosError "wikilink")                             | 301     |               |
| [Kernel/RtlRaiseException](/wiki/Kernel/RtlRaiseException "wikilink")                                     | 302     |               |
| [Kernel/RtlRaiseStatus](/wiki/Kernel/RtlRaiseStatus "wikilink")                                           | 303     |               |
| [Kernel/RtlTimeFieldsToTime](/wiki/Kernel/RtlTimeFieldsToTime "wikilink")                                 | 304     |               |
| [Kernel/RtlTimeToTimeFields](/wiki/Kernel/RtlTimeToTimeFields "wikilink")                                 | 305     |               |
| [Kernel/RtlTryEnterCriticalSection](/wiki/Kernel/RtlTryEnterCriticalSection "wikilink")                   | 306     |               |
| [Kernel/RtlUlongByteSwap](/wiki/Kernel/RtlUlongByteSwap "wikilink")                                       | 307     |               |
| [Kernel/RtlUnicodeStringToAnsiString](/wiki/Kernel/RtlUnicodeStringToAnsiString "wikilink")               | 308     |               |
| [Kernel/RtlUnicodeStringToInteger](/wiki/Kernel/RtlUnicodeStringToInteger "wikilink")                     | 309     |               |
| [Kernel/RtlUnicodeToMultiByteN](/wiki/Kernel/RtlUnicodeToMultiByteN "wikilink")                           | 310     |               |
| [Kernel/RtlUnicodeToMultiByteSize](/wiki/Kernel/RtlUnicodeToMultiByteSize "wikilink")                     | 311     |               |
| [Kernel/RtlUnwind](/wiki/Kernel/RtlUnwind "wikilink")                                                     | 312     |               |
| [Kernel/RtlUpcaseUnicodeChar](/wiki/Kernel/RtlUpcaseUnicodeChar "wikilink")                               | 313     |               |
| [Kernel/RtlUpcaseUnicodeString](/wiki/Kernel/RtlUpcaseUnicodeString "wikilink")                           | 314     |               |
| [Kernel/RtlUpcaseUnicodeToMultiByteN](/wiki/Kernel/RtlUpcaseUnicodeToMultiByteN "wikilink")               | 315     |               |
| [Kernel/RtlUpperChar](/wiki/Kernel/RtlUpperChar "wikilink")                                               | 316     |               |
| [Kernel/RtlUpperString](/wiki/Kernel/RtlUpperString "wikilink")                                           | 317     |               |
| [Kernel/RtlUshortByteSwap](/wiki/Kernel/RtlUshortByteSwap "wikilink")                                     | 318     |               |
| [Kernel/RtlWalkFrameChain](/wiki/Kernel/RtlWalkFrameChain "wikilink")                                     | 319     |               |
| [Kernel/RtlZeroMemory](/wiki/Kernel/RtlZeroMemory "wikilink")                                             | 320     |               |
| [Kernel/XboxEEPROMKey](/wiki/Kernel/XboxEEPROMKey "wikilink")                                             | 321     | Variable?     |
| [Kernel/XboxHardwareInfo](/wiki/Kernel/XboxHardwareInfo "wikilink")                                       | 322     | Variable?     |
| [Kernel/XboxHDKey](/wiki/Kernel/XboxHDKey "wikilink")                                                     | 323     | Variable?     |
| [Kernel/XboxKrnlVersion](/wiki/Kernel/XboxKrnlVersion "wikilink")                                         | 324     | Variable?     |
| [Kernel/XboxSignatureKey](/wiki/Kernel/XboxSignatureKey "wikilink")                                       | 325     | Variable?     |
| [Kernel/XeImageFileName](/wiki/Kernel/XeImageFileName "wikilink")                                         | 326     | Variable?     |
| [Kernel/XeLoadSection](/wiki/Kernel/XeLoadSection "wikilink")                                             | 327     |               |
| [Kernel/XeUnloadSection](/wiki/Kernel/XeUnloadSection "wikilink")                                         | 328     |               |
| [Kernel/READ\_PORT\_BUFFER\_UCHAR](/wiki/Kernel/READ_PORT_BUFFER_UCHAR "wikilink")                        | 329     |               |
| [Kernel/READ\_PORT\_BUFFER\_USHORT](/wiki/Kernel/READ_PORT_BUFFER_USHORT "wikilink")                      | 330     |               |
| [Kernel/READ\_PORT\_BUFFER\_ULONG](/wiki/Kernel/READ_PORT_BUFFER_ULONG "wikilink")                        | 331     |               |
| [Kernel/WRITE\_PORT\_BUFFER\_UCHAR](/wiki/Kernel/WRITE_PORT_BUFFER_UCHAR "wikilink")                      | 332     |               |
| [Kernel/WRITE\_PORT\_BUFFER\_USHORT](/wiki/Kernel/WRITE_PORT_BUFFER_USHORT "wikilink")                    | 333     |               |
| [Kernel/WRITE\_PORT\_BUFFER\_ULONG](/wiki/Kernel/WRITE_PORT_BUFFER_ULONG "wikilink")                      | 334     |               |
| [Kernel/XcSHAInit](/wiki/Kernel/XcSHAInit "wikilink")                                                     | 335     |               |
| [Kernel/XcSHAUpdate](/wiki/Kernel/XcSHAUpdate "wikilink")                                                 | 336     |               |
| [Kernel/XcSHAFinal](/wiki/Kernel/XcSHAFinal "wikilink")                                                   | 337     |               |
| [Kernel/XcRC4Key](/wiki/Kernel/XcRC4Key "wikilink")                                                       | 338     |               |
| [Kernel/XcRC4Crypt](/wiki/Kernel/XcRC4Crypt "wikilink")                                                   | 339     |               |
| [Kernel/XcHMAC](/wiki/Kernel/XcHMAC "wikilink")                                                           | 340     |               |
| [Kernel/XcPKEncPublic](/wiki/Kernel/XcPKEncPublic "wikilink")                                             | 341     |               |
| [Kernel/XcPKDecPrivate](/wiki/Kernel/XcPKDecPrivate "wikilink")                                           | 342     |               |
| [Kernel/XcPKGetKeyLen](/wiki/Kernel/XcPKGetKeyLen "wikilink")                                             | 343     |               |
| [Kernel/XcVerifyPKCS1Signature](/wiki/Kernel/XcVerifyPKCS1Signature "wikilink")                           | 344     |               |
| [Kernel/XcModExp](/wiki/Kernel/XcModExp "wikilink")                                                       | 345     |               |
| [Kernel/XcDESKeyParity](/wiki/Kernel/XcDESKeyParity "wikilink")                                           | 346     |               |
| [Kernel/XcKeyTable](/wiki/Kernel/XcKeyTable "wikilink")                                                   | 347     |               |
| [Kernel/XcBlockCrypt](/wiki/Kernel/XcBlockCrypt "wikilink")                                               | 348     |               |
| [Kernel/XcBlockCryptCBC](/wiki/Kernel/XcBlockCryptCBC "wikilink")                                         | 349     |               |
| [Kernel/XcCryptService](/wiki/Kernel/XcCryptService "wikilink")                                           | 350     |               |
| [Kernel/XcUpdateCrypto](/wiki/Kernel/XcUpdateCrypto "wikilink")                                           | 351     |               |
| [Kernel/RtlRip](/wiki/Kernel/RtlRip "wikilink")                                                           | 352     |               |
| [Kernel/XboxLANKey](/wiki/Kernel/XboxLANKey "wikilink")                                                   | 353     |               |
| [Kernel/XboxAlternateSignatureKeys](/wiki/Kernel/XboxAlternateSignatureKeys "wikilink")                   | 354     | Variable?     |
| [Kernel/XePublicKeyData](/wiki/Kernel/XePublicKeyData "wikilink")                                         | 355     | Variable?     |
| [Kernel/HalBootSMCVideoMode](/wiki/Kernel/HalBootSMCVideoMode "wikilink")                                 | 356     | Variable?     |
| [Kernel/IdexChannelObject](/wiki/Kernel/IdexChannelObject "wikilink")                                     | 357     | Variable?     |
| [Kernel/HalIsResetOrShutdownPending](/wiki/Kernel/HalIsResetOrShutdownPending "wikilink")                 | 358     |               |
| [Kernel/IoMarkIrpMustComplete](/wiki/Kernel/IoMarkIrpMustComplete "wikilink")                             | 359     |               |
| [Kernel/HalInitiateShutdown](/wiki/Kernel/HalInitiateShutdown "wikilink")                                 | 360     |               |
| [Kernel/RtlSnprintf](/wiki/Kernel/RtlSnprintf "wikilink")                                                 | 361     | Unused?       |
| [Kernel/RtlSprintf](/wiki/Kernel/RtlSprintf "wikilink")                                                   | 362     | Unused?       |
| [Kernel/RtlVsnprintf](/wiki/Kernel/RtlVsnprintf "wikilink")                                               | 363     | Unused?       |
| [Kernel/RtlVsprintf](/wiki/Kernel/RtlVsprintf "wikilink")                                                 | 364     | Unused?       |
| [Kernel/HalEnableSecureTrayEject](/wiki/Kernel/HalEnableSecureTrayEject "wikilink")                       | 365     |               |
| [Kernel/HalWriteSMCScratchRegister](/wiki/Kernel/HalWriteSMCScratchRegister "wikilink")                   | 366     |               |
|                                                                                                     | 367     | Unused?       |
|                                                                                                     | 368     | Unused?       |
|                                                                                                     | 369     | Unused?       |
|                                                                                                     | 370     | Unused?       |
|                                                                                                     | 371     | Unused?       |
|                                                                                                     | 372     | Unused?       |
|                                                                                                     | 373     | Unused?       |
| [Kernel/MmDbgAllocateMemory](/wiki/Kernel/MmDbgAllocateMemory "wikilink")                                 | 374     | Devkits only! |
| [Kernel/MmDbgFreeMemory](/wiki/Kernel/MmDbgFreeMemory "wikilink")                                         | 375     | Devkits only! |
| [Kernel/MmDbgQueryAvailablePages](/wiki/Kernel/MmDbgQueryAvailablePages "wikilink")                       | 376     | Devkits only! |
| [Kernel/MmDbgReleaseAddress](/wiki/Kernel/MmDbgReleaseAddress "wikilink")                                 | 377     | Devkits only! |
| [Kernel/MmDbgWriteCheck](/wiki/Kernel/MmDbgWriteCheck "wikilink")                                         | 378     | Devkits only! |

See Also
--------

[Hard Drive Files](/wiki/Hard_Drive_Files "wikilink")
