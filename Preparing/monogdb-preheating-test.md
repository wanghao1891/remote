# 系统环境
memory：512MB


## 1、no index
### mongod内存耗用
16.1%

### 查询结果
    > db.users.find({ status: 1, type: 2, tags: 100 }).sort({ compreordering: -1 }).explain();
    {
            "cursor" : "BasicCursor",
            "isMultiKey" : false,
            "n" : 4374,
            "nscannedObjects" : 37212,
            "nscanned" : 37212,
            "nscannedObjectsAllPlans" : 37212,
            "nscannedAllPlans" : 37212,
            "scanAndOrder" : true,
            "indexOnly" : false,
            "nYields" : 324,
            "nChunkSkips" : 0,
            "millis" : 184,
            "server" : "cd3e9cb8c20c:27017",
            "filterSet" : false
    }
    > db.users.find({ status: 1, type: 2, tags: 100 }).sort({ compreordering: -1 }).explain();
    {
            "cursor" : "BasicCursor",
            "isMultiKey" : false,
            "n" : 4374,
            "nscannedObjects" : 37212,
            "nscanned" : 37212,
            "nscannedObjectsAllPlans" : 37212,
            "nscannedAllPlans" : 37212,
            "scanAndOrder" : true,
            "indexOnly" : false,
            "nYields" : 324,
            "nChunkSkips" : 0,
            "millis" : 165,
            "server" : "cd3e9cb8c20c:27017",
            "filterSet" : false
    }
    > db.users.find({ status: 1, type: 2, tags: 100 }).sort({ compreordering: -1 }).explain();
    {
            "cursor" : "BasicCursor",
            "isMultiKey" : false,
            "n" : 4374,
            "nscannedObjects" : 37212,
            "nscanned" : 37212,
            "nscannedObjectsAllPlans" : 37212,
            "nscannedAllPlans" : 37212,
            "scanAndOrder" : true,
            "indexOnly" : false,
            "nYields" : 324,
            "nChunkSkips" : 0,
            "millis" : 167,
            "server" : "cd3e9cb8c20c:27017",
            "filterSet" : false
    }
    > db.users.find({ status: 1, type: 2, tags: 100 }).sort({ compreordering: -1 }).explain();
    {
            "cursor" : "BasicCursor",
            "isMultiKey" : false,
            "n" : 4374,
            "nscannedObjects" : 37212,
            "nscanned" : 37212,
            "nscannedObjectsAllPlans" : 37212,
            "nscannedAllPlans" : 37212,
            "scanAndOrder" : true,
            "indexOnly" : false,
            "nYields" : 324,
            "nChunkSkips" : 0,
            "millis" : 176,
            "server" : "cd3e9cb8c20c:27017",
            "filterSet" : false
    }
    > db.users.find({ status: 1, type: 2, tags: 100 }).sort({ compreordering: -1 }).explain();
    {
            "cursor" : "BasicCursor",
            "isMultiKey" : false,
            "n" : 4374,
            "nscannedObjects" : 37212,
            "nscanned" : 37212,
            "nscannedObjectsAllPlans" : 37212,
            "nscannedAllPlans" : 37212,
            "scanAndOrder" : true,
            "indexOnly" : false,
            "nYields" : 324,
            "nChunkSkips" : 0,
            "millis" : 171,
            "server" : "cd3e9cb8c20c:27017",
            "filterSet" : false
    }

### 平均响应时间
(184 + 165 + 167 + 176 + 171)/5 = 172.6

## 2、index
### 加索引后内存耗用
16.4%

### 添加索引
    > db.users.ensureIndex( {status: 1, type: 1, tags: 1, compreordering: -1} )
    {
            "createdCollectionAutomatically" : false,
            "numIndexesBefore" : 1,
            "numIndexesAfter" : 2,
            "ok" : 1
    }

### 查询结果
    > db.users.find({ status: 1, type: 2, tags: 100 }).sort({ compreordering: -1 }).explain();
    {
            "cursor" : "BtreeCursor status_1_type_1_tags_1_compreordering_-1",
            "isMultiKey" : true,
            "n" : 4374,
            "nscannedObjects" : 4374,
            "nscanned" : 4374,
            "nscannedObjectsAllPlans" : 4374,
            "nscannedAllPlans" : 4374,
            "scanAndOrder" : false,
            "indexOnly" : false,
            "nYields" : 34,
            "nChunkSkips" : 0,
            "millis" : 17,
            "indexBounds" : {
                    "status" : [
                            [
                                    1,
                                    1
                            ]
                    ],
                    "type" : [
                            [
                                    2,
                                    2
                            ]
                    ],
                    "tags" : [
                            [
                                    100,
                                    100
                            ]
                    ],
                    "compreordering" : [
                            [
                                    {
                                            "$maxElement" : 1
                                    },
                                    {
                                            "$minElement" : 1
                                    }
                            ]
                    ]
            },
            "server" : "cd3e9cb8c20c:27017",
            "filterSet" : false
    }
    > db.users.find({ status: 1, type: 2, tags: 100 }).sort({ compreordering: -1 }).explain();
    {
            "cursor" : "BtreeCursor status_1_type_1_tags_1_compreordering_-1",
            "isMultiKey" : true,
            "n" : 4374,
            "nscannedObjects" : 4374,
            "nscanned" : 4374,
            "nscannedObjectsAllPlans" : 4374,
            "nscannedAllPlans" : 4374,
            "scanAndOrder" : false,
            "indexOnly" : false,
            "nYields" : 34,
            "nChunkSkips" : 0,
            "millis" : 9,
            "indexBounds" : {
                    "status" : [
                            [
                                    1,
                                    1
                            ]
                    ],
                    "type" : [
                            [
                                    2,
                                    2
                            ]
                    ],
                    "tags" : [
                            [
                                    100,
                                    100
                            ]
                    ],
                    "compreordering" : [
                            [
                                    {
                                            "$maxElement" : 1
                                    },
                                    {
                                            "$minElement" : 1
                                    }
                            ]
                    ]
            },
            "server" : "cd3e9cb8c20c:27017",
            "filterSet" : false
    }
    > db.users.find({ status: 1, type: 2, tags: 100 }).sort({ compreordering: -1 }).explain();
    {
            "cursor" : "BtreeCursor status_1_type_1_tags_1_compreordering_-1",
            "isMultiKey" : true,
            "n" : 4374,
            "nscannedObjects" : 4374,
            "nscanned" : 4374,
            "nscannedObjectsAllPlans" : 4374,
            "nscannedAllPlans" : 4374,
            "scanAndOrder" : false,
            "indexOnly" : false,
            "nYields" : 34,
            "nChunkSkips" : 0,
            "millis" : 8,
            "indexBounds" : {
                    "status" : [
                            [
                                    1,
                                    1
                            ]
                    ],
                    "type" : [
                            [
                                    2,
                                    2
                            ]
                    ],
                    "tags" : [
                            [
                                    100,
                                    100
                            ]
                    ],
                    "compreordering" : [
                            [
                                    {
                                            "$maxElement" : 1
                                    },
                                    {
                                            "$minElement" : 1
                                    }
                            ]
                    ]
            },
            "server" : "cd3e9cb8c20c:27017",
            "filterSet" : false
    }
    > db.users.find({ status: 1, type: 2, tags: 100 }).sort({ compreordering: -1 }).explain();
    {
            "cursor" : "BtreeCursor status_1_type_1_tags_1_compreordering_-1",
            "isMultiKey" : true,
            "n" : 4374,
            "nscannedObjects" : 4374,
            "nscanned" : 4374,
            "nscannedObjectsAllPlans" : 4374,
            "nscannedAllPlans" : 4374,
            "scanAndOrder" : false,
            "indexOnly" : false,
            "nYields" : 34,
            "nChunkSkips" : 0,
            "millis" : 8,
            "indexBounds" : {
                    "status" : [
                            [
                                    1,
                                    1
                            ]
                    ],
                    "type" : [
                            [
                                    2,
                                    2
                            ]
                    ],
                    "tags" : [
                            [
                                    100,
                                    100
                            ]
                    ],
                    "compreordering" : [
                            [
                                    {
                                            "$maxElement" : 1
                                    },
                                    {
                                            "$minElement" : 1
                                    }
                            ]
                    ]
            },
            "server" : "cd3e9cb8c20c:27017",
            "filterSet" : false
    }
    > db.users.find({ status: 1, type: 2, tags: 100 }).sort({ compreordering: -1 }).explain();
    {
            "cursor" : "BtreeCursor status_1_type_1_tags_1_compreordering_-1",
            "isMultiKey" : true,
            "n" : 4374,
            "nscannedObjects" : 4374,
            "nscanned" : 4374,
            "nscannedObjectsAllPlans" : 4374,
            "nscannedAllPlans" : 4374,
            "scanAndOrder" : false,
            "indexOnly" : false,
            "nYields" : 34,
            "nChunkSkips" : 0,
            "millis" : 6,
            "indexBounds" : {
                    "status" : [
                            [
                                    1,
                                    1
                            ]
                    ],
                    "type" : [
                            [
                                    2,
                                    2
                            ]
                    ],
                    "tags" : [
                            [
                                    100,
                                    100
                            ]
                    ],
                    "compreordering" : [
                            [
                                    {
                                            "$maxElement" : 1
                                    },
                                    {
                                            "$minElement" : 1
                                    }
                            ]
                    ]
            },
            "server" : "cd3e9cb8c20c:27017",
            "filterSet" : false
    }

### 平均响应时间
(17 + 9 + 8 + 8 + 6)/5 = 9.6

## 3、预热数据和索引

### 内存耗用
34.0%

### preheating data and index
    db.runCommand({"touch" : "users", "data" : true, "index" : true})
    {
            "data" : {
                    "numRanges" : 10,
                    "millis" : 431
            },
            "indexes" : {
                    "num" : 2,
                    "numRanges" : 6,
                    "millis" : 119
            },
            "ok" : 1
    }

### 查询结果
    > db.users.find({ status: 1, type: 2, tags: 100 }).sort({ compreordering: -1 }).explain();
    {
            "cursor" : "BtreeCursor status_1_type_1_tags_1_compreordering_-1",
            "isMultiKey" : true,
            "n" : 4374,
            "nscannedObjects" : 4374,
            "nscanned" : 4374,
            "nscannedObjectsAllPlans" : 4374,
            "nscannedAllPlans" : 4374,
            "scanAndOrder" : false,
            "indexOnly" : false,
            "nYields" : 34,
            "nChunkSkips" : 0,
            "millis" : 12,
            "indexBounds" : {
                    "status" : [
                            [
                                    1,
                                    1
                            ]
                    ],
                    "type" : [
                            [
                                    2,
                                    2
                            ]
                    ],
                    "tags" : [
                            [
                                    100,
                                    100
                            ]
                    ],
                    "compreordering" : [
                            [
                                    {
                                            "$maxElement" : 1
                                    },
                                    {
                                            "$minElement" : 1
                                    }
                            ]
                    ]
            },
            "server" : "cd3e9cb8c20c:27017",
            "filterSet" : false
    }
    > db.users.find({ status: 1, type: 2, tags: 100 }).sort({ compreordering: -1 }).explain();
    {
            "cursor" : "BtreeCursor status_1_type_1_tags_1_compreordering_-1",
            "isMultiKey" : true,
            "n" : 4374,
            "nscannedObjects" : 4374,
            "nscanned" : 4374,
            "nscannedObjectsAllPlans" : 4374,
            "nscannedAllPlans" : 4374,
            "scanAndOrder" : false,
            "indexOnly" : false,
            "nYields" : 34,
            "nChunkSkips" : 0,
            "millis" : 9,
            "indexBounds" : {
                    "status" : [
                            [
                                    1,
                                    1
                            ]
                    ],
                    "type" : [
                            [
                                    2,
                                    2
                            ]
                    ],
                    "tags" : [
                            [
                                    100,
                                    100
                            ]
                    ],
                    "compreordering" : [
                            [
                                    {
                                            "$maxElement" : 1
                                    },
                                    {
                                            "$minElement" : 1
                                    }
                            ]
                    ]
            },
            "server" : "cd3e9cb8c20c:27017",
            "filterSet" : false
    }
    > db.users.find({ status: 1, type: 2, tags: 100 }).sort({ compreordering: -1 }).explain();
    {
            "cursor" : "BtreeCursor status_1_type_1_tags_1_compreordering_-1",
            "isMultiKey" : true,
            "n" : 4374,
            "nscannedObjects" : 4374,
            "nscanned" : 4374,
            "nscannedObjectsAllPlans" : 4374,
            "nscannedAllPlans" : 4374,
            "scanAndOrder" : false,
            "indexOnly" : false,
            "nYields" : 34,
            "nChunkSkips" : 0,
            "millis" : 8,
            "indexBounds" : {
                    "status" : [
                            [
                                    1,
                                    1
                            ]
                    ],
                    "type" : [
                            [
                                    2,
                                    2
                            ]
                    ],
                    "tags" : [
                            [
                                    100,
                                    100
                            ]
                    ],
                    "compreordering" : [
                            [
                                    {
                                            "$maxElement" : 1
                                    },
                                    {
                                            "$minElement" : 1
                                    }
                            ]
                    ]
            },
            "server" : "cd3e9cb8c20c:27017",
            "filterSet" : false
    }
    > db.users.find({ status: 1, type: 2, tags: 100 }).sort({ compreordering: -1 }).explain();
    {
            "cursor" : "BtreeCursor status_1_type_1_tags_1_compreordering_-1",
            "isMultiKey" : true,
            "n" : 4374,
            "nscannedObjects" : 4374,
            "nscanned" : 4374,
            "nscannedObjectsAllPlans" : 4374,
            "nscannedAllPlans" : 4374,
            "scanAndOrder" : false,
            "indexOnly" : false,
            "nYields" : 34,
            "nChunkSkips" : 0,
            "millis" : 15,
            "indexBounds" : {
                    "status" : [
                            [
                                    1,
                                    1
                            ]
                    ],
                    "type" : [
                            [
                                    2,
                                    2
                            ]
                    ],
                    "tags" : [
                            [
                                    100,
                                    100
                            ]
                    ],
                    "compreordering" : [
                            [
                                    {
                                            "$maxElement" : 1
                                    },
                                    {
                                            "$minElement" : 1
                                    }
                            ]
                    ]
            },
            "server" : "cd3e9cb8c20c:27017",
            "filterSet" : false
    }
    > db.users.find({ status: 1, type: 2, tags: 100 }).sort({ compreordering: -1 }).explain();
    {
            "cursor" : "BtreeCursor status_1_type_1_tags_1_compreordering_-1",
            "isMultiKey" : true,
            "n" : 4374,
            "nscannedObjects" : 4374,
            "nscanned" : 4374,
            "nscannedObjectsAllPlans" : 4374,
            "nscannedAllPlans" : 4374,
            "scanAndOrder" : false,
            "indexOnly" : false,
            "nYields" : 34,
            "nChunkSkips" : 0,
            "millis" : 11,
            "indexBounds" : {
                    "status" : [
                            [
                                    1,
                                    1
                            ]
                    ],
                    "type" : [
                            [
                                    2,
                                    2
                            ]
                    ],
                    "tags" : [
                            [
                                    100,
                                    100
                            ]
                    ],
                    "compreordering" : [
                            [
                                    {
                                            "$maxElement" : 1
                                    },
                                    {
                                            "$minElement" : 1
                                    }
                            ]
                    ]
            },
            "server" : "cd3e9cb8c20c:27017",
            "filterSet" : false
    }

### 平均响应时间
(12 + 9 + 8 + 15 + 11)/5 = 11

## 4、no index and preheating data
### 内存耗用
16.1%-32.9%

### 删除索引
    > db.users.dropIndexes();
    {
            "nIndexesWas" : 2,
            "msg" : "non-_id indexes dropped for collection",
            "ok" : 1
    }

### 预热数据
    > db.runCommand({"touch" : "users", "data" : true, "index" : false})
    { "data" : { "numRanges" : 10, "millis" : 81 }, "ok" : 1 }

### 查询结果
    > db.users.find({ status: 1, type: 2, tags: 100 }).sort({ compreordering: -1 }).explain();
    {
            "cursor" : "BasicCursor",
            "isMultiKey" : false,
            "n" : 4374,
            "nscannedObjects" : 37212,
            "nscanned" : 37212,
            "nscannedObjectsAllPlans" : 37212,
            "nscannedAllPlans" : 37212,
            "scanAndOrder" : true,
            "indexOnly" : false,
            "nYields" : 324,
            "nChunkSkips" : 0,
            "millis" : 176,
            "server" : "cd3e9cb8c20c:27017",
            "filterSet" : false
    }
    > db.users.find({ status: 1, type: 2, tags: 100 }).sort({ compreordering: -1 }).explain();
    {
            "cursor" : "BasicCursor",
            "isMultiKey" : false,
            "n" : 4374,
            "nscannedObjects" : 37212,
            "nscanned" : 37212,
            "nscannedObjectsAllPlans" : 37212,
            "nscannedAllPlans" : 37212,
            "scanAndOrder" : true,
            "indexOnly" : false,
            "nYields" : 324,
            "nChunkSkips" : 0,
            "millis" : 168,
            "server" : "cd3e9cb8c20c:27017",
            "filterSet" : false
    }
    > db.users.find({ status: 1, type: 2, tags: 100 }).sort({ compreordering: -1 }).explain();
    {
            "cursor" : "BasicCursor",
            "isMultiKey" : false,
            "n" : 4374,
            "nscannedObjects" : 37212,
            "nscanned" : 37212,
            "nscannedObjectsAllPlans" : 37212,
            "nscannedAllPlans" : 37212,
            "scanAndOrder" : true,
            "indexOnly" : false,
            "nYields" : 324,
            "nChunkSkips" : 0,
            "millis" : 167,
            "server" : "cd3e9cb8c20c:27017",
            "filterSet" : false
    }
    > db.users.find({ status: 1, type: 2, tags: 100 }).sort({ compreordering: -1 }).explain();
    {
            "cursor" : "BasicCursor",
            "isMultiKey" : false,
            "n" : 4374,
            "nscannedObjects" : 37212,
            "nscanned" : 37212,
            "nscannedObjectsAllPlans" : 37212,
            "nscannedAllPlans" : 37212,
            "scanAndOrder" : true,
            "indexOnly" : false,
            "nYields" : 324,
            "nChunkSkips" : 0,
            "millis" : 175,
            "server" : "cd3e9cb8c20c:27017",
            "filterSet" : false
    }
    > db.users.find({ status: 1, type: 2, tags: 100 }).sort({ compreordering: -1 }).explain();
    {
            "cursor" : "BasicCursor",
            "isMultiKey" : false,
            "n" : 4374,
            "nscannedObjects" : 37212,
            "nscanned" : 37212,
            "nscannedObjectsAllPlans" : 37212,
            "nscannedAllPlans" : 37212,
            "scanAndOrder" : true,
            "indexOnly" : false,
            "nYields" : 324,
            "nChunkSkips" : 0,
            "millis" : 163,
            "server" : "cd3e9cb8c20c:27017",
            "filterSet" : false
    }

### 平均响应时间
(176 + 168 + 167 + 175 + 163)/5 = 169.8

## index and preheating index
### 内存耗用
16.4% - 17.5%

### 添加索引
    db.users.ensureIndex( {status: 1, type: 1, tags: 1, compreordering: -1} )
    {
            "createdCollectionAutomatically" : false,
            "numIndexesBefore" : 1,
            "numIndexesAfter" : 2,
            "ok" : 1
    }

### 预热索引
    > db.runCommand({"touch" : "users", "data" : false, "index" : true})
    {
            "indexes" : {
                    "num" : 2,
                    "numRanges" : 6,
                    "millis" : 52
            },
            "ok" : 1
    }

### 查询结果
    > db.users.find({ status: 1, type: 2, tags: 100 }).sort({ compreordering: -1 }).explain();
    {
            "cursor" : "BtreeCursor status_1_type_1_tags_1_compreordering_-1",
            "isMultiKey" : true,
            "n" : 4374,
            "nscannedObjects" : 4374,
            "nscanned" : 4374,
            "nscannedObjectsAllPlans" : 4374,
            "nscannedAllPlans" : 4374,
            "scanAndOrder" : false,
            "indexOnly" : false,
            "nYields" : 34,
            "nChunkSkips" : 0,
            "millis" : 15,
            "indexBounds" : {
                    "status" : [
                            [
                                    1,
                                    1
                            ]
                    ],
                    "type" : [
                            [
                                    2,
                                    2
                            ]
                    ],
                    "tags" : [
                            [
                                    100,
                                    100
                            ]
                    ],
                    "compreordering" : [
                            [
                                    {
                                            "$maxElement" : 1
                                    },
                                    {
                                            "$minElement" : 1
                                    }
                            ]
                    ]
            },
            "server" : "cd3e9cb8c20c:27017",
            "filterSet" : false
    }
    > db.users.find({ status: 1, type: 2, tags: 100 }).sort({ compreordering: -1 }).explain();
    {
            "cursor" : "BtreeCursor status_1_type_1_tags_1_compreordering_-1",
            "isMultiKey" : true,
            "n" : 4374,
            "nscannedObjects" : 4374,
            "nscanned" : 4374,
            "nscannedObjectsAllPlans" : 4374,
            "nscannedAllPlans" : 4374,
            "scanAndOrder" : false,
            "indexOnly" : false,
            "nYields" : 34,
            "nChunkSkips" : 0,
            "millis" : 9,
            "indexBounds" : {
                    "status" : [
                            [
                                    1,
                                    1
                            ]
                    ],
                    "type" : [
                            [
                                    2,
                                    2
                            ]
                    ],
                    "tags" : [
                            [
                                    100,
                                    100
                            ]
                    ],
                    "compreordering" : [
                            [
                                    {
                                            "$maxElement" : 1
                                    },
                                    {
                                            "$minElement" : 1
                                    }
                            ]
                    ]
            },
            "server" : "cd3e9cb8c20c:27017",
            "filterSet" : false
    }
    > db.users.find({ status: 1, type: 2, tags: 100 }).sort({ compreordering: -1 }).explain();
    {
            "cursor" : "BtreeCursor status_1_type_1_tags_1_compreordering_-1",
            "isMultiKey" : true,
            "n" : 4374,
            "nscannedObjects" : 4374,
            "nscanned" : 4374,
            "nscannedObjectsAllPlans" : 4374,
            "nscannedAllPlans" : 4374,
            "scanAndOrder" : false,
            "indexOnly" : false,
            "nYields" : 34,
            "nChunkSkips" : 0,
            "millis" : 9,
            "indexBounds" : {
                    "status" : [
                            [
                                    1,
                                    1
                            ]
                    ],
                    "type" : [
                            [
                                    2,
                                    2
                            ]
                    ],
                    "tags" : [
                            [
                                    100,
                                    100
                            ]
                    ],
                    "compreordering" : [
                            [
                                    {
                                            "$maxElement" : 1
                                    },
                                    {
                                            "$minElement" : 1
                                    }
                            ]
                    ]
            },
            "server" : "cd3e9cb8c20c:27017",
            "filterSet" : false
    }
    > db.users.find({ status: 1, type: 2, tags: 100 }).sort({ compreordering: -1 }).explain();
    {
            "cursor" : "BtreeCursor status_1_type_1_tags_1_compreordering_-1",
            "isMultiKey" : true,
            "n" : 4374,
            "nscannedObjects" : 4374,
            "nscanned" : 4374,
            "nscannedObjectsAllPlans" : 4374,
            "nscannedAllPlans" : 4374,
            "scanAndOrder" : false,
            "indexOnly" : false,
            "nYields" : 34,
            "nChunkSkips" : 0,
            "millis" : 9,
            "indexBounds" : {
                    "status" : [
                            [
                                    1,
                                    1
                            ]
                    ],
                    "type" : [
                            [
                                    2,
                                    2
                            ]
                    ],
                    "tags" : [
                            [
                                    100,
                                    100
                            ]
                    ],
                    "compreordering" : [
                            [
                                    {
                                            "$maxElement" : 1
                                    },
                                    {
                                            "$minElement" : 1
                                    }
                            ]
                    ]
            },
            "server" : "cd3e9cb8c20c:27017",
            "filterSet" : false
    }
    > db.users.find({ status: 1, type: 2, tags: 100 }).sort({ compreordering: -1 }).explain();
    {
            "cursor" : "BtreeCursor status_1_type_1_tags_1_compreordering_-1",
            "isMultiKey" : true,
            "n" : 4374,
            "nscannedObjects" : 4374,
            "nscanned" : 4374,
            "nscannedObjectsAllPlans" : 4374,
            "nscannedAllPlans" : 4374,
            "scanAndOrder" : false,
            "indexOnly" : false,
            "nYields" : 34,
            "nChunkSkips" : 0,
            "millis" : 7,
            "indexBounds" : {
                    "status" : [
                            [
                                    1,
                                    1
                            ]
                    ],
                    "type" : [
                            [
                                    2,
                                    2
                            ]
                    ],
                    "tags" : [
                            [
                                    100,
                                    100
                            ]
                    ],
                    "compreordering" : [
                            [
                                    {
                                            "$maxElement" : 1
                                    },
                                    {
                                            "$minElement" : 1
                                    }
                            ]
                    ]
            },
            "server" : "cd3e9cb8c20c:27017",
            "filterSet" : false
    }

### 平均响应时间
(15 + 9 + 9 + 9 + 7)/5 = 9.8
