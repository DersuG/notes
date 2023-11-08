# Direct-Mapped Cache

## Direct-mapped cache
- simplest, efficient, and common
- each access is mapped to 1 location by the last few low-order bits
    - `mem_address % num_blocks`
    - so several addresses map to each block
- **cache index:** the lower-order bits
- **byte offset:** determines the byte offset of the start of the block in memory (?)
    - e.g. for block size of 4 words (16 bytes), byte offset needs 4 bits
- if number of cache blocks is a power of 2: `num_index_bits = log2(num_blocks)`
    - e.g. for 8 blocks (2^3), cache index is 3 bits
- **tags:** upper portion of cache entry that specifies which of the possible addresses that can map to this entry currently does
    - `num_tag_bits = num_mem_bits - num_cache_bits`
    - e.g. if 8 addresses (2^3) can map to one index, the tag is 16 bits
- **valid bit:** attached to each block, indicates if it's valid
- longer cache blocks take advantage of spatial locality
- if both index and tag matches the address, it's a hit

### E.g. MIPS direct-mapped cache
- each block is 1 word
- cache size is 1K (2^10) words
- cache index is 10 bits
- cache tag is 20 bits
- byte offset is 2 bits
- `[tag:20] [index:10] [offset:2]`

### E.g. multi-block direct-mapped cache
- each block is 4 words
- cache size is 1K (2^10) words
- cache index is 256 (2^8) bits, each for 4 words
- cache tag is 20 bits
- byte offset is 4 bits
- `[tag:20] [index:8] [offset:4]`

## Miss rate vs. block size vs. cache size
- as block size becomes a significant fraction of cache size, miss rate goes up
    - increases conflict misses

## Cache field sizes
- includes storage for data, tags, and valid bit
- `cache_size = 2^(num_index_bits + num_offset_bits)`
- e.g. for 2^n words (2^(n+2) bytes), n bits are used to address the word within the block and 2 bits are used to address the 4 bytes within the word
- for direct-mapped: `num_cache_bits = num_blocks * (block_size_bits + num_tag_bits + field_size_bits)`

