# WHvUnmapGpaRange
**Note: These APIs are not yet publicly available and will be included in a future Windows release.**

## Syntax
```C
HRESULT
WINAPI
WHvUnmapGpaRange(
    _In_ WHV_PARTITION_HANDLE Partition,
    _In_ WHV_GUEST_PHYSICAL_ADDRESS GuestAddress,
    _In_ UINT64 SizeInBytes
    );
```
### Parameters

`Partition`

Handle to the partition object

`GuestAddress`

Specifies the start address of the region in the VM’s physical address space that is unmapped

`SizeInBytes`

Specifies the number of pages that are unmapped
  

## Remarks

Unmapping a previously mapped GPA range (or parts of it) makes the memory range unavailable to the partition. Any further access by a virtual processor to the range will result in a memory access exit. 
