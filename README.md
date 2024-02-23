
# mysql-backup (version 1.0.0)

This tool, mysql-backup, save backups of all MySQL databasesm located on current MySQL server to the `/srv/mysql-backup` directory.

Backups are created of all existing atabases on current MySQL server, except four system databases: `information_schema`, `performance_schema`, `mysql`, `sys`.

By default, this tool save MysQL backup archives only for last `7 days`, after backup creation automatically delete all old archives, created more than `7 days` ago.

For exporting `*.sql` dumps from current MySQL database server used temporary subdirectory `tmp-sql-dump-dir-tmp`,
created inside directory `/dump` if directory `/dump` is exists and if directory `/dump` is mountpoint.
Otherwise - temporary files are created in temporary subdirectory `tmp-sql-dump-dir-tmp`,
created inside `/srv/mysql-backup` directory.

`xz` compressor in single thread mode is used for compressing tar archives with all databases backups, 
because `xz` provide 2x better compression quality and 2x smaller backup archive size, compared to `gzip`.

> [!CAUTION]
>
> This tool can be used to backup only transactional databases, like InnoDB, because only `--single-transaction` directive used.
>
> `mysqldump` directive `--lock-tables` not used, because this can prevent parallel queries to database, while mysqldump is being run.
>
> MyISAM storage engine is not supported and not recommented to use at all, because MyISAM storage engine uses table-level locking,
> what can lead to contention and bottlenecks when multiple transactions try to access the same table simultaneously.
>
> Row-level locking, used in InnoDB and all other MySQL transactional storage engines
> allow better concurrency and enables multiple transactions to work simultaneously on different parts of a table.

## Installation

> [!IMPORTANT]
> Python 3.11+ and [invoke](https://www.pyinvoke.org/) module required
```
dnf install python3.11 python3.11-pip ; \
ln -s /usr/bin/python3.11 /usr/bin/python ; \
ln -s /usr/bin/pip3.11 /usr/bin/pip ; \
pip install invoke

cd /opt ; \
git clone https://github.com/makhomed/mysql-backup.git
```

## Upgrade

```
cd /opt/mysql-backup ; \
git pull
```

## Usage

```
/opt/mysql-backup/mysql-backup
```

## Automation

```
# cat /etc/cron.d/mysql-backup

0 0 * * * root /opt/mysql-backup/mysql-backup
```

## TODO

- Add support for MyISAM storage engine, automatically detect MyISAM tables and use directive `--lock-tables`
  for databases with MyISAM tables instead of directive `--single-transaction` which should be used
  for databases when all database tables usees only transactional storage engines.

- Check [`https://github.com/igor-kremin/mysql-backup`](https://github.com/igor-kremin/mysql-backup) source code
  for ideas, how to speed up MySQL backup creation, using [`SELECT ... INTO OUTFILE Statement`](https://dev.mysql.com/doc/refman/8.0/en/select-into.html)
  and speed up backups restore, using [`LOAD DATA Statement`](https://dev.mysql.com/doc/refman/8.0/en/load-data.html).

