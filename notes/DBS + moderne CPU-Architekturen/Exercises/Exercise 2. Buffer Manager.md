see
- [[1.1 Storage#Buffer Management]]
- [[1.1 Storage#Buffer Management#2Q]]

*Reminders*
- *segments contain pages*

>[!info] split function already there
>`get_segment_id` and `get_segment_page_id`

# Tips:

>[!idea] general approach
>- global mutex so that tests work, then improve
>- from coarse to fine-grained blocking
>- ignore multi-threaded test cases at first

additional members in buffer frame: probably everything from slide 17 (except LSN)

how much memory do we need
`page_count`: 10? times `page_size`
-> constructor: allocate memory

`fix_page`
- load if necessary, return locked frame if possible (?)

`unfix_page`
- make slot available to other pages

>[!tip] performant version: only write to disk if necessary, aka if buffer is full (instead of every time when unfixing)

`get_fifo_list` and `get_lru_queue`
- mostly for testing purposes
- should be updated according to strategy

tests also check if memory is written correctly

holding latches
- when reading/writing data, other threads should not be blocked from accessing the buffer manager
- lines 400