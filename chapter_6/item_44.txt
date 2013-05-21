// Item 44

// Waiting on a group of tasks
dispatch_queue_t queue = dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0);
dispatch_group_t dispatchGroup = dispatch_group_create();

for (id object in collection) {
    dispatch_group_async(dispatchGroup,
                         queue,
                         ^{
                              [object performTask];
                          });
}

dispatch_group_wait(dispatchGroup, DISPATCH_TIME_FOREVER);

// Continue processing after completing tasks


// Using `dispatch_notify'
dispatch_queue_t notifyQueue = dispatch_get_main_queue();
dispatch_group_notify(dispatchGroup,
                      notifyQueue,
                      ^{
                           // Continue processing after completing tasks
                       });


// Multiple queues, same group
dispatch_queue_t lowPriorityQueue = dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_LOW, 0);
dispatch_queue_t highPriorityQueue = dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_HIGH, 0);
dispatch_group_t dispatchGroup = dispatch_group_create();

for (id object in lowPriorityObjects) {
    dispatch_group_async(dispatchGroup,
                         lowPriorityQueue,
                         ^{
                              [object performTask];
                          });
}

for (id object in highPriorityObjects) {
    dispatch_group_async(dispatchGroup,
                         highPriorityQueue,
                         ^{
                              [object performTask];
                          });
}

dispatch_queue_t notifyQueue = dispatch_get_main_queue();
dispatch_group_notify(dispatchGroup,
                      notifyQueue,
                      ^{
                           // Continue processing after completing tasks
                       });


// Similar thing to groups, using serial queue
dispatch_queue_t queue = dispatch_queue_create("com.effectiveobjectivec.queue", NULL);

for (id object in collection) {
    dispatch_async(queue,
                   ^{
                        [object performTask];
                    });
}

dispatch_async(queue,
               ^{
                    // Continue processing after completing tasks
                });



// `dispatch_apply' instead of a group
dispatch_queue_t queue = dispatch_queue_create("com.effectiveobjectivec.queue", NULL);
dispatch_apply(10, queue, ^(size_t i){
    // Perform task
});


// Same example as above but this time using `dispatch_apply'
dispatch_queue_t queue = dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0);

dispatch_apply(array.count, queue, ^(size_t i){
    id object = array[i];
    [object performTask];
});
