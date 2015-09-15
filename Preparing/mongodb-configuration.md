# 1、2015-05-29
* the format before version 2.6

      dbpath=/root/workspace/data/mongodb
      bind_ip = 0.0.0.0
      port=27017
      maxConns = 200
      logpath=/root/workspace/logs/mongodb.log
      fork = true
      #auth = true
      httpinterface = true
      rest = true
      replSet = rs0
      keyFile = /root/workspace/bin/mongodb/mongodb-keyfile

* the YAML-based configuration file in version 2.6
  * the arbiter
        systemLog:
           destination: file
           path: "/root/workspace/logs/mongodb.log"
           logAppend: true
        storage:
           journal:
              enabled: false
           smallFiles: true
           dbPath: "/root/workspace/data/mongodb"
        processManagement:
           fork: true
        net:
           bindIp: 0.0.0.0
           port: 27017
        setParameter:
           enableLocalhostAuthBypass: false
        replication:
           replSetName: rs0
        security:
           keyFile: /root/workspace/bin/mongodb/mongodb-keyfile

  * the secondary
         systemLog:
            destination: file
            path: "/root/workspace/logs/mongodb.log"
            logAppend: true
         storage:
            engine: wiredTiger
            dbPath: "/root/workspace/data/mongodb"
         processManagement:
            fork: true
         net:
            bindIp: 0.0.0.0
            port: 27017
         replication:
            replSetName: rs0
