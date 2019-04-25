# WHvMapGpaRange


## Syntax
```C
// Guest physical or virtual address
typedef UINT64 WHV_GUEST_PHYSICAL_ADDRESS;
typedef UINT64 WHV_GUEST_VIRTUAL_ADDRESS;


// Flags used by WHvMapGpaRange
typedef enum WHV_MAP_GPA_RANGE_FLAGS
{
    WHvMapGpaRangeFlagNone              = 0x00000000,
    WHvMapGpaRangeFlagRead              = 0x00000001,
    WHvMapGpaRangeFlagWrite             = 0x00000002,
    WHvMapGpaRangeFlagExecute           = 0x00000004,
    WHvMapGpaRangeFlagTrackDirtyPages   = 0x00000008,
} WHV_MAP_GPA_RANGE_FLAGS;

DEFINE_ENUM_FLAG_OPERATORS(WHV_MAP_GPA_RANGE_FLAGS);

HRESULT
WINAPI
WHvMapGpaRange(
    _In_ WHV_PARTITION_HANDLE Partition,
    _In_ VOID* SourceAddress,
    _In_ WHV_GUEST_PHYSICAL_ADDRESS GuestAddress,
    _In_ UINT64 SizeInBytes,
    _In_ WHV_MAP_GPA_RANGE_FLAGS Flags
    );
```
### Parameters

`Partition`

Handle to the partition object

`SourceAddress`

Specifies the page-aligned address of the memory region in the caller’s process that is the source of the mapping

`GuestAddress`

Specifies the destination address in the VM’s physical address space

`SizeInBytes`

Specifies the number of bytes that are to be mapped

`Flags`

Specifies the access flags for the mapping

## Remarks

Creating a mapping for a range in the GPA space of a partition sets a region in the caller’s process as the backing memory for that range. The operation replaces any previous mappings for the specified GPA pages.
