# WHvGetPartitionProperty
**Note: These APIs are not yet publically available and will be included in a future Windows release.**

## Syntax
```C
typedef VOID* WHV_PARTITION_HANDLE;

typedef enum WHV_PARTITION_PROPERTY_CODE
{
    WHvPartitionPropertyCodeExtendedVmExits         = 0x00000001,

    WHvPartitionPropertyCodeProcessorVendor         = 0x00001000,
    WHvPartitionPropertyCodeProcessorFeatures       = 0x00001001,
    WHvPartitionPropertyCodeProcessorClFlushSize    = 0x00001002,

    WHvPartitionPropertyCodeProcessorCount          = 0x00001fff
} WHV_PARTITION_PROPERTY_CODE;

//
// WHvGetPartitionProperty output buffer / WHvSetPartitionProperty input buffer
//
typedef struct WHV_PARTITION_PROPERTY
{
    WHV_PARTITION_PROPERTY_CODE PropertyCode;

    union
    {
         WHV_EXTENDED_VM_EXITS ExtendedVmExits;
         WHV_PROCESSOR_VENDOR ProcessorVendor;
         WHV_PROCESSOR_FEATURES ProcessorFeatures;
         UINT8 ProcessorClFlushSize;
         UINT32 ProcessorCount;
    };
} WHV_PARTITION_PROPERTY;

HRESULT
WINAPI
WHvGetPartitionProperty(
    _In_ WHV_PARTITION_HANDLE Partition,
    _In_ WHV_PARTITION_PROPERTY_CODE PropertyCode,
    _Out_writes_bytes_(PropertyBufferSizeInBytes) VOID* PropertyBuffer,
    _In_ UINT32 PropertyBufferSizeInBytes
    );
```
### Parameters

`Partition`

Handle to the partition object

`PropertyCode`

Specifies the property that is queried

`PropertyBuffer` 

Specifies the output buffer that receives the value of the requested property. 

`PropertyBufferSizeInBytes` 

Specifies the size of the output buffer, in bytes. For the currently available set of properties, the buffer should be large enough to hold a 64-bit value.  

## Remarks

The `WHvPartitionCodeExtendedVmExits` property controls the set of additional operations by a virtual processor that should cause the execution of the processor to exit and to return to the caller of the [`WHvRunVirtualProcessor`](WHvRunVirtualProcessor.md) function.

The `WHvPartitionPropertyCodeProcessorXXX` properties control the processor features that are made available to the virtual processor of the partition. These properties can only be configured during the initial creation of the partition, prior to calling `WHvSetupPartition`.