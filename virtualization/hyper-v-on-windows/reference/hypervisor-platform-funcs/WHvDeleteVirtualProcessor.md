# WHvDeleteVirtualProcessor
**Note: These APIs are not yet publically available and will be included in a future Windows release.**

## Syntax

```C
HRESULT
WINAPI
WHvDeleteVirtualProcessor(
    _In_ WHV_PARTITION_HANDLE Partition,
    _In_ UINT32 VpIndex
    );
```

### Parameters

`Partition` 

Handle to the partition object

`VpIndex`

 Specifies the index of the virtual processor that is deleted
  

## Remarks

The `WHvDeleteVirtualProcessor` function deletes a virtual processor in a partition. 
