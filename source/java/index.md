---
title: Java Basics
date: 2020-09-04 20:23:20
tags: [IT, Programming, Java]
categories: [IT, Programming, Java]
---
# Java Intro
## Write Once Run Anywhere
- write once: `Java src code` (.java file) ==> **Javac** (complier) () ==> `bytecode` (.class file)
- run anywhere: `byte code` ==> **JVM** (interpret) ==> `machine code` (binary file) ==> **device** (execute)
- compile once, copy bytecode to any platform having JVM to execute

## 3 versions [p2]

| Edition                    | Note                                                                |
|----------------------------|---------------------------------------------------------------------|
| Java SE (Standard)         | main for desktop app, inc Java lang fundation, JDBC, nework, thread |
| Java EE (Enterprise)       | main for distributed network, core as EJB                           |
| Java ME (Embeded)          | embeded system, e.g. pad，phone                                     |

## Configuration
搭建Java Env: 安装JDK & JRE，set env_vars

| Term                       | Note                                                                |
|----------------------------|---------------------------------------------------------------------|
| JDK                        | Java SE Development Kit, inc compiler (javac), debuger, app launcher, + JDK src code [p7] |
| JRE                        | run Java program [p7], inc **JVM** & GC         |
| JVM                        | interpret bytecode to machine code bf each exe (call `main()`)      |

# 8 primitive types
- `byte`, `short`, `int` (default), `long` (L); 
- `float` (F/f) & `double`; 
- `char`(Unicode char & escape seq), `boolean`

{% note info no-icon %}
- integer literal : 2进制 `0b`, 8进制 `0` [p28], 16进制 `0x`
- integer ovreflow: `(byte) 129` -> -127
- `(integer) float / double` || `integer / integer` ==> decimal part truncat, -1.2f -> -1, 3/2 = 1
	- want float result in integer devision => casting至少其中一个为float
- local var no default value 
- [ref](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/datatypes.html)
{% endnote %}

# Array
2-dim array `int[][] tdarr = new int[a][b]`
- a: tdarr.length (行); b: tdarr[i].length (列), can be spec later by `tdarr[0] = new int[2]`

# String
Impl by `char[]`, strVar -> obj (in heap) -> literal (in stack)

| Declaration Syntax             | Note                                                                |
|--------------------------------|---------------------------------------------------------------------|
| `".."` (char-str)              | ref share if literal exist in stack                                 |
| `new String("..")` \|\| `expr` | ref not share (always new a obj in heap)                            |
 
Check str empty: check `null` by `==`, check `""` by `equals()` (Object.equals(): check by `==`)
String methods: `length()`, `toCharArray()`, `format(..)`

| Type                       | Note                                                                |
|----------------------------|---------------------------------------------------------------------|
| `String`                   | literal immutable                                                   |
| `StringBuffer`             | thread safe （method *synchronized* so 同时只有一个线程运行该方法） |
| `StringBuilder`            | more efficient (no thread lock)                                     |

# Object oriented
Class basics
- Access ctrl modifier: `private` (本类), `_`(本包), `protected` (子类), `public` (所有类)
- `[static] {..}`: initializer block
	- `static`: run when obj declare (init as `null`)
	- non-static: run with `new`, bf `con()` (Java compiler copy init blocks into con(...))
- const: `static` + `final`
- defalut constructor (from compiler): `ClassName() {}`
- method: 6 components, method signature: name+param-list, method overload
	- `return this;` in method [p142]
	- `main()`: class main entrance [p164]
- `static method()`: access static member directly, access instance member by obj ref [..](https://docs.oracle.com/javase/tutorial/java/javaOO/classvars.html)

Inheritance & interface
- `interface`: contract btw class & outside-world (promises to provide the behavior of the interface)
	- member auto `public static final` [p174], cannot define inside a block [..](https://docs.oracle.com/javase/tutorial/java/javaOO/localclasses.html)
	- why only method no field: field usually private, method op on internal-state & method usually public --- serve as o2o interface with the outside world (data encapsulation)
- up-cast --- same as `Parent obj = new Child(); obj.xx`(xx must exist in Parent) [p164]
- `instanceof` check real type (constructor); `null instanceof xx` ==> false; `instanceof` bf down-cast 
- Top class/default parent class: `Object`
	- get class name: `Object.getClass().getName()` [p160]
- `abstract`: abstract-class vs interface [p174]

[Nested Class](https://docs.oracle.com/javase/tutorial/java/javaOO/nested.html)
- static nested class: access instance member by obj ref
- inner class: no own static members (e.g. `static {..}` & interface) except const
	- local class (block): access enclosing fn param / effectively `final` local var
		- in static method, can only refer to outer.staticMembers
	- anonymous class (expr of class declare + instanciate, e.g. return, = ~, arg)
		- same member-restriction & accessibity as local classes; no own con();
		- suitable for interface contains >=2 abstract methods, otherwise lambda expr
	- field shadowing (`OuterClass.this` => obj ref of OuterClass)

Others
- `package` --- namespace for related class & interface
	- `import java.util.*`
	- [Java SE API](https://docs.oracle.com/javase/8/docs/api/): set of pkgs (class lib)
- GC只能回收`new` obj --- `Object.finalize()` & `System.gc()`[p141]

# Concurency & Multi-thread
- 进程(process)：每个独立运行的程序; CUP时间片：每个不同进程轮流执行
- 当执行一个程序时，JVM自动产生一个线程来运行`main()` 运行；
- 在java中实现线程的2种方式：
	- extends `java.lang.Thread` & override `run()`
		- `run()`: 定义线程任务 (无限循环 + bool break 使线程一直运行下去)
		- `start()`: 启动线程 (call `run()` internal), 等待系统分配资源 & 运行
	- impl `java.lang.Runable` (含 `run()`) --- 继承非Thread类并在当前类中实现多线程
		- `Thread t = new Thread(new Runnable())`
- 线程的加入 `t2.join()` in `t1.run()`: 暂停当前进程，待t执行完毕后继续
- 线程的暂停 `wait()`与恢复 `nodify()`
- 线程的中断 `interrupt()`：
	- call `sleep() / wait()` 进入就绪状态，
	- call `interrupt()` 跳出`run()` & throws `InterruptedException`
- 线程同步 `synchronized`：防止多个线程访问资源的冲突，如dirty read --- 共享资源上锁，某个时间内只允许一个线程访问该资源
	- `synchronized (obj) {..}` & `synchronized void method()`: 放置对共享资源的操作
- 线程优先级 `Thread.MAX/MIN_PRIORITY` (1~5~10): 增加/减少线程优先执行的概率，不保证顺序

# Internet Communication
- **server**: provide init info & info processing; **client**: fetch info;
- **LAN**: Local Area Network; **WAN**(Wide ~): link LANs, e.g. Internet, the worldwide WAN
- IP 协议 & IP 地址； IPv4: 4字节/32位 binary; IPv6: 128 bits
- TCP/IP 协议: based on IP 协议 in Internet (4层)
	- TCP协议：一对一连接，，传输可靠（保证接收且顺序相同), 速度慢（需额外认证），e.g. HTTP, FTP, Telnet
	- UDP协议：一对多，无连接，不保证可靠；数据包之间相对独立（接收顺序不重要），如即时聊天；
- 端口 port：数据交流的出口（0～65535），e.g. HTTP 80；FTP 21； 套接字socket：连接app & port
- 相关java类 `java.net.*`
	- `InetAddress`: 封装IP地址;
	- `SocketAddress`: IP + port
	- `ServerSocket` & `Socket`: TCP 程序
	- `DatagramSocket` & `DatagramPacket`: UDP 程序

TCP程序设计: 
- server `new ServerSocket(port)` 等待client连接
- client `new Socket(host, port)`请求与server连接；
- server `accept()` 连接client并return `Socket`(如一直无client请求连接则阻塞)；
	- server创建新的Socket与client连接，
	- `server.accept()`return 该(与客户端Socket相连的)`Socket`；
- server继续等待新的client连接

note:
- server 一次与一个client socket连接；默认最大连接容量为50
- `server.accept().getOutputStream()` = `clientSocket.getInputStream()` (server向outputStream写入信息时，client可通过InputStream 来读取，反之亦然)

UDP程序设计:

# JDBC API & DB Access
## Overview
JDBC API: access tabular data (e.g. Realational DB, flat file or spreadsheet [..](https://docs.oracle.com/javase/tutorial/jdbc/basics/cachedrowset.html)), included in JavaSE & JavaEE [..](https://docs.oracle.com/javase/tutorial/jdbc/overview/index.html)
- JDBC 4.0 API into 2 pkgs: `java.sql` & Extension pkg `javax.sql`;
- supports both 2-tier & 3-tier processing models for database access

**JDBC driver**: Drivers that impl JDBC API, dev by DB vendor [..](https://docs.oracle.com/javase/tutorial/jdbc/basics/gettingstarted.html#step3), used to communicate with data source [..](https://docs.oracle.com/javase/tutorial/jdbc/overview/index.html)
- e.g. **MySQL Connector/J** (it impl network protocol to MySQL, client connects directly to MySQL) [..](https://docs.oracle.com/javase/tutorial/jdbc/basics/gettingstarted.html#step3)
- contains >= 1 JDBC driver class (class impl interface `java.sql.Driver` [..](https://docs.oracle.com/javase/tutorial/jdbc/basics/connecting.html))
- init/load by `Class.forName(java.sql.Driver)` (when `DriverManager` first attempts to connect [..](https://docs.oracle.com/javase/tutorial/jdbc/basics/connecting.html))

JDBC Architecture
- 2-tier model: **java app with JDBC driver** (client) -> DBMS-proprietary protocol -> **DBMS** (server);
	3-tire model: **Java applet/HTML browser** -> HTTP.. -> **app server with JDBC..** -> **DBMS** (DB server)
	- extra middle tier: control access & data updates; simplify the deployment of app

DB concepts ..
- table (aka relation): a collection of objects of the same type (rows)
- data in a table related by common keys or concepts

## Programming process
[Configuration](https://docs.oracle.com/javase/tutorial/jdbc/basics/gettingstarted.html)
- Install DBMS & JDBC driver from DB vendor (e.g. MySQL & Connector/J) 
- spec the JDBC driver class path (path of the JAR file that contains the [JDBC Driver] class specified in driver [..](https://docs.oracle.com/javase/tutorial/jdbc/basics/tables.html), e.g. `"com.mysql.cj.jdbc.Driver"`)

[Connect to data source](https://docs.oracle.com/javase/tutorial/jdbc/basics/connecting.html)
- init JDBC driver by `Class.forName(java.sql.Driver)` (auto-load by **DriverManager** class for JDBC 4.0+)
- get connection (to a JDBC driver [..](https://docs.oracle.com/javase/tutorial/jdbc/overview/index.html)) by `DriverManager.getConnection(DB_URL, connProps)`
	- DB_URL for MySQL Connector/J: `"jdbc:mysql://${DB.HOST}:${DB.PORT}/"`

{% note info no-icon %}
or connect by `DataSource` (interface in Java EE) [..](https://docs.oracle.com/javase/tutorial/jdbc/basics/sqldatasources.html#deploy_datasource)
- basic ds: JDBC driver vendor impl `DataSource` interface & deploy an obj for each data source
- **connection pool** ds: `ConnectionPoolDataSource` obj (impl by JDBC driver vendor) impl connection pooling for `DataSource` obj (impl by EJB server vendor)
- **distributed transaction** ds: `XADataSource` & `DataSource`
- connect by `ds.getConnection(..)`;
{% endnote %}
- close conn in `finally`

[Process SQL Stmts](https://docs.oracle.com/javase/tutorial/jdbc/basics/processingsqlstatements.html)
- 3 statements: `Statement`, `PreparedStatement`: precompiled, reusable stmt obj (with diff params by `ps.set*(..)`) [..](https://docs.oracle.com/javase/tutorial/jdbc/basics/prepared.html), `CallableStatement` & [stored procedure](https://docs.oracle.com/javase/tutorial/jdbc/overview/index.html "a group of SQL stmts that can be called by name (a static Java method), stored in DB metadata") [..]()
- `execute()`, `executeQuery(query)`, `executeUpdate()`
- batch updates: `int [] updateCounts = stmt.executeBatch()` (update count in int-arr returned by DBMS, batch list reset to empty)[..](https://docs.oracle.com/javase/tutorial/jdbc/basics/retrieving.html)
- `ResultSet` interface [..](https://docs.oracle.com/javase/tutorial/jdbc/basics/retrieving.html)
	- JDBC driver-spec `DatabaseMetaData`
	- retrive column value: `rs.get*(colIndex/name/alias)` & SQL `AS` in `SELECT`
	- insert/update/delete rows in rs: `rs.moveToInsertRow()`, `rs.update*(..)`, `rs.insert-/update-/deleteRow()`, `rs.moveToCurrentRow()` [..](https://docs.oracle.com/javase/tutorial/jdbc/basics/cachedrowset.html#updating-cachedrowset-object)
		- insertion made immediately af current row [..](https://docs.oracle.com/javase/tutorial/jdbc/basics/webrowset.html#data-webrowset)
- `RowSet` interface, JavaBeans Component, scrollable & updatable [..](https://docs.oracle.com/javase/tutorial/jdbc/basics/rowset.html)
	- 1 connected `JdbcRowSet` & 4 disconn (serializable & lightweight, ideal for sending data over a network)
	- `CatchedRowSet`: super-interface for all disconn (extra `crs.acceptChanges(con)` to write data to DB)
		- caches data in memory, read/write by a `RowSetReader/Writer` obj (provided by the assigned `SyncProvider` instance when crs created)
		- r/w work in bg: get conn, read/write data, close conn
		- ref impl of the default `SyncProvider` obj: instance of the `RIOptimisticProvider` impl: (not) write to db if any/no conflict (with no db locks)
		- `SyncProviderException` & `SyncResolver` when conflict: compare & choose one
	- `JoinRowSet`: join mtp disconn rs by `jrs.addRowSet(coffees, "SUP_ID");`
	- `WebRowSet`	: enable Web Services to send/receive data from db in XML doc (standard for Web Services comm)
		- sets `SyncProvider` obj to `RIXMLProvider` impl
		- `write-/readXml(..)`: convert wrs to/from xml
		- contain data + col metadata 
	- `FilteredRowSet`: `frs.setFilter(Predicate)` (CRUD on current visible rows)
- transaction & DBMS locks [..](https://docs.oracle.com/javase/tutorial/jdbc/basics/transactions.html)
	- default transaction isolation level depends on the DBMS & `DatabaseMetaData` 
	- `conn.rollback(SavePoint)` ..
- `conn/stmt/rs.close()` in `finally`: close conn also close stmt (good practice to close stmt explicitly) [..](https://docs.oracle.com/javase/tutorial/jdbc/basics/tables.html), close stmt also close rs
- `SQLException`: throw when error during interaction with data source [..](https://docs.oracle.com/javase/tutorial/jdbc/basics/sqlexception.html)
	- errorCode: (DB vendor) imp-spec
	- 3 subclass of SQLException JDBC driver-spec might throw ..
- JDBC 4.1 (Java SE 7+): allow `try-resrc` stmt [..](https://docs.oracle.com/javase/tutorial/essential/exceptions/tryResourceClose.html) to auto close conn/stmt/resultSet even exception occur when the try block terminates 
- [Adv DB datatype](https://docs.oracle.com/javase/tutorial/jdbc/basics/sqltypes.html): BLOB, CLOB, XML, Locator (ogical pointer) ..
	- BLOB, CLOB, NCLOB: materialize data as a stream
	- release resrc by `obj.free()`
	- MySQL not support XML (can store in LONGTEXT), ARRAY (max length) & UDT (DISTINCT, struc) [..](https://docs.oracle.com/javase/tutorial/jdbc/basics/sqlcustommapping.html), RowID (JDBC interface: address to a row in a database table, not a standard SQL type)
	- custom type mapping (from SQL data type to java data type), Datalink & URL ..

```java
CallableStatement cablStmt = conn.prepareCall("{call pro_GetInfo(?, ?)}"); 
cablStmt.setString(1, "张%"); // 将sql中第一个通配符("?")的值设为“张%” (动态sql)

// update
stmt.executeUpdate("delete..");
ps = conn.prepareStatement("insert/update..");
ps.executeUpdate();

// batch op
ps = conn.prepareStatement("insert/update..");
ps.clearBatch();
for (..) {
	ps.setString(1, records[i][1]);
	ps.setString(2, records[i][1]);
	ps.addBatch();
}
ps.executeBatch();

```

# Refs
[Oracle Help Center](https://docs.oracle.com/en/) / Java / Java SE doc / Java Tut / Learning the Java Lang
[Javase 8 API](https://docs.oracle.com/javase/8/docs/api/index.html)
《Java 从入门到精通》(项目案例版) 2017.9